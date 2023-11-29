<template>
    <div class="game-card">
        <div class="game-card-inner" :class="flip">
            <!-- 正面 -->
            <div class="card-front" @click="flipCard">
                <span v-if="flip" >{{ cardNumber }}</span>
            </div>

            <!-- 卡背 -->
            <div class="card-back" @click="flipCard">
                <div>{{ owner }}</div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    props: ['owner'],
    expose: ['flipCard', 'resetCard'],
    data() {
        return {
            flip: "",
            cardNumber: 0,
        }
    },
    methods: {
        flipCard(): number {
            this.flip = "flip-card";
            return this.cardNumber;
        },
        resetCard(num: number) {
            this.flip = "";
            this.cardNumber = num;
        }
    }
}
</script>

<style>
.game-card {
    background-color: transparent;
    width: 200px;
    height: 300px;
    perspective: 1000px;
    margin: 5px auto;
}

.game-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    border-radius: 15px;
    transition: transform 0.6s;
    transform-style: preserve-3d;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.flip-card {
    transform: rotateY(180deg);
}

.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 15px;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 32px;
}

.card-back {
    background-color: #A1C7E0;
}

.card-front {
    font-size: 60px;
    background-color: #eee;
    color: black;
    transform: rotateY(180deg);
}
</style>