<script>
  import axios from "axios"
  import { onMount, createEventDispatcher } from "svelte"
  import Chatbox from "./Chatbox.svelte"
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

<Chatbox {userinfo} {msglist} />

<div class="mx-5 mt-5 mb-2 flex w-full items-center gap-1 rounded-lg lg:gap-6 ">
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
