<template>
   <div class="question-box-container">
  <b-jumbotron>

    <template v-slot:lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group >
  <b-list-group-item 
  v-for="(ans, index) in answers" :key="index" 
  @click="selectAnswer(index)" 
  :class="answerClass(index)"
  >
        {{ans}}
  </b-list-group-item>
  </b-list-group>

    <b-button class="btn" variant="primary"
    @click="submitAnswer()"
    :disabled="selectedIndex === null || answered "
    >
    Submit
    </b-button>
    <b-button class="btn" @click="currentIndex" variant="success" >Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

    export default{
      props: {
              currentQuestion : Object,
              currentIndex: Function,
              increament: Function,
              
              
      },

      data() {
            return {
                  selectedIndex : null,
                  shuffledAnswer : [], 
                  correctIndex : null,
                  answered : false
                  
            }
      },
      watch: {
          currentQuestion: {
            immediate: true,
            handler(){
                this.selectedIndex = null
                this.shuffleAnswer()
                this.answered = false
            }
          }
      },

      computed: {
        answers() {
          let answers = [...this.currentQuestion.incorrect_answers]
          answers.push(this.currentQuestion.correct_answer)
          return answers
        }
      },
      methods:{
        selectAnswer(index){
          this.selectedIndex = index
          
        },
        submitAnswer() {

            let isCorrect = false

            if(this.selectedIndex === this.correctIndex) {
              isCorrect  = true
            }
            this.answered = true
            this.increament(isCorrect)
        },

        

        shuffleAnswer() {
          let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
          this.shuffledAnswer = _.shuffle(answers)
          this.correctIndex= this.shuffledAnswer.indexOf(this.currentQuestion.correct_answer)
        },

        answerClass(index){
          let answeredClass = ''

          if(!this.answered && this.selectedIndex === index){
            answeredClass = 'selected-ans'
          }
          else if(this.answered && this.correctIndex === index){
            answeredClass = 'correct-ans'
          }
          else if(this.answered && this.selectedIndex === index && this.correctIndex !== index ){
            answeredClass = 'incorrect-ans'
          }

          return answeredClass 
        }
      }
    }
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
}
.btn {
  margin: 0px 5px;
}
.list-group-item:hover{
  background-color: lightgrey;
  cursor: pointer;
}

.selected-ans{
  background-color: lightblue;
}
.correct-ans{
  background-color: lightgreen;
}
.incorrect-ans{
  background-color: lightcoral;
}
</style>