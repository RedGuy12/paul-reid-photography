<!-- @file A slider component for photos -->

<template>
	<vueper-slides
		fade
		fixed-height
		arrows-outside
		autoplay
		bullets-outside
		:duration="10000"
		:pause-on-hover="false"
		:pause-on-touch="false"
		:transition-speed="700"
		class="mb-20 mt-4"
	>
		<vueper-slide v-for="{ path } in photos" :key="path">
			<!-- todo it's backwards -->
			<template #content>
				<div class="flex h-full w-full">
					<img draggable="false" :src="path" class="m-auto" />
				</div>
			</template>
		</vueper-slide>
	</vueper-slides>
</template>

<script lang="ts">
	import { Options, Prop as Property, Vue } from "vue-property-decorator";
	import type { Photo } from "../../types/galleries";
	import { VueperSlide, VueperSlides } from "vueperslides";
	import "vueperslides/dist/vueperslides.css";

	@Options({ components: { VueperSlide, VueperSlides } })
	export default class PhotosSlider extends Vue {
		@Property() public photos!: readonly Photo[];
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.vueperslides--fixed-height {
		@apply h-[50rem];
	}
</style>
