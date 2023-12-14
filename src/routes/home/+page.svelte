<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import axios from "axios";

  import "../../app.css";

  // @ts-ignore
  $: data = {
    cpu: { brand: "Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz", frequency: 2592 },
    ram: 17085616128,
    ram_used: 10790658048,
    uptime: 1071341,
    os: "Windows 11 Pro",
    networks: [
      {
        name: "VMware Network Adapter VMnet1",
        total_income: 55,
        total_outcome: 2065,
      },
      { name: "vEthernet (WSL)", total_income: 1204, total_outcome: 8187102 },
      {
        name: "VMware Network Adapter VMnet8",
        total_income: 55,
        total_outcome: 5687,
      },
      { name: "Wi-Fi", total_income: 7985059091, total_outcome: 163631332 },
    ],
    disks: [
      { name: "", size: 317920899072, free: 183748341760, file_system: "NTFS" },
      {
        name: "Local Disk",
        size: 681451712512,
        free: 574148104192,
        file_system: "NTFS",
      },
    ],
  };

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
      const response = await axios.get(
        "http://localhostfutures_util:8000/api/sysinfo"
      );
      data = response.data;
      console.log(response);
    } catch (error) {
      console.error(error);
    }
  });
</script>

<section>
  <div class="block">
    <b>OS: </b> <i>{data.os}</i>
  </div>
  <div class="block">
    <b>CPU: </b>
    <br />
    <i>{data.cpu.brand}</i>
    <br />
    <br />
    <b>Frequency: </b><i>{Math.round((data.cpu.frequency / 1000) * 10) / 10}</i>
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
