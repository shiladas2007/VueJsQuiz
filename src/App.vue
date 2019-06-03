<template>
  <div id="app">
   
   <Header :numCorrect="numCorrect" :numTotal="numTotal" :TotalQ="TotalQ"/>
   <b-container class="bv-example-row">
  <b-row sm="6" offset="3">
    <b-col><question v-if="questions.length" :curQ="questions[index]"
    :next="next" :increment="increment" :lastQ="index+1===TotalQ"
    >
      </question></b-col>
    
  </b-row>
</b-container>
    

    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import question from './components/question.vue'
import Header from './components/Header.vue'
export default {
  name: 'app',
  components: {
    HelloWorld,  question, Header
  },
  data(){
return {
  questions: [],
  index:0, numCorrect:0, numTotal:0, TotalQ:4
}
  },
  methods:{
next(){
  if(this.index<this.TotalQ)
    this.index++;
},
increment(isCorrect){
  console.log('inc click');
  if(isCorrect) {
    this.numCorrect++;
  } 
  this.numTotal++;
}
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=4&category=18&difficulty=easy&type=multiple',{
      method: 'get'
    }).then((response) =>{
      return response.json()
    }).then(jsonData=>{
      this.questions= jsonData.results;
    })
  },

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
