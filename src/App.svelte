<script>
  import Auth from "./Component/Auth.svelte";
  import QRscan from "./Component/QRscan.svelte";
  import { onMount } from "svelte";
  let isLogin = $state(false);



  onMount(() => {
    //after we save token in local storage => check if it exists
    if (localStorage.getItem("token")) {
      isLogin = true;
    }
  });

  function onLoginSuccess() {
    isLogin = true;
  }

  function handleLogout() {
    localStorage.removeItem("token");
    isLogin = false;
  }
</script>

<div class="app-container">
  {#if !isLogin}
    <Auth {onLoginSuccess} />
  {:else}
    <div class="main-content">
      <QRscan />
      <div class="logout-wrapper">
        <button class="logout-btn" onclick={handleLogout}>Logout</button>
      </div>
    </div>
  {/if}
</div>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
      Helvetica, Arial, sans-serif;
  }

  .app-container {
    min-height: 100vh;
    background-color: #f9fafb;
    position: relative;
  }

  .main-content {
    display: flex;
    flex-direction: column;
    height: 100vh;
  }

  .logout-wrapper {
    padding: 20px;
    display: flex;
    justify-content: center;
    background-color: white;
    border-top: 1px solid #e5e7eb;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 100;
  }



  .logout-btn {
    padding: 12px 24px;
    font-size: 16px;
    font-weight: 500;
    background-color: #ef4444; 
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s ease;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    min-width: 120px;
  }

  .logout-btn:hover {
    background-color: #dc2626;
    transform: translateY(-1px);
    box-shadow: 0 4px 6px -1px rgba(239, 68, 68, 0.2);
  }

  .logout-btn:active {
    transform: translateY(0);
  }
</style>
