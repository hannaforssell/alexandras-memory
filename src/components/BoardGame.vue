<script setup lang="ts">
import GameCard from './GameCard.vue';

import carouselImg from '/public/cards/carousel.jpg';
import horseImg from '/public/cards/horse.jpg';
import lightouseImg from '/public/cards/light-house.jpg';
import tivoliByNightImg from '/public/cards/tivoli-by-night.jpg';
import tivoliImg from '/public/cards/tivoli.jpg';
import { ref, reactive } from 'vue';
import lodash from 'lodash';

const state = reactive({
    update: 0,
    victory: false
});

class BoardCard {
    constructor(
        public name: string,
        public img: string,
        public hasBeenRendered: number = 0,
        public flippedCard: boolean = false,
        public isShowing: boolean = false
    ) { }
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

const initCards = () => {
    const cardsToBePlaced: BoardCard[] = [
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
    ];

    const shuffledCards = lodash.shuffle(cardsToBePlaced);

    cards.value = shuffledCards;
}

const handleClick = (card: BoardCard) => {
    let counter = 0;

    console.log('clicked ' + card.name + ' ishowing: ' + card.isShowing, ' flipped: ' + card.flippedCard);

    if (lastClickedCard === card.name) {
        cards.value.map((card: BoardCard) => {
            if (card.name === lastClickedCard) {
                card.flippedCard = true;
            }
        });

        lastClickedCard = '';

    } else if (lastClickedCard === '') {
        card.isShowing = true;
        lastClickedCard = card.name;
    } else {
        card.isShowing = true;

        setTimeout(() => {
            cards.value.forEach(card => {
                card.isShowing = false;

                lastClickedCard = '';
                state.update++;
            })
        }, 3000);

    }
    state.update++;

    cards.value.map((card: BoardCard) => {
        if (card.flippedCard) {
            counter++;
        }
    });

    if (cards.value.length === counter) {
        state.victory = true;
    }
}

</script>

<template>
    <main>
        <div v-for="card in cards" :key="state.update">
            <GameCard @click="handleClick(card)" :flipped="card.flippedCard" :isShowing="card.isShowing" :name="card.name"
                :img="card.img" />
        </div>
        <div class="victory" v-show="state.victory">YAY! You won :)</div>
    </main>
</template>

<style scoped>
main {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(2, 1fr);
    gap: 10px;
}

.victory {
    font-size: 5rem;
    text-align: center;
}
</style>
