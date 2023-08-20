<template>
	<ul>
		<li>
			<button 
				@click.prevent="toggleMenu('profile')" 
				aria-label="Show and hide profile"
				class="text-xl font-medium text-left block w-full rounded-lg pr-2 mb-3"
			>
				Profile
				<span class='inline-block float-right -mt-[1px]'>
					<i :class="`fa fa-solid fa-chevron-${ settings === 'profile' ? 'down': 'up' } fa-sm`"></i>
				</span>
			</button>
		</li>
		<li v-if="settings === 'profile'">
			<Profile @updateProfile="updateProfile"/>
		</li>
		<li class="mt-6">
			<button 
				@click.prevent="toggleMenu('security')" 
				aria-label="Show and hide security"
				class="text-xl font-medium text-left block w-full rounded-lg pr-2 mb-3"
			>
				Security
				<span class='inline-block float-right -mt-[1px]'>
					<i :class="`fa fa-solid fa-chevron-${ settings === 'security' ? 'down': 'up' } fa-sm`"></i>
				</span>
			</button>
		</li>
		<li v-if="settings === 'security'">
			<Security @toggleMenu="toggleMenu($event)" />
		</li>
	</ul>
</template>

<script setup lang="ts">

	import Profile from '../forms/Profile.vue'
	import Security from '../forms/Security.vue'
	
	import { ref, type Ref } from 'vue'

	/** Defines the reactive settings state with profile */
	let settings: Ref<string> = ref('profile');

	/**
	 * This function defines the emits with updateProfile as event property 
	 * @return {void}
	 */
	const emit = defineEmits<{
		(event: 'updateProfile'): void
	}>();

	/**
	 * This function sends a signal to the parent component to update the reactive profile state and toggles the profile menu settings
	 * @function @name updateProfile - A function to send a signal to the parent component to update the reactive profile state
	 * @function @name toggleMenu  - A function to toggle the profile menu settings
	 * @return {void}
	 */
	const updateProfile = (): void => {
		emit('updateProfile');
		toggleMenu('profile');
	}

	/**
	 * This function shows and hides the profile menu settings based on type target and updates the current reactive settings state
	 * @param {string} target - A string variable
	 * @return {void}
	 */
	const toggleMenu = (target: string): void => {
		if (target !== settings.value) {
			settings.value = target
		} else {
			settings.value = ''
		}
	}

</script>
