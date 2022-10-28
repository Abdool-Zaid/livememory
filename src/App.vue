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
let startTime;
let moves = 0;
let highScore
let GameLevel
function generateRandomColor() {
  let letters = "0123456789ABCDEF";
  let color = "#";
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}
let checkGameState = () => {
  let opacity = 0;
  compareValues = [];
  document.querySelectorAll(".gameImage").forEach((tile) => {
    opacity = eval(opacity + parseInt(tile.style.opacity));
  });
  if (opacity == GameLevel * 2) {
    navOffCanvas.innerHTML=`level score ${Math.floor(((Date.now()-startTime)/100)/moves)}`
    field.innerHTML = "";
    tileArray = [];
    randomArray = [];
    compareValues = [];
    moves = 0;
    GameLevel++;
    difficultyLevel.value=GameLevel
    gameButton.innerHTML=`next level`
  }
};
let evaluating = () => {
  if (compareValues[0].value == compareValues[1].value) {
    alert("correct");
    checkGameState();
  } else {
    alert("incorrect");
    compareValues.forEach((tile) => {
      document.querySelector("#" + tile.id).style = `
      opacity: 0;
      `;
    });
    checkGameState();
  }
};
let styleTiles = () => {
  document.querySelectorAll(".gameTile").forEach((tile) => {
    tile.style = `
    background-color: ${generateRandomColor()};
    width:100px;
    aspect-ratio: 1;
-webkit-user-drag: none;
-khtml-user-drag: none;
-moz-user-drag: none;
-o-user-drag: none;
user-drag: none;
    `;
  });
  document.querySelectorAll(".gameImage").forEach((tile) => {
    tile.style = `
    opacity: 0;
    margin: 1%;
    `;
  });
};
let index = () => {
  let index = Math.floor(tileArray.length * Math.random());
  randomArray.push(tileArray.splice(index, 1)[0]);
};

let startNewgame = () => {
  startTime = Date.now();
  field.innerHTML = "";
  GameLevel = parseInt(difficultyLevel.value);
  for (i = 0; i < GameLevel; i++) {
    tileArray.push({ src: imageSRC + i, id: "gameTilez1z" + i });
    tileArray.push({ src: imageSRC + i, id: "gameTilez2z" + i });
  }
  for (i = 0; i < GameLevel * 2; i++) {
    index();
  }
  for (i = 0; i < randomArray.length; i++) {
    field.innerHTML += `
  <div class="gameTile">
  <img src="${randomArray[i].src}" class="gameImage" id="${randomArray[i].id}" >
  </div>
  `;
    styleTiles();
    document.querySelectorAll(".gameImage").forEach((tile) => {
      tile.addEventListener("click", (e) => {
        moves++;
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
  GameLevel = parseInt(difficultyLevel.value);
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
  <div  id="gameBar">
    <input type="number" id="difficultyLevel" value="1" />
    <button class="btn" @click="startNewgame()" id="gameButton">new game</button>
  </div>

  <nav-bar></nav-bar>
  <div
    id="field"
    class="d-flex flex-wrap justify-content-evenly align-items-center overflow-scroll"
  ></div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
::-webkit-scrollbar {
  display: none;
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
#gameBar{
  position: fixed;
  top: 0;
  left: 50%;
  margin: 1%;
transform: translate(-50%);
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
