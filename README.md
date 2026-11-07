
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Diwali Ghost Tap 🪔</title>
<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 18px;
  color: #fff7e6;
  font-family: Arial, sans-serif;
  background: radial-gradient(circle at top, #45205a, #160d27 60%, #090713);
}

main {
  width: min(100%, 460px);
  text-align: center;
}

h1 {
  color: #ffd36b;
  font-size: 32px;
}

.sub { color: #d9c6ec; }

.stats {
  display: flex;
  gap: 8px;
  margin-bottom: 12px;
}

.stat {
  flex: 1;
  padding: 11px 6px;
  background: #251735;
  border: 1px solid #68477e;
  border-radius: 12px;
}

.stat b {
  display: block;
  color: #ffd36b;
  font-size: 21px;
}

.arena {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  padding: 13px;
  background: #21152f;
  border: 1px solid #68477e;
  border-radius: 18px;
}

.hole {
  aspect-ratio: 1;
  border: 1px solid #704e83;
  border-radius: 15px;
  background: #38244b;
  font-size: 45px;
  cursor: pointer;
}

.hole.active {
  background: #67421d;
}

#start {
  width: 100%;
  margin-top: 14px;
  padding: 14px;
  border: none;
  border-radius: 13px;
  background: linear-gradient(90deg, #ffbf42, #ffe18b);
  color: #29152c;
  font-size: 17px;
  font-weight: bold;
  cursor: pointer;
}

#message {
  min-height: 26px;
  color: #f4d8ff;
}

.rules {
  color: #cdbbd9;
  font-size: 13px;
}

footer {
  margin-top: 20px;
  color: #a995b9;
  font-size: 12px;
  line-height: 1.8;
}

.developer {
  color: #ffd36b;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 2px;
}
</style>
</head>

<body>
<main>

<h1>🪔 Diwali Ghost Tap 👻</h1>
<p class="sub">A tiny festival game. Catch the ghosts!</p>

<div class="stats">
  <div class="stat">⭐ Score<b id="score">0</b></div>
  <div class="stat">⏱️ Time<b id="time">20</b></div>
  <div class="stat">🏆 Best<b id="best">0</b></div>
</div>

<section class="arena">
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
  <button class="hole">🌑</button>
</section>

<button id="start">🎆 START GAME</button>

<p id="message">You have 20 seconds. Ready?</p>

<p class="rules">
👻 Tap a ghost to score +1 point.<br>
🪔 Diyas are safe, but don't tap empty tiles!
</p>

<footer>
Made for the Festival of Lights ✨
<br>
<span class="developer">DEVELOPER: PRATHAM NAYEK</span>
</footer>

</main>

<script>
const tiles = document.querySelectorAll('.hole');

const scoreEl = document.getElementById('score');
const timeEl = document.getElementById('time');
const bestEl = document.getElementById('best');
const message = document.getElementById('message');
const startBtn = document.getElementById('start');

let score = 0;
let time = 20;
let active = -1;
let kind = '';
let running = false;
let spawnTimer;
let clockTimer;

let best = Number(
  localStorage.getItem('diwaliGhostTapBest') || 0
);

bestEl.textContent = best;

function clearBoard() {
  tiles.forEach(tile => {
    tile.textContent = '🌑';
    tile.classList.remove('active');
  });
  active = -1;
  kind = '';
}

function showNext() {
  clearBoard();

  active = Math.floor(Math.random() * tiles.length);
  kind = Math.random() < 0.78 ? '👻' : '🪔';

  tiles[active].textContent = kind;
  tiles[active].classList.add('active');
}

function finish() {
  running = false;

  clearInterval(spawnTimer);
  clearInterval(clockTimer);

  clearBoard();

  if (score > best) {
    best = score;
    bestEl.textContent = best;
    localStorage.setItem(
      'diwaliGhostTapBest',
      String(best)
    );
  }

  message.textContent =
    `🎉 Time's up! You scored ${score} points.`;

  startBtn.textContent = '🔄 PLAY AGAIN';
}

function startGame() {
  if (running) return;

  score = 0;
  time = 20;
  running = true;

  scoreEl.textContent = score;
  timeEl.textContent = time;

  message.textContent = 'Catch those ghosts!';
  startBtn.textContent = 'GAME IN PROGRESS...';

  showNext();

  spawnTimer = setInterval(showNext, 720);

  clockTimer = setInterval(() => {
    time--;
    timeEl.textContent = time;

    if (time <= 0) {
      finish();
    }
  }, 1000);
}

tiles.forEach((tile, index) => {
  tile.addEventListener('click', () => {
    if (!running || index !== active) return;

    if (kind === '👻') {
      score++;
      scoreEl.textContent = score;
      message.textContent = '✨ Ghost caught! +1 point';
    } else {
      message.textContent = '🪔 Friendly diya! No points.';
    }

    showNext();
  });
});

startBtn.addEventListener('click', startGame);
</script>

</body>
</html>
