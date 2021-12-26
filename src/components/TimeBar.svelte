<script>
  import {configGuessingTime} from "../config/config";

  export let guessingTime = configGuessingTime;
  export let timeLeft = configGuessingTime;
  $: timerLeftInPercentage = timeLeft * 100 / guessingTime;
</script>


<div class="time-bar-container">
    <span class="timer">{timeLeft}</span>
    <div class="time-bar" style="width: {timerLeftInPercentage}%"></div>
</div>


<style>
  .time-bar-container {
    background-color: #1D9DD1;
    width: 70%;
    border-radius: 10px;
    height: 25px;
    position: relative;
  }

  .time-bar {
    height: 25px;
    border-radius: 10px;
    padding: 3px 0;
    background-color: rgba(255, 168, 0, 0.9);
    transition: 1s;
  }

  .timer {
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    color: white;
    font-weight: bold;
    font-size: 16px;
  }
</style>
