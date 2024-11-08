<script setup>
import { shofflArray } from '@/functions/array';
import { computed, onMounted, ref, watch } from 'vue';
import Answer from './answer.vue';

const props = defineProps({
    question: Object
});

const answer = ref(null);
const emits = defineEmits(['answer']);
const hasAnswer = computed(() => answer.value !== null);
const notification = ref(''); // Message de notification

let timer;

//  pour la première question
onMounted(() => {
    timer = setTimeout(() => {
        answer.value = '';
        notification.value = 'Temps écoulé ! Vous avez perdu un point.'; // Message de notification
        onAnswer();
    }, 3000);
});

// Fonction pour lancer le timer
const startTimer = () => {
    clearTimeout(timer);
    timer = setTimeout(() => {
        answer.value = '';
        notification.value = 'Temps écoulé ! Vous avez perdu un point.'; // Message de notification
        onAnswer();
    }, 3000);
};

// Déclencher le timer à chaque changement de question
watch(() => props.question, () => {
    answer.value = null;
    notification.value = ''; // Réinitialisation du message
    startTimer(); // Démarrage du timer pour chaque nouvelle question
});

const randomChoices = computed(() => {
    return shofflArray(props.question.choices);
});

const onAnswer = () => {
    clearTimeout(timer);
    timer = setTimeout(() => {
        emits('answer', answer.value);
    }, 1000);
};

// Nettoyage du timer quand le composant est détruit
onMounted(() => {
    return () => clearTimeout(timer);
});
</script>

<template>
    <div class="question">
      <h3>
        {{ question.question }}
      </h3>
      <table class="choices-table">
        <thead>
          <tr>
            <th>Réponses</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(choice, index) in randomChoices" :key="choice">
            <td>
              <Answer
                :id="`answer${index}`"
                :disabled="hasAnswer"
                :value="choice"
                v-model="answer"
                :correctAnswer="question.correct_answer"
                @change="onAnswer"
              />
            </td>
          </tr>
        </tbody>
      </table>
      <p v-if="notification" class="notification">{{ notification }}</p>
    </div>
</template>

<style>

h3 {
    font-size:large;
}
.question {
    padding-top: 1rem;
}

.question button {
    margin-left: auto;
    display: block;
}

.notification {
    color: red;
    font-weight: bold;
    margin-top: 1rem;
}

.choices-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

.choices-table th,
.choices-table td {
  border: 1px solid #ccc;
  padding: 0.5rem;
}

.choices-table td {
    text-align: left;
}

.choices-table th {
    text-align: center;
    font-size: larger;
}

.notification {
  color: red;
  font-weight: bold;
  margin-top: 1rem;
}
</style>