<script>
  import axios from "axios"
  import { onMount, createEventDispatcher } from "svelte"
  import Leftbutton from "./leftbutton.svelte"
  import Sendbutton from "./Sendbutton.svelte"
  export let userinfo, joinroom, socket
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

  $: socket.off("receive_msg").on("receive_msg", (data) => {
    console.log(data)
    msglist = [...msglist, data]
    console.log(msglist)
  })
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
  <Leftbutton on:click={() => dispatch("leftchat")} />

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

  <Sendbutton on:click={displaymsg} />
</div>

<style>
  .height {
    height: 28rem;
  }
</style>
