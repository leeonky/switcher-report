<template>
    <div id="app">
        <main-report msg="电路"/>
    </div>
</template>

<script>
    import MainReport from './components/MainReport.vue'

    export default {
        name: 'App',
        components: {
            MainReport: MainReport
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
