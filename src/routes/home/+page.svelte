<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import axios from "axios";

  import "../../app.css";

  // @ts-ignore
  $: data = {};
  $: isVisible = false;

  function toHHMMSS(seconds) {
    const hours = Math.floor(seconds / 3600);
    const minutes = Math.floor((seconds - hours * 3600) / 60);
    const secs = seconds - hours * 3600 - minutes * 60;

    return [hours, minutes, secs]
      .map((val) => (val < 10 ? "0" + val : val))
      .join(":");
  }

  onMount(async () => {
    try {
      const response = await axios.get("http://localhost:8181/sysinfo");
      data = response.data;
      isVisible = true;
    } catch (error) {
      console.error(error);
    }
  });
</script>

<section>
  {#if isVisible}
    <div class="block">
      <b>OS: </b> <i>{data.os}</i>
    </div>
    <div class="block">
      <b>CPU: </b>
      <br />
      <i>{data.cpu.brand}</i>
      <br />
      <br />
      <b>Frequency: </b><i
        >{Math.round((data.cpu.frequency / 1000) * 10) / 10}</i
      >
      GHz
    </div>
    <div class="block">
      <b>RAM: </b>
      <i>{Math.round((data.ram_used / 1024 / 1024 / 1024) * 10) / 10} GB</i> /
      <i>{Math.round((data.ram / 1024 / 1024 / 1024) * 10) / 10} GB</i>
    </div>
    <div class="block">
      <b>Uptime: </b>
      <i>{toHHMMSS(data.uptime)}</i>
    </div>
  {/if}
</section>

<style>
  * {
    transition: all 0.2s;
  }

  .block {
    width: fit-content;
    background: #1f2335;
    padding: 20px;
    margin: 10px;
    border-radius: 15px;
  }
</style>
