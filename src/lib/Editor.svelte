<script lang="ts">
	import type ExoEditor from './ExoEditor.js';
	import type IExoModuleData from './IExoModuleData.js';
	import '@fortawesome/fontawesome-free/css/all.min.css';
	import ExoInstance from './ExoInstance.js';
	import Container from './modules/container/Container.svelte';

	let {
		exo_editor: editor,
		data = $bindable()
	}: { exo_editor: ExoEditor; data: IExoModuleData[] } = $props();

	$effect(() => {
		if (data.length === 0) {
			data = [editor.buildBlock(editor.default_module)];
		}
	});

	let edition = $state(-1);
	let hovered = $state(-1);

	class ExoInstanceImpl extends ExoInstance {
		getEditor: () => ExoEditor = () => editor;
		getBlocks: () => IExoModuleData[] = () => data;
		setBlocks: (blocks: IExoModuleData[]) => void = (blocks) => {
			data = blocks;
		};
		getFocus: () => number = () => 0;
		setFocus: (v: number) => void = (v) => {};
		getEdition: () => number = () => edition;
		setEdition: (v: number) => void = (v) => {
			edition = v >= this.getBlocks().length ? this.getBlocks().length - 1 : v;
		};
		getHovered: () => number = () => hovered;
		setHovered: (v: number) => void = (v) => {
			hovered = v >= this.getBlocks().length ? this.getBlocks().length - 1 : v;
		};
	}
	let instance = new ExoInstanceImpl();
</script>

<div
	class="rounded-3x relative min-h-20 w-full bg-surface-50 py-10 text-body-color-dark shadow-2xl"
	role="none"
>
	<Container bind:data {instance} index={0} id={'document'} onchange={() => {}} />
</div>
