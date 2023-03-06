<script setup>
import { computed } from "@vue/reactivity";
import { ref } from "vue";
import { useRoute } from "vue-router";
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue";
import quizData from "../data/quiz.json";
const route = useRoute();
const quizId = parseInt(route.params.id);
const quiz = quizData.find((quizes) => quizes.id === quizId);
const currentQuizObject = ref(0);
const numberOfCorrectAns = ref(0);
const questionStatus = computed(
  () => `${currentQuizObject.value}/${quiz.questions.length}`
);

const barpercantage = computed(
  () => `${(currentQuizObject.value / quiz.questions.length) * 100}%`
);

const onOptionSelected = (isCorrect) => {
  if (isCorrect === true) {
    numberOfCorrectAns.value++;
  }
  currentQuizObject.value++;
};
</script>
<template>
  <div>
    <QuizHeader
      :questionStatus="questionStatus"
      :barPerctange="barpercantage"
    />
    <div>
      <Question
        :question="quiz.questions[currentQuizObject]"
        @selectedOption="onOptionSelected"
      />
    </div>
  </div>
  {{ currentQuizObject }}
  <button @click="currentQuizObject++">Next Question</button>
  <button @click="currentQuizObject--">Previous</button>
</template>
