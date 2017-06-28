<template>
    <div class="hello">
        <h2>{{ msg }}</h2>
        <h3>{{msgfromfather}}</h3>
        <button type="button" @click="clickMe">点击我！11！</button>
        <button type="button" @click="tellFather">告诉爸爸</button>
    </div>
</template>

<script>
    export default {
      data () {
        return {
            msg: '一个来自组件A的msg',
            boySay:"FUCK YOU!!!"
        }
      },
      props:["msgfromfather"],
      methods:{
            clickMe:function(){
                console.log(this.msgfromfather)
            },
            tellFather:function(){
//                debugger;
                console.log(this.boySay)
                this.$emit('child-tell-me',this.boySay);  //、$emit()触发自定义的事件
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3, h2 {
    font-weight: normal;
    font-size: 45px;
    color: #42b983;    
}
    .hello{
        text-align: center;
    }

a {
  color: #42b983;
}
</style>
