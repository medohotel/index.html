<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>QR Snake Game</title>

<style>

body{
text-align:center;
font-family:Arial;
background:white;
color:red;
}

canvas{
background:white;
border:4px solid red;
margin-top:10px;
}

#score{
font-size:20px;
margin-top:10px;
}

#leaderboard{
margin-top:20px;
width:200px;
margin-left:auto;
margin-right:auto;
}

button{
width:70px;
height:70px;
font-size:25px;
margin:5px;
border:none;
background:red;
color:white;
border-radius:10px;
}

</style>
</head>

<body>

<img src="images/logo.png" width="120">

<h1>QR Snake Game</h1>

<div id="score">
Score: <span id="scoreValue">0</span>
</div>

<canvas id="game" width="400" height="400"></canvas>

<div>
<br>
<button onclick="setDir('UP')">⬆️</button>
<br>
<button onclick="setDir('LEFT')">⬅️</button>
<button onclick="setDir('DOWN')">⬇️</button>
<button onclick="setDir('RIGHT')">➡️</button>
</div>

<h2>Leaderboard</h2>

<div id="leaderboard"></div>

<script>

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let box = 20;
let snake = [{x:200,y:200}];

let food = {
x:Math.floor(Math.random()*20)*box,
y:Math.floor(Math.random()*20)*box
};

let direction;
let score = 0;

function setDir(dir){
direction = dir;
}

function draw(){

ctx.fillStyle="white";
ctx.fillRect(0,0,400,400);

for(let i=0;i<snake.length;i++){
ctx.fillStyle = i==0 ? "red" : "#cc0000";
ctx.fillRect(snake[i].x,snake[i].y,box,box);
}

ctx.fillStyle="#990000";
ctx.fillRect(food.x,food.y,box,box);

let snakeX = snake[0].x;
let snakeY = snake[0].y;

if(direction=="LEFT") snakeX -= box;
if(direction=="UP") snakeY -= box;
if(direction=="RIGHT") snakeX += box;
if(direction=="DOWN") snakeY += box;

if(snakeX==food.x && snakeY==food.y){

score++;
document.getElementById("scoreValue").innerText = score;

food={
x:Math.floor(Math.random()*20)*box,
y:Math.floor(Math.random()*20)*box
};

}else{

snake.pop();

}

let newHead = {x:snakeX,y:snakeY};

if(
snakeX<0 || snakeX>=400 ||
snakeY<0 || snakeY>=400 ||
collision(newHead,snake)
){

gameOver();

}

snake.unshift(newHead);

}

function collision(head,array){

for(let i=0;i<array.length;i++){
if(head.x==array[i].x && head.y==array[i].y){
return true;
}
}

return false;

}

function gameOver(){

let name = prompt("Game Over! Name eingeben:");

let scores = JSON.parse(localStorage.getItem("snakeLeaderboard")) || [];

scores.push({name:name,score:score});

scores.sort((a,b)=>b.score-a.score);

scores = scores.slice(0,5);

localStorage.setItem("snakeLeaderboard",JSON.stringify(scores));

showLeaderboard();

location.reload();

}

function showLeaderboard(){

let scores = JSON.parse(localStorage.getItem("snakeLeaderboard")) || [];

let html="";

scores.forEach((s,i)=>{
html += (i+1)+". "+s.name+" - "+s.score+"<br>";
});

document.getElementById("leaderboard").innerHTML = html;

}

showLeaderboard();

let game = setInterval(draw,100);

</script>

</body>
</html>
