<template>
    <div>
      <el-form :inline="true" :model="formData" ref='formRef' class="form" :label-width="formData.labelWidth">
        <el-row>
          <el-col :span="item.span" v-for="(item, index) in formData.searchList" :key="index">
            <!-- 下拉框 -->
            <el-form-item v-if="item.type == 'select'" :label="item.label" :rules="item.rules" :prop="'searchList.'+index+'.value'" >
              <el-select v-model.trim="item.value" :placeholder="item.placeholder || '请选择'" clearable>
                <el-option v-for="(sub, index) in item.options" :key="index + 'op'" :value="sub.subVal" :label="sub.label" />
              </el-select>
            </el-form-item>
            <!-- 输入框 -->
            <el-form-item v-if="item.type == 'input'" :label="item.label" :rules="item.rules" :prop="'searchList.'+index+'.value'">
              <el-input v-model.trim="item.value" :placeholder="item.placeholder || '请选择'" clearable />
            </el-form-item>
            <!-- 自定义 slot -->
            <el-form-item v-if="item.type == 'slot'" :label="item.label" :rules="item.rules">
              <slot :name="item.name"></slot>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </template>
  
  
  <script setup>
  import { ref, reactive, toRefs, onMounted } from 'vue'
  import { useRouter } from 'vue-router'
  const router = useRouter()
  
  
  // 使用
  /* const formData = reactive({
    labelWidth:'120px',
    searchList:[
      {
        id:'1',
        type:'input',//select
        label:'用户ID',//el-form-item label='xxx'
        placeholder:"请输入",
        value:"6927",
        rules:[{}]
      },
      {
        id:'2',
        type:'select',//select
        label:'用户ID',//el-form-item label='xxx'
        placeholder:"请输入",
        value:"6927",
        options:[
          {
            subVal:'a',
            label:'a'
          }
        ],
        rules:[{}]
      },
    ]
  }) */
  
  
  const props = defineProps({
    formData: Object,
    rules: Object
  })
  </script>
  
  
  <style lang='scss' scoped>
  
  .el-form-item {
    margin: 0 !important;
    width: 100% !important;
    margin-bottom: 1rem !important;
  }
  .el-row {
    height: auto;
  }
  .el-input {
    width: 100%;
  }
  .el-select {
    width: 100%;
  }
  </style>