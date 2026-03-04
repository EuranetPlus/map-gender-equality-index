<script>
	import { formatInt } from '$lib/utils/formatNumbers';
	import { config } from '$lib/stores/config-features';

	let width;

	export let classes = [];
	export let clusters = []; // bleibt aus Kompatibilitätsgründen drin
	export let scaleMin;
	export let scaleMax;

	function formatValue(number) {
		if (number === null || number === undefined) return '';
		if (config.datasetUnit == 'percent') return `${formatInt(number * 100)}`;
		if (config.datasetUnit == 'fullNumbers') return `${number}`;
		return `${number}`;
	}

	function displayDigit(index, total) {
		if (index === 0) return formatValue(scaleMin);
		if (index === total - 1) return formatValue(scaleMax);
		return '';
	}

	function labelAlignClass(index, total) {
		if (index === 0) return 'text-left';
		if (index === total - 1) return 'text-right';
		return 'text-center';
	}
</script>

<div
	class="scale text-sm absolute top-3 rounded bg-white p-3 border"
	bind:clientWidth={width}
	style={`left: calc(50% - ${width / 2}px); --n:${classes.length};`}
>
	<!-- Farbkacheln -->
	<div class="flex justify-center">
		{#each classes as swatch}
			<div class="swatch" style="background: {swatch};" />
		{/each}
	</div>

	<!-- Labels: scaleMin links, scaleMax rechts (egal ob 5 oder 7 oder mehr) -->
	<div class="flex justify-center">
		{#each classes as _, index}
			<div class="swatch text-xs {labelAlignClass(index, classes.length)}">
				{displayDigit(index, classes.length)}
			</div>
		{/each}
	</div>
</div>

<style lang="scss">
	/* Skaliert automatisch mit Anzahl Kacheln:
	   - maximal ca. 70vw Gesamtbreite (wie vorher grob 5*15vw=75vw)
	   - jede Kachel bekommt 70vw / n
	*/
	.swatch {
		width: calc(70vw / var(--n));
		height: 1.2vh;
	}

	.swatch:first-of-type {
		border-top-left-radius: 5px;
		border-bottom-left-radius: 5px;
	}

	.swatch:last-of-type {
		border-top-right-radius: 5px;
		border-bottom-right-radius: 5px;
	}
</style>
