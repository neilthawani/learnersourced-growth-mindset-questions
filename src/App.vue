<template>
    <div class="app container">
      <div class="row justify-content-center">
        <div class="col">
        <question :text="q_title" :a="q_answer[0]" :b="q_answer[1]" :c="q_answer[2]" :d="q_answer[3]" :feedback="q_feedback"></question>
        </div>
      </div>
        <div class="row justify-content-center">
            <div class="col-5 btn btn-primary" @click="nextQuestion">Next Question</div>
            <div class="offset-1 col btn btn-warning" v-if="q_n!=1">Last Question</div>
        </div>
    </div>
</template>

<script>
    import json from '../public/export_dataframe.json';
    import question from "./components/question";


    export default {
        name: 'App',
        components: {
            question
        },
        methods: {
            onValidate(results) {
                this.results = results;
            },
            nextQuestion(){
                this.q_n++;
            }
        },
        data() {
            let answers = [];
            let ids = [];
            let isCorrects = [];
            let titles = null;
            let feedback = null;
            let n_questions = 1;
            for (let key in json.Question_id) {
                console.log(key);
                console.log(`value of ${key} right now: ${json.Question_id[key]}`);
                if (json.Question_id[key] == n_questions) {
                    ids.push(key);
                    console.log("ids right now: " + ids);
                }
            }
            for (let key in json.Answer_text) {
                if (ids.includes(key))
                    answers.push(json.Answer_text[key]);
            }
            for (let key in json.is_correct) {
                if (ids.includes(key))
                    isCorrects.push(json.is_correct[key]);
            }
            for (let key in json.Question_text) {
                if (ids.includes(key))
                    titles = json.Question_text[key];
            }
            for(let key in json.feedback_text){
              if(ids.includes(key))
                feedback = json.feedback_text[key];
            }

            return {
                df: json,
                currentQ: n_questions,
                q_answer: answers,
                q_id: ids,
                q_isCorrect: isCorrects,
                q_title: titles,
                q_n: n_questions,
                q_feedback: feedback,
            };
        },
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
    @import'~bootstrap/dist/css/bootstrap.css';
</style>
