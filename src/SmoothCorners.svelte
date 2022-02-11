<script>
	function uuidv4() {
	  return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
		(c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
	  );
	}
	
	export let name = "clip-path-" + uuidv4()
	export let w, h
	
	export let cornerRadius
	export let cornerSharpness
	export let clipPathOnly = false
	
	$: console.log("h:", h, "w:", w)
	
	function getPath(w, h, cornerRadius, cornerSharpness) {
		let shortestEdge = w >= h ? h : w
		
		cornerRadius = cornerRadius >= shortestEdge / 2 ? shortestEdge / 2 : cornerRadius
		cornerSharpness = cornerRadius * cornerSharpness
		
		let cornerLength = {
			x: cornerRadius / w,
			y: cornerRadius / h
		}
		
		let cornerAngle = {
			x: (cornerSharpness / w),
			y: (cornerSharpness / h)
		}
		
		let corners = {
			s: cornerSharpness,
			a: cornerAngle,
			tl: {
				xa: 0.0,
				ya: cornerLength.y,
				xb: cornerLength.x,
				yb: 0.0
			},
			tr: {
				xa: 1 - cornerLength.x,
				ya: 0.0,
				xb: 1,
				yb: cornerLength.y
			},
			bl: {
				xa: cornerLength.x,
				ya: 1,
				xb: 0,
				yb: 1 - cornerLength.y
			},
			br: {
				xa: 1,
				ya: 1 - cornerLength.y,
				xb: 1 - cornerLength.x,
				yb: 1
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
		
		return path
	}
	
	$: outerCorners = getPath(w, h, cornerRadius, cornerSharpness)
	
</script>


<style>
	.smooth-corners {
		display: block;
		clip-path: var(--clippath);
		-webkit-clip-path: var(--clippath);
	}
	svg {
		visibility: none;
		position: absolute;
	}
</style>


<svg width="0" height="0">
  <defs>
	<clipPath id={name} clipPathUnits="objectBoundingBox">
	{#if w && h}
	<path
		d={outerCorners}
	/>
	{/if}
	</clipPath>
  </defs>
</svg>
{#if clipPathOnly == false}
	<div class="smooth-corners" bind:clientWidth={w} bind:clientHeight={h} style="--clippath: url(#{name}); --width: {w}px; --height: {h}px">
		<slot></slot>
	</div>
{/if}

