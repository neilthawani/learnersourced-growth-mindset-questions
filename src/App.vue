<template>
    <div class="app container">
        <div id="quiz_question" v-if="!isQuizSubmitted" class="row justify-content-center">
            <div class="col">
                <question v-if="q_answers.length" :title-num="q_n" :text="q_title" :a="q_answers[q_n-1][0]" :b="q_answers[q_n-1][1]" :c="q_answers[q_n-1][2]"
                          :d="q_answers[q_n-1][3]"
                          :feedback="q_feedback"></question>
            </div>
        </div>
        <div id="score_report" v-if="isQuizSubmitted" class="row justify-content-center">
            <div class="col">
                <report :student-answers="studentAnswerText" :answers="correct_answer" :feedback="q_feedback"></report>
            </div>
        </div>
        <div class="row mb-3 justify-content-center">
            <div class="col-3  btn btn-warning" v-if="q_n!=1 && !isQuizSubmitted" @click="lastQuestion">Last Question
            </div>
            <div class="offset-1 col-3 btn btn-primary" v-if="q_n<maxQ && !isQuizSubmitted" @click="nextQuestion">
                Next Question
            </div>
            <div class="offset-1 col-3 btn btn-danger" v-if="q_n==maxQ && !isQuizSubmitted" @click="submitQuiz">Submit
            </div>
            <div class="col-3 btn btn-info" v-if="isQuizSubmitted" @click="retakeQuiz">Retake Quiz</div>
        </div>
    </div>
</template>

<script>
    import json from '../public/export_dataframe.json';
    import question from "./components/question";
    import report from "./components/report";
    import $ from 'jquery'


    export default {
        name: 'App',
        components: {
            question,
            report
        },
        mounted(){
            this.assignAnswers();
        },
        updated(){
            console.log("updated()");
          this.assignAnswers();
        },
        methods: {
            onValidate(results) {
                this.results = results;
            },
            nextQuestion() {
                if (this.q_n < this.maxQ) {
                    this.saveAnswer();
                    this.assignAnswers();
                    this.q_n++;
                    $("#check"+ this.studentAnswers[this.q_n - 1]).prop("checked", true);
                }


            },
            lastQuestion() {
                if (this.q_n > 1) {
                    this.saveAnswer();
                    this.q_n--;
                    $("#check"+ this.studentAnswers[this.q_n - 1]).prop("checked", true);
                }
            },
            saveAnswer() {
                // save student answers
                let ans = $('input:checked').val();
                let ansText = $('input:checked+label').html();
                // console.log(ansText);
                this.studentAnswers[this.q_n - 1] = ans;
                this.studentAnswerText[this.q_n - 1] = ansText;
                console.log("student Answer for question number " + this.q_n + ": " + this.studentAnswerText[this.q_n - 1]);
                $("input").prop("checked", false);

            },
            submitQuiz() {
                //submit quiz with data in studentAnswers
                this.saveAnswer();
                this.isQuizSubmitted = true;

            },
            retakeQuiz(){
                this.q_n = 1;
                this.isQuizSubmitted = false;
                // look at the answer array and randomize it
            },
            assignAnswers(){
                if(this.q_answers[this.q_n-1] == null){
                    this.q_answers.push(this.q_answer);
                }
                // console.log(this.q_answers);
            }
        },
        data() {
            let n_questions = 1;
            var arr = Object.keys(json.Question_id).map(function (key) {
                return json.Question_id[key];
            });
            var maxQ = Math.max.apply(null, arr);
            // console.log(maxQ);

            return {
                feedback:[],
                df: json,
                maxQ: maxQ,
                q_n: n_questions,
                nextBtnText: "Next Question",
                q_answers: [],
                studentAnswers: [],
                studentAnswerText: [],
                isQuizSubmitted: false,
            };
        },
        computed: {
            q_id: function () {
                let ids = [];
                for (let key in json.Question_id) {
                    if (json.Question_id[key] == this.q_n) {
                        ids.push(key);
                    }
                }
                return ids;
            },
            q_answer: function () {
                function shuffle(array) {
                    for (let i = array.length - 1; i > 0; i--) {
                        let j = Math.floor(Math.random() * (i + 1)); // random index from 0 to i

                        // swap elements array[i] and array[j]
                        // using "destructuring assignment" syntax to achieve that
                        // same can be written as:
                        // let t = array[i]; array[i] = array[j]; array[j] = t
                        [array[i], array[j]] = [array[j], array[i]];
                    }
                }

                let answers = [];
                for (let key in json.Answer_text) {
                    if (this.q_id.includes(key))
                        answers.push(json.Answer_text[key]);
                }
                shuffle(answers);
                function isCorrectAnsIn (ans) {
                    let correct;
                    for(let key in json.is_correct){
                        if(json.is_correct[key] == 1){
                            if(ans.includes(json.Answer_text[key]))
                                correct = json.Answer_text[key];
                        }
                    }
                    // console.log(correct);
                    return(ans.slice(0,4).includes(correct));
                }
                let tf = isCorrectAnsIn(answers);
                // console.log(tf);
                while(!tf){
                    shuffle(answers);
                    tf= isCorrectAnsIn(answers);
                }
                return answers;
            },
            q_isCorrect: function () {
                let isCorrects = [];
                for (let key in json.is_correct) {
                    if (this.q_id.includes(key))
                        isCorrects.push(json.is_correct[key]);
                }
                return isCorrects;
            },
            q_title: function () {
                let titles = null;
                for (let key in json.Question_text) {
                    if (this.q_id.includes(key))
                        titles = json.Question_text[key];
                }
                return titles;
            },
            q_feedback: function () {
                let feedback = [];
                for (let key in json.feedback_text) {
                    // if (this.q_id.includes(key))
                        feedback.push(json.feedback_text[key]);
                }
                let fSet = new Set(feedback);
                feedback = [...fSet];

                console.log("feedback:" + feedback);
                return feedback;
            },
            correct_answer: function(){
                let answers = [];
                for(let key in json.is_correct){
                    if(json.is_correct[key] == 1){
                        answers.push(json.Answer_text[key]);
                        for(let i = 0; i<this.q_answers.length; i++){
                        if(json.Answer_text[key] == this.q_answers[i][0])
                            answers[i] = "A. " + answers[i];
                        else if(json.Answer_text[key] == this.q_answers[i][1])
                            answers[i] = "B. " + answers[i];
                        else if(json.Answer_text[key] == this.q_answers[i][2])
                            answers[i] = "C. " + answers[i];
                        else if(json.Answer_text[key] == this.q_answers[i][3])
                            answers[i] =
                                "D. " + answers[i];
                        console.log(answers[this.q_n-1]);
                        }
                    }
                }
                return answers;
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

    @import '~bootstrap/dist/css/bootstrap.css';
</style>
