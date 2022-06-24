<template>
  <div>

      <template v-if="answerIndex >= 10">
        <h1>Parabens,Quiz Terminado.</h1>
        <h1>Fique com seu Placar.</h1>
        <scoreBoard :player="player" :computer="computer" />
      </template>

      <template v-if="this.question">

        <scoreBoard :player="player" :computer="computer" />

        <h1 v-html="question">
        </h1>

        <template v-for="answer in this.answers" :key="answer">
          <input
          :disabled="this.answerSubmited" 
          type="radio" 
          name="options" 
          :value="answer"
          v-model="this.chosenAnswer">
          <label v-html="answer"></label><br>
        </template>


        <button v-if="!this.answerSubmited" @click="this.submitAnswer()" class="send" type="button">Send</button>

          <section v-if="this.answerSubmited" class="result">

            <h4 v-if="this.chosenAnswer == this.correctAnswer"
            v-html="'&#9989; Parabéns, a resposta ' + this.correctAnswer + ' está correta..'"></h4>

            <h4 v-else
            v-html="'&#10060; Que pena, a resposta está errada. A resposta correta é  ' + this.correctAnswer"></h4>

            <button @click="this.getNewQuestion()" class="send" type="button">Próxima pergunta</button>
          </section>

      </template>
    
  </div>
</template>


<script>

//https://opentdb.com/api.php?amount=10&category=31&difficulty=medium

import scoreBoard from "./components/scoreBoard.vue";

export default {

  name: 'App',

  components:{
    scoreBoard
  },

  data(){
    return{
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer:undefined,
      answerSubmited:false,
      player:0,
      computer:0,
      answerIndex:0

    }
  },

  methods:{
    submitAnswer(){
      if(!this.chosenAnswer){
        alert('Select one Option')
      }else{
        if(this.chosenAnswer == this.correctAnswer){
          this.answerSubmited = true
          this.player ++ 
          console.log("Resposta Correta!!!")
        }else{
          this.answerSubmited = true
           this.computer ++
          console.log('Erraste Menino')
        }
      }
    },

    getNewQuestion(){

      this.answerSubmited = false
      this.chosenAnswer = undefined
      this.question = undefined
      this.answerIndex ++

      this.axios.get("https://opentdb.com/api.php?amount=10&category=31&difficulty=medium")
    .then(res => {
      this.question = res.data.results[this.answerIndex].question
      this.incorrectAnswers = res.data.results[this.answerIndex].incorrect_answers
      this.correctAnswer = res.data.results[this.answerIndex].correct_answer
    })
    }
  },

  computed:{
    answers(){
      let answers = structuredClone(this.incorrectAnswers);
      answers.splice(Math.round(Math.random() * answers.length) ,0, this.correctAnswer)
      return answers
    }
  },
  created(){
    this.getNewQuestion();
  }
}
</script>

<style lang="scss">
#app {
  font-family: Roboto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio]{
    margin: 12px 4px;
  }

  .send{
    margin-top: 12px;
    height:40px ;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }

  .score{
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;
  }

  .placarNumb{
    font-size: 20px;
    border:solid 1px black;
    padding: 10px;
  }
}
</style>
