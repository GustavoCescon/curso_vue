<template>
    <div class="MessageContainer">
        <p>{{ msg }}</p>
    </div>
</template>

<script>
    export default {
        name: "Message",
        props:{
            msg: String
        }
    }
</script>

<style scoped>
    .MessageContainer{
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0 auto;
        margin-bottom: 20px;
        color: #004085;
        background-color: #CCE5FF;
        border: 2px solid #88DAFF;
        border-radius: 5px;
        padding: 10px;
        max-width: 400px;
        width: 30px auto;
    }
</style>
