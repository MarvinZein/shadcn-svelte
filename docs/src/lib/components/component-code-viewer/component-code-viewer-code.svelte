<script lang="ts">
	import ComponentCodeViewerCodeTitle from "./component-code-viewer-code-title.svelte";
	import ComponentCodeViewerFileTree from "./component-code-viewer-file-tree.svelte";
	import { ComponentCodeViewerContext } from "./component-code-viewer.svelte";

	const ctx = ComponentCodeViewerContext.get();
	const file = $derived(ctx.highlightedFiles.find((f) => f.target === ctx.activeFile));
</script>

{#if file}
	<div
		class="bg-code text-code-foreground h-(--height) flex overflow-hidden rounded-xl border group-data-[view=preview]/block-view-wrapper:hidden"
	>
		<div class="hidden w-72 md:block">
			<ComponentCodeViewerFileTree />
		</div>
		<figure
			data-rehype-pretty-code-figure
			class="mt-0 flex min-w-0 flex-1 flex-col rounded-xl border-none"
		>
			<ComponentCodeViewerCodeTitle />
			<div
				class="no-scrollbar overflow-y-auto"
				{@attach (node) => {
					if (file.highlightedContent) {
						ctx.activeFileCodeToCopy = node.innerText;
					}
				}}
			>
				<!-- eslint-disable-next-line svelte/no-at-html-tags -->
				{@html file.highlightedContent}
			</div>
		</figure>
	</div>
{/if}
