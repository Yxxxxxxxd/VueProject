<template>
  <div id="app">
    <div v-text="msg"></div>
    <input v-model="newItem" @keyup.enter="doPushItem">
    <ul>
      <li v-for="item in items" :class="{finished:item.isFinished}" v-on:click="doThisClick">{{item.label}}</li>
    </ul>
    <p>child words to :{{childWords}}</p>
    <components-a msgFromFather="you die!" v-on:child-tell-me-somthing="listenToMyBoy"></components-a>
  </div>
</template>

<script>
  import Store from './store'
  import componentsA from './componentsA'
  export default {
    name: 'HelloWorld',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        items:Store.fetch(),
        newItem:'',
        childWords:''
      }
    },
    components:{
      componentsA
    },
    watch:{
        items:{
          handler:function(val,oldVal){
            console.log(val,oldVal)
          }  ,
          deep:true
        }
    },
    events:{
      'child-tell-me-somthing':function (msg) {
        this.childWords = msg;
      }
    },
    methods:{
        doThisClick:function () {
           console.log(this.msg)
        },
        doPushItem:function () {
            this.items.push({
              label:this.newItem,
              isFinished:true
            });
            this.newItem = '';
            Store.save(this.items);
        },
      listenToMyBoy:function (msg) {
        this.childWords = msg;
      }
    }
  }
</script>

<style>
.finished{
  text-decoration: underline;
}
</style>
