<script setup lang="ts">


import { log } from 'console';
import { ref } from 'vue';
const visible = ref(true);
let  snakeLength = ref(4)
let controllerIsCliked = false
let buttonValue = ''
let count = 0
let lastChosenDirection 
let W = ref(false)
let A = ref(false)
let S = ref(false)
let D = ref(false)
let inverse = []
function change(value  ){
buttonValue = value
 lastChosenDirection = value

  controllerIsCliked = true
}
let arrayToExpandSnake = []
let arrayToAddAllIndexes = []
function play(){
  //let snakeFood = Math.floor(Math.random() * 400)
  let snakeFood = 194
  let squareClass = new SquareHTMLelement()
  squareClass.number = snakeFood
  squareClass.additionalArg = 0
 let  snakeFoodSquare = squareClass.GetSquare()
   snakeFoodSquare.style.backgroundColor = 'red';


visible.value = false
  //let number = Math.floor(Math.random() * 400)
  let number = 200
 let square
for (let i=0;i< 4;i++){

  let squareClass = new SquareHTMLelement()
  squareClass.number = number
  squareClass.additionalArg = i
  
  square  =  squareClass.GetSquareMinus()
  
  square.style.backgroundColor = 'yellow';
  arrayToAddAllIndexes.push(number - i )
   if (i == 3){
    arrayToExpandSnake.push(number - 4)
   }

}

function moveSnake(){

  let buttonValuesObject = {
    W: -1,
    A: -20,
    S: 1,
    D: 20,
  }
  let invertButton = {
    W: 'S',
    A: 'D',
    S: 'W',
    D: 'A'

  }
  let lastarrayToExpandSnake
  lastarrayToExpandSnake = arrayToExpandSnake[arrayToExpandSnake.length - 1]

  if (controllerIsCliked == false){

S.value = true
buttonValue = 'W'
  let  squareClass = new SquareHTMLelement()
  squareClass.number = lastarrayToExpandSnake
  squareClass.additionalArg = 0
  let squareHtml = squareClass.GetSquare()
  arrayToExpandSnake.push(lastarrayToExpandSnake - 1)
 square = squareHtml
  square.style.backgroundColor = 'yellow'
  arrayToAddAllIndexes.push(lastarrayToExpandSnake )
} else{

   inverse.push(invertButton[buttonValue]); 
   S.value = false

  let stupidArray = {
    W,
    A,
    S,
    D,}
  if (inverse.length > 2){
    inverse.shift()
  }

  stupidArray[inverse[0]].value = false 
 //cases when snake goes out of game bounds 
if( arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1] - 20 <= 0){
  buttonValuesObject.A = 381

if (arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1 ]  == 1){
  buttonValuesObject.W = 399
}

}
if( arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1] + 20 >= 401){
  buttonValuesObject.D = -379
  if (arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1 ]  == 400){
  buttonValuesObject.S = -399
}
}




    stupidArray[inverse[inverse.length -1 ]].value = true 
      lastChosenDirection = buttonValuesObject[buttonValue]
      
  let  squareClass = new SquareHTMLelement()
  squareClass.number =  lastarrayToExpandSnake + 1

  squareClass.additionalArg = buttonValuesObject[buttonValue]
   square = squareClass.GetSquare( )
   arrayToExpandSnake.push( lastarrayToExpandSnake  + buttonValuesObject[buttonValue])
   arrayToAddAllIndexes.push( lastarrayToExpandSnake  + buttonValuesObject[buttonValue] + 1)
  square.style.backgroundColor = 'yellow'

}



 let  squareClass = new SquareHTMLelement()

  squareClass.number =  arrayToAddAllIndexes[0]

  squareClass.additionalArg = 0
   const elementToDelete = squareClass.GetSquare()
   elementToDelete.style.backgroundColor = 'rgb(201, 204, 184)'
   
   let firstSnakeElementBeforeShift = arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1] 
  

   count +=1
 
  
  let include = arrayToAddAllIndexes.slice(0, arrayToAddAllIndexes.length - 1).includes(arrayToAddAllIndexes[arrayToAddAllIndexes.length -1])
 if (include){
  alert("you've lost!")
  window.location.reload()
}
   if (snakeFood == arrayToAddAllIndexes[arrayToAddAllIndexes.length - 1] ){
    
    
      squareClass = new SquareHTMLelement()
  squareClass.number = firstSnakeElementBeforeShift

  squareClass.additionalArg = 0
   square = squareClass.GetSquare()


    snakeLength.value +=1


   square.style.backgroundColor= 'yellow'

   
   

    snakeFood = Math.floor(Math.random() * 400)
    squareClass = new SquareHTMLelement()
  squareClass.number = snakeFood
  squareClass.additionalArg = 0
   square = squareClass.GetSquare()
   square.style.backgroundColor = 'red'
 
     squareClass = new SquareHTMLelement()

squareClass.number =  arrayToAddAllIndexes[0] 

squareClass.additionalArg = 0
 const elementToDelete = squareClass.GetSquare()
elementToDelete.style.backgroundColor = 'rgb(201, 204, 184)'
  } else{
    arrayToAddAllIndexes.shift()
   
   }
 
   
} 



