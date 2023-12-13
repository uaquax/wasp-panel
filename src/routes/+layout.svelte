<script>
  import "../app.css";
</script>

<section>
  <header>
    <div class="logo">WASP</div>
  </header>
  <slot />
</section>

<style>
  header {
    text-align: center;
    background: #1f2335;
    padding: 5px;
  }

  .logo {
    color: #7aa2f7;
  }
</style>
