<template>
    <h1>RECAP</h1>

    <p>
        {{ hasOn ? quiz.success_message : quiz.failure_message }}
    </p>
    <p>
        Score : {{ score }}/{{ quiz.questions.length }}
    </p>
</template>

<script setup>
import { computed } from 'vue';
import Quiz from './quiz.vue';
import Question from './question.vue';

    const props = defineProps ({
        quiz: Object,
        answers: Array
    })

    const score = computed (() => {
        return props.quiz.questions.reduce((acc, question, k) => {
            if (question.correct_answer === props.answers[k]) {
                return acc+1;
            }
            return acc
        }, 0)
    })

    const hasOn = computed (() => score.value >= props.quiz.minimum_score)
</script>