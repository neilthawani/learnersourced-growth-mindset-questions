## To see the demo website

Go to https://lioninawhat.com/learnersourced-growth-mindset-questions for the learnersourced practice, feedback, and growth mindset quiz with learnersourced feedback.

Check out the data folder for the Jupyter Notebook which parsed the data to generate the questions and answers on the quiz.

## Summary

- Method used for which response to pick for "correct"/"incorrect" option

We use each students’ answers to open-ended questions as the choices of the new quiz. If the students chose the correct answers in multiple choice or select-all-that-apply questions, the corresponding explanations in the open-ended questions will be the correct choices in the quizzes we create. On the contrary, if they chose the wrong answers, the corresponding explanations will be the wrong answers.

In the given dataset, "correct" options are found by cross-referencing the correct answer (given in the Questions csv file) with students' answers (given in the Answers file). The two data sets are cross-referenced, and if a student's answer matches the correct answer given, then their explanation is saved as a "correct" option. Similar to the method used for choosing "correct" options, "incorrect" options are found by cross-referencing the correct answer (given in the Questions csv file) with students' answers (given in the Answers file). The two data sets are cross-referenced, and if a student's answer doesn't match the correct answer given, then their explanation is saved as an "incorrect" option.

Reason: We assume that students who can choose the correct answers have the correct understanding of the questions and can provide their perspectives for their peers to understand how they get the right answers.

- How the quiz picks options for students who get the question wrong initially

If students choose the wrong answers and submit the quiz, they need to retake the quiz. They will retake the quiz after they finish all the questions, submit it and get feedback. The choices of each question will be randomly chosen from students’ answers to the open-ended questions generated by the selection mechanism.

As for how we implement it, the output json file has one correct option and up to six incorrect options per question. If a student tries to retry the question/quiz, the frontend is given the option to choose from one of these six incorrect options, allowing up to one unique retry for each question in the data set. It was assumed on the backend that the frontend side of the application would slice the given array between indices [1, 3] inclusive for the first question attempt and [4, 6] inclusive for the second.

*Reason:* We don’t want students to retake the same quiz with the same choices as last time. Because students may remember the choices and choose an answer randomly without further learning. So we take other students’ open-ended answers to truly test if the students have a deep understanding of the problem when we change the choices.

- Feedback shown immediately after students attempt a question

We provide corrective feedback for students. The feedback follows the structure: Recap on the student’s choice + Correct / Incorrect + What is the correct answer + The reason why it is the correct answer (detailed feedback). The detailed feedback is the original correct answer in the dataset.

The feedback given to students matches the grammatically synthesized and concatenated "correct" answers obtained from the Questions data set. Like the way of choosing "correct" and "incorrect" answers, this was the most logical approach.

Reason: We think corrective feedback can help the students focus on the task details. By giving them the possible right answer which is not exactly the same as the choices in their own quiz, they can be inspired by the possibility of the correct answers and focus on the rationale behind the choice. When they are focusing on the task details, it can help them reflect on why they were wrong and try to overcome misconceptions, which can improve learning.

- Results of running the Jupyter Notebook code on the validation data

A change to the code was required when the validation set was imported to the Jupyter Notebook. This is because the code assumed that Question_id's would start at 1. However, the first Question_id in the validation set was 5. This comment is noted in the first few lines of the Jupyter Notebook code.

After this change was made, the code proved to be robust and a new dataset with two questions was generated for testing on the frontend application.

- Future Improvement Reflection

Although our project can generate new quizzes easily by using students' answers, we can't be 100% sure that the answers from students are correct/incorrect based only by cross-referencing their explanations with their actual choices. Even if the student chose the right choice, they may still include their misconceptions in their open-ended questions. To improve our model, we need to utilize multiple factors in the dataset to determine which answers from students are correct/incorrect. For example, students with higher scores on the whole quiz may have more serious attitude towards the quiz, or their grasp of the entire module might be more accurate. We can try to explore the relationship between students' learning behaviors and their learning outcomes (quiz score) if we can get more data.

## Project setup Local
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
