<template>
  <ul @click="onClick">
    <li data-index="red" :class="{active: props.modelValue === 'red'}">red</li>
    <li data-index="green" :class="{active: props.modelValue === 'green'}">green</li>
    <li data-index="blue" :class="{active: props.modelValue === 'blue'}">blue</li>
  </ul>
</template>

<script lang="ts" setup>
import { inject  } from 'vue';

const props = defineProps<{ modelValue: string | undefined }>()
const emit = defineEmits<{
  (e: 'update:modelValue', modelValue: string | undefined): void,
}>()
const fn:any = inject('onUpdate')
const onClick =(e:Event)=>{
  if (e.target) {
    emit('update:modelValue',(e.target as HTMLInputElement).dataset.index)
    console.log('fn',fn);
    fn()
    console.log('test modelValue',props.modelValue);
  }
}

</script>

<style scoped>
.active {
  background: blueviolet;
}
</style>