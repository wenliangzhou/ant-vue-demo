<template>
  <!-- 第一种 直接绑定props的属性 -->
  <!-- <a-form :model="props.modelValue" name="basic" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" autocomplete="off"
    @finish="onFinish" @finishFailed="onFinishFailed">
    <a-form-item label="color" name="color"
      :rules="[{ required: true, message: 'Please select favourite color!', trigger: 'change' }]">
      <a-select :allowClear="true" v-model:value="props.modelValue.color" placeholder="Please select favourite color">
        <a-select-option value="red">Red</a-select-option>
        <a-select-option value="green">Green</a-select-option>
        <a-select-option value="blue">Blue</a-select-option>
      </a-select>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
      <a-button type="primary" html-type="submit">Submit</a-button>
    </a-form-item>
  </a-form> -->
  <!-- 第二种 通过emit新值到父级 -->
  <a-form :model="props.modelValue" name="basic" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" autocomplete="off"
    @finish="onFinish" @finishFailed="onFinishFailed">
    <a-form-item label="color" name="color"
      :rules="[{ required: true, message: 'Please select favourite color!', trigger: 'change' }]">
      <a-select :allowClear="true" v-model:value="color" @change="onChange" placeholder="Please select favourite color">
        <a-select-option value="red">Red</a-select-option>
        <a-select-option value="green">Green</a-select-option>
        <a-select-option value="blue">Blue</a-select-option>
      </a-select>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
      <a-button type="primary" html-type="submit">Submit</a-button>
    </a-form-item>
  </a-form>
</template>
<script lang="ts" setup>
import { computed, watch } from 'vue';
import cloneDeep from 'lodash/cloneDeep'
const props = defineProps<{ modelValue: FormState }>()
const emit = defineEmits<{
  (e: 'update:modelValue', modelValue: FormState): void,
}>()

interface FormState {
  color: string | undefined;
}

function setNewValue<T extends keyof FormState>(key: T, val: FormState[T]) {
  let newValue = cloneDeep(props.modelValue);
  newValue[key] = val;
  emit('update:modelValue', newValue)
}

const onChange = () =>{
  console.log('select Change props.modelValue.color:',props.modelValue.color);
}

watch(props, () => {
  console.log('监听到basic props', props.modelValue.color);
}, {
  deep: true
})

// 第二种
const color = computed({
  get() {
    return props.modelValue.color;
  },
  set(newValue) {
    setNewValue('color', newValue);
  }
})
const onFinish = (values: any) => {
  console.log('Success:', values);
};

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};
</script>