<template>
    <div class="question">
        <div id="question" class="form-group my-4">
            <div class="row justify-content-center my-1">
                <div class="col-10">
                    <strong>{{title}}</strong>
                </div>
            </div>
            <div class="row justify-content-center my-1 mb-3">
                <div class=" col-10">
                    {{text}}
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" id="checkA" value="A">
                        <label class="form-check-label" :for="title + a">
                            A. {{a}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" id="checkB" value="B">
                        <label class="form-check-label" :for="title + b">
                            B. {{b}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" id="checkC" value="C">
                        <label class="form-check-label" :for="title + c">
                            C. {{c}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" id="checkD" value="D">
                        <label class="form-check-label" :for="title + d">
                            D. {{d}}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: "question",
        props:{
            titleNum: Number,
            text: String,
            a: String,
            b: String,
            c: String,
            d: String,
            feedback: Array,
        },
        data: function () {
            return {
                isTesting: true,
            }
        },
        computed:{
            title: function(){
                return("Question "+ this.titleNum);
            },
        },
            methods: {
                submitQuiz: function () {
                    this.isTesting = false;
                }
            }
        }


</script>

<style scoped>

</style>