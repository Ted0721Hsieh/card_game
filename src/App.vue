<template>
    <div id="home">
        <!-- 遊戲區塊 -->
        <BContainer>
            <BRow>
                <!-- 你的牌 -->
                <BCol md="4" sm="12">
                    <game-card ref="yourCard" owner="你的牌"></game-card>
                </BCol>

                <!-- 操作區塊 -->
                <BCol md="4" sm="12" alignSelf="center">
                    <!-- 輸贏訊息 -->
                    <div v-show="!startGame">
                        <div id="result-msg">{{ resultMsg }}</div>
                        <BButton class="" @click="playGame" v-show="!start">再來一局</BButton>
                    </div>
                    <!-- 選擇大小 -->
                    <div v-show="startGame">
                        <div>猜你的牌大還小</div>
                        <BButton class="" @click="guess(HIGH)" v-show="!start">猜大</BButton>
                        <BButton class="" @click="guess(LOW)" v-show="!start">猜小</BButton>
                    </div>
                </BCol>

                <!-- 對手的牌 -->
                <BCol md="4" sm="12">
                    <game-card ref="computerCard" owner="對手的牌"></game-card>
                </BCol>
            </BRow>
        </BContainer>
    </div>
</template>

<script lang="ts">
import GameCard from './components/Card.vue'

const HIGH = "High";
const LOW = "Low";
const cards = [1, 2, 3, 4, 5, 6, 7, 8, 9];

export default {
    components: {
        GameCard
    },
    data() {
        return {
            start: false,
            HIGH,
            LOW,
            startGame: true,
            resultMsg: "",
        }
    },
    methods: {
        guess(chose: string) {
            //取得卡片數字
            var yourNumber = this.flipCard("yourCard");
            var computerNumber = this.flipCard("computerCard");

            //猜大小結果
            var result = yourNumber > computerNumber ? HIGH : LOW;
            this.resultMsg = result === chose ? "猜對了" : "猜錯了";

            //遊戲結束
            this.startGame = false;
        },
        playGame() {
            var tmpCards = Array.from(cards);
            //重設你的卡片
            var index = Math.floor(Math.random() * tmpCards.length);
            var num = tmpCards.splice(index, 1)[0];
            this.resetCard("yourCard", num);
            //重設對手卡片
            index = Math.floor(Math.random() * tmpCards.length);
            num = tmpCards.splice(index, 1)[0];
            this.resetCard("computerCard", num);
            //開始遊戲
            this.startGame = true;
        },
        resetCard(cardId: string, num: number) {
            //重設該卡片子元件
            (this.$refs[cardId] as typeof GameCard).resetCard(num);
        },
        flipCard(cardId: string): number {
            //翻轉該卡片子元件，並取得數字
            var num = (this.$refs[cardId] as typeof GameCard).flipCard();
            return num;
        }
    },
    mounted() {
        //初始化遊戲
        this.playGame();
    }
}


</script>

<style scoped>
#home {
    position: absolute;
    top: 0px;
    right: 0px;
    left: 0px;
    bottom: 0px;
    padding-top: 10px;
    background-color: bisque;
    overflow: auto;
}

#result-msg {
    font-size: 28px;
    color: brown;
}
</style>
