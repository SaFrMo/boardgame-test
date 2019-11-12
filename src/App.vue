<template>
    <div id="app">
        <div class="board" v-if="state">
            <button
                v-for="(cell, i) in gameState.cells"
                :key="i"
                @click="clickCell(i)"
            >
                {{ cell ? (cell == 0 ? 'X' : 'O') : '' }}
            </button>
        </div>

        <h3 class="winner" v-if="state && state.ctx && state.ctx.gameover">
            Game over!
            <span key="win" v-if="state.ctx.gameover.winner">
                Winner: {{ state.ctx.gameover.winner == 0 ? 'X' : '0' }}</span
            >
            <span key="draw" v-else>Draw!</span>
            <br />
            <button @click="client.reset">Restart</button>
        </h3>
    </div>
</template>

<script>
import { Client } from 'boardgame.io/client'
import { TicTacToe } from './game'

export default {
    data() {
        return {
            client: null
        }
    },
    mounted() {
        this.client = new Client({ game: TicTacToe, debug: true })
        this.client.start()
    },
    computed: {
        state() {
            if (!this.client) return null

            // TODO: figure out how to react to this better - subscriber on client.store?
            console.log(this.client.log)

            return this.client.store.getState()
        },
        gameState() {
            return this.state ? this.state.G : null
        }
    },
    methods: {
        clickCell(index) {
            if (this.gameState.cells[index] === null) {
                this.client.moves.clickCell(index)
                this.client.events.endTurn()
            }
        }
    }
}
</script>

<style lang="scss">
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    margin-right: 280px;

    .board {
        display: grid;
        grid-template-columns: repeat(3, 100px);
        grid-template-rows: repeat(3, 100px);
        justify-content: center;
    }
}
</style>
