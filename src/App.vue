<template>
  <div id="app">
    <div class="head"></div>
    <showAnimal></showAnimal>

<!--    <sendAnimal></sendAnimal>-->
  </div>
</template>

<script>

import showAnimal from './components/showAnimal'
import sendAnimal from './components/sendAnimal'


export default {
  name: 'app',
  data(){
    return{
    }
  },
  methods:{


  },
  components:{
    showAnimal,
    sendAnimal
  }
}
</script>

<style>
#app {
  width: 100%;
  height: 100%;
  margin: 0;
  background: radial-gradient(circle, #1CB5E0, #000046 100%)
}
  .head{
    width: 100%;
    height: 100px;
  }

</style>
