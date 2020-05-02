<template>
    <div class="report">
        <div class="my-3" v-for="(a,index) in studentAnswers" :key="a.id">
            <div class="mt-5 row justify-content-center">
                <div class="col-2 font-weight-bold">Question {{index+1}}</div>
                <div class="col-10"></div>
            </div>
            <div class="my-3 row justify-content-center">
                <div class="col">You answered <strong>{{a}}</strong></div>
            </div>
            <div class="row justify-content-center my-2">
                <div v-if="answers[index].trim() === a.trim()">Correct!</div>
<!--                <div v-if="answers[index] != a" class="col">The correct answer is <strong>{{answers[index]}}</strong></div>-->
                <div v-if="answers[index].trim() !== a.trim()" class="col">The correct answer should be {{answers[index]}}</div>
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
            feedback: String,

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

</style>