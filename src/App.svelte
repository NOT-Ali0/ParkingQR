<script>
  import Auth from "./Component/Auth.svelte";
  import QRscan from "./Component/QRscan.svelte";
  import { onMount } from "svelte";
  let isLogin = $state(false);
  
  onMount(() => {
    if (localStorage.getItem("token")) {
      isLogin = true;
    }
  });
  // localStorage.removeItem(x"token");
</script>
{#if !isLogin}
<Auth />
{/if}
<button class="auth-btn"> Logout</button>
<QRscan />


<style>
  .auth-btn {
        padding: 15px;
        font-size: 16px;
        background-color: #2563eb;
        color: white;
        border: none;
        cursor: pointer;
    }
</style>
