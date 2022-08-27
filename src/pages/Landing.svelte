<script lang="ts">
  import { supabase } from "../lib/supabaseClient";

  let email = "";
  let password = "";

  const handleLogin = async () => {
    try {
      const { error } = await supabase.auth.signIn({ email, password });
      if (error) {
        throw new Error(error.message);
      }
    } catch (err) {
      alert(err.error_description || err.message);
    }
  };
</script>

<main class="container">
  <div class="info">
    <h1 class="text-5xl font-bold">Orillia Tennis Club</h1>
    <p class="py-5 text-lg">Interested in joining?</p>
    <button class="btn btn-secondary"><a href="/#/signup">Sign Up!</a></button>
  </div>
  <form
    class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100"
    on:submit|preventDefault={handleLogin}
  >
    <div class="card-body">
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
        <input
          type="password"
          placeholder="password"
          class="input input-bordered"
          required
          bind:value={password}
        />
        <label for="forgot-password" class="label">
          <a href="#" class="label-text-alt link link-hover">Forgot password?</a
          >
        </label>
      </div>
      <div class="form-control mt-6">
        <button type="submit" class="btn btn-primary">Login</button>
      </div>
    </div>
  </form>
</main>

<style>
  main {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 100px;
    flex-wrap: wrap;
  }

  /* input {
    background-color: #242424;
  } */

  .container {
    max-width: 960px;
    margin: auto;
  }

  /* .card {
    background-color: #242424;
  } */

  /* h1 {
    color: #535bf2;
  } */

  /* .info {
    text-align: center;
  } */
</style>
