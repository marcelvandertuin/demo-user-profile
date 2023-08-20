<template>
	<section class="col-start-3 col-span-2 py-4 mb-8">
		<form spellcheck="false">
			<fieldset>
				<legend>
					<h3 aria-hidden="true" class="sr-only">
						Security
					</h3>
				</legend>
				<ul>
					<li>
						<label class="text-sm font-medium capitalize text-gray-500 block mb-1" for="current-password">
							current password
							<input
								type="password"
								id="current-password"
								name="current-password"
								v-model="inputCurrentPassword"
								class="text-black border border-gray-300 h-10 w-full rounded mb-3 pl-1 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
							/>
						</label>
					</li>
					<li>
						<label class="text-sm font-medium capitalize text-gray-500 block mb-1" for="new-password">
							new password
							<input
								type="password"
								id="new-password"
								name="new-password"
								v-model="inputNewPassword"
								class="text-black border border-gray-300 h-10 w-full rounded mb-3 pl-1 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
							/>
						</label>
					</li>
					<li>
						<ul class="flex justify-between sm:float-right space-x-3">
							<li>
								<button 
									@click.prevent="saveChanges('cancel')" 
									aria-label="Cancel all changes"
									class="text-sm font-medium capitalize border border-gray-300 h-[36px] block rounded hover:bg-gray-50 py-2 px-4 mt-3"
								>
									cancel
								</button>
							</li>
							<li>
								<button 
									@click.prevent="saveChanges('confirm')" 
									aria-label="Save all changes"
									class="text-sm font-semibold text-sky-700 capitalize block rounded bg-sky-100 hover:bg-sky-400 py-2 px-4 mt-3"
								>
									Confirm
								</button>
							</li>
						</ul>
					</li>
				</ul>
			</fieldset>
		</form>
	</section>
</template>

<script setup lang="ts">

	import { ProfileInterface } from '../models/types'
	import { ref, type Ref } from 'vue'

	/** Declares variable profile from local storage as interface ProfileInterface
	 * @type {import('./models/types') ProfileInterface } 
	 */
	const profile: ProfileInterface = JSON.parse(localStorage.getItem('profile') as string);

	/** Declares the variables in the reactive state based upon the password form
	 * @type {string} inputCurrentPassword - the current password of the user
	 * @type {string} inputNewPassword - the new password of the user
	 */
	let inputCurrentPassword: Ref<string> = ref(profile.password);
	let inputNewPassword: Ref<string> = ref('');

	/**
	 * This function defines the emits with toggleMenu as event property and target as payload property
	 * @return {void}
	 */
	const emit = defineEmits<{
		(event: 'toggleMenu', target: string): void
	}>();

	/**
	 * This function updates the reactive state of the current password, new password, local storage and sends a signal to the parent component to show or hide the profile menu settings
	 * @param {string} target - A string variable
	 * @function @name updateChanges - A function to update the reactive state of the current and new password and the local storage
	 * @function @name toggleMenu  - sends a signal to the parent component to show or hide the profile menu settings
	 * @return {void}
	 */
	const saveChanges = (target: string): void => {
		if (target === 'confirm') {
			updateChanges();
		}
		toggleMenu();
	}

	/**
	 * This function updates the reactive state of the current password, new password, local storage and sends a signal to the parent component to show or hide the profile menu settings
	 * @return {void}
	 */
	const updateChanges = (): void => {
		
		/** Deconstruct the profile object and update the password property with the reactive new password state */
		let updateProfile = { ...profile, password:inputNewPassword.value } 

		/** Update the current password with the reactive new password state and clear the reactive new password state */
		inputCurrentPassword.value = inputNewPassword.value;
		inputNewPassword.value = '';

		/** Update local storage with the new profile object */
		localStorage.setItem('profile', JSON.stringify(updateProfile));
	}

	/**
	 * This function sends a signal to the parent component to show or hide the profile menu settings based on string target as payload
	 * @return {void}
	 */
	const toggleMenu = (): void => {
		emit('toggleMenu', 'security');
	}

</script>
