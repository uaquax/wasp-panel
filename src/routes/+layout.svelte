<script>
  import "../app.css";
</script>

<section>
  <div class="container">
    <div class="side">
      <div class="side-menu">
        <a href="/home" class="side-tab">Home</a>
        <a href="/terminal" class="side-tab">Terminal</a>
        <!-- <a href="/" class="side-tab">Xray</a> -->
        <!-- <a href="/" class="side-tab">TextGW</a> -->
      </div>
    </div>
    <div class="content">
      <slot />
    </div>
  </div>
</section>

<style>
  .container {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    height: 100vh;
    margin: 0;
    padding: 0;
    width: 100vw;
  }

  .content {
    width: 100%;
    height: 100%;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .side {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    height: 100vh;
    margin: 0;
    padding: 0;
  }

  .side-menu {
    color: #eee;
    background: #1f2335;
    max-width: 18rem;
    min-width: 18rem;
    min-height: 100vh;
    margin: 0;
    display: flex;
    flex-direction: column;
  }

  .side-tab {
    margin: 1rem;
  }

  a {
    text-decoration: none;
    outline: none;
    color: #eee;
  }

  a:hover {
    color: #bb9af7;
  }
</style>
