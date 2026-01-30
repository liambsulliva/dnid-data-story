<script lang="ts">
	import { onMount } from 'svelte';
	import { base } from '$app/paths';

	import BarChart from '$lib/BarChart.svelte';
	import InteractiveCollage from '$lib/InteractiveCollage.svelte';
	import Link from '$lib/Link.svelte';
	import ComparisonChart from '$lib/ComparisonChart.svelte';

	let isTouchDevice = false;
	onMount(() => {
		if ('ontouchstart' in window || navigator.maxTouchPoints > 0) {
			isTouchDevice = true;
		}
	});
</script>

<main class="mx-auto w-full max-w-[1200px]">
	<div class="flex justify-center">
		<div class="relative inline-flex">
			<h1 class="m-24 text-center font-freightText text-5xl font-bold max-md:mx-6">
				How Expensive Are Fruits, Really?
			</h1>
			<div class="absolute bottom-[5rem] left-12 right-12 h-[2px] bg-black"></div>
			<div class="absolute bottom-[4.75rem] left-12 right-12 h-[2px] bg-black"></div>
		</div>
	</div>
	<section class="sm:mt-8 lg:mx-8">
		<div class="px-6">
			<div class="flex flex-col items-center gap-16 lg:flex-row">
				<div>
					<p class="py-3 font-proximaNova text-lg">
						There has been a ton of chatter about the grocery prices as of late. Inflation ballooned
						during the pandemic period, and despite the federal reserve’s best efforts, many are
						still reeling from the higher prices that resulted from that period of high inflation.
					</p>
					<p class="py-3 font-proximaNova text-lg">
						One of the major points that president-elect Donald Trump ran on in his 2024 campaign
						was lowering the cost of grocery items. Namely, the <Link
							href="https://www.newsweek.com/trump-promised-cheaper-eggscan-he-crack-it-2020449"
							>cost of eggs</Link
						>.
					</p>
					<p class="py-3 font-proximaNova text-lg">
						But are groceries as expensive as people make them out to be? It depends. Staple complex
						carbohydrates like corn and wheat are grown domestically in the United States. <Link
							href="https://en.wikipedia.org/wiki/Corn_syrup">Corn syrup</Link
						> is used in so many processed foods because of how cheap it is to manufacture. It doesn't
						require any sort of foreign imports to make, while being irresistibly delicious. The domestic
						availability factor is the reason we consistently reach out to the corn substance and not,
						say, agave syrup.
					</p>
				</div>
				<div>
					<div class="relative">
						<div
							class="text-md absolute -top-2.5 right-20 z-10 border border-[#000] bg-white px-4 py-2 font-proximaNova text-lg font-bold shadow-[4px_4px_0px_#000] lg:-right-2.5"
						>
							{isTouchDevice ? 'Drag Me!' : 'Hover Me!'}
							<div class="absolute -right-20 -top-12 w-24 rotate-12">
								<img src="{base}/emphasis-scribble.svg" alt="Emphasis Scribble" />
							</div>
						</div>
						<InteractiveCollage />
					</div>
				</div>
			</div>
			<p class="py-3 font-proximaNova text-lg">
				Fruit is a little different, though. Fruits are seasonally grown, can only grow in specific
				environments, and need to be kept in specific conditions to remain fresh for the end
				consumer. As such, there is a large degree of variability in how much certain fruits cost.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				This makes fruits act as a microcosm of the grocery market. Looking at the prices of fruits
				can leave clues as to the economic viability of foreign imports from certain countries over
				others. It can also reveal characteristics of the fruits themselves. Which are more likely
				to spoil? Which are harder to grow?
			</p>
			<p class="py-3 font-proximaNova text-lg">
				This write-up will explore these trends and help to demystify the sometimes massive
				differences in how certain fruits are priced, relative to the rest of the market. It'll
				tackle all of fruit's different marketable forms. Fresh, canned, juiced, nothing is off
				limits.
			</p>
			<p class="py-3 font-proximaNova text-lg">Let's dive in.</p>
		</div>
	</section>
	<section class="mt-32 lg:mx-8">
		<div class="relative mx-6 mb-12 inline-flex items-center">
			<h2 class="mr-2 text-center font-freightText text-5xl font-bold">Fresh</h2>
			<img src="{base}/Apple.svg" alt="Apple" />
			<div class="absolute bottom-0 left-0 right-0">
				<img src="{base}/header-scribble.svg" alt="Underline" class="w-32" />
			</div>
		</div>
		<div class="mx-6">
			<BarChart
				form={'Fresh'}
				fruits={[
					'Apples',
					'Bananas',
					'Blueberries',
					'Blackberries',
					'Cherries',
					'Grapes',
					'Kiwi',
					'Mangoes',
					'Nectarines',
					'Pears',
					'Pineapple',
					'Plum',
					'Raspberries',
					'Strawberries',
					'Watermelon'
				]}
			/>
			<div class="my-8">
				<p class="mt-8 py-3 font-proximaNova text-lg">
					Looking at the full graph, there are a few trends that jump out immediately. For one,
					small fruits like berries are far more likely to be expensive. Interestingly, tropical
					fruits are the least expensive, despite needing to grow in very specific environments.
				</p>
			</div>
			<BarChart
				form={'Fresh'}
				fruits={['Raspberries', 'Blueberries', 'Blackberries', 'Cherries', 'Strawberries']}
			/>
			<div class="my-8">
				<p class="mt-8 py-3 font-proximaNova text-lg">
					Raspberries ($7.73/lb) and blackberries ($6.75/lb) are among the most expensive fruits
					when bought fresh. This is because they are both small, fragile berries. Berries are
					always hand-picked, being very difficult to automate due to their variability in time to
					ripen and their small size. Raspberries are especially bad, being extremely quick to spoil
					and picky about the season they grow in.<Link
						href="https://www.mashed.com/196372/why-are-raspberries-so-expensive/"
						><sup>1</sup></Link
					> Special care needs to be put into their harvest and transport to ensure that they aren't
					crushed or spoiled by the time they reach the final buyer. This explains its price nearly doubling
					that of the third most expensive fruit, cherries.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					Cherries ($4.66/lb) also fall victim to the small fruit problem and are difficult to
					automate because of that fact. They're hard to grow well, being grown mainly in a 4-month
					period between late spring and early summer in California, Oregon, and Washington. They
					are also not ripe for very long, needing to be carefully monitored to ensure they don't
					spoil.<Link href="https://www.mashed.com/194002/why-are-cherries-so-expensive/"
						><sup>2</sup></Link
					>
				</p>
				<p class="py-3 font-proximaNova text-lg">
					Strawberries ($2.97/lb) suffer from the berry problem, but are a little bigger and
					tougher. Additionally, strawberries can grow in a variety of climates and soil types,
					which gives growers much more flexibility in how they are grown. They have a longer
					growing season, which helps to extend their supply window.
				</p>
			</div>
			<div class="relative">
				<BarChart form={'Fresh'} fruits={['Mangoes', 'Bananas', 'Pineapple', 'Watermelon']} />
				<div
					class="-top-2.5 right-20 z-10 w-full border border-[#000] bg-white px-4 py-2 font-proximaNova text-lg shadow-[4px_4px_0px_#000] lg:absolute lg:w-96"
				>
					<h3 class="font-proximaNova text-2xl font-bold">An aside...</h3>
					<p class="font-proximaNova text-lg">
						The abundance and affordability of tropical fruits is deeply tied to Caribbean labor
						exploitation dating back to the late 1800s. You can read more about it <Link
							href="https://www.thecollector.com/how-tropical-fruits-became-commonplace-usa/"
							>here</Link
						>.
					</p>
				</div>
			</div>

			<div class="my-8">
				<p class="py-3 font-proximaNova text-lg">
					Narrowing in on the tropical fruits reveals that mangoes ($1.47/lb) are easily the most
					expensive of the bunch, easily doubling the price of pineapples ($0.62/lb) and bananas
					($0.60/lb). Considering that they all grow in similar climates, this doesn't make much
					sense at first. However, mangoes are heavier than bananas without having the rugged
					exterior that pineapples and watermelons do. They need to be specially packaged to ensure
					that they both won't smash other fruits and won't get smashed themselves.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					But watermelons ($0.38/lb) are incredibly heavy, right? So why would they be the cheapest
					fruit to buy fresh? For one, watermelons have tough rinds that keep bacteria and animals
					out. This means that growers don't need to worry about cutting into their profit margins
					buying pesticides. Since watermelons greatly vary in size, grocery supply chains will
					usually buy the cheapest watermelons (small ones) in order to pass those savings on to the
					consumer. Buyers won't notice a small drop in quality, but they will notice an increase in
					price.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					Since we are comparing prices pound-for-pound, watermelons enjoy a distinct advantage
					here, being both heavy (full of water!) and easy to distribute since it doesn't take many
					of them to fill up a pallet.
				</p>
			</div>
		</div>
	</section>
	<section class="mt-36 lg:mx-8">
		<div class="relative mx-6 mb-12 inline-flex items-center">
			<h2 class="mr-2 text-center font-freightText text-5xl font-bold">Frozen</h2>
			<img src="{base}/Plum.svg" alt="Plum" />
			<div class="absolute bottom-0 left-0 right-0">
				<img src="{base}/header-scribble.svg" alt="Underline" class="w-36" />
			</div>
		</div>
		<div class="mx-6 my-8">
			<p class="mt-8 py-3 font-proximaNova text-lg">
				At first glance, frozen fruit seems to mirror that of fresh berries. In some ways, this is
				true. The rankings are exactly the same, with raspberries being on top for most expensive,
				followed by blackberries, blueberries, and strawberries. However, the deviation between
				fruit prices is much tighter this time around.
			</p>
		</div>
		<ComparisonChart
			forms={['Frozen', 'Fresh']}
			fruits={['Blueberries', 'Blackberries', 'Raspberries', 'Strawberries']}
		/>
		<div class="mx-6 my-8">
			<p class="py-3 font-proximaNova text-lg">
				Try swapping between frozen and fresh prices and pay close attention to the levels. You can
				see the gap between blackberries and blueberries visibly close when the label swaps from
				fresh to frozen. Looking at the y-axis, you can see that frozen raspberries ($6.16/lb) cost
				over a dollar less than buying them fresh ($7.73/lb). Frozen berries just cost less than
				their fresh counterparts.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				Why? Because frozen produce can be kept nearly indefinitely. Since berries are quick to
				perish, distributors can keep their existing supply of berries for longer, and have a grace
				period between harvesting and delivering to the end consumer.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				This is absolutely vital when you consider that berries very beholden to their harvest
				seasons.<Link
					href="https://www.gardeningknowhow.com/edible/fruits/berries/harvesting-berries.htm"
					><sup>3</sup></Link
				> When frozen, berries can last well into the winter months where fresh fruit would go bad. The
				savings that come from this long-term storage are passed on to the consumer, leading to frozen
				berries costing anywhere from $0.34 to $1.57 less than they would if they were picked fresh.
			</p>
		</div>
	</section>
	<section class="mt-36 lg:mx-8">
		<div class="relative mx-6 mb-12 inline-flex items-center">
			<h2 class="mr-2 text-center font-freightText text-5xl font-bold">Canned</h2>
			<img src="{base}/Pineapple.svg" alt="Pineapple" />
			<div class="absolute -bottom-1 left-0 right-0">
				<img src="{base}/header-scribble.svg" alt="Underline" class="w-40" />
			</div>
		</div>
		<div class="mx-6 my-8">
			<p class="mt-8 py-3 font-proximaNova text-lg">
				Cherries immediately jump out as an outlier here. Most of the fruits average around $2/lb,
				but cherries sit at a staggering $5.24/lb. This is for a few reasons. For one, the other
				fruits are significantly larger than cherries, and thus distributors need to fill a single
				can with way more cherries for a can of the same volume. Also, the canning process can add a
				decent amount to cost. Even if the aluminum used in the cans themselves are cheap, the
				cherries need to be processed in a factory that they wouldn't have otherwise visited, which
				adds a non-insignificant amount to shipping costs, especially when travelling
				internationally. As stated, cherries are also just expensive.
			</p>
		</div>
		<div class="mx-6 my-8">
			<ComparisonChart
				forms={['Canned', 'Fresh']}
				fruits={['Apricots', 'Cherries', 'Peaches', 'Pears', 'Pineapple']}
			/>
		</div>
		<div class="mx-6 my-8">
			<p class="py-3 font-proximaNova text-lg">
				Comparing to fresh fruit again, flipping back-and-forth shows that cherries are expensive no
				matter the form. Switching to fresh only subtracts $0.58 from the final cost. The other
				fruits are far more interesting, however.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				It's important to mention that while the canning process can be pricey and time-consuming,
				the resulting product is shelf stable, unlike fresh fruit. This gives canned fruits a
				distinct advantage in the supply chain.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				This is particularly relevant when talking about apricots, which sell at $3.62/lb when
				fresh, and drop down to $2.24 when canned. That is a $1.38 difference! This goes against
				conventional wisdom in that you pay a premium for canned goods and their shelf stability.
				While not as extreme, peaches follow this trend too, falling $0.17 under the cost of buying
				fresh.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				So why is this? The best explanation can be found by observing their peak season. Peaches
				and apricots are both at their peak harvest during the summer months. Apricots specifically
				tend to grow erratically, having a tiny window in which they are ripe and ready to eat.<Link
					href="https://www.washingtonpost.com/archive/lifestyle/wellness/1996/08/06/the-tasty-apricot-is-a-misunderstood-fruit/b47d4e49-44b7-449e-a5e7-09f62882bf69/"
					><sup>4</sup></Link
				> Distributors trying to sell fresh apricots need to time their shipping to ensure that the apricot
				gets in the hands of the consumer with at least a day or two's worth of a grace period before
				ripeness. Even if everything goes perfectly, grocery stores can't cycle out apricots daily, and
				some apricots may sit on the shelf longer than others.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				If distributors can the fruits themselves, however, they are able to capture the fruit in
				its peak ripeness and keep it from developing any further. This gives suppliers much more
				control over how the consumer will enjoy the product instead of passing the fresh fruit over
				to a grocer and hoping for the best.
			</p>
		</div>
	</section>
	<section class="mt-36 lg:mx-8">
		<div class="relative mx-6 mb-12 inline-flex items-center">
			<h2 class="mr-4 text-center font-freightText text-5xl font-bold">Dried</h2>
			<img src="{base}/Tomato.svg" alt="Tomatoes" />
			<div class="absolute bottom-0 left-0 right-0">
				<img src="{base}/header-scribble.svg" alt="Underline" class="w-32" />
			</div>
		</div>
		<div class="mx-6">
			<BarChart
				form={'Dried'}
				fruits={[
					'Pineapple',
					'Dates',
					'Figs',
					'Cranberries',
					'Apricots',
					'Grapes',
					'Plum (prunes)',
					'Papaya'
				]}
			/>
			<p class="mt-8 py-3 font-proximaNova text-lg">
				Dried fruits are more expensive on average than any other type of fruit for one simple
				reason. Volume. The majority of what gives fruits their shape is water. It's why grapes
				explode in your mouth and why watermelon tastes so crunchy. Water is a large part of what
				makes fruits work, along with the natural sugars (fructose and glucose), of course.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				Having little to no water means that you can pack far more of a given fruit in the same
				container. You can easily put double the number of dried cherries in the same container as
				one with fresh cherries, for example.<Link
					href="https://kingorchards.com/product/1-lb-unsweetened-dried-cherries/"
					><sup>5</sup></Link
				> It's important to note raisins as an important outlier here, though. When considering their
				significant shrink in volume, they are really not that much more expensive than raw grapes ($3.99/lb).
				This is often due to raisins being made from lower quality "Thompson" grapes.<Link
					href="https://www.sandiegoreader.com/news/2012/aug/22/straight-cost-grapes-raisins/"
					><sup>6</sup></Link
				>
			</p>
			<ComparisonChart
				forms={['Fresh', 'Canned']}
				fruits={[
					'Pineapple',
					'Dates',
					'Figs',
					'Cranberries',
					'Apricots',
					'Grapes',
					'Plum',
					'Papaya'
				]}
			/>
			<p class="py-3 font-proximaNova text-lg">
				As a refresher, here they are compared to their fresh and canned forms. Even by looking at
				the y-axis ticks alone, it's clear that dried fruit are in an entirely different league of
				pricing. You're going to be paying a lot more than canned options, and fresh fruit is still
				significantly cheaper if you don't mind losing the shelf stability.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				Any kind of advantage that dried fruit would have had due to shelf stability is completely
				lost in the volume argument. That doesn't make dried fruit <i>bad</i>, though. Just
				different. You will likely end up paying more upfront, but you will receive more in caloric
				intake for that money.
			</p>
		</div>
	</section>
	<section class="mt-36 lg:mx-8">
		<div class="relative mx-6 mb-12 inline-flex items-center">
			<h2 class="mr-2 text-center font-freightText text-5xl font-bold">... Juice?</h2>
			<img src="{base}/Lemons.svg" alt="Lemons" />
			<div class="absolute bottom-0 left-1 right-0">
				<img src="{base}/header-scribble.svg" alt="Underline" class="w-36" />
			</div>
		</div>
		<div class="mx-6">
			<BarChart
				form={'Juice'}
				fruits={['Apples', 'Grapefruit', 'Grapes', 'Oranges', 'Pineapple', 'Pomegranate']}
			/>
			<p class="mt-8 py-3 font-proximaNova text-lg">
				It might seem silly to place <i>Juice</i> on the same playing field as fresh, frozen, or
				canned options (despite <Link href="https://www.myplate.gov/eat-healthy/fruits"
					>MyPlate</Link
				> pushing it since 2011). In some ways, it is. Nutritionally, fruit juices bypass the stomach's
				digestion of the fiber present in whole fruits that support your gut microbiome.<Link
					href="https://www.houstonmethodist.org/blog/articles/2022/sep/7-benefits-of-fiber-that-should-convince-you-to-eat-enough-of-it/"
					><sup>7</sup></Link
				> They also tend to include nasty sweeteners like corn syrup. With that said, fruit juice prices
				are still tied to the abundance of the fruits that they are derived from.
			</p>
			<p class="py-3 font-proximaNova text-lg">
				The standout here is pomegranate ($3.28/pint). It nearly triples everything else on the
				chart, with everything else hovering around a little over a dollar. So why is this?
			</p>
			<p class="py-3 font-proximaNova text-lg">
				It's surprisingly simple. Only 56% of a given pomegranate is directly edible.<Link
					href="https://www.ers.usda.gov/data-products/fruit-and-vegetable-prices"
					><sup>8</sup></Link
				> The inconvenience of needing to ship the entire fruit only for the seeds to be thrown out before
				being processed in a juicer is a waste of space and weight that could have gone to another fruit
				instead. Additionally, the fruit is quite difficult to cut, so very often humans have to prepare
				the fruits for processing by hand. It's a very expensive process, and those costs are handed
				off to the consumer.
			</p>
		</div>
	</section>
	<section class="relative">
		<div class="flex w-full flex-col justify-start">
			<div class="relative w-full">
				<h1 class="mx-14 my-24 text-left font-freightText text-5xl font-bold max-md:mx-6">
					In Conclusion...
				</h1>
				<div class="absolute bottom-[5rem] left-9 right-9 h-[2px] bg-black"></div>
				<div class="absolute bottom-[4.75rem] left-9 right-9 h-[2px] bg-black"></div>
			</div>
			<div class="mx-14 mb-24">
				<ComparisonChart
					forms={['Fresh', 'Frozen', 'Canned', 'Dried', 'Juice']}
					fruits={[
						'Apples',
						'Apricots',
						'Bananas',
						'Blueberries',
						'Blackberries',
						'Cranberries',
						'Dates',
						'Figs',
						'Grapefruit',
						'Grapes',
						'Cherries',
						'Grapes',
						'Kiwi',
						'Mangoes',
						'Nectarines',
						'Oranges',
						'Papaya',
						'Peaches',
						'Pears',
						'Pineapple',
						'Plum',
						'Pomegranate',
						'Raspberries',
						'Strawberries',
						'Watermelon'
					]}
				/>
				<p class="mt-8 py-3 font-proximaNova text-lg">
					Fruits have a lot more character than meets the eye. The way they are bought, sold, and
					moved around is directly linked to each fruit's distinct set of quirks. There is always a
					reason a particular fruit is priced the way it is. Oftentimes it comes down to how much
					manpower (or lack thereof) is required to get it from the plant to your grocery store.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					I exclusively leaned on bar graphs for this story because they are exceptionally good at
					showing off comparisons. I was initially planning on using a line graph to show the
					differences in pricing over time, playing off of the <Link
						href="https://www.cnbc.com/2022/07/13/inflation-rose-9point1percent-in-june-even-more-than-expected-as-price-pressures-intensify.html"
						>COVID-era inflationary spike</Link
					>. Unfortunately, the data I needed didn't offer the granularity I was looking for, so I
					decided to keep things simple instead.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					If I were to expand upon this project in the future, I would include animations to grow
					and shrink the bars when the button to change the form of fruit was clicked. I would also
					include range sliders on the pricing axis, so viewers can zoom in on minute differences
					between bars. This would have helped in the <i>Canned</i> and <i>Juice</i> sections, since
					it is quite difficult to tell the difference between the bottom 4 bars unless you rely heavily
					on the hover/tap tooltip.
				</p>
				<p class="py-3 font-proximaNova text-lg">
					Design wise, I would also include illustrations on top of each bar. It would give each
					fruit a visual representation apart from the colored bar, making it easier for the viewer
					to make a visual connection between the fruit and its cost. I would also replace the
					header illustrations with more relevant illustrations (i.e. a vintage ice cube for the <i
						>Frozen</i
					> section), but that would be highly dependent on the assets I have at my disposal. Despite
					these small gripes, I would largely consider this project a success.
				</p>
			</div>
		</div>
	</section>
</main>
<footer class="m-6 font-proximaNova">
	<div class="flex justify-between max-md:flex-col">
		<p class="font-proximaNova text-lg">
			Made with ❤️ by <Link href="https://liambsullivan.com">Liam Sullivan</Link>
		</p>
		<p class="font-proximaNova text-lg max-md:hidden">
			Sourced from <Link href="https://www.ers.usda.gov/data-products/fruit-and-vegetable-prices">
				The US Department of Agriculture</Link
			>
		</p>
		<p class="font-proximaNova text-lg md:hidden">
			using <Link href="https://www.ers.usda.gov/data-products/fruit-and-vegetable-prices">
				US Dept. of Agriculture</Link
			> datasets
		</p>
	</div>
</footer>
