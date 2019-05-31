<template>

<div class="question-box-container">
  <b-jumbotron >
    <template slot="lead">
       {{currentQuestion.question}}
    </template>

    <hr class="my-4" />

    <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" 
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="[ !answered && selectedIndex === index ? 'selected' : 
        answered && selectedIndex === index ? 'correct' : 
        answered && selectedIndex === index && selectedIndex !== index ? 'incorrect' : ''
        ]"
        >
        {{answer}}
        </b-list-group-item>
    </b-list-group>

    <b-button 
    variant="primary" 
    @click="submitAnswer"
    :disabled="selectedIndex === null || answered"
    >submit</b-button>
    <b-button @click="next" variant="success" href="#">next</b-button>
  </b-jumbotron>
</div>
    
</template>

<script>
export default {
    props:{
        currentQuestion: Object,
        next: Function,
        increment : Function
    },
    data: function(){
        return{
            selectedIndex:null,
            shuffledAnswers:[],
            correctIndex: null,
            answered: false            
        } 
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer) 
            return answers
        }
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex =null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },

    methods:{
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
            let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex =  this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }
    }

}
</script>

<style scoped>
.list-group{ 
    margin-bottom:15px;
}
.btn{
    margin:0 5px;
}
.list-group-item:hover{
    background:#EEE;
    cursor: pointer;
}
.selected{
    background-color : lightblue;
}
.correct{
    background-color: lightgreen;
}
.incorrect{
    background-color: red;
}
</style>

