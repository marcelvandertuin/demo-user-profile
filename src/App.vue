<template>
	<header></header>
	<main class="grid grid-cols-1 lg:grid-cols-7 2xl:grid-cols-5 gap-4 lg:min-h-screen lg:py-12">
		<h1 aria-hidden="true" class="sr-only">Profile</h1>
		<section class="col-start-1 col-span-1 lg:col-start-2 lg:col-span-2 2xl:col-start-2 2xl:col-span-1 text-center h-[35vh]">
			<h2 aria-hidden="true" class="sr-only">Profile Details</h2>
			<ProfileDetails :updatedProfile="updatedProfile" />
		</section>
		<section class="col-start-1 col-span-1 lg:col-start-4 lg:col-span-3 2xl:col-span-2 h-[73vh] rounded-t-[27px] lg:rounded-xl bg-white lg:drop-shadow-none drop-shadow-2xl p-6 pt-8">
			<h2 aria-hidden="true" class="sr-only">Profile Menu</h2>
			<ProfileMenu @updateProfile="updateProfile" />
		</section>
	</main>
	<footer></footer>
</template>

<script setup lang="ts">

	import ProfileDetails from './components/ProfileDetails.vue'
	import ProfileMenu from './components/ProfileMenu.vue'

	import { ProfileInterface, ProfileDetailsInterface } from './models/types'
	import { ref, type Ref } from 'vue'

	/** Initializes the default profile as interface ProfileInterface
	 * @type {import('./models/types') ProfileInterface } 
	 */
	const defaultProfile: ProfileInterface = {
		name: 'Anna Valerie',
		email: 'annavalerie@mail.com',
		address: 'Houston, TX',
		phone: '21 (356) 79783',
		password: 'password12345',
		image: '/assets/anna-valarie.jpg'
	}

	const defaultProfileDetails: ProfileDetailsInterface = {
		name: 'Anna Valerie',
		email: 'annavalerie@mail.com',
		address: 'Houston, TX',
		phone: '21 (356) 79783',
		image: '/assets/anna-valarie.jpg'
	}

	/** Defines the reactive updatedProfile state with default string as interface ProfileInterface
	 * @type {import('./models/types') ProfileDetailsInterface } 
	 */
	const updatedProfile: Ref<ProfileDetailsInterface> = ref(defaultProfileDetails);

	/** Checks if the object profile exists in local storage and set it if it does not */
	if (localStorage.getItem('profile') === null) {
		localStorage.setItem('profile', JSON.stringify(defaultProfile));
	}
	
	/**
	* This function updates the object profile from local storage in the reactive updatedProfile state
	* @return {void}
	*/
	const updateProfile = (): void => { 
		updatedProfile.value = JSON.parse(localStorage.getItem('profile') as string);
	}

	updateProfile();

</script>
