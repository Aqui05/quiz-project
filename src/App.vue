<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>Impossible de charger le quiz</p>
    </div>
    <div :aria-busy="state === 'loading'">
      <div v-if="quiz" class="card">
        <Quiz :quiz="quiz" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Quiz from './components/quiz.vue';

const quiz = ref(null);
const state = ref('loading');

onMounted(() => {
  fetch('/quiz.json')
    .then(r => {
      if (r.ok) {
        return r.json();
      } else {
        throw new Error('Impossible de récupérer le JSON');
      }
    })
    .then(data => {
      quiz.value = data;
      state.value = 'idle';
    })
    .catch(e => {
      state.value = 'error';
    });
});
</script>

<style scoped>
.container {
  margin-top: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.card {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
}
</style>
