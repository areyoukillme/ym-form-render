对输入值或输出值进行转化 通过outformat 和informat 属性方法
```vue
<template>
  <div>
    <ym-form-render ref="formRender" v-model="formData" :formMap="formMap"/>
    <p>输出值：</p>
    <p v-for="(i,key) of formData" :key="key">{{key}}:{{i}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
      },
      formMap: {
        birthDay: {
          label: '生日',
          type: 'date-picker',
          props: {
            type: 'daterange',
            clearble: true
          },
          outFormat: (value)=>{
            if(value){
              return {
                startTime: value[0],
                endTime: value[1]
              }
            }else{
              return {
                startTime: '',
                endTime: ''
              }
            }
          },
          inFormat: (formData)=>[formData.startTime,formData.endTime]
        }
      }
    }
  }
}
</script>
```