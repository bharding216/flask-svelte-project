<script>
	import { Route, Router } from 'svelte-routing';
	import About from './About.svelte';
	import Layout from './Layout.svelte';
	// export let api_url;

	let enteredName = ''; // The name entered by the user.
	let serverResponse = ''; // The response received from the server.
	// const apiURL = process.env.API_URL;
	// console.log("API URL:", apiURL);

	async function getName() {
		let usersName = enteredName.trim();
		if (!usersName) {
			return;
		}

		let response = await fetch('http://localhost:2000/say-hi', {
			method: 'POST',
			headers: {
			'Content-Type': 'application/json',
			},
			body: JSON.stringify({ name: usersName }),
		});

		let data = await response.json();

		if (response.ok && usersName === enteredName) {
			serverResponse = data.message;
		}
	};
</script>



<Router>
	<Layout>
		<Route path="/" let:params>
			<h1>Received Message: {serverResponse}!</h1> 

			<input type="text" placeholder="Enter your name" bind:value={enteredName} />
			<button on:click={getName} disabled={!enteredName.trim()}>Send Message</button>
		</Route>

		<Route path="/about" component={About} />
	</Layout>
</Router>