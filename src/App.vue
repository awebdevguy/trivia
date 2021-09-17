<template>
  <div id="app">
    <Header />

    <b-container class="bv-example-row">
        <b-row>
            <b-col sm="6" offset="3">
                <QuestionBox 
                    v-if="questions.length"
                    :currentQuestion="questions[index]"
                    :next="next"
                />
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
    data() {
        return {
            questions: [],
            index: 0
        }
    },
    methods: {
        next() {
            this.index++
        }
    },
    // promises
//   mounted: function() {
//         fetch('https://opentdb.com/api.php?amount=10', {
//             method: 'get'
//         })
//         .then((response) => {
//             return response.json()
//         })
//         .then((json) => {
//             this.questions = json.results
//         })
//     },
    // async await
    mounted: async function() {
        try {
            const response = await fetch('https://opentdb.com/api.php?amount=10', { method: 'get'})
            const json = await response.json()
            console.log(json.results);
            this.questions = json.results
        }
        catch(err) {
            console.log(err);
        }
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
