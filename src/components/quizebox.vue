<template>
    <div>
        <b-jumbotron>
            <template slot="lead">
            {{ currentquestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in shuffledanswers" :key="index"
                v-on:click="selectanswer(index)"
                :class="[
                        !answered && selectedindex === index ? 'selected' : 
                        answered && correctindex === index ? 'correct' :
                        answered && selectedindex === index && correctindex !== index ? 'incorrect' : 
                        ''
                 ]">
                {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary"
            @click="submitanswers"
            :disabled="selectedindex === null || answered">
            Submit
            </b-button>
            
            <b-button variant="success" v-on:click="next" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';
export default {
    
    props:{
        currentquestion: Object,
        next : Function,
        increment : Function
    },
    data(){
        return {
            selectedindex : null,
            shuffledanswers: [],
            correctindex: null,
            answered: false
        }
    },
    watch:{
        currentquestion:{
            immediate: true, //Making shuffle answer work at the begenning itself
            handler() {
            this.selectedindex = null
            this.answered = false
            this.shuffleanswer()
            }
        }
    },
    computed:{
        answers(){
            let answers = [...this.currentquestion.incorrect_answers]
            answers.push(this.currentquestion.correct_answer)
            return answers
        }
    },
    methods: {
        selectanswer(index){
            this.selectedindex = index;
        },
        shuffleanswer(){
            let answers = [...this.currentquestion.incorrect_answers,this.currentquestion.correct_answer]
            console.log(answers);
            this.shuffledanswers = _.shuffle(answers)
            this.correctindex = this.shuffledanswers.indexOf(this.currentquestion.correct_answer)
        },
        submitanswers(){
            let iscorrect = false
            if(this.selectedindex === this.correctindex){
                iscorrect = true
            }
            this.answered =true
            this.increment(iscorrect)
        }
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom:15px;
}    
.list-group-item:hover{
    background: #EEE;
    cursor:pointer;
}    
.btn{
    margin: 0 5px;
}
.selected{
    background-color:lightblue;
}
.correct{
    background-color:lightgreen;
}
.incorrect{
    background-color:red;
}
</style>