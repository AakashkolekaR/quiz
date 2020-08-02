<template>
  <div id="app">
    <Header 
      v-bind:numCorrect="numCorrect"
      :numTotal="numTotal"
    
    />
    <br>
    <br>
    <br>
    <br>
    <br>
<!-- Wrapped the question box in the container becuse it did not want the question div to flow full screen-->
  <b-container class="bv-example-row">
    <b-row>
      <b-col sm="6" offset="3"> <!-- takes 6 out of 12 coulumns and centres it, offsets 3cols from the left and right respectively-->
        <QuestionBox 
          v-if="questions.length"
          :currentQuestion = "questions[index]"  
          :next="next"
          :increment="increment"
        /> 
         <!-- v-bind lets me the question or data from the data function into the questionbox vue and everytime the index will increment whenever pressed next  matlab 0th entry diya ab view mai .daalke access karneka -->
        <!-- Added the v-if because I was getting my questions as undefined because of a delay from the server. I was mounting the questions before coming fro the server, to prevent that I added v-if -->
       
      </b-col>
    </b-row>
</b-container>


  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() { //we have to return the data in order to use it in the following methods
    return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0
    }
  },
  methods:{
    next(){    //next() is a shorthand for next: function(){} having key as next and the value as a function
      this.index++
    },
    increment(isCorrect){
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function(){  //mounted gets called by the vue just like created and destroyed
    fetch('https://opentdb.com/api.php?amount=10&category=21&type=multiple',{
      method: 'get'
    })
    .then((response) => {
      return (response.json())
    })
      .then((jsonData) => {
        // this is where I will access the data from the application, jsonData is the json coming from the api

        this.questions = jsonData.results

      })
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



<!-- Here I register all my components and add them in the main template -->

