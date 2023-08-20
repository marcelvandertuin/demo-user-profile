<template>
	<section class="col-start-2 col-span-1 text-center rounded-none lg:rounded-xl bg-none lg:bg-white p-6 pt-8 pb-16">
		<h3 aria-hidden="true" class="sr-only">Profile details</h3> 
		<ul>
			<li class="mx-auto h-32 w-32">
				<img 
					:aria-label="`Profile image ${updatedProfile.name}`"
					class="mx-auto border-4 border-sky-500 h-32 w-32 rounded-full"
					:src="updatedProfile.image"
					:alt="`Profile image ${updatedProfile.name}`" 
					width="128"
					height="128" 
				/>
			</li>
			<li>
				<h4 class="text-2xl font-medium block mt-5">
					{{ updatedProfile.name }}
				</h4>
			</li>
			<li>
				<a class="text-gray-500 font-medium block mt-[2px]" :href="`mailto:${updatedProfile.email}`">
					<i class="fa fa-regular fa-envelope text-gray-500 text-md align-middle"></i>
					{{ updatedProfile.email }}
				</a>
			</li>
			<li>
				<span class="text-gray-500 font-medium block">
					<i class="fa fa-solid fa-location-dot text-gray-500 text-md align-middle"></i>
					{{ updatedProfile.address }}
				</span>
			</li>
			<li> 
				<a class="text-gray-500 font-medium mt-1" :href="`tel:${updatedProfile.phone}`">
					<i class="fa fa-phone text-xs text-gray-500 text-md align-middle -mt-[1px]"></i>
					{{ updatedProfile.phone }}
				</a>      
			</li>
		</ul>
	</section>
</template>

<script setup lang="ts">

	import { ProfileDetailsInterface }  from '../models/types'

	/**
	 * This function defines the props with the updatedProfile property of interface ProfileDetailsInterface
	 * @param {import('./models/types') ProfileDetailsInterface } 
	 */
	defineProps<{ updatedProfile: ProfileDetailsInterface }>();

</script>
