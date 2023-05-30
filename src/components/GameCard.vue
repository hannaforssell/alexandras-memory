<script lang="ts">
import { reactive, defineComponent } from 'vue';

export default defineComponent({
    name: 'GameCard',
    props: {
        name: String,
        img: String,
        flipped: Boolean,
        isShowing: Boolean
    },
    setup(props) {
        const cardName = props.name;
        const cardImg = props.img;

        const state = reactive({
            isFlipped: props.flipped,
            isShowing: props.isShowing
        });

        return {
            cardName,
            cardImg,
            state
        }
    }
})

</script>

<template>
    <div :class="{ cardFront: !state.isShowing }">
        <img v-if="state.isShowing || state.isFlipped" :src="cardImg" />
    </div>
</template>

<style scoped>
div {
    width: 200px;
    height: 200px;
}

.cardFront {
    background-color: cornflowerblue;
}
</style>
