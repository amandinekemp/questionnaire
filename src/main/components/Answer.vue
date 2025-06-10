<template>
  <label :for="id" :class="computedClasses">
    <input
        :disabled="disabled"
        :id="id"
        type="radio"
        :name="name"
        :value="value"
        :checked="modelValue === value"
        @change="onchange"
    />
    {{ value }}
  </label>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  id: String,
  name: String,
  disabled: Boolean,
  value: String,
  correctAnswer: String,
  modelValue: String
})

const emits = defineEmits(['change', 'update:modelValue'])

const onchange = (event) => {
  emits('change', event)
  emits('update:modelValue', event.target.value)
}

const computedClasses = computed(() => ({
  disabled: props.disabled,
  right: props.disabled && props.value === props.correctAnswer,
  wrong:
      props.disabled &&
      props.value !== props.correctAnswer &&
      props.modelValue === props.value
}))
</script>

<style>
.disabled {
  opacity: 0.5;
}
.right {
  color: green;
  font-weight: bold;
}
.wrong {
  color: red;
  font-weight: bold;
}
</style>
