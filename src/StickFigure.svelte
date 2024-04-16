<script lang="ts">
	export let x = 400
	export let y = 400

	export let scale = 10

	export let angle = Math.PI / 2

	export let fill = 'white'
	$: stroke = fill

	let strokeWidth = 0.3 * scale

	function sph2cart(origin, r, theta) {
		return {
			x: origin.x + r * Math.sin(theta),
			y: origin.y + r * Math.cos(theta),
		}
	}

	$: center = { x, y }
	$: hip_length = 0.5 * scale

	$: hip = sph2cart(center, hip_length, angle)
	// $: hip = { x: x, y: y + 2 * scale }

	$: foot_length = 6 * scale

	$: left_foot = sph2cart(center, foot_length, angle + Math.PI / 6)
	$: right_foot = sph2cart(center, foot_length, angle - Math.PI / 6)

	$: neck_length = 4 * scale

	$: neck = sph2cart(center, neck_length, angle - Math.PI)

	$: shoulder_length = 2 * scale

	$: shoulder = sph2cart(center, shoulder_length, angle - Math.PI)

	$: hand_length = 5 * scale

	$: right_hand = sph2cart(shoulder, hand_length, angle + 1 * Math.PI + Math.PI / 4)
	$: left_hand = sph2cart(shoulder, hand_length, angle + 1 * Math.PI - Math.PI / 4)

	$: r = 1.5 * scale
	$: head_length = neck_length + r
	$: head = sph2cart(center, head_length, angle - Math.PI)
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
<circle cx={head.x} cy={head.y} r={r} {fill} stroke-width={strokeWidth} />
