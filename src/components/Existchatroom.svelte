<script>
  export let existingroom
</script>

<div class="order-2 w-full rounded-lg bg-slate-200 p-4 md:order-none md:w-1/2">
  <h1 class="text-xl font-semibold text-slate-700">Existing Chatrooms</h1>
  <div class="mt-4">
    {#each existingroom as newroom}
      <div class="mb-2 w-1/2 rounded-lg bg-slate-300 p-2">
        <h1 class="text-xl font-semibold text-slate-700">
          {newroom.room}
        </h1>
      </div>
    {/each}
  </div>
</div>
