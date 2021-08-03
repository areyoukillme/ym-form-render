
```vue
<template>
  <ym-form-render :columnNum="2" v-model="formData" :formMap="formMap"/>
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
          required: true,
        },
        sex:{
          label: '性别',
          type: 'select',
          required: true,
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
        birthDay: {
          label: '生日',
          type: 'date-picker',
          props: {
            type: 'datetime',
          }
        },
        radio:{
          label: '单选框组',
          type: 'radio-group',
          required: true,
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
        switch: {
          label: '开关',
          type: 'switch',
          props: {
            activeText:"按月付费",
            inactiveText:"按年付费"
          }
        },
        slide: {
          label: '滑块',
          type: 'slider'
        },
        rate: {
          label: '评分',
          type: 'rate'
        },

      }
    }
  }
}
</script>
```