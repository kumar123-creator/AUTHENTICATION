<script>
	import { authHandlers, authStore } from '../stores/authStore';

	let register = false;
	let email = '';
	let password = '';
	let confirmPassword = '';

	async function handleSubmit() {
		if (!email || !password || (register && !confirmPassword)) {
			return;
		}

		if (register && password === confirmPassword) {
			try {
				await authHandlers.signup(email, password);
			} catch (err) {
				console.log(err);
			}
		} else {
			try {
				await authHandlers.login(email, password);
			} catch (err) {
				console.log(err);
			}
		}
		if ($authStore.currentUser) {
			window.location.href = '/privatedashboard';
		}
	}
</script>

<div class="container">
	<h1>{register ? 'Register' : 'Log in'}</h1>
	<form>
		<label>
			<input bind:value={email} type="email" placeholder="Email" />
		</label>
		<label>
			<input bind:value={password} type="password" placeholder="Password" />
		</label>
		{#if register}
			<label>
				<input bind:value={confirmPassword} type="password" placeholder="Confirm Password" />
			</label>
		{/if}
		<button on:click={handleSubmit}>Submit</button>
	</form>
	{#if register}
		<div on:click={() => {register = false;}} on:keydown={() => {}}>
			Already have an account?
			<p>Log in</p>
		</div>
	{:else}
		<div on:click={() => {register = true;}} on:keydown={() => {}}>
			Don't have an account? 
            <p>Sign Up</p>
		</div>
		<div on:click={() => {authHandlers.resetPassword(email)}} on:keydown={() => {}}>
			Forgot Password?
		</div>
	{/if}
</div>

<style>

h1 {
      font-size: 24px; /* You can adjust the font size as needed */
      font-weight: bold;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      flex: 1;
    }
  
    .container form {
      display: flex;
      flex-direction: column;
      max-width: 300px;
      margin: 0 auto;
    }
  
    input[type="email"],
    input[type="password"] {
      padding: 10px;
      margin: 5px;
      border: 2px solid #ddd;
      border-radius: 5px;
      font-size: 16px;
    }
  
    button {
      padding: 10px 20px;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
      margin-top: 10px;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    .toggle-link {
      cursor: pointer;
      color: #007BFF;
      text-decoration: underline;
    }
  
    .toggle-link:hover {
      color: #0056b3;
    }
  </style>
  