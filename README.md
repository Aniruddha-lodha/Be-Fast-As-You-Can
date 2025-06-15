# Be-Fast-As-You-Can
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

let player = {
  x: 100,
  y: 300,
  width: 40,
  height: 40,
  yVelocity: 0,
  jumpPower: -12,
  grounded: false,
};

let gravity = 0.7;
let speed = 4;
let coins = [];
let score = 0;

document.addEventListener("keydown", (e) => {
  if (e.code === "Space" && player.grounded) {
    player.yVelocity = player.jumpPower;
    player.grounded = false;
  }
});

function spawnCoin() {
  coins.push({
    x: canvas.width,
    y: 280 + Math.random() * -100,
    width: 20,
    height: 20,
  });
}

setInterval(spawnCoin, 1500);

function update() {
  player.yVelocity += gravity;
  player.y += player.yVelocity;

  if (player.y + player.height >= canvas.height - 40) {
    player.y = canvas.height - 40 - player.height;
    player.yVelocity = 0;
    player.grounded = true;
  }

  coins.forEach((coin, index) => {
    coin.x -= speed;
    if (
      player.x < coin.x + coin.width &&
      player.x + player.width > coin.x &&
      player.y < coin.y + coin.height &&
      player.y + player.height > coin.y
    ) {
      coins.splice(index, 1);
      score++;
    }
  });
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Ground
  ctx.fillStyle = "#333";
  ctx.fillRect(0, canvas.height - 40, canvas.width, 40);

  // Player
  ctx.fillStyle = "yellow";
  ctx.fillRect(player.x, player.y, player.width, player.height);

  // Coins
  ctx.fillStyle = "gold";
  coins.forEach((coin) => {
    ctx.beginPath();
    ctx.arc(coin.x, coin.y, coin.width / 2, 0, Math.PI * 2);
    ctx.fill();
  });

  // Score
  ctx.fillStyle = "#fff";
  ctx.font = "20px Arial";
  ctx.fillText(`Coins: ${score}`, 10, 30);
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}

loop();
