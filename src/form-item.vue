<template>
  <customComponent
    :size="propsItem.props && propsItem.props.size || 'small'"
    :props-item="propsItem"
    :style="itemStyle"
    v-bind="propsItem.props"
    :value="itemValue"
    :disabled="_disabled"
    :readonly="_readonly"
    v-on="listener"
  />
</template>

<script>
export default {
  components: {
    customComponent: {
      functional: true,
      render: (h, { props, data, children }) => {
        const item = props.propsItem
        const tag = item.type ? 'el-' + item.type : item.component || 'el-input'
        if(item.component){
          return h(item.component,data,children)
        }
        if(item.render){
          return item.render(h,props.value)
        }
        if (item.type === 'select') {
          children = item.options && item.options.map(i => h('el-option', { props: {
            label: i.label, value: i.value
          }}))
        }
        if (item.type === 'radio-group') {
          children = item.options && item.options.map(i => h('el-radio', { props: {
            label: i.value
          }}, i.label))
        }
        const readonly = props.readonly
        if(readonly){
          const value = props.inFormat?props.inFormat(props.value):props.value
          if(item.type==='input'){
            return h('span',value)
          }
          if(item.type==='select'){
            let label
            item.options.forEach(item=>{
              if(item.value===value){
                label = item.label
              }
            })
            return h('span',label)
          }
        }
        return h(tag, data, children)
      }
    }
  },
  props: {
    propsItem: {
      type: Object,
      default: null
    },
    formValue: {
      type: null,
      default: null
    },
    readonly: {
      type: Boolean,
      default: false
    },
    disabled:{
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      value: this.formValue[this.propsItem.key]
    }
  },
  computed: {
    _disabled() {
      if(Object.keys(this.propsItem).includes('disabled')){
        return this.propsItem.disabled
      }else{
        return this.disabled
      }
    },
    _readonly() {
      if(Object.keys(this.propsItem).includes('readonly')){
        return this.propsItem.readonly
      }else{
        return this.readonly
      }
    },
    itemValue() {
      const inFormat = this.propsItem.inFormat
      if (inFormat && typeof inFormat === 'function') {
        const value = inFormat(this.formValue)
        // 专门为date-picker做处理 value 为数组时且item都为空值时返回''
        if(Array.isArray(value) && !value.some(item=>item)){
          return ''
        }
        return value
      } else {
        return this.formValue[this.propsItem.key]
      }
    },
    itemStyle() {
      if (this.propsItem.width) {
        return { width: this.propsItem.width }
      } else {
        return { flex: 1 }
      }
    },
    listener() {
      return {
        input: (res) => {
          const { propsItem } = this
          if (propsItem.outFormat && typeof (propsItem.outFormat) === 'function') {
            this.$emit('input', propsItem.outFormat(res, this.formValue))
          } else {
            this.$emit('input', { [propsItem.key]: res })
          }
          if (propsItem.on && propsItem.on.input) {
            const func = propsItem.on.input
            this.$emit('itemOn', func, res, propsItem.key)
          }
        },
        change: (res) => {
          const { propsItem } = this
          if (propsItem.outFormat && typeof (propsItem.outFormat) === 'function') {
            this.$emit('input', propsItem.outFormat(res, this.formValue))
          } else {
            this.$emit('input', { [propsItem.key]: res })
          }
          if (propsItem.on && propsItem.on.change) {
            const func = propsItem.on.change
            this.$emit('itemOn', func, res, propsItem.key)
          }
        },
        focus: (res) => {
          if (this.propsItem.on && this.propsItem.on.focus) {
            const func = this.propsItem.on.focus
            this.$emit('itemOn', func, res, this.propsItem.key)
          }
        },
        blur: (res) => {
          if (this.propsItem.on && this.propsItem.on.blur) {
            const func = this.propsItem.on.blur
            this.$emit('itemOn', func, this.this.propsItem.key)
          }
        }
      }
    }
  }
}
</script>
