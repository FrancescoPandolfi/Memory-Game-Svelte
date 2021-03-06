<script>
  import Header from './components/Header.svelte'
  import Card from "./components/Card.svelte";
  import {onMount} from 'svelte';
  import shuffle from "./helpers/shuffle"
  import {
    configGuessingTime,
    GAME_LOST,
    GAME_MEMORIZATION, GAME_PLAYING,
    GAME_STANDBY,
    GAME_WON,
    globalMemorizationTime,
    points
  } from "./config/config";
  import Modal from "./components/Modal.svelte";
  import Button from "./components/Button.svelte";


  let score = 0;
  let level = 0;
  let showModal = false;
  let gameState = GAME_STANDBY;
  let isGameStarted = false;
  let guessingTime;
  let remainingTime;
  let memorizationTime = globalMemorizationTime;
  let timeInterval;


  let cards = [
    {id: 1, src: 'img/bananas.svg', isShowed: true, isGuessed: false},
    {id: 2, src: 'img/bananas.svg', isShowed: true, isGuessed: false},
    {id: 3, src: 'img/arrow.svg', isShowed: true, isGuessed: false},
    {id: 4, src: 'img/arrow.svg', isShowed: true, isGuessed: false},
    {id: 5, src: 'img/leaves.svg', isShowed: true, isGuessed: false},
    {id: 6, src: 'img/leaves.svg', isShowed: true, isGuessed: false},
    {id: 7, src: 'img/spider.svg', isShowed: true, isGuessed: false},
    {id: 8, src: 'img/spider.svg', isShowed: true, isGuessed: false},
    {id: 9, src: 'img/gun.svg', isShowed: true, isGuessed: false},
    {id: 10, src: 'img/gun.svg', isShowed: true, isGuessed: false},
    {id: 11, src: 'img/monkey.svg', isShowed: true, isGuessed: false},
    {id: 12, src: 'img/monkey.svg', isShowed: true, isGuessed: false}
  ];

  onMount(() => {
    shuffle(cards);
    guessingTime = configGuessingTime;
    remainingTime = configGuessingTime;
  });

  /**
   * Start the game
   * */
  const startGame = () => {
    isGameStarted = true;
    gameState = GAME_MEMORIZATION;
    const interval = setInterval(() => {
      memorizationTime--;
      if (memorizationTime === 0) {
        clearInterval(interval);
        coverAllCards();
        startGuessing();
      }
    }, 1000);
  }

  /**
   * Cover all the cards
   * */
  const coverAllCards = () => {
    cards.map(card => card.isShowed = false);
    cards = cards
  }

  /**
   * Start to match cards
   * */
  const startGuessing = () => {
    gameState = GAME_PLAYING;
    timeInterval = setInterval(() => {
      remainingTime--;
      if (remainingTime === 0) {
        clearInterval(timeInterval);
        showHideModal(GAME_LOST);
        const audio = new Audio('audio/lose-sound.mp3');
        audio.play();
      }
    }, 1000)
  }

  /**
   * Card turn handler
   * */
  const handleTurn = (id) => {
    showClickedCard(id);
    checkCards();
  }

  /**
   * Show the clicked card
   * */
  const showClickedCard = (id) => {
    let flippedCards = cards.filter(card => card.isShowed && !card.isGuessed);
    if (flippedCards.length <= 1) {
      cards.map(card => {
        if (card.id === id && !card.isShowed) {
          card.isShowed = true;
        }
      });
      cards = cards
    }


  }

  /**
   * Check the the selected cards and assign points
   * */
  const checkCards = () => {
    let flippedCards = cards.filter(card => card.isShowed && !card.isGuessed);
    if (flippedCards.length === 2) {
      setTimeout(() => {
        cards.map(card => {
          if (card.id === flippedCards[0].id || card.id === flippedCards[1].id) {
            // If cards are different
            if (flippedCards[0].src !== flippedCards[1].src) {
              card.isShowed = false;
              score = score - points;
              // If cards are the same
            } else {
              card.isGuessed = true;
              score = score + points;
            }
          }
        })
        cards = cards;
        // If all the cards are matched the game is won
        if (cards.every(card => card.isGuessed)) {
          clearInterval(timeInterval);
          gameState = GAME_WON;
          showHideModal(GAME_WON);
          new Audio('audio/win.wav').play();
          memorizationTime = globalMemorizationTime;
        }
      }, 500)
    }
  }

  /**
   * Show or hide modal
   * */
  const showHideModal = (state) => {
    if (state) {
      gameState = state;
    }
    showModal = !showModal;
  }

  /**
   * Reset the game
   * */
  const reset = () => {
    cards.map(card => {
      card.isGuessed = false;
      card.isShowed = true;
    });
    cards = cards
    showHideModal();
    isGameStarted = false;
    shuffle(cards);

    if (gameState === GAME_WON) {
      goNextLevel();
    } else {
      score = 0;
      level = 0;
      memorizationTime = globalMemorizationTime;
      remainingTime = configGuessingTime;
      guessingTime = configGuessingTime;
    }
  }

  /**
   * Go next level
   * */
  const goNextLevel = () => {
    level++;
    guessingTime = Math.floor(configGuessingTime - configGuessingTime / 100 * (5 * level));
    remainingTime = guessingTime;
    startGame();
  }
</script>


<main>
    <Header {isGameStarted} timeLeft={remainingTime} {guessingTime}/>
    <div class="card-board">
        {#if !isGameStarted}
            <Button text="START" clicked="{startGame}"/>
        {/if}
        {#if isGameStarted}
            {#each cards as {id, src, isShowed}, i}
                <Card {isShowed} {handleTurn} {src} {id}/>
            {/each}
        {/if}
    </div>
    {#if showModal}
        <Modal {gameState} {reset} {score} {level} timeToSolveTheLevel="{guessingTime - remainingTime}"/>
    {/if}
</main>


<style>
  main {
    height: 100vh;
    text-align: center;
    margin: 0 auto;
    max-width: 450px;
  }

  .card-board {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    height: 85vh;
    padding: 0 30px 30px;
  }

  @media (hover: none) and (pointer: coarse) {
    main {
      height: 85vh;
    }

    .card-board {
      height: 70vh;
    }
  }

</style>
