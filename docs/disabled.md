disabled 属性可以全局设置，也可以form-item单独设置，在表单联动的时候可以通过 this.formMap['key'].disabled 赋值动态改变disabled属性
```vue
<template>
  <ym-form-render :disabled="true" v-model="formData" :formMap="formMap"/>
</template>

<script>
export default {
  data() {
    return {
      formData: {
      },
      formMap: {
        name:{
          label: '姓名',
          type: 'input',
        },
        sex: {
          label: '性别',
          type: 'input',
          disabled: false
        }
      }
    }
  }
}
</script>
```