setInterval(moveSnake, 250)
 
}

 

</script>
<script lang="ts">
class SquareHTMLelement {
  number: number = 0

additionalArg: number = 0

  GetSquare() {
let square
    square  =  document.getElementById(`squares-${this.number + this.additionalArg}`)

    return  square
  }
  GetSquareMinus(){
    let square
    square  =  document.getElementById(`squares-${this.number - this.additionalArg}`)

    return  square
  }
}
</script>
<template>
  <div class="container">
    <button v-if="visible" @click="play" class="playTheGame">  play the game
    </button>
    <div class="layout">
      <div v-for="i in 20" :key="i">
        <div :id="`squares-${(i - 1) * 20 + j}`"   v-for="j in 20" :key="j" class="squares">
          

        </div>
      </div>
      

    </div>
    <div class="rightContentContainer">
    <div class="snakeLength">Snake length: <span> <div> {{ snakeLength }}</div></span></div>
    
      <div  class="controllerButton">
      <button @click="change('W')" :disabled="W"   value="W" class="controlButton">W </button>
<div>
  <button @click="change('A')" :disabled="A"   value="A" class="controlButton">A </button>
  <button @click="change('S')" :disabled="S"  value="S"class="controlButton">S </button>
<button @click="change('D')" :disabled="D"  value="D"class="controlButton">D </button>
</div>
      </div>
    </div>


  </div>
</template>

<style scoped>
.container {
  justify-content: center;
  display: flex;
  flex-direction: row;
  align-items: center;
}

.layout {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.squares {
  border: 1px solid black;
  width: 40px;
  height: 40px;
  color: black;
  margin: 5px;
}
@media screen and (max-width: 1300px) {
  .squares {
  border: 1px solid black;
  width: 10px;
  height: 10px;
  color: black;
  margin: 5px;
}
}
@media screen and (max-width: 1542px) {
  .playTheGame {
  height: 100px;
  width: 100px;

  font-size: 10px;
  font-weight: 700;
  background-color: rgb(201, 204, 184);

  align-items: center;
}
}

@media screen and (max-width: 694px) {
  .rightContentContainer{
  display: flex;
  flex-direction: column;
  width: 75px;
}
}




.playTheGame {
  height: 100%;
  font-size: 25px;
  font-weight: 700;
  background-color: rgb(201, 204, 184);

  align-items: center;
}
.controllerButton{
  
display: flex;
flex-direction: column;
}
.controlButton{
font-size: 70px;
}
.rightContentContainer{
  display: flex;
  flex-direction: column;
}
.snakeLength{
 
  font-size: 25px;
font-weight: 800;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  span{
    color: red;
  }
}
</style>
