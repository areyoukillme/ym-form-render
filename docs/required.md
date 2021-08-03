是否必填属性，在表单联动的时候可以通过 this.formMap['key'].required 赋值动态改变
注：element的required属性提示语是英文。我这里做了转换会自动往form-item属性里注入一条提示语为“请填写/选择XXX”的rules规则，当required属性为false时，会过滤这条规则。
所以required属性为false，rules里添加required规则会失效。
```vue
<template>
  <div>
    <ym-form-render ref="formRender" v-model="formData" :formMap="formMap"/>
    <el-button type="primary" @click="submit">提交</el-button> 
    <el-button type="primary" @click="reset">重置</el-button> 
    <el-button @click="clearValidate">清除校验</el-button> 
    
  </div>
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
          rules: [{min:3,message:'不能少于三位字符串'}]
        },
        sex: {
          label: '性别',
          type: 'select',
          required: true,
          options: [{
            label: '男',
            value: 1
          }]
        }
      }
    }
  },
  methods:{
    submit(){
      this.$refs['formRender'].validate(res=>{
        console.log(res)
      })
    },
    reset(){
      this.$refs['formRender'].resetFields()
    },
    clearValidate(){
      this.$refs['formRender'].clearValidate()
    }
  }
}
</script>
```