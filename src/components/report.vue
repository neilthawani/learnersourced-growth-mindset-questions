<template>
    <div class="report">
        <div class="questionSet my-3 p-3" v-for="(a,index) in studentAnswers" :key="a.id">
            <div class=" row justify-content-center">
                <div class="col-2 font-weight-bold">Question {{index+1}}</div>
                <div class="col-10"></div>
            </div>
            <div class="my-1 row"><div class="col"><strong>You chose...</strong></div></div>
            <div class="my-3 row">
                <div class="col">{{a}}</div>
            </div>
            <div class="my-1 row"><div class="col"><strong>It was...</strong></div></div>
            <div class="row my-3">
                <div class="col" v-if="answers[index].trim() === a.trim()">Correct!</div>
<!--                <div v-if="answers[index] != a" class="col">The correct answer is <strong>{{answers[index]}}</strong></div>-->
                <div class="col" v-if="answers[index].trim() !== a.trim()" >Incorrect. It should be <br>{{answers[index]}}</div>

            </div>
            <div class="my-1 row"><div class="col"><strong>Feedback</strong></div></div>
            <div class="row">
                <div class="col">{{feedback[index]}}</div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "report",
        props: {
            studentAnswers: Array,
            answers: Array,
            feedback: Array,

        },
        data: function () {
            return {
                qNum: 0,
                q_id: [1, 2, 3, 4],
            }
        },
        computed: {
            q_title: function () {
                return ("Question " + this.qNum);
            }
        },
        methods:{

        }
    }
</script>

<style scoped>
    .questionSet{
        background-color: rgba(126, 176, 219, 0.50);
        border-radius: 10px;
        box-shadow: 10px 10px;

    }
</style>