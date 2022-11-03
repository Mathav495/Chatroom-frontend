<script>
  import axios from "axios"
  import { createEventDispatcher } from "svelte"
  import ChitChat from "../components/ChitChat.svelte"
  import ChitChatsm from "../components/ChitChatsm.svelte"
  const dispatch = createEventDispatcher()
  let username,
    usercolor,
    disabled = false,
    error = ""

  const adduserdata = async () => {
    if (username == null) {
      error = "Username cannot be empty"
    } else {
      const sample = {
        user: username,
        color: usercolor,
      }
      const { data } = await axios.post(
        "http://localhost:5000/api/users",
        sample
      )
      console.log(data)
      dispatch("transfer", data)
      username = ""
    }
  }
</script>

<div class="background h-screen w-screen overflow-x-hidden">
  <div
    class="mt-0 flex  flex-col gap-2 p-6 md:mt-12 md:flex-row md:p-8 lg:mt-20 lg:gap-24 lg:p-10">
    <ChitChatsm />

    <div
      class="w-full rounded-lg bg-slate-200 p-5 opacity-90 md:w-1/2 lg:w-1/3">
      <h1 class="mb-5 text-3xl font-semibold text-slate-500">Sign in</h1>
      <form class="mb-10 flex flex-col" on:submit|preventDefault={adduserdata}>
        <label for="user" class="mt-8 text-xl font-semibold text-slate-800"
          >Username</label>
        <input
          bind:value={username}
          type="text"
          id="user"
          class="mt-2 mb-1  w-full  rounded border border-gray-300 bg-white py-1 px-3 text-base leading-8 text-gray-700 outline-none transition-colors duration-200 ease-in-out focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200" />
        <h1 class="text-base font-semibold text-rose-500">{error}</h1>
        <label for="Color" class="text-xl font-semibold text-slate-800"
          >Color</label>
        <select
          class="mb-4 mt-2 h-12 w-full rounded-lg text-base font-semibold"
          id="color"
          bind:value={usercolor}>
          <option class="p-2 text-lg font-semibold" value="bg-slate-300"
            >Slate</option>
          <option class="p-2 text-lg font-semibold" value="bg-gray-300"
            >Gray</option>
          <option class="p-2 text-lg font-semibold" value="bg-rose-300"
            >Rose</option>
          <option class="p-2 text-lg font-semibold" value="bg-emerald-300"
            >Emerald</option>
          <option class="p-2 text-lg font-semibold" value="bg-yellow-300"
            >Yellow</option>
          <option class="p-2 text-lg font-semibold" value="bg-teal-300"
            >Teal</option>
          <option class="p-2 text-lg font-semibold" value="bg-pink-300"
            >Pink</option>
          <option class="bg- p-2 text-lg font-semibold" value="bg-green-300"
            >Green</option>
        </select>

        <button
          {disabled}
          class="mt-6 w-36 rounded border-0 bg-indigo-500 py-2 px-6 text-lg text-white disabled:cursor-not-allowed disabled:bg-indigo-300"
          >Sign in</button>
      </form>
    </div>

    <ChitChat />
  </div>
</div>

<style>
  :global .background {
    background-image: url("https://cdn.dribbble.com/users/1162077/screenshots/4318436/media/1e17490dd92ca27c4a6274ab7bff5b11.png?compress=1&resize=400x300");
  }
</style>
