<script setup>
import { ref, watch } from "vue";
import quizData from "../data/quiz.json";
import Card from "../components/Card.vue";
import gsap from 'gsap'
const quizes = ref(quizData);
const search = ref("");

watch(search, () => {
  quizes.value = quizData.filter((quiz) =>
    quiz.name.toLowerCase().includes(search.value.toLowerCase())
  );
});

const afterEnter = () => {}
const beforeEnter = (el) => {
  el.style.opacity = 0
}
const enter = (el) => {
  gsap.to(el,{
    y:0,
    opacity:1,
    duration:0.4,
    delay:5
  })
  el.style.opacity  = 1
  el.style.transform = "translateY(0)"
}
</script>

<template>
  <div class="container">
    <header>
      <h1>Quizes</h1>
      <input v-model.trim="search" type="text" placeholder="Search..." />
    </header>
    <div class="options-container">
      <TransitionGroup name="card" appear @after-enter="afterEnter" @before-enter="beforeEnter" @enter="enter">
      <!-- passing props in cards -->
      <Card v-for="quiz in quizes" :key="quiz.id" :quiz="quiz" />
      </TransitionGroup>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 1000px;
  margin: 0 auto;
}

header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  font-weight: bold;
  margin-right: 30px;
}

header input {
  border: none;
  background-color: rgba(128, 128, 128, 0.1);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 40px;
}

/* CARD */

.card-enter-from{
  transform: translateY(-50px);
  opacity: 0;
}

.card-enter-to{
  transform:translateY(0);
  opacity: 1;
}

.card-enter-active{
  transition: all 4s ease-in;
}
</style>
