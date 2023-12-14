<script>
  // @ts-nocheck
  import { onMount } from "svelte";

  let ws;
  $: command = "";
  $: lastCommand = "";
  $: messages = [];

  $: isLoading = false;

  function sendCommand() {
    console.log(command.length);
    if (command.length >= 1) {
      isLoading = true;
      ws.send(command);
      console.log(command);

      lastCommand = command;
      command = "";
    }
  }

  function sendCommandKeyboard(event) {
    if (event.key === "Enter") {
      sendCommand();
    }
  }

  function updateScroll() {
    setTimeout(() => {
      var element = document.querySelector(".output");
      element.scrollTop = element.scrollHeight;
    }, 0);
  }

  onMount(() => {
    ws = new WebSocket("ws://127.0.0.1:3030/terminal");
    ws.onopen = function () {
      console.log("Connected to the server");
    };

    ws.onmessage = function (event) {
      if (event.data.length >= 1) {
        messages = [...messages, { command: lastCommand, output: event.data }];
        updateScroll();
        isLoading = false;
      }
    };
  });
</script>

<div class="container">
  {#if isLoading}
    <p>Loading ...</p>
  {/if}
  <div class="output">
    {#each messages as message}
      <div class="msg-container">
        $ <i>{message.command}</i>
        <pre class="message">{message.output}</pre>
      </div>
    {/each}
  </div>
  <div class="input">
    <input
      on:keydown={sendCommandKeyboard}
      type="text"
      bind:value={command}
      placeholder="Enter a command..."
      class="terminal-input"
    />
    <button class="terminal-input-btn" on:click={sendCommand}>Enter</button>
  </div>
</div>

<style>
  .container {
    width: 100%;
    height: 100%;
    max-width: calc(100vw - 18rem);
  }

  .output {
    margin: 20px;
    border-radius: 2rem;
    padding: 15px;
    min-height: calc(100vh - 7.8rem);
    max-height: calc(100vh - 7.8rem);
    max-width: calc(100vw - 18rem);
    background: #1f2335;
    overflow: auto;
  }

  .output .message {
    background: #24283b;
    padding: 15px;
    text-align: left;
    border-radius: 0.5rem;
    margin: 10px;
    word-wrap: break-word;
    white-space: pre-wrap;
    overflow-wrap: break-word;
  }

  .output .msg-container {
    text-align: left;
  }

  .output .msg-container i {
    color: #707070;
  }

  .input {
    display: flex;
    align-items: center;
    position: absolute;
    bottom: 0;
    left: 18rem;
    width: calc(100% - 18rem);
    padding: 1rem;
  }

  .terminal-input {
    flex-grow: 1;
    border: none;
    color: #eee;
    background: #1f2335;
    outline: none;
    padding: 20px;
    border-radius: 2rem;
  }

  .terminal-input-btn {
    margin: 8px;
    padding: 15px;
    border-radius: 1rem;
    border: none;
    color: #eee;
    background-image: linear-gradient(45deg, #f7768e, #bb9af7, #2ac3de);
    background-size: 200% 200%;
    transition: all 0.3s;
    opacity: 0.8;
  }

  .terminal-input-btn:hover {
    opacity: 1;
    animation: GradientAnimation 2s ease infinite;
  }

  @keyframes GradientAnimation {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }

  .terminal-input-btn:active {
    margin: 8px;
    padding: 15px;
    border-radius: 1rem;
    border: none;
    background-color: #7aa2f7;
    border: 1px #7aa2f7 solid;
    color: white;
    transition: all 0.2s;
  }
</style>
