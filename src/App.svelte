<script>
  import Footer from "./components/Footer.svelte";

  import Router from "svelte-spa-router";
  import Landing from "./pages/Landing.svelte";
  import Signup from "./pages/Signup.svelte";
  import Profile from "./pages/Profile.svelte";

  import { user } from "./lib/sessionStore";
  import { supabase } from "./lib/supabaseClient";

  user.set(supabase.auth.user());

  supabase.auth.onAuthStateChange((state, session) => {
    user.set(state === "SIGNED_IN" && session.user);
  });

  const loggedInRoutes = {
    // Exact path
    "/": Profile,
    "/signup": Signup,
  };

  const notLoggedInRoutes = {
    // Exact path
    "/": Landing,
    "/signup": Signup,
  };
</script>

{#if $user}
  <Router routes={loggedInRoutes} />
{:else}
  <Router routes={notLoggedInRoutes} />
{/if}
<Footer />
