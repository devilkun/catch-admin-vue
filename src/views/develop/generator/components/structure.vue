<template>
  <div>
    <el-table :data="structures">
      <el-table-column prop="field" :label="$t('generate.schema.structure.field_name.name')" width="100px" />
      <el-table-column prop="label" :label="$t('generate.schema.structure.form_label')" width="150px">
        <template #default="scope">
          <el-input v-model="scope.row.label" />
        </template>
      </el-table-column>
      <el-table-column prop="label" :label="$t('generate.schema.structure.form_component')" width="110px">
        <template #default="scope">
          <el-select v-model="scope.row.form_component" class="w-full" filterable>
            <el-option v-for="component in formComponents" :key="component" :label="component" :value="component" />
          </el-select>
        </template>
      </el-table-column>
      <el-table-column prop="list" :label="$t('generate.schema.structure.list')">
        <template #default="scope">
          <el-switch v-model="scope.row.list" inline-prompt :active-text="$t('system.yes')" :inactive-text="$t('system.no')" width="45px" />
        </template>
      </el-table-column>
      <el-table-column prop="form" :label="$t('generate.schema.structure.form')">
        <template #default="scope">
          <el-switch v-model="scope.row.form" inline-prompt :active-text="$t('system.yes')" :inactive-text="$t('system.no')" width="45px" />
        </template>
      </el-table-column>
      <el-table-column prop="search" :label="$t('generate.schema.structure.search')">
        <template #default="scope">
          <el-switch v-model="scope.row.search" inline-prompt :active-text="$t('system.yes')" :inactive-text="$t('system.no')" width="45px" />
        </template>
      </el-table-column>
      <el-table-column prop="search_op" :label="$t('generate.schema.structure.search_op.name')" width="150px">
        <template #default="scope">
          <el-select v-model="scope.row.search_op" :placeholder="$t('generate.schema.structure.search_op.placeholder')" class="w-full">
            <el-option v-for="op in operates" :key="op" :label="op" :value="op" />
          </el-select>
        </template>
      </el-table-column>
      <el-table-column prop="validates" :label="$t('generate.schema.structure.rules.name')" width="250px">
        <template #default="scope">
          <el-select v-model="scope.row.validates" :placeholder="$t('generate.schema.structure.rules.placeholder')" multiple filterable allow-create clearable class="w-full">
            <el-option v-for="validate in validates" :key="validate" :label="validate" :value="validate" />
          </el-select>
        </template>
      </el-table-column>
      <!--<el-table-column prop="comment" label="注释" />-->
      <el-table-column prop="id" :label="$t('generate.schema.structure.operate')" width="120px">
        <template #default="scope">
          <el-button type="danger" :icon="Delete" @click="deleteField(scope.row.field)" size="small" />
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script lang="ts" setup>
// @ts-nocheck
import { ref } from 'vue'
import { Structure as StructureInterface } from './structure'
import { Delete } from '@element-plus/icons-vue'

const structures = ref<StructureInterface>()

const operates: string[] = ['=', '!=', '>', '>=', '<', '<=', 'like', 'RLike', 'LLike', 'in']

const formComponents: string[] = ['cascader', 'date', 'datetime', 'input', 'input-number', 'radio', 'rate', 'select', 'tree', 'tree-select', 'textarea', 'upload']
</script>
