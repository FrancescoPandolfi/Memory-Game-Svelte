<script>
    // the variables are updated whenever the props values changes...
    export let isShowed;
    export let handleTurn;
    export let src;
    export let id;
</script>


<div class="flip-card" on:click="{handleTurn(id)}">
    <div class="flip-card-inner {isShowed ? 'flip-the-card' : ''}">
        <div class="flip-card-front">
            <img width="55%" src="img/card-bg.svg">
        </div>
        <div class="flip-card-back">
            <img width="55%" {src}>
        </div>
    </div>
</div>


<style>
  .flip-card {
    background-color: transparent;
    width: 30%;
    height: 22%;
    perspective: 1000px;
    cursor: pointer;
    transition: 0.3s;
  }
  .flip-card:hover {
    transform: translate(0, -5px);
  }
  @media (pointer:coarse) {
    .flip-card:hover {
      transform: none;
    }
  }

  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  }

  .flip-the-card {
    transform: rotateY(180deg);
  }

  .flip-card-front, .flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    border-radius: 5px;
  }

  .flip-card-front {
    background-color: #1D9DD1;
    color: black;
  }

  .flip-card-back {
    background-color: #eeeeee;
    color: white;
    transform: rotateY(180deg);
  }
</style>
