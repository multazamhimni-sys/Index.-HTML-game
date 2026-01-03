<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Catch the Box - Game Sederhana</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      background: #111;
      font-family: Arial, Helvetica, sans-serif;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }
    #game-container {
      position: relative;
      width: 400px;
      height: 600px;
      background: linear-gradient(to bottom, #0f0c29, #302b63, #24243e);
      border: 4px solid #444;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 0 30px rgba(100, 200, 255, 0.4);
    }
    #player {
      position: absolute;
      bottom: 20px;
      width: 80px;
      height: 40px;
      background: #00d4ff;
      border-radius: 10px;
      border: 3px solid white;
      transition: left 0.08s;
      box-shadow: 0 0 15px #00d4ff;
    }
    .box {
      position: absolute;
      width: 40px;
      height: 40px;
      background: #ffdd00;
      border: 3px solid #ff6a00;
      border-radius: 8px;
      box-shadow: 0 0 12px #ffdd00;
    }
    #score {
      position: absolute;
      top: 15px;
      left: 20px;
      font-size: 28px;
      font-weight: bold;
      text-shadow: 0 0 10px #fff;
    }
    #game-over {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 48px;
      color: #ff0044;
      text-shadow: 0 0 20px #ff0044;
      display: none;
      text-align: center;
    }
    #restart {
      margin-top: 20px;
      padding: 12px 30px;
      font-size: 22px;
      background: #00d4ff;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-weight: bold;
      display: none;
    }
  </style>
</head>
<body>

<div id="game-container">
  <div id="score">Score: 0</div>
  <div id="player"></div>
  <div id="game-over">
    GAME OVER!<br>
    <button id="restart">Main Lagi</button>
  </div>
</div>

<script>
const container = document.getElementById('game-container');
const player = document.getElementById('player');
const scoreElement = document.getElementById('score');
const gameOverScreen = document.getElementById('game-over');
const restartBtn = document.getElementById('restart');

let score = 0;
let gameActive = true;
let playerX = 160; // posisi awal tengah

// kontrol pemain
document.addEventListener('keydown', (e) => {
  if (!gameActive) return;
  
  if (e.key === 'ArrowLeft' || e.key === 'a') {
    playerX = Math.max(0, playerX - 25);
  }
  if (e.key === 'ArrowRight' || e.key === 'd') {
    playerX = Math.min(320, playerX + 25);
  }
  player.style.left = playerX + 'px';
});

// kontrol sentuh (untuk HP)
let touchStartX = 0;
container.addEventListener('touchstart', e => {
  touchStartX = e.touches[0].clientX;
});
container.addEventListener('touchmove', e => {
  if (!gameActive) return;
  const touchX = e.touches[0].clientX;
  const diff = touchX - touchStartX;
  playerX += diff / 3;
  playerX = Math.max(0, Math.min(320, playerX));
  player.style.left = playerX + 'px';
  touchStartX = touchX;
});

// buat kotak jatuh
function createBox() {
  if (!gameActive) return;

  const box = document.createElement('div');
  box.classList.add('box');
  
  const randomX = Math.floor(Math.random() * 360);
  box.style.left = randomX + 'px';
  box.style.top = '-50px';
  
  container.appendChild(box);

  let boxY = -50;
  const speed = 4 + score * 0.15; // makin tinggi score makin cepat

  const fall = setInterval(() => {
    if (!gameActive) {
      clearInterval(fall);
      box.remove();
      return;
    }

    boxY += speed;
    box.style.top = boxY + 'px';

    // cek tabrakan
    if (boxY > 520 && boxY < 560) {
      const playerLeft = playerX;
      const playerRight = playerX + 80;
      const boxLeft = parseInt(box.style.left);
      const boxRight = boxLeft + 40;

      if (boxRight > playerLeft && boxLeft < playerRight) {
        score++;
        scoreElement.textContent = `Score: ${score}`;
        box.remove();
        clearInterval(fall);
        // efek suara sederhana
        new Audio('https://assets.codepen.io/6058/coin-drop.mp3').play().catch(()=>{});
      }
    }

    // kotak keluar bawah → game over
    if (boxY > 650) {
      gameActive = false;
      gameOverScreen.style.display = 'block';
      restartBtn.style.display = 'block';
      clearInterval(fall);
      box.remove();
    }
  }, 20);

  // buat kotak baru lagi setelah beberapa waktu
  setTimeout(createBox, 800 - score * 20);
}

// restart game
function restart() {
  score = 0;
  scoreElement.textContent = 'Score: 0';
  gameActive = true;
  gameOverScreen.style.display = 'none';
  restartBtn.style.display = 'none';
  playerX = 160;
  player.style.left = '160px';
  
  // hapus semua kotak yang mungkin tertinggal
  document.querySelectorAll('.box').forEach(b => b.remove());
  
  createBox();
}

restartBtn.addEventListener('click', restart);

// mulai game
createBox();

// klik di layar juga bisa gerakin pemain (untuk HP)
container.addEventListener('click', (e) => {
  if (!gameActive) return;
  const rect = container.getBoundingClientRect();
  playerX = e.clientX - rect.left - 40;
  playerX = Math.max(0, Math.min(320, playerX));
  player.style.left = playerX + 'px';
});
</script>

</body>
</html>
