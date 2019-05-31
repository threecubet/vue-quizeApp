<template>
  <div id="app">
    <Header 
    :numcorrect = "numcorrect"
    :numtotal = "numtotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
        <Quizebox 
        v-if="questions.length"
          :currentquestion = "questions[index]"
          :next = "next"
          :increment ="increment"
        />
        </b-col>
      </b-row>
    </b-container>
    
  </div>
</template>

<script>
import Header from './components/header.vue'
import Quizebox from './components/quizebox.vue'

export default {
  name: 'app',
  components: {
    Header,
    Quizebox
  },
  data(){
    return {
      questions : [],
      index : 0,
      numcorrect : 0,
      numtotal: 0
    }
  },

  methods: {
    next(){
      this.index++
    },
    increment(iscorrect){
      if(iscorrect){
        this.numcorrect++
      }
      this.numtotal++
    }
  },

  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=21&type=multiple',
    {
      method: 
      'get'
    }).then((response)=>{
      return response.json();
    }).then((jsonresponse)=>{
      this.questions = jsonresponse.results;
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
