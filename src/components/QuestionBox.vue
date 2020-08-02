/*global currentQuestion */

<template>
    <div class="question-box-container">
    <b-jumbotron>
        <template v-slot:header>MyQuizApp</template>

        <template v-slot:lead> <!-- json object ke 0th element ka attribute reference kiya -->
            {{ currentQuestion.question }}
        </template>

        <hr class="my-4">

        <b-list-group>
            <!-- It references the answers() function--> <!-- Every list-group-item will have a unique key which is index in the v-bind.   (answer,index) was written becuase OI wanted to show index isn't unique--> <!-- we will use v-bind to change the background color so as to select a correct class, if not selected do not apply any class-->
            <b-list-group-itemn 
            v-for="(answer,index) in answers" v-bind:key="index" 
            @click="selectAnswer(index)" 
            v-bind:class="answerClass(index)"
            >
                
                {{ answer }}
            </b-list-group-itemn> <!-- to select the class for the styling it will always call the answerClass method -->
            
    </b-list-group>

        <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered"> <!-- the submit will be disabled if any option is not selected and it has already been answered-->
            Submit
            </b-button>
        <b-button @click="next" variant="success" href="#">
            Next
        </b-button>
    </b-jumbotron>
</div>
</template>

<script> //to recieve variables from parent appvue to the child, we have to recieve it in the props and specify its type, only then we can use it in the chils component
//watch function will be added, it is same as a computed method i.e it activates when a change occurs (Here it is used to shuffle the answers) I can watch for changes in my props, computed also watches for changes, when currentQuestion changes in the props it will call the function inside watch used to shuffle answers
import _ from 'lodash'  //lodash is used for shuffling writing _ instead of lodash is a convention
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function 
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctIndex: null
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers] //here by writing ... I made a copy of the incorrect_answers array instead of directly referencing it. since it is in a props I had to use this.currentQuestion
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: {
      currentQuestion: { //we chnaged the currentQuestion to an object so that we could set immediate to true so as to shuffle the first question as well
          immediate: true,
          handler(){
            this.selectedIndex = null
            this.shuffleAnswers()
            this.answered=false
          }
      }
    },
    methods:{
        selectAnswer(index) {
            this.selectedIndex = index
        },
        submitAnswer(){
            let isCorrect = false
            if (this.selectedIndex === this.correctIndex){
                isCorrect=true
            }
            this.answered = true
            // passed the boolean so that through propos I could tell my app.cue and then use it as my counter in the questionbox, I passed this prop in my props after creation
            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index){
            let answerClass= ''

            if(!this.answered && this.selectedIndex === index){
                answerClass='selected'
            }else if(this.answered && this.correctIndex === index ){
                answerClass='correct'
            }else if(this.answered && this.selectedIndex === index && this.correctdIndex !== index){
                answerClass='incorrect'
            }

            return answerClass
        }
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}
.list-group-item{
    margin-bottom: 5px;
    background-color: #eee;
    cursor: pointer;
}
.btn{
    margin: 0 5px;
}

.selected{
    background-color: lightblue;
}

.correct{
    background-color: lightgreen;
}

.incorrect{
    background-color: red;
}
</style>