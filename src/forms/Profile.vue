<template>
	<section class="col-start-3 col-span-2 py-4 mb-8">
		<form spellcheck="false">
			<fieldset>
				<legend>
					<h3 aria-hidden="true" class="sr-only">
						Profile
					</h3>
				</legend>
				<ul>
					<li>
						<span class="text-sm font-medium text-gray-500 capitalize block">profile image</span>
					</li>
					<li>
						<ul class="mb-3 overflow-hidden border border-gray-300 rounded pt-3 px-1 pb-3">
							<li class="float-left h-14 w-14">
								<img 
									:aria-label="`Profile image ${inputName}`"
									class="border-2 border-sky-500 h-14 w-14 rounded-full"
									:src="inputImage"
									:alt="`Profile image ${inputName}`" 
									width="56"
									height="56" 
								/>
							</li>
							<li class="float-left overflow-hidden pt-2">
								<label for="image">
									<span class="sr-only">Choose profile image</span>
									<input type="file" class="text-sm font-medium text-gray-500 block pl-2 file:mr-4 file:py-2 file:px-4 file:rounded file:border-0 file:text-sm file:font-semibold file:bg-sky-100 file:text-sky-700 hover:file:bg-sky-400 file:cursor-pointer"
									id="image"
									name="image"
									@change="getImage" 
									/>
								</label>
							</li>
						</ul>
					</li>
					<li>
						<label class="text-sm font-medium text-gray-500 capitalize block mb-1" for="name">
							your name
							<input
								type="text"
								id="name"
								name="name"
								v-model="inputName"
								class="text-black border border-gray-300 h-10 w-full rounded pl-1 mb-3 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
							/>
						</label>
					</li>
					<li>
							<label class="text-sm font-medium text-gray-500 capitalize block mb-1" for="email">
								email address
								<input
									type="email"
									id="email"
									name="email"
									v-model="inputEmail"
									class="text-black border border-gray-300 h-10 w-full rounded pl-1 mb-3 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
								/>
							</label>
					</li>
					 <li>
							<label class="text-sm font-medium text-gray-500 capitalize block mb-1" for="address">
								your location
								<input
									type="text"
									id="address"
									name="address"
									v-model="inputAddress"
									class="text-black border border-gray-300 h-10 w-full rounded pl-1 mb-3 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
								/>
							</label>
					</li>
					<li>
							<label class="text-sm font-medium text-gray-500 capitalize block mb-1" for="phone">
								phone number
								<input 
									type="text"
									id="phone"
									name="phone"
									v-model="inputPhone"
									class="text-black border border-gray-300 h-10 w-full rounded pl-1 mb-3 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
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

	/** Declares the variables in the reactive state based upon the profile form
	 * @type {string} inputName - the name of the user
	 * @type {string} inputEmail - the e-mail of the user
	 * @type {string} inputAddress - the address of the user
	 * @type {string} inputPhone - the phone of the user
	 * @type {string} inputImage - the profile image of the user
	 */
	const inputName: Ref<string> = ref(profile.name);
	const inputEmail: Ref<string> = ref(profile.email);
	const inputAddress: Ref<string> = ref(profile.address);
	const inputPhone: Ref<string> = ref(profile.phone);
	const inputImage: Ref<any> = ref(profile.image);

	/** 
	 * This function defines the emits with updateProfile as event property
	 * @return {void}
	 */
	const emit = defineEmits<{
		(event: 'updateProfile'): void
	}>();

	/**
	 * This function initialize the FileReader to convert uploaded profile image into a data URL
	 * @param {*} event - A any variable 
	 * @function @name FileReader - A function to covert an image into a data URL
	 * @function @name readAsDataURL- A function to read the value of the input file
	 * @function @name addEventListener - A function to listen for the value to be loaded
	 */
	const getImage = (event: any) => {

		/** Initialize the FileReader and use it to read the value of the input file as a data URL */
		const fr = new FileReader();
		fr.readAsDataURL(event.target.files[0]);

		/** Updates the reactive profile image state after the data URL is loaded */
		fr.addEventListener('load', () => {
			inputImage.value = fr.result;
		});
	}

	/**
	 * This function updates the reactive state of the profile form variables, update local storage and send a signal to the parent component to update the reactive profile state
	 * @param {string} target - A string variable
	 * @function @name updateStorage - A function to update the reactive state of the profile form variables and local storage
	 * @function @name updateProfile  - A function to send a signal to the parent component to update the reactive profile state
	 * @return {void}
	 */
	const saveChanges = (target: string): void => {
		if (target === 'confirm') {
			updateStorage();
		}
		updateProfile();
	}

	/**
	 * Declares a new profile object as interface ProfileInterface based upon the values of the profile form inputs and updates local storage
	 * Checks if the profile exists in local storage and set it if it does not
	 * @return {void}
	 */
	const updateStorage = (): void => {

		/** Declares a new profile object as interface ProfileInterface based upon the values of the profile form inputs and checks if the input values exist
		* @type {import('./models/types') ProfileInterface } 
		*/
		const updateProfile: ProfileInterface =  {
			name: (document.getElementById('name') as HTMLInputElement).value.length == 0 ? profile.name : inputName.value,
			email: (document.getElementById('email') as HTMLInputElement).value.length == 0 ? profile.email : inputEmail.value,
			address: (document.getElementById('address') as HTMLInputElement).value.length == 0 ? profile.address : inputAddress.value,
			phone: (document.getElementById('phone') as HTMLInputElement).value.length == 0 ? profile.phone : inputPhone.value,
			password: profile.password,
			image: (document.getElementById('image') as HTMLInputElement).files?.length == 0 ? profile.image : inputImage.value
		}

		/** Update local storage with the new profile object */
		localStorage.setItem('profile', JSON.stringify(updateProfile));
	}

	/**
	* This function sends a signal to the parent component to update the reactive profile state
	* @return {void}
	*/
	const updateProfile = (): void => {
		emit('updateProfile');
	}

</script>
