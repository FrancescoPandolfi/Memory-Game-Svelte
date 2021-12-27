<script>
  import TimeBar from "./TimeBar.svelte";

  export let isGameStarted = false;
  export let timeLeft;
  export let guessingTime;
</script>


<div class="header">
    {#if !isGameStarted}
        <img src="img/logo.svg" alt="Memory logo">
    {:else}
        <TimeBar {timeLeft} {guessingTime} />
    {/if}
</div>


<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 15vh;
  }

  img {
    width: 200px;
  }
</style>
