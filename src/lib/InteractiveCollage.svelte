<script lang="ts">
	import { spring } from 'svelte/motion';
	import { base } from '$app/paths';

	// 3D position for spring()
	interface Position {
		x: number;
		y: number;
	}

	export let size: number = 300;

	// Default positions
	const defaults = {
		svg1: { x: -80, y: -60 },
		svg2: { x: 80, y: -60 },
		svg3: { x: 20, y: 60 }
	};

	// Spring stores for each SVG's position
	const pos1 = spring<Position>(defaults.svg1, {
		stiffness: 0.1,
		damping: 0.6
	});
	const pos2 = spring<Position>(defaults.svg2, {
		stiffness: 0.1,
		damping: 0.6
	});
	const pos3 = spring<Position>(defaults.svg3, {
		stiffness: 0.1,
		damping: 0.6
	});

	let isDragging = false;
	let startX: number, startY: number;
	let currentPos: typeof pos1;
	let currentDefault: Position;

	function handleMouseMove(event: MouseEvent, pos: typeof pos1, defaultPos: Position) {
		const container = event.currentTarget as HTMLDivElement;
		const rect = container.getBoundingClientRect();
		const mouseX = event.clientX - rect.left;
		const mouseY = event.clientY - rect.top;

		// Calc center of object
		const centerX = rect.width / 2;
		const centerY = rect.height / 2;

		// Further mouse is away from center, the more it sticks to original point
		const deltaX = (mouseX - centerX) / 10;
		const deltaY = (mouseY - centerY) / 10;

		pos.set({
			x: defaultPos.x + deltaX,
			y: defaultPos.y + deltaY
		});
	}

	function handleMouseLeave(pos: typeof pos1, defaultPos: Position) {
		pos.set({ ...defaultPos });
	}

	function handleTouchStart(event: TouchEvent, pos: typeof pos1, defaultPos: Position) {
		isDragging = true;
		currentPos = pos;
		currentDefault = defaultPos;

		const touch = event.touches[0];
		const container = event.currentTarget as HTMLDivElement;
		const rect = container.getBoundingClientRect();

		startX = touch.clientX - rect.left;
		startY = touch.clientY - rect.top;
	}

	function handleTouchMove(event: TouchEvent) {
		if (!isDragging) return;

		const touch = event.touches[0];
		const container = event.currentTarget as HTMLDivElement;
		const rect = container.getBoundingClientRect();

		const touchX = touch.clientX - rect.left;
		const touchY = touch.clientY - rect.top;

		// Reduce delta to make dragging feel more responsive
		const deltaX = (touchX - startX) / 2.5;
		const deltaY = (touchY - startY) / 2.5;

		currentPos.set({
			x: currentDefault.x + deltaX,
			y: currentDefault.y + deltaY
		});
	}

	function handleTouchEnd() {
		if (isDragging) {
			isDragging = false;
			currentPos.set({ ...currentDefault });
		}
	}
</script>

<div class="collage-container" on:touchmove|preventDefault>
	<div
		class="svg-container"
		role="img"
		style="
		  transform: translate({$pos1.x}px, {$pos1.y}px);
		  width: {size}px;
		  height: {size}px;
		"
		on:mousemove={(e) => handleMouseMove(e, pos1, defaults.svg1)}
		on:mouseleave={() => handleMouseLeave(pos1, defaults.svg1)}
		on:touchstart={(e) => handleTouchStart(e, pos1, defaults.svg1)}
		on:touchmove={handleTouchMove}
		on:touchend={handleTouchEnd}
	>
		<img src="{base}/Pomegranate.svg" alt="Pomegranate" />
	</div>

	<div
		class="svg-container"
		role="img"
		style="
		  transform: translate({$pos2.x}px, {$pos2.y}px);
		  width: {size}px;
		  height: {size}px;
		"
		on:mousemove={(e) => handleMouseMove(e, pos2, defaults.svg2)}
		on:mouseleave={() => handleMouseLeave(pos2, defaults.svg2)}
		on:touchstart={(e) => handleTouchStart(e, pos2, defaults.svg2)}
		on:touchmove={handleTouchMove}
		on:touchend={handleTouchEnd}
	>
		<img src="{base}/Peach.svg" alt="Peach" />
	</div>

	<div
		class="svg-container"
		role="img"
		style="
		  transform: translate({$pos3.x}px, {$pos3.y}px);
		  width: {size}px;
		  height: {size}px;
		"
		on:mousemove={(e) => handleMouseMove(e, pos3, defaults.svg3)}
		on:mouseleave={() => handleMouseLeave(pos3, defaults.svg3)}
		on:touchstart={(e) => handleTouchStart(e, pos3, defaults.svg3)}
		on:touchmove={handleTouchMove}
		on:touchend={handleTouchEnd}
	>
		<img src="{base}/Grapes.svg" alt="Grapes" />
	</div>
</div>

<style>
	.collage-container {
		position: relative;
		width: 450px;
		height: 450px;
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
	}

	.svg-container {
		position: absolute;
		cursor: pointer;
		touch-action: none;
	}

	.svg-container :global(svg) {
		width: 100%;
		height: 100%;
	}
</style>
