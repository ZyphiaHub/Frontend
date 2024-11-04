<script>
    
    export let card;
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    function flipCard() {
        // @ts-ignore
        if (!card.flipped) {
            dispatch('flip');
        }
    }
</script>

<button class="card" on:click={flipCard} aria-label="Flip card">
    <div class="card-inner {card.flipped ? 'flipped' : ''}">
        <div class="card-front"></div>
        <div class="card-back">
            <img src={card.img} alt="" />
        </div>
    </div>
</button>


<style>
    .card {
        width: 100px;
        height: 100px;
        perspective: 1000px;
        animation: float 3s ease-in-out infinite; /* Lebegő animáció */
    }

    .card-inner {
        position: relative;
        width: 100%;
        height: 100%;
        transition: transform 0.6s;
        transform-style: preserve-3d;
    }

    .card-inner.flipped {
        transform: rotateY(180deg);
    }

    .card-front,
    .card-back {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        border-radius: 10px;
    }

    .card-front {
        background-color: #333; /* Sötét háttér a kártya hátlapján */
        border: 2px solid #ffa500; /* Narancssárga keret */
    }

    .card-back {
        transform: rotateY(180deg);
    }

    .card-back img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    /* Lebegő effekt */
    @keyframes float {
        0%, 100% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(-5px);
        }
    }
</style>
