<template>
  <div class="wrap">
    <Basic v-model="formState"></Basic>
  </div>
</template>
<script lang="ts" setup>
import { ref ,watch} from 'vue';
import Basic from './basic.vue'

interface FormState {
  color: string | undefined;
}
const formState = ref<FormState>({
  color: undefined,
});

watch(formState, () => {
  console.log('监听到wrap props', formState.value.color);
}, {
  deep: true
})
</script>