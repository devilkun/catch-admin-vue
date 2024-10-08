<template>
  <el-form :model="formData" label-width="120px" ref="form" v-loading="loading" class="pr-4">
    <el-form-item
      :label="$t('module.form.name.title')"
      prop="title"
      :rules="[
        {
          required: true,
          message: $t('module.form.name.required')
        }
      ]"
    >
      <el-input v-model="formData.title" />
    </el-form-item>
    <el-form-item
      :label="$t('module.form.path.title')"
      prop="path"
      :rules="[
        {
          required: true,
          message: $t('module.form.path.required')
        }
      ]"
    >
      <el-input v-model="formData.path" :disabled="!!primary" />
    </el-form-item>
    <el-form-item :label="$t('module.form.keywords.title')" prop="keywords">
      <el-input v-model="formData.keywords" />
    </el-form-item>
    <el-form-item :label="$t('module.form.desc.title')" prop="desc">
      <el-input v-model="formData.description" type="textarea" />
    </el-form-item>

    <el-form-item :label="$t('module.form.dirs.title')" prop="dirs" v-if="!primary">
      <el-checkbox v-model="formData['dirs']['controllers']" label="Controllers" size="large" />
      <el-checkbox v-model="formData['dirs']['models']" label="Models" size="large" />
      <el-checkbox v-model="formData['dirs']['database']" label="Database" size="large" />
      <el-checkbox v-model="formData['dirs']['requests']" label="Requests" size="large" />
    </el-form-item>

    <div class="flex justify-end">
      <el-button type="primary" @click="submitForm(form)">{{ $t('system.confirm') }}</el-button>
    </div>
  </el-form>
</template>

<script lang="ts" setup>
// @ts-nocheck
import { useCreate } from '@/composables/curd/useCreate'
import { useShow } from '@/composables/curd/useShow'

import { onMounted } from 'vue'

const props = defineProps({
  primary: [String, Number],
  api: String
})

const { formData, form, loading, submitForm, close } = useCreate(
  props.api as string,
  props.primary,
  Object.assign({
    title: '',
    path: '',
    keywords: '',
    description: '',
    dirs: {
      controllers: true,
      models: true,
      database: true,
      requests: false
    }
  })
)

const emit = defineEmits(['close'])

if (props.primary) {
  useShow(props.api as string, props.primary, formData)
}

onMounted(() => {
  close(() => emit('close'))
})
</script>
