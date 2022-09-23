<template>
    <div class="mensagem-container">
        <p>{{msg}}</p>
    </div>
</template>

<script>
    export default{
        name: "Mensagem",
        props: {
            msg: String
        }
    }
</script>

<style scoped>
    .mensagem-container{
        color:blue;
        background-color: aqua;
        border: 2px solid aquamarine;
        border-radius: 5px;
        padding: 10px;
        margin: 30px auto;
        max-width: 400px;
    }
</style>