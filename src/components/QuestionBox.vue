<template>
    <div class="question-box-jumbotron">
        <b-jumbotron  lead="Bootstrap v4 Components for Vue.js 2">
           
            <template v-slot:lead>
                <span v-html = 'curQuestion.question'> </span>  <!-- Treat the question text as html, since it is html-encoded -->
            </template>

            <hr class="my-4">

            <b-list-group>
                <!-- Use the index i in the loop as the key that will be injected into DOM for each element -->
                <!-- When elem is clicked, this key i will be passed to chooseAnswer -->
                <b-list-group-item 
                    v-for="(answer, i) in answers" :key="i" 
                    @click="chooseAnswer(i)"
                    :class = "getAnswerClass(i)">
                    <span v-html="`${i+1}. ${answer}`"></span>

                </b-list-group-item>
            </b-list-group>
           

            <b-button variant="primary" @click="submitAnswer"
              :disabled = "curSelected < 0 || answered">
                Submit
             </b-button>

            <b-button variant="success" 
            @click="next">
                Next
            </b-button>

        </b-jumbotron>
    </div>
</template>



<script>

import * as _ from 'lodash';

export default {
    props: {
        curQuestion: Object,
        next: Function,
        increment: Function
    },

    data: function() {
        return {
            answers: [],
            curSelected: -1,
            correctIndex: -1,
            answered : false
        };
    },
    
    watch: {
        /*Run the watch handler whenever value of curQuestion changes, and immediately as well */
        curQuestion: {
            immediate: true,
            handler: function(/*oldQuestion, newQuestion*/) {
                         console.log('cur question changed!');
                        this.curSelected = -1;
                        this.answered = false;
                        this.correctIndex = -1;
                        this.shuffleAnswers();
            }
        }
    },

    methods: {
        chooseAnswer(selected) {
            this.curSelected = selected;
        },

        shuffleAnswers() {
            let answers = [...this.curQuestion.incorrect_answers, this.curQuestion.correct_answer];
            this.answers = _.shuffle(answers);            //Use lodash to shuffle array
            // eslint-disable-next-line vue/no-side-effects-in-computed-properties
            this.correctIndex = answers.indexOf(this.curQuestion.correct_answer);
            this.answers = answers;
        },

        submitAnswer() {
            this.answered = true;
            this.increment(this.curSelected === this.correctIndex);            
        },

        getAnswerClass(index) {
        
            if (index === null || index < 0)
                return '';
            
            if (this.answered === false && this.curSelected === index) 
                return 'selected';
            
                        
            if (this.answered === true) {
                if (this.curSelected === index) {
                    /*Correct answer selected */
                    if(this.curSelected === this.correctIndex ) 
                        return 'correct';
                    
                    /*Wrong answer selected */
                    return 'incorrect';
                }
                else if (this.curSelected !== this.correctIndex && this.correctIndex == index)
                    return 'correct';
            }
        }
    }

}
    

</script>



<style scoped>
    
    .list-group {
        margin-bottom: 2rem;
    }

    .list-group-item {
        margin-bottom: 20px;
    }

    .list-group-item:hover {
        cursor: crosshair;
        background-color: darkgrey;
        color: aliceblue
    }

    .list-group-item.selected {
        background-color:darkcyan
    }

    .list-group-item.incorrect {
        background-color: red;
    }

    .list-group-item.correct {
         background-color: green;
    }
    



</style>