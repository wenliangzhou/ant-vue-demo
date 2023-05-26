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
  <a-form :model="data" name="basic" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" autocomplete="off"
    @finish="onFinish" @finishFailed="onFinishFailed">
    <a-form-item label="绑定data.color" name="color"
      :rules="[{ required: true, message: 'Please select favourite color!', trigger: 'change' }]">
      <a-select :allowClear="true" v-model:value="data.color" @change="onDataChange"
        placeholder="Please select favourite color">
        <a-select-option value="red">Red</a-select-option>
        <a-select-option value="green">Green</a-select-option>
        <a-select-option value="blue">Blue</a-select-option>
      </a-select>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
      <a-button type="primary" html-type="submit">Submit</a-button>
    </a-form-item>
  </a-form>


  <!-- props -->
  <a-form :model="props.modelValue" name="basic" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" autocomplete="off"
    @finish="onFinish" @finishFailed="onFinishFailed">
    <a-form-item label="绑定计算属性props.color 通过emit更新" name="color"
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


  <!-- cloneProps  -->
  <a-form :model="cloneProps" name="basic" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" autocomplete="off"
    @finish="onFinish" @finishFailed="onFinishFailed">
    <a-form-item label="绑定cloneProps 通过emit更新" name="color"
      :rules="[{ required: true, message: 'Please select favourite color!', trigger: 'change' }]">
      <a-select :allowClear="true" v-model:value="cloneProps.color" @change="onCloneChange"
        placeholder="Please select favourite color">
        <a-select-option value="red">Red</a-select-option>
        <a-select-option value="green">Green</a-select-option>
        <a-select-option value="blue">Blue</a-select-option>
      </a-select>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
      <a-button type="primary" html-type="submit">Submit</a-button>
    </a-form-item>
  </a-form>

  <test v-model="data.color"></test>
</template>
<script lang="ts" setup>
import { computed, watch, ref, reactive, provide } from 'vue';
import cloneDeep from 'lodash/cloneDeep'
import Test from './test.vue'
const props = defineProps<{ modelValue: FormState }>()
const emit = defineEmits<{
  (e: 'update:modelValue', modelValue: FormState): void,
}>()

interface FormState {
  color: string | undefined;
}


watch(props, () => {
  console.log('监听到basic props', props.modelValue.color);
}, {
  deep: true
})

// -----------------------------------------------------------------------------------

const data = ref<FormState>({
  color: undefined,
});

watch(data, () => {
  console.log('监听到basic data', data.value.color);
}, {
  deep: true
})
const onDataChange = () => {
  console.log('select Change data.color:', data.value.color);
}


// -----------------------------------------------------------------------------------

const color = computed({
  get() {
    return props.modelValue.color;
  },
  set(newValue) {
    setNewValue('color', newValue);
  }
})
const onChange = () => {
  console.log('select Change props.modelValue.color:', props.modelValue.color);
}
function setNewValue<T extends keyof FormState>(key: T, val: FormState[T]) {
  let newValue = cloneDeep(props.modelValue);
  newValue[key] = val;
  emit('update:modelValue', newValue)
}

// -----------------------------------------------------------------------------------

const cloneProps = reactive(cloneDeep(props.modelValue));

watch(cloneProps, (value) => {
  console.log('监听到cloneProps 变化， eimt', value.color);
  emit('update:modelValue', value)
}, {
  deep: true
})
const onCloneChange = () => {
  console.log('select onCloneChange cloneProps.color:', cloneProps.color);
}

// ------------------------------------------------------------------------------------

const onUpdate = () => {
  console.log('test update 校验', data.value.color);
}

provide('onUpdate', onUpdate)

const onFinish = (values: any) => {
  console.log('Success:', values);
};

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};
</script>