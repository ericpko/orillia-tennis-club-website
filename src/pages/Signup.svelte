<script lang="ts">
  import { supabase } from "../lib/supabaseClient";

  let firstName = "";
  let lastName = "";
  let email = "";
  let password = "";
  let confirmPassword = "";
  let pwStrengthValue = 0;
  let pwFlags = [false, false, false, false, false, false];
  let pwStrengthColor = "progress-error";

  $: {
    // pw length is at least 8 chars
    if (password.length >= 8 && pwFlags[0] === false) {
      pwStrengthValue += 20;
      pwFlags[0] = true;
    } else if (password.length < 8 && pwFlags[0] === true) {
      pwFlags[0] = false;
      pwStrengthValue -= 20;
    }

    // pw has at least one lower case char
    if (password.match(/(?=.*[a-z])/) && pwFlags[1] === false) {
      pwStrengthValue += 20;
      pwFlags[1] = true;
    } else if (!password.match(/(?=.*[a-z])/) && pwFlags[1] === true) {
      pwFlags[1] = false;
      pwStrengthValue -= 20;
    }

    // pw has at least one capital letter
    if (password.match(/(?=.*[A-Z])/) && pwFlags[2] === false) {
      pwStrengthValue += 20;
      pwFlags[2] = true;
    } else if (!password.match(/(?=.*[A-Z])/) && pwFlags[2] === true) {
      pwFlags[2] = false;
      pwStrengthValue -= 20;
    }

    // pw has at least one digit
    if (password.match(/(?=.*[0-9])/) && pwFlags[3] === false) {
      pwFlags[3] = true;
      pwStrengthValue += 20;
    } else if (!password.match(/(?=.*[0-9])/) && pwFlags[3] === true) {
      pwFlags[3] = false;
      pwStrengthValue -= 20;
    }

    // pw has at least one special character
    if (password.match(/(?=.*[^A-Za-z0-9])/) && pwFlags[4] == false) {
      pwFlags[4] = true;
      pwStrengthValue += 20;
    } else if (!password.match(/(?=.*[^A-Za-z0-9])/) && pwFlags[4] == true) {
      pwFlags[4] = false;
      pwStrengthValue -= 20;
    }
  }

  $: {
    if (pwStrengthValue >= 80) {
      pwStrengthColor = "progress-success";
    } else if (pwStrengthValue >= 40 && pwStrengthValue <= 60) {
      pwStrengthColor = "progress-warning";
    } else {
      pwStrengthColor = "progress-error";
    }
  }

  async function handleSignup() {
    try {
      const { user, session, error } = await supabase.auth.signUp(
        { email, password },
        {
          data: {
            first_name: firstName,
            last_name: lastName,
          },
        }
      );
      if (error) {
        throw new Error(error.message);
      }
      alert("Check your email for confirmation.");
    } catch (err) {
      alert(err.error_description || err.message);
    }
  }
</script>

<main>
  <h1 class="text-3xl mb-5 text-center">Sign up to the Orillia Tennis Club!</h1>
  <form
    class="card flex-shrink-0 w-full max-w-max shadow-2xl bg-base-100"
    on:submit|preventDefault={handleSignup}
  >
    <div class="card-body">
      <div class="flex gap-6">
        <input
          type="text"
          placeholder="First name"
          class="input input-bordered"
          required
          bind:value={firstName}
        />
        <input
          type="text"
          placeholder="Last name"
          class="input input-bordered"
          required
          bind:value={lastName}
        />
      </div>
      <div class="form-control">
        <label for="email" class="label">
          <span class="label-text">Email</span>
        </label>
        <input
          type="email"
          placeholder="someone@example.com"
          class="input input-bordered"
          required
          bind:value={email}
        />
      </div>
      <div class="form-control">
        <label for="password" class="label">
          <span class="label-text">Password</span>
        </label>
        <div class="flex gap-6">
          <input
            type="password"
            placeholder="password"
            class="input input-bordered"
            required
            bind:value={password}
          />
          <input
            type="password"
            placeholder="Confirm password"
            class="input input-bordered"
            required
            bind:value={confirmPassword}
          />
        </div>
        {#if password !== confirmPassword}
          <div class="pt-1 pl-1">
            <p class="text-xs text-error">Passwords don't match.</p>
          </div>
        {/if}
        <div class="mt-2">
          <progress
            class="progress {pwStrengthColor}"
            value={pwStrengthValue}
            max="100"
          />
        </div>
      </div>
      <div class="form-control mt-6">
        <button type="submit" class="btn btn-primary">Sign Up</button>
      </div>
    </div>
  </form>
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    margin: auto;
  }
</style>
