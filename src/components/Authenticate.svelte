<script>
	import { authHandlers } from '../store/store';

	let email = '';
	let password = '';
	let confirmPassword = '';
	let error = false;
	let register = false;
	let authenticating = false;

	async function handleAuth() {
		if (authenticating) return;

		if (!email || !password || (register && !confirmPassword)) {
			error = true;
			return;
		}

		authenticating = true;

		try {
			if (!register) {
				await authHandlers.login(email, password);
			} else {
				await authHandlers.signup(email, password);
			}
		} catch (error) {
			console.error('There was an Auth error: ', error);
			error = true;
			authenticating = false;
		}

		authenticating = false;
	}

	function handleRegister() {
		register = !register;
	}
</script>

<div class="authContainer">
	<form on:submit={register ? handleRegister : handleAuth}>
		<h1>{register ? 'Register' : 'Login'}</h1>
		{#if error}
			<p class="error">The information you have entered is not correct</p>
		{/if}
		<label>
			<p class={email ? ' above' : ' center'}>Email</p>
			<input bind:value={email} type="email" placeholder="Email" />
		</label>
		<label>
			<p class={password ? ' above' : ' center'}>Password</p>
			<input bind:value={password} type="password" placeholder="Pasword" />
		</label>
		{#if register}
			<label>
				<p class={confirmPassword ? ' above' : ' center'}>Confirm Password</p>
				<input bind:value={confirmPassword} type="password" placeholder="Confirm Password" />
			</label>
		{/if}
		<button on:click={handleAuth} type="button" class="submitBtn">
			{#if authenticating}
				<i class="fa-solid fa-spinner spin" />
			{:else}
				Submit
			{/if}
		</button>
	</form>
	<div class="options">
		<p>Or</p>
		{#if register}
			<div>
				<p>Already have an account?</p>
				<p on:click={handleRegister} on:keydown={() => {}}>Login</p>
			</div>
		{:else}
			<div>
				<p>Don't have an account?</p>
				<p on:click={handleRegister} on:keydown={() => {}}>Register</p>
			</div>
		{/if}
	</div>
</div>

<style>
	.authContainer {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		flex: 1;
		padding: 24px;
	}

	form {
		display: flex;
		flex-direction: column;
		gap: 14px;
		width: 400px;
		max-width: 100%;
		margin: 0 auto;
	}

	form input {
		width: 100%;
		border: none;
		background: transparent;
		color: white;
		padding: 8px 14px;
	}

	form input:focus {
		outline: none;
		border: none;
	}

	form label {
		position: relative;
		border: 1px solid navy;
		border-radius: 4px;
	}

	form label:focus-within {
		border: 1px solid blue;
	}

	form button {
		background: navy;
		color: white;
		border: none;
		padding: 14px 0;
		border-radius: 4px;
		cursor: pointer;
		font-size: 1rem;
		display: grid;
		place-items: center;
	}

	.spin {
		animation: spin 2s infinite;
	}

	@keyframes spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}

	form button:hover {
		background: blue;
	}

	.options {
		padding: 14px 0;
		overflow: hidden;
		width: 400px;
		max-width: 100%;
		margin: 0 auto;
		font-size: 0.9rem;
		display: flex;
		flex-direction: column;
		gap: 4px;
	}

	.options > p {
		position: relative;
		text-align: center;
		width: fit-content;
		margin: 0 auto;
		padding: 0 8px;
	}

	.options > p::after,
	.options > p::before {
		position: absolute;
		content: '';
		top: 50%;
		transform: translateY(-50%);
		width: 100vw;
		height: 1.5px;
		background: white;
	}

	.options > p::after {
		right: 100%;
	}

	.options > p::before {
		left: 100%;
	}

	.options div {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 8px;
	}

	.options div p:last-of-type {
		color: cyan;
		cursor: pointer;
	}

	.above,
	.center {
		position: absolute;
		transform: translateY(-50%);
		pointer-events: none;
		color: white;
		border-radius: 4px;
		padding: 0 6px;
		font-size: 0.8rem;
	}

	.above {
		top: 0;
		left: 24px;
		background: navy;
		border: 1px solid blue;
		font-size: 0.7rem;
	}

	.center {
		top: 50%;
		left: 6px;
		border: 1px solid transparent;
		opacity: 0;
	}

	h1 {
		text-align: center;
		font-size: 3rem;
	}

	.error {
		color: coral;
		font-size: 0.9rem;
		text-align: center;
	}
</style>
