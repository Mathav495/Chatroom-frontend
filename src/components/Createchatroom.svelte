<script>
  import { createEventDispatcher } from "svelte"
  const dispatch = createEventDispatcher()
  export let userinfo
  let join
</script>

<div class="{userinfo.color} mx-20 mt-5 h-96 rounded-lg p-4">
  <form
    class="flex flex-col"
    on:submit|preventDefault={() => dispatch("display", join)}>
    <label for="Color" class="text-xl font-semibold text-slate-800"
      >Join Chatroom</label>
    <select
      bind:value={join}
      class="mb-4 mt-2 h-12 w-1/2 rounded-lg text-base font-semibold"
      id="color">
      <option class="p-2 text-lg font-semibold" value="Javascript"
        >Javascript</option>
      <option class="p-2 text-lg font-semibold" value="HTML">HTML</option>
      <option class="p-2 text-lg font-semibold" value="CSS">CSS</option>
    </select>

    <button
      class="w-36 rounded border-0 bg-indigo-500 py-2 px-6 text-lg text-white disabled:cursor-not-allowed disabled:bg-indigo-300"
      >Join</button>
  </form>
</div>
