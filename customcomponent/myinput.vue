<template>
  <div class="myinput">
    <el-input v-bind="$attrs" v-model="valueData"></el-input>
    <el-button type="text" size="mini" style="marginLeft: 1em" @click="reset">重置为1</el-button>
  </div>
</template>
<script>
export default {
  props: {
    value: ''
  },
  /*
  * 内部手动emit input事件，或绑定$listeners 接收el-input 内部emit 事件
  */
  watch: {
    valueData(res){
      this.$emit('input',res)
    }
  },
  data(){
    return {
      valueData: this.value
    }
  },
  methods:{
    reset(){
      this.valueData='1'
    }
  }
}
</script>
<style scoped>
  .myinput{
    display: flex;
  }
</style>