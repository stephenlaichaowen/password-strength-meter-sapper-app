<script>
	import { onMount } from 'svelte'
	
	let strengthMeter
	let password = ''	
	let message = ''
	let score
	let strength = 0
	
	onMount(() => updateStrengthMeter())	
	
	$: if (score >=100) score = 100 		
	$: if (score >= 40) message = 'Perfect'	
	$: if (score < 40) message = 'Great'
	$: if (score < 20) message = 'Good'
	$: if (score < 10) message = 'Weak'
	$: if (score < 5) message = 'Poor' 
	
	function updateStrengthMeter() {
		score = password.length
		if (/[A-Z]/.test(password)) strength = score *= 1.25
		if (/[a-z]/.test(password)) strength = score *= 1.25
		if (/[0-9]/.test(password)) strength = score *= 1.25
		if (/[^A-Za-z0-9]/.test(password)) strength = score *= 1.25		
		
		if (score == 1.25) {
			score = 0
			strength = 0
		}
		strengthMeter.style.setProperty('--strength', strength)
	}	
</script>

<div class="body">
	<h1>Password Strength Meter</h1>
	<div bind:this={strengthMeter} class="strength-meter"></div>
	<input on:input={updateStrengthMeter} bind:value={password} class="password-input" type="text" aria-labelledby="password" placeholder="Please enter password">
	<div class="reasons">
		<p>score : {score}</p>
		{message}
	</div>	
</div>

<style>	
	.body {
		height: 100vh;
		padding: 1rem;
		background-color: hsl(261, 88%, 17%);
		color: hsl(261, 88%, 90%);
		text-align: center;
	}
	.strength-meter {
		height: 2rem;
		margin: 0 auto;
		width: 90%;
		border: 3px solid hsl(261, 88%, 57%);
		border-radius: 1rem;		
		position: relative;
		overflow: hidden;
	}
	.strength-meter::before {	
		content: '';
		position: absolute;
		left: 0;
		width: calc(1% * var(--strength, 0));
		height: 100%;
		background-color: hsl(261, 88%, 67%);
		transition: width 200ms;
	}
	.password-input {
		margin-top: 1rem;
		width: 90%;
		padding: .25rem .75rem;
		background-color: hsl(261, 88%, 25%);
		color: white;
		border: 1px solid hsl(261, 88%, 57%);
		outline: none;
		text-align: center;
		border-radius: .3rem;
	}
	.password-input:focus {
		border-color: hsl(261, 88%, 70%);
	}
	.reasons > * {
		margin-top: .5rem;
		color: hsl(261, 88%, 80%);
	}
</style>