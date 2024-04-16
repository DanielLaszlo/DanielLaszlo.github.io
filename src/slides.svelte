<script lang="ts">
	import { Presentation, Slide, Code } from '@components'
	import { signal } from '@motion'

	import StickFigure from './StickFigure.svelte'

	const R = 40
	const scale = R / 10

	const O = { x: 200, y: 200 }

	// const circle = signal(
	// 	{ x: 0, y: 200, r: R, fill: '#00ffff' },
	// 	{ duration: 6000 }
	// )

	function perimeter(r) {
		return 2 * Math.PI * r
	}

	const around_big_circle = signal({ t: 0 }, { duration: 10000 })
	const movement_R = R * 3 + R

	$: distance_travelled =
		perimeter(movement_R) * ($around_big_circle.t / (2 * Math.PI))

	$: circle = {
		x: O.x + movement_R * Math.cos($around_big_circle.t),
		y: O.y + movement_R * Math.sin($around_big_circle.t),
		r: R,
		// fill: '#00ffff',
		fill: '#F2A65A'
	}

	const big_circle = {
		x: O.x,
		y: O.y,
		r: 3 * R,
		fill: '#09814A',
		stroke: '#363537',
	}

	const line = signal(
		{ x1: 0, y1: 200, x2: -R, y2: 200, stroke: '#00aaff' },
		{ duration: 2000 }
	)

	async function animate() {
		while (true) {
			// await circle.to({ x: 3 * 2 * R * Math.PI, fill: '#ffff00' })
			// await circle.to({ x: 0, fill: '#00ffff' })
			await around_big_circle.to({ t: 2 * Math.PI })
			await around_big_circle.to({ t: 0 * Math.PI })
		}
	}

	function wtf(x, y, r) {
		console.log(x, y, r)
	}

	// $: angle = 2 * Math.PI * (circle.x / (2 * circle.r * Math.PI))
	$: angle = 2 * Math.PI * (distance_travelled / perimeter(R))

	const n_spokes = 8
	$: spoke_angles = Array.from({ length: n_spokes }, (_, i) => {
		const a = 2 * Math.PI * (i / n_spokes) + angle
		return a
	})

	// const phase = Math.PI
	const phase = 0
	$: dx = circle.r * Math.cos(angle - phase / 4)
	$: dy = circle.r * Math.sin(angle - phase / 4)
</script>

<Presentation>
	<Slide animate>
		<p class="font-bold text-8xl">◉</p>
		<p class="font-bold text-8xl">Circling a circle with a circle</p>
		(press Space)
	</Slide>

	<Slide animate on:in={animate}>
		<svg
			class="w-full h-[400px] mx-auto"
			viewBox="0 0 400 400"
			data-id="animation"
		>
			<circle cx={circle.x} cy={circle.y} r={circle.r} fill={circle.fill} />

			{#each spoke_angles as angle}
				<line
					x1={circle.x + 0.9 * circle.r * Math.cos(angle)}
					y1={circle.y + 0.9 * circle.r * Math.sin(angle)}
					x2={circle.x + circle.r * Math.cos(angle)}
					y2={circle.y + circle.r * Math.sin(angle)}
					stroke={$line.stroke}
					stroke-width={'4'}
				/>
			{/each}

			<!-- The big circle -->
			<circle
				cx={big_circle.x}
				cy={big_circle.y}
				r={big_circle.r}
				fill={big_circle.fill}
				stroke={big_circle.stroke}
				stroke-width={'4'}
			/>

			<!-- The signaller -->
			<line
				x1={circle.x}
				y1={circle.y}
				x2={circle.x + dx}
				y2={circle.y + dy}
				stroke={$line.stroke}
				stroke-width={'4'}
			/>
			<StickFigure
				{scale}
				x={circle.x}
				y={circle.y}
				angle={-$around_big_circle.t - Math.PI / 2}
				fill={'grey'}
			/>

			<text
				x={circle.x}
				y={circle.y}
				font-size={circle.r * 0.4}
				font-family="JetBrains Mono"
				text-anchor="middle"
				dominant-baseline="middle"
			>
				{(angle / (2 * Math.PI)).toFixed(2)}
			</text>
		</svg>
	</Slide>

	<!-- <Slide animate> -->
	<!-- 	<p class="font-bold text-6xl mt-8">🪄 Animotion</p> -->
	<!---->
	<!-- 	<div class="w-max mx-auto mt-16"> -->
	<!-- 		<Code lang="ts" lines="1,4|2|3|1-4"> -->
	<!-- 			{` -->
	<!-- 				const circle = signal( -->
	<!-- 					{ x: 0, y: 200, r: 80, fill: '#00ffff' }, -->
	<!-- 					{ duration: 2000 } -->
	<!-- 				) -->
	<!-- 			`} -->
	<!-- 		</Code> -->
	<!-- 	</div> -->
	<!-- </Slide> -->
	<!---->
	<!-- <Slide on:out={() => circle.reset()} animate> -->
	<!-- 	<p class="font-bold text-6xl mt-8">🪄 Animotion</p> -->
	<!---->
	<!-- 	<div class="w-max mx-auto mt-16"> -->
	<!-- 		<Code -->
	<!-- 			lang="ts" -->
	<!-- 			lines="1,4|2|3|1-4" -->
	<!-- 			steps={[ -->
	<!-- 				['2', async () => await circle.to({ x: 400, fill: '#ffff00' })], -->
	<!-- 				['3', async () => await circle.to({ x: 0, fill: '#00ffff' })], -->
	<!-- 				['1-4', () => circle.reset()], -->
	<!-- 			]} -->
	<!-- 		> -->
	<!-- 			{` -->
	<!-- 				async function animate() { -->
	<!-- 					await circle.to({ x: 400, fill: '#ffff00' }) -->
	<!-- 					await circle.to({ x: 0, fill: '#00ffff' }) -->
	<!-- 				} -->
	<!-- 			`} -->
	<!-- 		</Code> -->
	<!-- 	</div> -->
	<!---->
	<!-- 	<svg -->
	<!-- 		class="w-full h-[400px] mx-auto" -->
	<!-- 		viewBox="0 0 400 400" -->
	<!-- 		data-id="animation" -->
	<!-- 	> -->
	<!-- 		<circle cx={$circle.x} cy={$circle.y} r={$circle.r} fill={$circle.fill} /> -->
	<!-- 		<text -->
	<!-- 			x={$circle.x} -->
	<!-- 			y={$circle.y} -->
	<!-- 			font-size={$circle.r * 0.4} -->
	<!-- 			font-family="JetBrains Mono" -->
	<!-- 			text-anchor="middle" -->
	<!-- 			dominant-baseline="middle" -->
	<!-- 		> -->
	<!-- 			{$circle.x.toFixed(0)} -->
	<!-- 		</text> -->
	<!-- 	</svg> -->
	<!-- </Slide> -->
	<!---->
	<!-- <Slide animate> -->
	<!-- 	<p class="font-bold text-6xl mt-8">🪄 Animotion</p> -->
	<!-- 	<p class="mt-16 text-3xl"> -->
	<!-- 		Learn more by reading the -->
	<!-- 		<a -->
	<!-- 			class="underline" -->
	<!-- 			href="https://animotion.pages.dev/docs" -->
	<!-- 			target="_blank" -->
	<!-- 		> -->
	<!-- 			Animotion docs -->
	<!-- 		</a>. -->
	<!-- 	</p> -->
	<!-- </Slide> -->
</Presentation>
