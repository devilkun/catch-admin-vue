<template>
  <el-switch @change="enabled(api, id)" :active-value="activeValue" :inactive-value="inactiveValue" :model-value="modelValue" :loading="loading" />
</template>

<script lang="ts" setup>
import { useEnabled } from '@/composables/curd/useEnabled'
import { Status } from '@/enum/app'
import { ref } from 'vue'

const props = defineProps({
  modelValue: [Boolean, Number, String],
  api: {
    required: true,
    type: String
  },
  id: {
    required: true,
    type: [String, Number]
  }
})

const emits = defineEmits(['update:modelValue', 'refresh'])
// @ts-ignore
const { enabled, success, loading, afterEnabled } = useEnabled()

const activeValue = ref<boolean | number | string>()
const inactiveValue = ref<boolean | number | string>()

if (typeof props.modelValue === 'boolean') {
  activeValue.value = true
  inactiveValue.value = false
} else {
  activeValue.value = Status.ENABLE
  inactiveValue.value = Status.DISABLE
}

success(() => {
  emits('update:modelValue', props.modelValue === activeValue.value ? inactiveValue.value : activeValue.value)
})

afterEnabled.value = () => {
  emits('refresh')
}
</script>
