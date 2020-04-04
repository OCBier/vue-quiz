<template>
  <div id="app">
    <HeaderThing :numCorrect = "numCorrect" :numTotal = "totalAnswered" />

    <b-container class="bv-example-row">
        <b-row>
            <b-col sm="6" offset="3">
              <!-- Don't try access questions array and render this component until questions are available --> 
              <QuestionBox v-if = 'questions.length > 0' 
                :curQuestion = 'questions[index]' 
                :next = 'next'
                :increment = 'updateScore'/>
            </b-col>    <!--There are a total of 12 vertical columns -->
        </b-row>
    </b-container>
  </div>
</template>

<script>
import HeaderThing from './components/HeaderThing'
import QuestionBox from './components/QuestionBox'

export default {
  name: 'App',
  components: {
    HeaderThing,
    QuestionBox
  },
  data: function() {
    return {
      questions: [],
      index: 0,
      totalAnswered: 0,
      numCorrect: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },

    updateScore(isCorrect) {
      if (isCorrect === true) {
        this.numCorrect++;
      }
      this.totalAnswered++;
    }
  },


  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple', {method: 'get'}).then(res => {
      console.log(res.statusText);
      return res.json();
    }).then(theJSON => {
      console.log(theJSON.results);
      this.questions = theJSON.results;
    });
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
