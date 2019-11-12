<template>
    <div id="app"></div>
</template>

<script>
import { Client } from 'boardgame.io/client'
import { TicTacToe as game } from './game'

export default {
    mounted() {
        const c = new Client({ game, debug: true })
        c.start()
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
}
</style>
