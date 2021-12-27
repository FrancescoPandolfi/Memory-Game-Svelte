<script>
  export let text;
  export let clicked;
</script>


<div class="container">
    <button on:click={clicked}>{text}</button>
</div>


<style>
  .container {
    width: 100%;
    text-align: center;
  }

  button {
    cursor: pointer;
    padding: 5px 20px;
    border: 0;
    color: white;
    font-family: 'Sarpanch', sans-serif;
    background-color: #1D9DD1;
    border-radius: 10px;
    font-size: 18px;
  }
</style>

