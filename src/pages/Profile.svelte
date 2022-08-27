<script lang="ts">
  import { supabase } from "../lib/supabaseClient";
  import { user } from "../lib/sessionStore";

  async function signOut() {
    try {
      const { error } = await supabase.auth.signOut();
      if (error) {
        throw new Error(error.message);
      }
    } catch (err) {
      alert(err.message);
    }
  }
</script>

<header class="navbar bg-primary text-primary-content">
  <div class="nav-container container">
    <nav>
      <ul>
        <li>
          <a href="/" class="btn btn-ghost normal-case">My Profile</a>
        </li>
      </ul>
    </nav>
    <nav>
      <ul>
        <li>
          <button class="btn btn-ghost normal-case" on:click={signOut}
            >Sign Out</button
          >
        </li>
      </ul>
    </nav>
  </div>
</header>

<style>
  .container {
    max-width: 960px;
  }

  .nav-container {
    display: flex;
    justify-content: space-between;
    margin: auto;
  }

  ul {
    list-style: none;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0;
    padding: 0;
    gap: 32px;
  }
</style>
