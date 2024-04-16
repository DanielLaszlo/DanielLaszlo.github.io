<script lang="ts">
	export let x = 400
	export let y = 400

	export let scale = 10

	export let fill = 'white'
	$: stroke = fill

	let strokeWidth = 0.3 * scale

	$: hip = { x: x, y: y - 5 * scale }

	$: left_foot = { x: x - 2 * scale, y: y }
	$: right_foot = { x: x + 2 * scale, y: y }

	$: neck = { x: hip.x, y: hip.y - 8 * scale }

	$: shoulder = { x: neck.x, y: neck.y + 2 * scale }

	$: right_hand = { x: shoulder.x + 4 * scale, y: shoulder.y - 2 * scale }
	// console.log(right_hand);
	$: left_hand = { x: shoulder.x - 4 * scale, y: shoulder.y - 2 * scale }
	// console.log(left_hand);

	$: r = 1.5 * scale
	$: head = { x: neck.x, y: neck.y - r, r: r }

</script>

<!-- Legs -->
<line
	x1={left_foot.x}
	y1={left_foot.y}
	x2={hip.x}
	y2={hip.y}
	{stroke}
	stroke-width={strokeWidth}
/>
<line
	x1={right_foot.x}
	y1={right_foot.y}
	x2={hip.x}
	y2={hip.y}
	{stroke}
	stroke-width={strokeWidth}
/>

<!-- Body -->
<line
	x1={hip.x}
	y1={hip.y}
	x2={neck.x}
	y2={neck.y}
	{stroke}
	stroke-width={strokeWidth}
/>

<!-- Arms -->
<line
	x1={shoulder.x}
	y1={shoulder.y}
	x2={right_hand.x}
	y2={right_hand.y}
	{stroke}
	stroke-width={strokeWidth}
/>
<line
	x1={shoulder.x}
	y1={shoulder.y}
	x2={left_hand.x}
	y2={left_hand.y}
	{stroke}
	stroke-width={strokeWidth}
/>

<!-- Head -->
<circle cx={head.x} cy={head.y} r={head.r} {fill} stroke-width={strokeWidth} />
