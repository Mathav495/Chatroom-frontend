<script>
  import { createEventDispatcher, onMount } from "svelte"
  import axios from "axios"
  import io from "socket.io-client"
  import Chatpage from "../components/Chatpage.svelte"
  import Socialmedia from "../components/socialmedia.svelte"
  import Existchatroom from "../components/Existchatroom.svelte"
  const dispatch = createEventDispatcher()
  export let userinfo
  let data = false
  let show = true
  let joinroom
  let roomname
  let existingroom = []
  let errormsg = ""
  let errorjoin = ""

  const socket = io.connect("http://localhost:5000")

  const joinoption = async () => {
    console.log(joinroom)
    socket.emit("join_room", joinroom)
    if (joinroom == null) {
      errorjoin = "Enter the correct chatroom name"
      console.log(errorjoin)
    } else {
      const { data } = await axios.get(
        `http://localhost:5000/api/roomlist/${joinroom}`
      )
      if (data == "Data not existed") {
        errorjoin = "Group not created"
      } else {
        show = false
        errorjoin = ""
      }
    }
  }

  onMount(async () => {
    const { data } = await axios.get(`http://localhost:5000/api/roomlist/`)
    console.log(data)
    existingroom = data
    console.log(existingroom)
  })

  const createchat = async () => {
    if (roomname == null) {
      errormsg = "Enter the correct valid name"
    } else {
      console.log(roomname)
      const sample = {
        room: roomname,
      }
      const { data } = await axios.post(
        `http://localhost:5000/api/roomlist/`,
        sample
      )
      console.log(data)
      if (data == "Already existed") {
        errormsg = "Group already existed"
      } else {
        errormsg = ""
        existingroom = [...existingroom, data]
        roomname = ""
      }
    }
  }

  const leftchat = () => {
    show = true
  }
</script>

<div class="background h-screen w-screen overflow-x-hidden">
  <div
    class="mx-10 mt-5 flex rounded-lg {userinfo.color} items-center p-4 opacity-95">
    <img class="h-12 w-12 rounded-lg" src="/assets/chat.png" alt="logo" />
    <h1 class="text-2xl font-bold text-slate-800">{userinfo.user}</h1>

    <button
      on:click={() => dispatch("display", data)}
      class="ml-auto inline-flex w-36 items-center gap-2 rounded border-0 bg-indigo-500 py-2 px-6 text-lg text-white disabled:cursor-not-allowed disabled:bg-indigo-300">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="white"
        class="h-8 w-8">
        <path
          fill-rule="evenodd"
          d="M7.5 3.75A1.5 1.5 0 006 5.25v13.5a1.5 1.5 0 001.5 1.5h6a1.5 1.5 0 001.5-1.5V15a.75.75 0 011.5 0v3.75a3 3 0 01-3 3h-6a3 3 0 01-3-3V5.25a3 3 0 013-3h6a3 3 0 013 3V9A.75.75 0 0115 9V5.25a1.5 1.5 0 00-1.5-1.5h-6zm5.03 4.72a.75.75 0 010 1.06l-1.72 1.72h10.94a.75.75 0 010 1.5H10.81l1.72 1.72a.75.75 0 11-1.06 1.06l-3-3a.75.75 0 010-1.06l3-3a.75.75 0 011.06 0z"
          clip-rule="evenodd" />
      </svg>

      Logout</button>
  </div>

  {#if show}
    <div class="mx-10 mt-7 flex flex-col gap-5 md:flex-row">
      <!--Existing Chatrooms-->
      <Existchatroom {existingroom} />

      <!--Second Content-->
      <div class="order-1 flex w-full flex-col gap-6 md:order-none md:w-1/2">
        <!--Create Chatroom-->
        <div class="rounded-lg bg-slate-200 p-4">
          <h1 class="text-xl font-semibold text-slate-700">Create Chatroom</h1>
          <div class="flex items-center gap-4">
            <input
              bind:value={roomname}
              type="text"
              placeholder="Enter Chatroom name"
              class="mt-4 mb-4  w-1/2 rounded-lg border border-gray-800 bg-slate-200 py-1 px-3 text-base leading-8 text-gray-700 outline-none transition-colors duration-200 ease-in-out focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200" />
            <button on:click={createchat}>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                class="h-8 w-8  fill-slate-500">
                <path
                  d="M3.478 2.405a.75.75 0 00-.926.94l2.432 7.905H13.5a.75.75 0 010 1.5H4.984l-2.432 7.905a.75.75 0 00.926.94 60.519 60.519 0 0018.445-8.986.75.75 0 000-1.218A60.517 60.517 0 003.478 2.405z" />
              </svg>
            </button>
          </div>
          <h1 class="text-lg font-semibold text-rose-600">{errormsg}</h1>
        </div>

        <!--Join Chatroom-->
        <div class=" rounded-lg bg-slate-200 p-4">
          <h1 class="text-xl font-semibold text-slate-700">Join Chatroom</h1>
          <div class="flex items-center gap-4">
            <input
              bind:value={joinroom}
              type="text"
              placeholder="Enter Chatroom name"
              class="mt-4 mb-4  w-1/2 rounded-lg border border-gray-800 bg-slate-200 py-1 px-3 text-base leading-8 text-gray-700 outline-none transition-colors duration-200 ease-in-out focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200" />
            <button on:click={joinoption}>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                class="h-8 w-8  fill-slate-500">
                <path
                  d="M3.478 2.405a.75.75 0 00-.926.94l2.432 7.905H13.5a.75.75 0 010 1.5H4.984l-2.432 7.905a.75.75 0 00.926.94 60.519 60.519 0 0018.445-8.986.75.75 0 000-1.218A60.517 60.517 0 003.478 2.405z" />
              </svg>
            </button>
          </div>
          <h1 class="text-lg font-semibold text-rose-600">{errorjoin}</h1>
        </div>
        <Socialmedia />
      </div>
    </div>
  {:else}
    <Chatpage {userinfo} {joinroom} {socket} on:leftchat={leftchat} />
  {/if}
</div>

<style global>
  h1 {
    font-family: "Acme";
  }
  button {
    font-family: "Acme";
  }
  input {
    font-family: "Acme";
  }
  label {
    font-family: "Acme";
  }
</style>
