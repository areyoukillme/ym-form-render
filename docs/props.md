组件el属性，通过props属性配置
```vue
<template>
  <div>
    <ym-form-render v-model="formData" :formMap="formMap"/>
    <p v-for="(i,key) of formData" :key="key">{{key}}:{{i}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: 1
      },
      formMap: {
        name:{
          label: '姓名',
          type: 'input',
          props: {
            type: 'textarea',
            size: 'large'
          }
        },
      }
    }
  }
}
</script>
```