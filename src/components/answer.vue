<template>
    <label :for="id" :class="classes">
        <input 
            :disabled="disabled" 
            :id="id" 
            type="radio" 
            :name="id" 
            :value="value" 
            :checked="modelValue === value"
            @change="onChange"
        />
        {{ value }}
    </label>
</template>

<script setup>
    import { computed } from 'vue';

    const emits = defineEmits(['update:modelValue', 'change']);

    const props = defineProps({
        id: String,
        disabled: Boolean,
        value: String,
        modelValue: String,
        correctAnswer: String
    });

    const onChange = () => {
        emits('update:modelValue', props.value);
        emits('change');
    };

    const classes = computed(() => ({
        disabled: props.disabled,
        right: props.disabled && props.value === props.correctAnswer,
        wrong: props.disabled && props.value !== props.correctAnswer && props.modelValue === props.value
    }));
</script>

<style>
    .right {
        color: aqua;
        opacity: 1;
    }

    .wrong {
        color: red;
        opacity: 1;
    }

    .disabled {
        opacity: 0.5;
    }
</style>
