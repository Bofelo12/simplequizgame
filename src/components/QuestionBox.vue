<template>
  <div class="question-box-container">
    <b-jumbotron>

    <template v-slot:lead>
      {{question.question}}
    </template>

    <hr class="my-4">

    <b-list-group> 
        <b-list-group-item
            v-for="(answer, index) in answers" 
            :key="index" 
            @click.prevent="selectAnswer(index)"
            :class="checkAnswer(index)">
        
                {{ answer }}
        </b-list-group-item>
    </b-list-group>

    <b-button variant="primary" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
    >Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
   
  </div>
</template>

<script>
import _ from 'lodash'
    export default{
        props: {
            question: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        computed: {
            answers(){
                let answers = [...this.question.incorrect_answers]
                answers.push(this.question.correct_answer)  
                return answers
            }
        }, 
        watch: {
            question: {
            
             immediate: true,
             handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffledAnswers()
             }
            }

        },
        methods: {
            selectAnswer(index){
                this.selectedIndex = index
            },
            submitAnswer(){
                let isCorrect = false
                if(this.selectedIndex === this.correctIndex){
                    isCorrect = true
                }
                this.answered = true
                this.increment(isCorrect)
            },
            shuffleAnswers(){
                let answers = [...this.question.incorrect_answers, this.question.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.questtion.correct_answer)
            },
            checkAnswer(index){
                let checkAnswer = ''
                if(!this.answered && this.selectedIndex === index ){
                    checkAnswer = 'selected'
                }else if(this.answered && this.correctIndex === index){
                    checkAnswer = 'correct'
                }
                else if(this.answered && this.selectedIndex === index &&  this.correctIndex !== index ){
                    checkAnswer = 'incorrect'
                }
               return checkAnswer
            }
        },

    }
</script>


<style scoped>
    .list-group{
        margin-bottom: 15px;
    }
    .list-group-item:hover {
        background: grey;
        cursor: pointer;
    }
    .btn {
        margin: 0 5px;
    }
    .selected {
        background-color: lightblue;
    }
    .correct {
        background-color: lightgreen ;
    }
    .incorrect {
        background-color: red;
    }
</style>