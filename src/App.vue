<template>
  <div id="app">
    <div id="nav">
    </div>
    <router-view/>
  </div>
</template>

<style lang="stylus" scoped>
    *{
        margin: 0;
        padding 0;
    }
    #app {
        width: 100%;
        height: 100%;
        background: radial-gradient(circle, #1CB5E0, #000046 100%)
    }
    .head{
        width: 100%;
        height: 100px;
    }

</style>
