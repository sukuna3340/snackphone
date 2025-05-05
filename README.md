<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Snake Mobile</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background-color: #0d1117;
      color: #fff;
      text-align: center;
    }
    h1 {
      color: #00ff88;
    }
    canvas {
      border: 2px solid #00ff88;
      background-color: #161b22;
      display: block;
      margin: auto;
      max-width: 90vw;
      max-height: 90vw;
    }
    #controls {
      margin-top: 10px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 5px;
    }
    .btn-row {
      display: flex;
      gap: 10px;
    }
    button {
      width: 60px;
      height: 60px;
      font-size: 24px;
      background: #00ff88;
      color: #000;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>🐍 Snake Game</h1>
  <div id="score">Score: 0</div>
  <canvas id="game" width="300" height="300"></canvas>

  <div id="controls">
    <button onclick="setDirection('UP')">⬆️</button>
    <div class="btn-row">
      <button onclick="setDirection('LEFT')">⬅️</button>
      <button onclick="setDirection('DOWN')">⬇️</button>
      <button onclick="setDirection('RIGHT')">➡️</button>
    </div>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

    const box = 15;
    const rows = canvas.height / box;
    const cols = canvas.width / box;

    let snake, food, dir, score, speed = 120;
    let interval;

    function init() {
      snake = [{ x: 5, y: 5 }];
      dir = { x: 1, y: 0 };
      score = 0;
      placeFood();
      document.getElementById("score").innerText = "Score: " + score;
      clearInterval(interval);
      interval = setInterval(update, speed);
    }

    function placeFood() {
      food = {
        x: Math.floor(Math.random() * cols),
        y: Math.floor(Math.random() * rows)
      };
    }

    function update() {
      const head = { x: snake[0].x + dir.x, y: snake[0].y + dir.y };

      // Game over conditions
      if (
        head.x < 0 || head.y < 0 ||
        head.x >= cols || head.y >= rows ||
        snake.some(p => p.x === head.x && p.y === head.y)
      ) {
        alert("Game Over! Final Score: " + score);
        return init();
      }

      snake.unshift(head);

      // Eat food
      if (head.x === food.x && head.y === food.y) {
        score++;
        document.getElementById("score").innerText = "Score: " + score;
        placeFood();
      } else {
        snake.pop();
      }

      draw();
    }

    function draw() {
      ctx.fillStyle = "#161b22";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Draw snake
      ctx.fillStyle = "#00ff88";
      snake.forEach(p => {
        ctx.fillRect(p.x * box, p.y * box, box, box);
      });

      // Draw food
      ctx.fillStyle = "#ff0033";
      ctx.fillRect(food.x * box, food.y * box, box, box);
    }

    function setDirection(d) {
      if (d === "UP" && dir.y !== 1) dir = { x: 0, y: -1 };
      else if (d === "DOWN" && dir.y !== -1) dir = { x: 0, y: 1 };
      else if (d === "LEFT" && dir.x !== 1) dir = { x: -1, y: 0 };
      else if (d === "RIGHT" && dir.x !== -1) dir = { x: 1, y: 0 };
    }

    // Keyboard support (desktop)
    document.addEventListener("keydown", e => {
      if (e.key === "ArrowUp") setDirection("UP");
      if (e.key === "ArrowDown") setDirection("DOWN");
      if (e.key === "ArrowLeft") setDirection("LEFT");
      if (e.key === "ArrowRight") setDirection("RIGHT");
    });

    init();
  </script>
</body>
</html>
