<!DOCTYPE html>
<html>
<head>
    <title>Simple Platformer</title>
    <style>
        body { margin: 0; overflow: hidden; background: #222; font-family: sans-serif; }
        canvas { display: block; background: #87CEEB; margin: 20px auto; border: 4px solid #fff; }
        .info { color: white; text-align: center; }
    </style>
</head>
<body>
    <div class="info">Стрелки: Движение и Прыжок. Цель: Желтый квадрат.</div>
    <canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
canvas.width = 800; canvas.height = 400;

// Настройки игрока
let player = { x: 50, y: 300, w: 30, h: 30, dx: 0, dy: 0, grounded: false, jumpPower: 12, speed: 5 };
let gravity = 0.6;
let keys = {};

// Платформы и цель
let platforms = [
    { x: 0, y: 380, w: 800, h: 20 },   // Пол
    { x: 200, y: 300, w: 100, h: 15 },
    { x: 400, y: 220, w: 100, h: 15 },
    { x: 600, y: 140, w: 100, h: 15 }
];
let goal = { x: 640, y: 100, w: 20, h: 20 };

window.addEventListener('keydown', e => keys[e.code] = true);
window.addEventListener('keyup', e => keys[e.code] = false);

function update() {
    // Управление
    if (keys['ArrowLeft']) player.dx = -player.speed;
    else if (keys['ArrowRight']) player.dx = player.speed;
    else player.dx = 0;

    if (keys['ArrowUp'] && player.grounded) {
        player.dy = -player.jumpPower;
        player.grounded = false;
    }

    // Физика
    player.dy += gravity;
    player.x += player.dx;
    player.y += player.dy;
    player.grounded = false;

    // Проверка столкновений с платформами
    platforms.forEach(p => {
        if (player.x < p.x + p.w && player.x + player.w > p.x &&
            player.y + player.h > p.y && player.y + player.h < p.y + player.dy + 10) {
            player.grounded = true;
            player.dy = 0;
            player.y = p.y - player.h;
        }
    });

    // Проверка победы
    if (player.x < goal.x + goal.w && player.x + player.w > goal.x &&
        player.y < goal.y + goal.h && player.y + player.h > goal.y) {
        alert("Победа!");
        player.x = 50; player.y = 300; // Рестарт
    }

    // Падение в бездну
    if (player.y > canvas.height) { player.x = 50; player.y = 300; }
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Рисуем платформы
    ctx.fillStyle = '#654321';
    platforms.forEach(p => ctx.fillRect(p.x, p.y, p.w, p.h));

    // Рисуем цель
    ctx.fillStyle = '#FFD700';
    ctx.fillRect(goal.x, goal.y, goal.w, goal.h);

    // Рисуем игрока
    ctx.fillStyle = '#FF4500';
    ctx.fillRect(player.x, player.y, player.w, player.h);
}

function loop() {
    update();
    draw();
    requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>


__________________________________________________________________________________________________________________________________________________________________________________________________________________________



<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Платформер</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { background: #87CEEB; display: block; margin: auto; }
    </style>
</head>
<body>
<canvas id="gameCanvas"></canvas>
<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
canvas.width = 800;
canvas.height = 400;

let player = { x: 50, y: 300, width: 40, height: 40, dy: 0, jump: false };
let gravity = 0.5;
let platforms = [{ x: 0, y: 350, width: 800, height: 10 }];
let keys = {};

function update() {
    if (keys['ArrowRight']) player.x += 5;
    if (keys['ArrowLeft']) player.x -= 5;
    if (player.jump) {
        player.dy += gravity;
        player.y += player.dy;
        if (player.y + player.height >= 350) {
            player.y = 350;
            player.dy = 0;
            player.jump = false;
        }
    }

    if (player.y < 350) player.dy += gravity;

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    draw();
    requestAnimationFrame(update);
}

function draw() {
    ctx.fillStyle = 'red';
    ctx.fillRect(player.x, player.y, player.width, player.height);
    ctx.fillStyle = 'green';
    platforms.forEach(p => ctx.fillRect(p.x, p.y, p.width, p.height));
}

document.addEventListener('keydown', (e) => {
    keys[e.key] = true;
    if (e.key === 'ArrowUp' && player.y === 350) {
        player.jump = true;
        player.dy = -10;
    }
});

document.addEventListener('keyup', (e) => {
    keys[e.key] = false;
});

update();
</script>
</body>
</html>
