<script>
// @ts-nocheck

    import { onMount } from 'svelte';
    import Card from './Card.svelte';
    import { images, difficulties } from './gameData.js';

    let score = 100;
    let selectedDifficulty = 'easy';
    let cards = [];
    let flippedCards = [];
    let matchedCards = [];

    function startGame() {
        // Keverjük össze a képeket és párosítsuk
        let shuffledImages = [...images, ...images].slice(0, difficulties[selectedDifficulty] * 2);
        shuffledImages = shuffledImages.sort(() => 0.5 - Math.random());
        
        cards = shuffledImages.map((img, index) => ({ id: index, img, flipped: false }));
        score = 100;
        matchedCards = [];
    }

    function handleCardFlip(card) {
        if (flippedCards.length < 2 && !card.flipped && !matchedCards.includes(card.id)) {
            card.flipped = true;
            flippedCards = [...flippedCards, card];
        }

        if (flippedCards.length === 2) {
            if (flippedCards[0].img === flippedCards[1].img) {
                matchedCards = [...matchedCards, ...flippedCards.map(c => c.id)];
            } else {
                score--;
            }

            setTimeout(() => {
                flippedCards.forEach(c => c.flipped = false);
                flippedCards = [];
            }, 1000);
        }
    }

    onMount(startGame);
</script>

<select bind:value={selectedDifficulty} on:change={startGame}>
    <option value="easy">Easy</option>
    <option value="medium">Medium</option>
    <option value="hard">Hard</option>
</select>

<div class="score">Score: {score}</div>

<div class="game-board">
    {#each cards as card}
        <Card {card} on:flip={() => handleCardFlip(card)} />
    {/each}
</div>

<style>
    :global(body) {
        background-color: #2a2b2d; /* Sötét háttér */
        color: #ffa500; /* Narancssárga szöveg a Halloween hangulatért */
        font-family: 'Creepster', cursive;
    }
    
    .game-board {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
        gap: 15px;
        padding: 20px;
    }

    .score {
        font-size: 2em;
        color: #ffa500;
        margin: 20px 0;
    }

    .halloween-select {
        background-color: #444;
        color: #ffa500;
        border: none;
        padding: 10px;
        font-size: 1.2em;
    }
</style>
