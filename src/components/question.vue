<template>
 <div class="question-box-container">
  <b-jumbotron>
   

    <template slot="lead">
      {{curQ.question}}
    </template>

    <hr class="my-4">
<b-list-group>
  <b-list-group-item  
  v-for="(answer,i) in shuffledAnswers" :key="i" 
  @click.prevent="selectAns(i)"
  :class="answerClass(i)"
  >
      {{answer}}
  </b-list-group-item>
</b-list-group>
   
    <b-button variant="primary" 
    @click="submitAnswer"
    :disabled="selectedIndex===null || answered"
    >Submit</b-button>
    <b-button @click="next" variant="success" :disabled="lastQ" >Next</b-button>
  </b-jumbotron>
</div>
</template>
<script>
import _ from 'lodash';
export default {
    props: {
        curQ: Object, lastQ:Boolean, next: Function, increment: Function
    },
    data(){
        return {
            selectedIndex: null, correctIndex: null,
            shuffledAnswers: [], answered: false
        }
    },
    computed: {
        answers(){
            let answers=[...this.curQ.incorrect_answers];
            answers.push(this.curQ.correct_answer);

            return answers;        
            }
    },
    watch: {
        /*  curQ() {
            this.selectedIndex= null;
            this.shuffleAnswers();
        }  */
         curQ: {
            immediate: true,
            handler() {
            this.selectedIndex= null;
            this.answered =false;
            this.shuffleAnswers(); 
            }
        } 
    },
    methods: {
        selectAns(index) {
            this.selectedIndex= index;
            console.log(index);
        },
        submitAnswer(){
            console.log('submit click');
            this.answered =true;
            let isCorrect=false;
            if(this.selectedIndex == this.correctIndex) {
                isCorrect=true;
                console.log('submit cor');
            }
            this.increment(isCorrect);
        },
        shuffleAnswers() {
            let answers=[...this.curQ.incorrect_answers,this.curQ.correct_answer];           
            this.shuffledAnswers = _.shuffle(answers);    
            this.correctIndex =this.shuffledAnswers.indexOf(this.curQ.correct_answer);       
        },
        answerClass(i){
            let answerClass ='';
            if(!this.answered && this.selectedIndex===i){
                answerClass='selected' ;
            } else if(this.answered){
                if(i===this.correctIndex){
answerClass='correct' ;
                } else if(this.selectedIndex===i){
                    answerClass='incorrect' ;
                } else {
                     answerClass='' ;
                }
            }
            return answerClass;
        }
    },
    mounted(){
        // this.shuffleAnswers();
        console.log(this.curQ);
    }
}
</script>
<style scoped>
.list-group{
    margin-bottom: 15px;
}
.list-group-item:hover{
   background: #eee; cursor: pointer;
}
.btn {margin: 0 10px;}
.selected {background-color: lightblue}
.correct {background-color: green}
.incorrect {background-color: red}
</style>
