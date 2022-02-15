<script>
	function uuidv4() {
	  return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
		(c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
	  );
	}
	
	export let name = "clip-path-" + uuidv4()
	export let w = 1
	export let h = 1
	
	export let svg = false
	export let clipPath = ""
	
	export let clientSize = true
	export let cornerRadius
	export let cornerSharpness
	export let clipPathOnly = false
	
	function getPath(w, h, cornerRadius, cornerSharpness) {
		let normalized = svg
		
		let shortestEdge = w >= h ? h : w
		
		cornerRadius = cornerRadius >= shortestEdge / 2 ? shortestEdge / 2 : cornerRadius
		cornerSharpness = cornerRadius * cornerSharpness
		
		let cornerLength = {
			x: cornerRadius / w,
			y: cornerRadius / h
		}
		
		
		let normalization = {
			x: normalized ? 1 : w,
			y: normalized ? 1 : h
		}
		
		let cornerAngle = {
			x: (cornerSharpness / w) * normalization.x,
			y: (cornerSharpness / h) * normalization.y
		}
		
		let corners = {
			s: cornerSharpness,
			a: cornerAngle,
			tl: {
				xa: 0.0,
				ya: cornerLength.y * normalization.y,
				xb: cornerLength.x * normalization.x,
				yb: 0.0
			},
			tr: {
				xa: (1 - cornerLength.x) * normalization.x,
				ya: 0.0,
				xb: 1 * normalization.x,
				yb: cornerLength.y * normalization.y
			},
			bl: {
				xa: cornerLength.x * normalization.x,
				ya: 1 * normalization.y,
				xb: 0,
				yb: (1 - cornerLength.y) * normalization.y
			},
			br: {
				xa: 1 * normalization.x,
				ya: (1 - cornerLength.y) * normalization.y,
				xb: (1 - cornerLength.x) * normalization.x,
				yb: 1 * normalization.y
			}
		}
		
		let path = `M ${corners.tl.xa} ${corners.tl.ya}
		C ${corners.tl.xa} ${corners.tl.ya - corners.a.y}, ${corners.tl.xb - corners.a.x} ${corners.tl.yb}, ${corners.tl.xb} ${corners.tl.yb}
		L ${corners.tr.xa} ${corners.tr.ya}
		C ${corners.tr.xa + corners.a.x} ${corners.tr.ya}, ${corners.tr.xb} ${corners.tr.yb - corners.a.y}, ${corners.tr.xb} ${corners.tr.yb}
		L ${corners.br.xa} ${corners.br.ya}
		C ${corners.br.xa} ${corners.br.ya + corners.a.y}, ${corners.br.xb + corners.a.x} ${corners.br.yb}, ${corners.br.xb} ${corners.br.yb}
		L ${corners.bl.xa} ${corners.bl.ya}
		C ${corners.bl.xa - corners.a.x} ${corners.bl.ya}, ${corners.bl.xb} ${corners.bl.yb + corners.a.y}, ${corners.bl.xb} ${corners.bl.yb}
		Z`
		
		return path.replace(/\n/g, '')
	}
	
	$: path = getPath(w, h, cornerRadius, cornerSharpness)
	
	$: clipPath = svg ? `url('#${name}')` : `path('${path}')`
	
	$: style = `clip-path: ${clipPath}; -webkit-clip-path: ${clipPath};`
	
</script>


<style>
	.smooth-corners {
		display: block;
		position: relative;
		height: 100%;
		width: 100%;
	}
	svg {
		visibility: none;
		position: absolute;
	}
</style>

{#if svg}
<svg width="0" height="0">
  <defs>
	<clipPath id={name} clipPathUnits="objectBoundingBox">
		{#if h && w}
		<path
			d={path}
		/>
		{/if}
	</clipPath>
  </defs>
</svg>
{/if}

{#if clipPathOnly == false}
	{#if clientSize}
		<div class="smooth-corners" bind:clientWidth={w} bind:clientHeight={h} {style}>
			<slot></slot>
		</div>
	{/if}
	
	{#if !clientSize}
		<div class="smooth-corners" {style}>
			<slot></slot>
		</div>
	{/if}
{/if}

{#if clipPathOnly == true}
	<slot></slot>
{/if}