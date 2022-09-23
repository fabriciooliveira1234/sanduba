<template>
    <div id="main-bann">
        
    </div>
</template>

<script>
    export default {
        name: "Banner"
    }
</script>

<style scoped>
    #main-bann{
        background-image: url('/public/img/banner.jpg');
        background-position: 0 -250px;
        background-size: cover;
        height: 450px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-bann h1 {
        color: azure;
        text-align: center;
        font-size: 60px;
        padding: 20px 40px;
    }
</style>