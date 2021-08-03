是否显示属性，在表单联动的时候可以通过 this.formMap['key'].hidden 赋值动态改变
隐藏时会从v-model里把值清除
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
        name: '只读',
        sex: 1
      },
      formMap: {
        sex: {
          label: '动态显示',
          type: 'select',
          options: [
            {
              label: '显示',
              value: 1
            },
            {
              label: '隐藏',
              value: 2
            }
          ],
          on: {
            change: (value)=>{
              this.formMap['name'].hidden = value===2
            }
          }
        },
        name:{
          label: '姓名',
          type: 'input'
        },
      }
    }
  }
}
</script>
```