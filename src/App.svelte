<script>
  import Form from "./Pages/Form.svelte"
  import Userpage from "./Pages/Userpage.svelte"
  let display = false,
    userinfo = []

  const directform = (e) => {
    display = e.detail
  }

  const ontransfer = (e) => {
    userinfo = e.detail
    display = true
  }
</script>

<main>
  {#if display}
    <Userpage {userinfo} on:display={directform} />
  {:else}
    <Form on:transfer={ontransfer} />
  {/if}
</main>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
