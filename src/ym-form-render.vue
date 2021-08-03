<template>
  <el-form v-on="$listeners" ref="elform" v-bind="Object.assign({labelWidth:'86px',labelSuffix:' : '},$attrs)" :model="formData" class="formRender">
    <el-row :gutter="gutter">
      <template v-for="(item,key) in _formMap">
        <el-col v-if="!isFunction(item.hidden)" :key="key" :span="item.span || 24/columnNum">
          <el-form-item :label="item.label" :rules="item.rules" :prop="key">
            <render :readonly="readonly" :disabled="disabled" class="form-item" :props-item="{...item,key}" :form-value="value" @input="formchange" @itemOn="on" />
          </el-form-item>
        </el-col>
      </template>
    </el-row>
  </el-form>
</template>

<script>
import render from './form-item.vue'
export default {
  name: 'YmFormRender',
  components: {
    render
  },
  props: {
    /**
     *设置表单有几列，会给form-item 分配24/columnNum 的span。也可以form-item单独设置span
     */
    columnNum: {
      type: Number,
      default: 1
    },
    /**
     * @model
     */
    value: {
      type: Object,
      default: null
    },
    /**
     * 表单配置项options
     */
    formMap: {
      type: Object,
      default: () => ({})
    },
    /**
     * 全局是否只读模式，也可以form-item 单独设置
     */
    readonly: {
      type: Boolean,
      default: false
    },
    /**
     * 全局disabled属性，也可以form-item 单独设置
     */
    disabled: {
      type: Boolean,
      default: false
    },
    /**
     * form-item之间的留白间距
     */
    gutter: {
      type: Number,
      default: 24
    }
  },
  data() {
    return {
      formData: this.value,
      arr: [],
      changeByExtraOn: false
    }
  },
  computed: {
    _formMap() {
      const Map = {}
      for (const i in this.formMap) {
        const item = this.formMap[i]
        if (item.required) {
          if (item.rules && Array.isArray(item.rules)) {
            if (!item.rules.find(item => item.required)) {
              item.rules.push(
                {
                  required: true,
                  message: `请${item.type === 'input' ? '填写' : '选择'}${item.label}`,
                }
              )
            }
          } else {
            item.rules = [
              {
                required: true,
                message: `请${item.type === 'input' ? '填写' : '选择'}${item.label}`,
              }
            ]
          }
        } else {
          if (Array.isArray(item.rules)) { item.rules = item.rules.filter(item => !item.required) }
        }
        if (item.rules) {
          item.rules = [...item.rules]
        } 
        Map[i] = item
      }
      return Map
    },
  },

  watch: {
    value: {
      handler: function(val) {
        this.formData = val
      },
      deep: true
    },
    formData(value,oldValue){
      // 获取哪些值从有值变为空值
      if(this.changeByExtraOn){
        Object.keys(value).forEach(key=>{
          if(oldValue[key] && !value[key]){
            this.arr.push(key)
          }
          if(!oldValue[key] && value[key]){
            this.arr = this.arr.filter(item=>item!=key)
          }
        })
      }
    },
  },
  methods: {
    // 接受子组件emit值改变事件，emit input事件实现双向绑定
    formchange(res,key) {
      this.arr = []
      this.formData = Object.assign({}, this.formData, res)
      this.$emit('input', this.formData)
    },
    on(func, res, key) {
      if (typeof (func) === 'function') {
        this.changeByExtraOn = true
        func(res)
        setTimeout(()=>{
          this.changeByExtraOn = false
          this.setValidateEvent(key)
        })
        // 隐藏某个form-item时同时数据删除该项
        for (const i in this.formMap) {
          if (this.formMap[i].hidden) {
            if (this.formMap[i].outFormat) {
              for (const j in this.formMap[i].outFormat()) {
                delete this.formData[j]
              }
            } else {
              delete this.formData[i]
            }
          }
          this.$emit('input', this.formData)
        }
      }
    },
    // 设置某些校验项不校验
    setValidateEvent(key){
      this.arr.forEach(item=>{
        if(item!=key){
          this.clearValidate(item)
        }
      })
    },
    /**
     * 重置表单 同element-form
     * @public This is a public method
     */
    resetFields() {
      this.$refs.elform.resetFields()
    },
    /**
     * 校验 同element-form
     * @public This is a public method
     */
    validate(callback) {
      this.$refs.elform.validate(callback)
    },
    /**
     * 校验某一项 同element-form
     * @public This is a public method
     */
    validateField(props,callback){
      this.$refs.elform.validateField(props,callback)
    },
    /**
     * 清除校验 同element-form
     * @public This is a public method
     */
    clearValidate(props){
      this.$refs.elform.clearValidate(props)
    },
    // 判断是否是函数，是取函数结果，不是直接取值
    isFunction(val) {
      if (typeof (val) === 'function') {
        return val()
      }
      return val
    }
  }
}
</script>

<style lang="less" scoped>
.formRender{
  /deep/
  .el-form-item__content{
    display: flex;
  }
  /deep/
  .el-radio{
    line-height: 40px;
  }
  /deep/
  .el-switch{
    line-height: 40px;
    .el-switch__core{
      margin-top: 20px;
    }
  }
  /deep/
  .el-rate{
    line-height: 40px;
    .el-rate__item{
      .el-rate__icon{
        margin-top: 10px;
      }
    }
  } 
}
</style>
