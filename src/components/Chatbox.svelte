<script>
  export let msglist, userinfo
</script>

<div
  class="height  mx-10 mt-5 overflow-y-auto  rounded-lg bg-slate-300 p-4 opacity-90">
  {#each msglist as msg}
    <div
      class="mb-2 flex {userinfo.user == msg.user
        ? 'justify-end'
        : 'justify-start'}">
      <div
        class="items-center rounded-lg {userinfo.user == msg.user
          ? ' bg-emerald-500'
          : ' bg-rose-500'}  p-2">
        <h1 class="pb-1 text-start text-sm font-semibold text-slate-800 ">
          {userinfo.user == msg.user ? "You" : msg.user}
        </h1>
        <div class="flex items-center gap-4">
          <h1 class="text-base font-semibold text-gray-50">{msg.message}</h1>
          <h1 class="ml-auto text-xs font-semibold text-slate-800">
            {msg.time}
          </h1>
        </div>
      </div>
    </div>
  {/each}
</div>

<style>
  .height {
    height: 28rem;
  }
</style>
