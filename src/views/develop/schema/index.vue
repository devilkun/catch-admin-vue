<template>
  <div>
    <Search :search="search" :reset="reset">
      <template v-slot:body>
        <el-form-item label="模块名称">
          <el-input v-model="query.module" name="module" clearable />
        </el-form-item>
        <el-form-item label="Schema 名称">
          <el-input v-model="query.name" name="name" clearable />
        </el-form-item>
      </template>
    </Search>
    <div class="table-default">
      <Operate :show="open">
        <template #operate>
          <el-button @click="existSchemaVisible = true">选择已有表</el-button>
        </template>
      </Operate>
      <el-table :data="tableData" class="mt-3" v-loading="loading">
        <el-table-column prop="module" label="所属模块" />
        <el-table-column prop="name" label="schema 名称" />
        <el-table-column prop="columns" label="字段">
          <template #default="scope">
            <el-button size="small" type="success" @click="view(scope.row.id)"><Icon name="eye" class="w-3 mr-1" /> 查看</el-button>
          </template>
        </el-table-column>
        <el-table-column prop="is_soft_delete" label="?软删">
          <template #default="scope">
            <el-tag v-if="scope.row.is_soft_delete">是</el-tag>
            <el-tag type="danger" v-else>否</el-tag>
          </template>
        </el-table-column>
        <el-table-column prop="created_at" label="创建时间" />
        <el-table-column label="操作" width="250">
          <template #default="scope">
            <router-link :to="'/develop/generate/' + scope.row.id">
              <el-button type="warning" size="small"><Icon name="wrench-screwdriver" class="w-3 mr-1" /> 生成代码</el-button>
            </router-link>
            <Destroy @click="destroy(api, scope.row.id)" class="ml-2" />
          </template>
        </el-table-column>
      </el-table>
      <Paginate />
    </div>

    <!-- schema 创建 -->
    <Dialog v-model="visible" :title="title" width="650px" destroy-on-close>
      <Create @close="close(reset)" :api="api" />
    </Dialog>

    <!-- schema 表结构 -->
    <Dialog v-model="schemaVisible" title="Schema 结构" width="650px" destroy-on-close>
      <Show :id="id" :api="api" />
    </Dialog>

    <!-- 从已有 Schema 选择 -->
    <Dialog v-model="existSchemaVisible" title="选择已有表" width="650px" destroy-on-close>
      <addExistSchema :api="api" @close="closeExistSchemaVisible" />
    </Dialog>
  </div>
</template>

<script lang="ts" setup>
// @ts-nocheck
import { computed, onMounted, ref } from 'vue'
import Create from './create.vue'
import Show from './show.vue'
import addExistSchema from './addExistSchema.vue'
import { useGetList } from '@/composables/curd/useGetList'
import { useDestroy } from '@/composables/curd/useDestroy'
import { useOpen } from '@/composables/curd/useOpen'

const schemaVisible = ref<boolean>(false)
const existSchemaVisible = ref<boolean>(false)

const api = 'schema'

const { data, query, search, reset, loading } = useGetList(api)
const { destroy, deleted } = useDestroy('确认删除吗? 删除后数据表将会保留，如需删除相关表，请手动进行删除!')
const { open, close, title, visible, id } = useOpen()

const tableData = computed(() => data.value?.data)

const view = primaryId => {
  schemaVisible.value = true

  id.value = primaryId
}
const closeExistSchemaVisible = () => {
  existSchemaVisible.value = false
  reset()
}
onMounted(() => {
  search()

  deleted(reset)
})
</script>
