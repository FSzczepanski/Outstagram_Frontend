
<template>

    <MainPage />

</template>
<script>
import MainPage from './MainPage.vue'

export default {
  name: 'App',
  components: {
    MainPage
  }
}
</script>

<style>

body {
  
  position: relative;
  width: 1920px;
  height: 880px;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  background: linear-gradient(252.44deg, #5FD2C4 10%, rgba(0, 167, 255, 0.67) 99.99%, rgba(139, 21, 124, 0) 100%, #672991 100%);
  filter: drop-shadow(0px 4px 4px rgba(0, 0, 0, 0.25));
  border-radius: 40px;
  color: white;
  max-width: 100%;
  
}
.container {
  
  max-width: 100%;
  margin: 30px auto;
  overflow: auto;
  height:auto;
  align-content: center;
  padding: auto;
  position: fixed;
  
}
.element::-webkit-scrollbar {
  display: none;
}
.element {
  display: flex;
justify-content: center;
  margin: 30px auto;
  overflow: auto;
  width: 560px;
  background: rgba(250,250,250,0.5);
  height: 300px;
  border-radius: 49px;
  font-size: 30px;
  
  margin: 30px;
}
.elementFloat{
  float:left;
}
</style>
