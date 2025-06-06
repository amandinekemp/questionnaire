<template>
  <label :for="id" :class="classes">
    <input :disabled="disabled" :id="`answer${index}`" type="radio" name="answer" :value="value" @change="onchange" v-model="model">
    {{ value }}
  </label>
</template>


<script>
import {computed} from "vue";
import classes from "*.module.css";
import {defineComponent} from "vue";

export default defineComponent({
  computed: {
    classes() {
      return classes
    }
  }
})

const props = defineProps({
  id: String,
  disabled: Boolean,
  value: String,
  correctAnswer: String

})
const emits = defineEmits(['change'])
const onchange = (event) => {
  emits('change', event)
}
const model = defineModel()
const classes = computed(() => ({
  disabled: props.disabled,
  right: props.disabled && props.value === props.correctAnswer,
  wrong: props.disabled && props.value !== props.correctAnswer && model.value === props.value
}))
</script>

<style>
.disabled {
  opacity: .5;
}
.right {
  opacity: 1;
  color: green;
}
.wrong {
  opacity: 1;
  color: red;
}
</style>