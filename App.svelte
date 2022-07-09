<script>
	import { fly } from "svelte/transition";
	import { tweened } from "svelte/motion";

	// Define themes and corresponding icons
	let themes = ["dark", "light"];
	let themeIcons = ["🌙", "☀️"];

	// Index keeps track of the current theme
	let index = 0;

	// Set up tweened to transition between the initial progress point ('0') and the final progress point ('1') for 800ms. In combination with a shorter Svelte transition speed, this allows the toast to stay in place for a couple seconds before transitioning out
	let toastProgress = tweened(0, { duration: 800 });

	// Set toastProgress to '1' to start tweening from '0' to '1'.
	let toggleTheme = () => {
	  index = index === 0 ? 1 : 0;
	  toastProgress.set(1);
	};

	// Set progress to '0' to start a transition back out when the 'in' transition is finished
	$: if ($toastProgress >= 1) {
	  toastProgress.set(0);
	}
</script>

<!-- button toggles the theme when clicked -->
<button
	on:click={toggleTheme}
	title='Toggle theme'
	aria-label='toggle theme'
> 
<b>Toggle theme</b>
	</button
>

<!-- toast only shown if tweening is in progress -->
{#if $toastProgress > 0}
	<div
		class="toast"
		transition:fly={{ y: 100, duration: 600 }}
	>
		<span>Theme set to <b>{themes[index]}</b><i
			class='toast-icon'
			aria-label="{themes[index]} theme icon"
		>{themeIcons[index]}</i></span>
		</div>
{/if} 

<style>
	:root {
	  --base: #191724;
	  --border: #403d52;
	  --text: #e0def4;

	  background-color: var(--base);
	  color: var(--text);
	  font-family: sans-serif;
	  height: 100vh;
	}

	button {
	  color: inherit;
	  background-color: inherit;
	  font-size: inherit;
	  border: 4px solid var(--border);
	  border-radius: 0.75rem;
	  padding: 0.75rem;
	  cursor: pointer;
	}

	button:hover {
	  background-color: var(--border);
	}

	.toast {
	  background: var(--base);
	  border: 4px solid var(--border);
	  border-radius: 0.75rem;
	  padding: 1rem;
	  position: fixed;
	  right: 2rem;
	  bottom: 4rem;
	  z-index: 20;
	}

	.toast-icon {
	  margin-left: 0.5rem;
	}
</style>