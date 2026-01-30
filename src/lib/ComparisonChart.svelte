<script lang="ts">
	import * as d3 from 'd3';
	import { base } from '$app/paths';

	interface FruitData {
		Fruit: string;
		Form: string;
		RetailPrice: number;
		RetailPriceUnit: string;
		Yield: number;
		CupEquivalentSize: number;
		CupEquivalentUnit: string;
		CupEquivalentPrice: number;
		FruitColor: string;
	}

	const { fruits = [], forms = [] as string[] } = $props<{
		fruits: string[];
		forms?: string[];
	}>();

	let activeForm = $state(forms[0] ?? '');
	let chartContainer: HTMLElement;
	let isTouchDevice =
		typeof window !== 'undefined' && ('ontouchstart' in window || navigator.maxTouchPoints > 0);
	let previousWidth = 0;

	// Parse CSV
	const loadData = async (): Promise<FruitData[]> => {
		const response = await fetch(`${base}/Fruit-Prices-2022-Modified.csv`);
		const text = await response.text();
		return d3.csvParse(text) as unknown as FruitData[];
	};

	const createChart = async () => {
		d3.select(chartContainer).selectAll('*').remove();

		// Load data based on props
		const data = await loadData();
		const filteredData = data
			.filter((d) => {
				const matchesFruit = fruits.includes(d.Fruit);
				const matchesForm = activeForm ? d.Form === activeForm : true;
				return matchesFruit && matchesForm;
			})
			.sort((a, b) => b.RetailPrice - a.RetailPrice);

		// Get container dimensions
		const containerWidth = chartContainer.clientWidth;
		const isMobileLayout = containerWidth < 768;
		const numberOfBars = filteredData.length;

		// Mobile-specific dimensions
		const barHeight = 30;
		const barSpacing = 10;

		// Responsive margins
		const margin = {
			top: 20,
			right: isMobileLayout ? 10 : containerWidth < 450 ? 10 : containerWidth < 800 ? 20 : 30,
			bottom: isMobileLayout ? 50 : containerWidth < 450 ? 80 : containerWidth < 800 ? 60 : 70,
			left: isMobileLayout
				? containerWidth < 450
					? 75
					: 120
				: containerWidth < 450
					? 40
					: containerWidth < 800
						? 60
						: 80
		};

		// Calculate dimensions
		const width = containerWidth - margin.left - margin.right;
		let height = isMobileLayout
			? numberOfBars * barHeight + (numberOfBars - 1) * barSpacing
			: Math.min(500, containerWidth * 0.6) - margin.top - margin.bottom;

		// Create SVG
		const svg = d3
			.select(chartContainer)
			.append('svg')
			.attr('width', '100%')
			.attr('height', height + margin.top + margin.bottom)
			.attr(
				'viewBox',
				`0 0 ${width + margin.left + margin.right} ${height + margin.top + margin.bottom}`
			)
			.append('g')
			.attr('transform', `translate(${margin.left},${margin.top})`);

		// Create scales
		let x, y;
		if (isMobileLayout) {
			x = d3
				.scaleLinear()
				.domain([0, d3.max(filteredData, (d) => d.RetailPrice) || 0])
				.range([0, width]);

			y = d3
				.scaleBand()
				.domain(filteredData.map((d) => d.Fruit))
				.range([height, 0])
				.padding(barSpacing / (barHeight + barSpacing));
		} else {
			x = d3
				.scaleBand()
				.domain(filteredData.map((d) => d.Fruit))
				.range([0, width])
				.padding(containerWidth < 450 ? 0.05 : containerWidth < 800 ? 0.1 : 0.15);

			y = d3
				.scaleLinear()
				.domain([0, d3.max(filteredData, (d) => d.RetailPrice) || 0])
				.range([height, 0]);
		}

		// Create tooltip
		const tooltip = d3
			.select(chartContainer)
			.append('div')
			.attr('class', 'tooltip')
			.style('opacity', 0)
			.style('position', 'absolute')
			.style('background-color', 'rgba(255, 255, 255, 1)')
			.style('border', '1px solid #000')
			.style('padding', containerWidth < 800 ? '8px' : '12px')
			.style('font-family', 'proxima-nova')
			.style('font-size', containerWidth < 450 ? '12px' : containerWidth < 800 ? '14px' : '16px')
			.style('pointer-events', 'none')
			.style('box-shadow', '4px 4px 0px #000');

		// Create bars
		const bars = svg
			.selectAll('rect')
			.data(filteredData)
			.enter()
			.append('rect')
			.attr('fill', (d) => d.FruitColor)
			.attr('stroke', 'black')
			.attr('stroke-width', 2)
			.attr('rx', containerWidth < 450 ? 2 : containerWidth < 800 ? 4 : 6);

		const darkenColor = (color: string, factor = 0.8): string => {
			const c = d3.color(color);
			return c ? c.darker(factor).toString() : color;
		};

		const originalColor = (d: any) => d.FruitColor;

		// Construct bars based on layout
		if (isMobileLayout) {
			bars
				.attr('x', 0)
				.attr('y', (d) => (y as d3.ScaleBand<string>)(d.Fruit)!)
				.attr('width', (d) => (x as d3.ScaleLinear<number, number>)(d.RetailPrice))
				.attr('height', (y as d3.ScaleBand<string>).bandwidth());
		} else {
			bars
				.attr('x', (d) => (x as d3.ScaleBand<string>)(d.Fruit)!)
				.attr('y', (d) => (y as d3.ScaleLinear<number, number>)(d.RetailPrice))
				.attr('width', (x as d3.ScaleBand<string>).bandwidth())
				.attr('height', (d) => height - (y as d3.ScaleLinear<number, number>)(d.RetailPrice));
		}

		// Hover/Touch handling (same as previous implementation)
		if (isTouchDevice) {
			let activeBar: any = null;
			bars.on('click', function (event, d) {
				event.stopPropagation();
				const isActive = activeBar === this;

				if (isActive) {
					d3.select(this).attr('fill', darkenColor(d.FruitColor));
					tooltip.style('opacity', 0);
					activeBar = null;
				} else {
					if (activeBar) d3.select(activeBar).attr('fill', originalColor);
					activeBar = this;
					d3.select(this).attr('fill', darkenColor(d.FruitColor));
					tooltip
						.style('opacity', 1)
						.html(
							`
							<div style="font-weight: bold">${d.Fruit}</div>
							<div>$${Number(d.RetailPrice).toFixed(2)} ${d.RetailPriceUnit}</div>
						`
						)
						.style('left', `${event.pageX + 10}px`)
						.style('top', `${event.pageY - 28}px`);
				}
			});

			d3.select('body').on('click', () => {
				if (activeBar) {
					tooltip.style('opacity', 0);
					d3.select(activeBar).attr('fill', originalColor);
					activeBar = null;
				}
			});
		} else {
			bars
				.on('mouseover', function (event, d) {
					d3.select(this).transition().duration(200).attr('fill', darkenColor(d.FruitColor));
					tooltip
						.style('opacity', 1)
						.html(
							`
							<div style="font-weight: bold">${d.Fruit}</div>
							<div>$${Number(d.RetailPrice).toFixed(2)} ${d.RetailPriceUnit}</div>
						`
						)
						.style('left', `${event.pageX + 10}px`)
						.style('top', `${event.pageY - 28}px`);
				})
				.on('mousemove', function (event) {
					tooltip.style('left', `${event.pageX + 10}px`).style('top', `${event.pageY - 28}px`);
				})
				.on('mouseout', function () {
					d3.select(this).transition().duration(200).attr('fill', originalColor);
					tooltip.style('opacity', 0);
				});
		}

		// Create axes
		if (isMobileLayout) {
			const xAxis = svg
				.append('g')
				.attr('transform', `translate(0, ${height})`)
				.call(
					d3
						.axisBottom(x as d3.ScaleLinear<number, number>)
						.ticks(5)
						.tickFormat((d) => `$${Number(d).toFixed(2)}`)
				);

			xAxis
				.selectAll('text')
				.style('font-size', containerWidth < 450 ? '10px' : '12px')
				.style('font-family', 'proxima-nova');

			const yAxis = svg.append('g').call(d3.axisLeft(y as d3.ScaleBand<string>));

			yAxis
				.selectAll('text')
				.style('font-size', containerWidth < 450 ? '10px' : '12px')
				.style('font-family', 'proxima-nova');

			svg
				.append('text')
				.attr('transform', `translate(${width / 2}, ${height + margin.bottom - 10})`)
				.style('text-anchor', 'middle')
				.style('font-family', 'proxima-nova')
				.style('font-size', containerWidth < 450 ? '12px' : '14px')
				.text('Retail Price ($)');
		} else {
			const xAxis = svg
				.append('g')
				.attr('transform', `translate(0,${height})`)
				.call(d3.axisBottom(x as d3.ScaleBand<string>));

			xAxis
				.selectAll('text')
				.attr('transform', 'rotate(-45)')
				.style('text-anchor', 'end')
				.style('font-size', containerWidth < 450 ? '10px' : containerWidth < 800 ? '12px' : '14px')
				.style('font-family', 'proxima-nova');

			const yAxis = svg.append('g').call(d3.axisLeft(y as d3.ScaleLinear<number, number>));

			if (containerWidth < 450) {
				yAxis.selectAll('text').style('font-size', '10px').style('font-family', 'proxima-nova');
				yAxis.call(
					d3
						.axisLeft(y as d3.ScaleLinear<number, number>)
						.tickFormat((d) => `$${Number(d).toFixed(2)}`)
						.ticks(5)
				);
			} else if (containerWidth < 800) {
				yAxis.selectAll('text').style('font-size', '12px').style('font-family', 'proxima-nova');
				yAxis.call(
					d3
						.axisLeft(y as d3.ScaleLinear<number, number>)
						.tickFormat((d) => `$${Number(d).toFixed(2)}`)
						.ticks(7)
				);
			} else {
				yAxis.selectAll('text').style('font-size', '14px').style('font-family', 'proxima-nova');
				yAxis.call(
					d3
						.axisLeft(y as d3.ScaleLinear<number, number>)
						.tickFormat((d) => `$${Number(d).toFixed(2)}`)
						.ticks(10)
				);
			}

			if (containerWidth >= 450) {
				svg
					.append('text')
					.attr('transform', 'rotate(-90)')
					.attr('y', 0 - margin.left)
					.attr('x', 0 - height / 2)
					.attr('dy', '1em')
					.style('text-anchor', 'middle')
					.style('font-family', 'proxima-nova')
					.style('font-size', containerWidth < 800 ? '12px' : '18px')
					.text('Retail Price ($)');
			}
		}
	};

	let resizeObserver: ResizeObserver;

	$effect(() => {
		if (fruits.length > 0 && forms.length > 0) {
			createChart();

			if (!resizeObserver) {
				resizeObserver = new ResizeObserver(
					debounce((entries: ResizeObserverEntry[]) => {
						const currentWidth = entries[0].contentRect.width;

						// Only re-render if width has meaningfully changed
						if (Math.abs(currentWidth - previousWidth) > 10) {
							createChart();
							previousWidth = currentWidth;
						}
					}, 250)
				);
				resizeObserver.observe(chartContainer);
			}
		}

		return () => {
			if (resizeObserver) {
				resizeObserver.disconnect();
			}
		};
	});

	function debounce(func: Function, wait: number) {
		let timeout: number;
		return function executedFunction(...args: any[]) {
			const later = () => {
				clearTimeout(timeout);
				func(...args);
			};
			clearTimeout(timeout);
			timeout = setTimeout(later, wait);
		};
	}

	const cycleForm = () => {
		const currentIndex = forms.indexOf(activeForm);
		const nextIndex = (currentIndex + 1) % forms.length;
		activeForm = forms[nextIndex];
		createChart();
	};
</script>

<div class="relative">
	{#if forms.length > 1}
		<button
			class="text-md absolute right-6 top-4 z-0 border border-[#000] bg-white px-4 py-2 font-proximaNova text-lg font-bold shadow-[4px_4px_0px_#000] hover:bg-gray-100"
			onclick={cycleForm}
		>
			{activeForm}
		</button>
	{/if}
</div>

<div class="chart-container" bind:this={chartContainer}></div>

<style>
	.chart-container {
		width: 100%;
		max-width: 1200px;
		height: auto;
		min-height: 100px;
	}
</style>
