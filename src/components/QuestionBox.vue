<template>
    <div class="question-box-container">
        <br>
        <b-jumbotron>
            <template #lead>
                {{ decodedQuestion }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in answers" 
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || isAnswered"
            >
                Submit
            </b-button>

            <b-button 
                @click="next" 
                variant="success"
                :disabled="selectedIndex === null || !isAnswered || numTotal === 10"
            >
                Next
            </b-button>

            <b-button
                @click="restart"
            >
                Re-Start
            </b-button>

        </b-jumbotron>
    </div>
</template>

<script>

import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        restart: Function,
        numTotal: Number
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            isAnswered: false,
            decodedQuestion: ""
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            let decoded = []
            answers.push(this.currentQuestion.correct_answer)
            answers.forEach((ans, i) => {
                decoded[i] = this.decodeHTMLChars(ans)
            });
            return decoded
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.isAnswered = false
                this.shuffleAnswers()
                this.decodedQuestion = this.decodeHTMLChars(this.currentQuestion.question)
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },

        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },

        submitAnswer() {
            // eslint-disable-next-line no-unused-vars
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.increment(isCorrect)
            this.isAnswered = true
        },

        answerClass(index) {
            
            if(!this.isAnswered && this.selectedIndex === index) {
                return 'selected'
            }

            if(this.isAnswered && this.correctIndex === index) {
                return 'correct'
            }

            if(this.isAnswered && this.selectedIndex === index && index !== this.correctIndex) {
                return 'incorrect'
            }
        },

        decodeHTMLChars(str) {
            let txt = document.createElement("textarea")
            txt.innerHTML = str
            return txt.value
        }
    }
}
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item {
        text-align: left;
    }

    .list-group-item:hover {
        background: lightgray;
        cursor: pointer;
    }

    .btn {
        margin: .5rem;
    }

    .selected {
        background: lightblue;
    }

    .correct {
        background: lightgreen;
    }

    .incorrect {
        background: red;
    }
</style>