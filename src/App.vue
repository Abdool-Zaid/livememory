<script setup>
import NavBar from "./components/NavBar.vue";
import { onMounted } from "vue";
components: { NavBar }
let imageSRC='https://picsum.photos/100/100?random='
let i
let compareValues=[]
let checkResults=(src)=>{
  console.log('checking')
// if(compareValues.length<2){
//   compareValues.push(src)
// }else if(compareValues.length=2){
//   if(compareValues[0]==compareValues[1]){
//     console.log('correct')
//   compareValues=[]
//   }
// else{
//   console.log('incorrect')
//   compareValues=[]
// }
// }
}
onMounted(() => {
  //   <img src="https://picsum.photos/200/300?random=1">
// <img src="https://picsum.photos/200/300?random=2">
// field.innerHTML='
for ( i = 0; i < 10; i++) {
field.innerHTML+=`
<img src="${imageSRC+i}" onclick="checkResults(${i})">
<img src="${imageSRC+i}" onclick="checkResults(${i})">
`  
}
})
  </script>
<template>
<button class="btn " type="button" data-bs-toggle="offcanvas" id="navOffCanvas" data-bs-target="#offcanvasExample" aria-controls="offcanvasExample">
Memory tester</button>
<nav-bar></nav-bar>
<div id="field" class=""></div>

</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  width: 100vw;
  height: 100vh;
}
#navOffCanvas{
  position: fixed;
  top:0;
  left:0;
  margin: 1%;
}
#field{
  position: fixed;
  left: 50%;
  top: 50%;
 transform: translate(-50%,-50%);
  width:70vw;
  height: 80vh;
  background-color: green;
}
</style>
