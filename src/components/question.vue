<template>
    <div class="question">
        <h3>
            {{ question.question }}
        </h3>
        <ul>
            <li v-for="(choice, index) in randomChoices" :key="choice">
                <Answer
                    :id="`answer${index}`"
                    :disabled="hasAnswer"
                    :value="choice"
                    v-model="answer"
                    :correctAnswer="question.correct_answer"
                    @change="onAnswer"
                />
            </li>
        </ul> 
    </div>
</template>

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

    watch(() => props.question, () => {
        answer.value = null;
    });

    const randomChoices = computed(() => {
        return shofflArray(props.question.choices);
    });

    let timer;

    const onAnswer = () => {
        clearTimeout(timer);
        timer = setTimeout(() => {
            emits('answer', answer.value);
        }, 1000);
    };

    onMounted(() => {
        timer = setTimeout(() => {
            answer.value = '';
            onAnswer();
        }, 3000);
    });

    // Nettoyage du timer quand le composant est détruit
    onMounted(() => {
        return () => clearTimeout(timer);
    });
</script>

<style>
    .question {
        padding-top: 2rem;
    }

    .question button {
        margin-left: auto;
        display: block;
    }
</style>
