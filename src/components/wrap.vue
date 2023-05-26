<template>
  <div class="wrap">
    <Basic v-model="formState"></Basic>
  </div>
</template>
<script lang="ts" setup>
import { reactive, ref } from 'vue';
import Basic from './basic.vue'

interface FormState {
  color: string | undefined;
}
const formState = ref<FormState>({
  color: undefined,
});
</script>