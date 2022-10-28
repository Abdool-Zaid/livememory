<script setup>
import NavBar from "./components/NavBar.vue";
import { onMounted } from "vue";
components: {
  NavBar;
}
let imageSRC = "https://picsum.photos/100/100?random=";
let i;
let tileArray = [];
  let randomArray = [];
let compareValues = [];
function generateRandomColor() {
  let letters = "0123456789ABCDEF";
  let color = "#";
  // Math.floor(Math.random()*0xffffff).toString(16)
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}

let evaluating = () => {
  if (compareValues[0].value == compareValues[1].value) {
    alert('correct')
    compareValues = [];
  } else {
    alert('incorrect')
    compareValues.forEach((tile) => {
      document.querySelector("#" + tile.id).style = `
      opacity: 0;
      `;
    });
    compareValues = [];
  }
};
let styleTiles = () => {
  document.querySelectorAll(".gameTile").forEach((tile) => {
    tile.style = `
    background-color: ${generateRandomColor()};
    width:100px;
    aspect-ratio: 1;
    
    `;
  });
  document.querySelectorAll(".gameImage").forEach((tile) => {
    tile.style = `
    opacity: 0;
    margin: 1%;
    `;
  });
};
let index =()=>{
  let index=Math.floor(tileArray.length * Math.random());
randomArray.push(tileArray.splice(index,1)[0])
} 

let startNewgame = () => {
  field.innerHTML = null;
  let GameLevel = difficultyLevel.value;
 
  for (i = 0; i < GameLevel; i++) {
    tileArray.push({ 'src': imageSRC + i, 'id': "gameTilez1z" + i });
    tileArray.push({ 'src': imageSRC + i, 'id': "gameTilez2z" + i });
  }
  for(i=0;i<(GameLevel*2);i++){
index()
  }
  console.log(randomArray)
  for (i=0;i<randomArray.length;i++){
    field.innerHTML += `
  <div class="gameTile">
  <img src="${randomArray[i].src}" class="gameImage" id="${randomArray[i].id}">
  </div>
  `;
  styleTiles();
    document.querySelectorAll(".gameImage").forEach((tile) => {
      tile.addEventListener("click", (e) => {
        console.log("clicked");
        document.querySelector("#" + e.target.id).style = `
  opacity: 1;
  `;
        let src = e.target.id;
        src = src.split("z");
        if (compareValues.length < 1) {
          compareValues.push({ value: src[2], id: e.target.id });
        } else if (compareValues.length < 2) {
          compareValues.push({ value: src[2], id: e.target.id });
          evaluating();
        }
      });
    });
  }
};

onMounted(() => {
});
</script>
<template>
  <button
    class="btn"
    type="button"
    data-bs-toggle="offcanvas"
    id="navOffCanvas"
    data-bs-target="#offcanvasExample"
    aria-controls="offcanvasExample"
  >
    Memory tester
  </button>
  <div class="position-absolute top-50 start-50">
    <input type="number" id="difficultyLevel" value="10" min="5" max="15" />
    <button class="btn" @click="startNewgame()">new game</button>
  </div>

  <nav-bar></nav-bar>
  <div
    id="field"
    class="d-flex flex-wrap justify-content-evenly align-items-center"
  ></div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  width: 100vw;
  height: 100vh;
}
#navOffCanvas {
  position: fixed;
  top: 0;
  left: 0;
  margin: 1%;
}
#field {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 70vw;
  height: 80vh;
  background-color: green;
}
</style>
