<template>
    <label class="question">
        <div class="form-group my-4">
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
                        <input class="form-check-input" type="radio" :name="title" :id="title + a" value="optionA">
                        <label class="form-check-label" :for="title + a">
                            A. {{a}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" :id="title + b" value="optionB">
                        <label class="form-check-label" :for="title + b">
                            B. {{b}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" :id="title + c" value="optionC">
                        <label class="form-check-label" :for="title + c">
                            C. {{c}}
                        </label>
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-10">
                    <div class="form-check">
                        <input class="form-check-input" type="radio" :name="title" :id="title + d" value="optionD">
                        <label class="form-check-label" :for="title + d">
                            D. {{d}}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </label>
</template>

<script>

    export default {
        name: "question",
        props:{

            text: String,
            a: String,
            b: String,
            c: String,
            d: String,
            feedback: String,
        },
        data: function () {
            return {
                isTesting: true,
                titleNum: 1,
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