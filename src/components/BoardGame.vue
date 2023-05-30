<script setup lang="ts">
import GameCard from './GameCard.vue';

import carouselImg from '/public/cards/carousel.jpg';
import horseImg from '/public/cards/horse.jpg';
import lightouseImg from '/public/cards/light-house.jpg';
import tivoliByNightImg from '/public/cards/tivoli-by-night.jpg';
import tivoliImg from '/public/cards/tivoli.jpg';
import { ref } from 'vue';

class BoardCard {
    constructor(
        public name: string,
        public img: string,
        public hasBeenRendered: number = 0,
        public flippedCard: boolean = false,
        public isShowing: boolean = false
    ) {}
}

const cards = ref<BoardCard[]>([
    new BoardCard('tivoliByNight', tivoliByNightImg),
    new BoardCard('carousel', carouselImg),
    new BoardCard('horse', horseImg),
    new BoardCard('lighthouse', lightouseImg),
    new BoardCard('tivoliByNight', tivoliByNightImg),
    new BoardCard('tivoli', tivoliImg),
    new BoardCard('lighthouse', lightouseImg),
    new BoardCard('tivoli', tivoliImg),
    new BoardCard('carousel', carouselImg),
    new BoardCard('horse', horseImg)
]);

let lastClickedCard = '';

const handleClick = (card: BoardCard) => {
    console.log('clicked ' + card.name + ' ishowing: ' + card.isShowing, ' flipped: ' + card.flippedCard);

    if (lastClickedCard == card.name) {
        card.flippedCard = true;
    } else {
        card.isShowing = true;
        lastClickedCard = card.name;

        // cards.value.forEach(card => {
        //     card.isShowing = false;
        // })
        
    }
}

</script>

<template>
    <main>
        <div v-for="card in cards">
            <GameCard @click="handleClick(card)"
            :flipped="card.flippedCard"
            :isShowing="card.isShowing"
            :name="card.name" :img="card.img" />
        </div>
    </main>
</template>

<style scoped>
main {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(2, 1fr);
    gap: 10px;
}
</style>
