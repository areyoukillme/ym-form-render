readonly 属性可以全局设置，也可以form-item单独设置，在表单联动的时候可以通过 this.formMap['key'].readonly 赋值动态改变
输入框和选择框 设置readonly时会渲染 span标签，select会直接渲染label
```vue
<template>
  <div>
  <ym-form-render :readonly="readonly" v-model="formData" :formMap="formMap"/>
  <el-button @click="readonly=!readonly" type="primary">是否只读</el-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      readonly: false,
      formData: {
        name: '只读',
        sex: 1
      },
      formMap: {
        name:{
          label: '姓名',
          type: 'input',
        },
        sex: {
          label: '性别',
          type: 'select',
          options: [
            {
              label: '男',
              value: 1
            },
            {
              label: '女',
              value: 2
            }
          ]
        },
        birthday: {
          label: '生日',
          type: 'date-picker'
        }
      }
    }
  }
}
</script>
```