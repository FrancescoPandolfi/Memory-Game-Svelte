<script>
  import Button from "./Button.svelte";
  import {GAME_WON} from "../config/config";

  export let score;
  export let timeToSolveTheLevel;
  export let reset;
  export let gameState;
  export let level;
</script>


<div class="backdrop">
    <div class="modal">
        {#if gameState === GAME_WON}
            <img src="img/win.svg">
            <p>Level {level + 1} passed</p>
        {:else }
            <img src="img/lost.svg">
            <p>You lose</p>
        {/if}
        <span class="results">Time: {timeToSolveTheLevel} Score: {score}</span>
        <Button text="{gameState === GAME_WON ? 'Go next level' : 'Play again'}" clicked="{reset}"/>
    </div>
</div>


<style>
  img {
    width: 40%;
    margin-bottom: 5px;
  }
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 10;
    background-color: rgba(0, 0, 0, 0.7);
  }

  .results {
    margin-bottom: 20px;
    display: block;
  }

  p {
    padding: 0;
    margin: 0;
    font-weight: bold;
    font-size: 20px;
  }

  .modal {
    position: absolute;
    z-index: 20;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    padding: 20px;
    border-radius: 5px;
    background-color: #e1e1e1;
  }
</style>
