<script>
  import axios from "axios"
  import { onMount } from "svelte"
  export let userinfo, joinroom, socket
  import { createEventDispatcher } from "svelte"
  const dispatch = createEventDispatcher()

  let currentMessage
  let msglist = []

  onMount(async () => {
    console.log(joinroom)
    const { data } = await axios.get(
      `http://localhost:5000/api/chatroom/${joinroom}`
    )
    console.log(data)
    console.log(msglist)
    msglist = data
    console.log("data", msglist)
  })

  const displaymsg = async () => {
    const msgdata = {
      room: joinroom,
      user: userinfo.user,
      message: currentMessage,
      time:
        new Date(Date.now()).getHours() +
        ":" +
        new Date(Date.now()).getMinutes(),
    }
    await socket.emit("send_message", msgdata)
    msglist = [...msglist, msgdata]
    currentMessage = ""
  }

  let count = 0

  $: socket.off("receive_msg").on("receive_msg", (data) => {
    console.log(data)
    msglist = [...msglist, data]
    console.log(msglist)
  })

  $: console.log(count)
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

<div
  class="mx-10 mt-5 mb-2 flex w-full items-center gap-2 rounded-lg lg:gap-6 ">
  <button on:click={() => dispatch("leftchat")}>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 24 24"
      stroke-width="1.5"
      stroke="currentColor"
      class="ml-3 h-10 w-10  fill-gray-300">
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
    </svg>
  </button>

  <div class="hidden w-1/5 rounded-full bg-slate-300 p-3  lg:flex">
    <h1 class=" text-center text-xl font-semibold ">
      {joinroom} Chatroom
    </h1>
  </div>
  <input
    bind:value={currentMessage}
    type="text"
    id="user"
    placeholder="Enter Message"
    class=" w-3/5 rounded-full border border-gray-300 bg-slate-200 py-2 px-3 text-base leading-8 text-gray-700 outline-none transition-colors duration-200 ease-in-out focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200" />

  <button on:click={displaymsg}>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      class="h-10 w-10  fill-gray-300">
      <path
        d="M3.478 2.405a.75.75 0 00-.926.94l2.432 7.905H13.5a.75.75 0 010 1.5H4.984l-2.432 7.905a.75.75 0 00.926.94 60.519 60.519 0 0018.445-8.986.75.75 0 000-1.218A60.517 60.517 0 003.478 2.405z" />
    </svg></button>
</div>

<style>
  .height {
    height: 28rem;
  }
</style>
