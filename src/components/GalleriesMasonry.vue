<!-- @file A masonry design for galleries. -->

<template>
	<!-- Sizing helper - This is never show to the user. -->
	<div ref="spacerElement" style="width: var(--gallery-item-width)" />
	<div ref="grid">
		<router-link
			v-for="i in galleries"
			:key="i.slug"
			style="width: var(--gallery-item-width)"
			class="gallery inline-block"
			:to="i.slug"
		>
			<!-- card -->
			<div style="transform-style: preserve-3d" class="duration-500 transition-all">
				<!-- front -->
				<img class="m-0" :src="i.featured?.path" />

				<!-- back -->
				<div
					class="absolute bg-opacity-25 bg-white bottom-0 h-full w-full"
					style="backface-visibility: hidden; transform: rotateY(180deg)"
				>
					<div
						class="absolute bg-opacity-75 bg-white bottom-0 m-5 rounded-lg shadow-2xl text-center w-[-webkit-fill-available]"
					>
						<h3 class="m-0">{{ i.title }}</h3>
						<i v-if="i.firstPhoto" class="text-stone-900">{{
							new Date(i.firstPhoto.date).toLocaleDateString()
						}}</i>
					</div>
				</div>
			</div>
		</router-link>
	</div>
</template>

<script lang="ts">
	import Masonry from "masonry-layout";
	import { Prop as Property, Vue } from "vue-property-decorator";

	import waitForImages from "../lib/waitForImages";

	import type { Gallery } from "../../types/galleries";

	export default class GalleriesMasonry extends Vue {
		@Property() public galleries!: readonly Gallery[];

		public override async mounted(): Promise<void> {
			const { grid, spacerElement } = this.$refs;

			if (!(grid instanceof Element)) throw new Error("Grid element not found");

			await waitForImages(grid);

			if (!(spacerElement instanceof HTMLDivElement))
				throw new ReferenceError("Spacer element not found");

			new Masonry(grid, {
				columnWidth: spacerElement.getBoundingClientRect().width,
				gutter: 0,
				horizontalOrder: true,
				itemSelector: ".gallery",
				percentPosition: true,
				transitionDuration: 0,
			}).layout?.();
		}
	}
</script>

<style>
	:root {
		/* TODO: use `scoped` and define the properties on the component root element
		 This determines the width of each block. */
		--gallery-item-width: calc(100% / var(--column-count));

		--column-count: 3;
	}

	.gallery:hover > div {
		transform: rotateY(180deg);
	}
</style>
