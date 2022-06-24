<template>
        <section class="score"> 👦Jogador(a) <span class="placarNumb">{{player}}</span> x 
        <span class="placarNumb">{{computer}}</span> Computador🤖 </section>
</template>

<script>
export default {
    props:{
        computer:{
            type: Number
        },

        player:{
            type:Number
        }
    }
}
</script>

<style>

</style>