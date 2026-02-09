
const canvas = document.getElementById("tree");
const ctx = canvas.getContext("2d");

canvas.width = 400;
canvas.height = 450;

// Cargar la foto
const img = new Image();
img.src = "foto.jpg";

img.onload = () => {
  animate();
};

// Tronco
function drawTrunk() {
  ctx.fillStyle = "#8B4513";
  ctx.fillRect(185, 270, 30, 110);
}

// Dibujo de la foto circular
function drawPhoto() {
  ctx.save();
  ctx.beginPath();
  ctx.arc(200, 165, 48, 0, Math.PI * 2);
  ctx.closePath();
  ctx.clip();
  ctx.drawImage(img, 152, 117, 96, 96);
  ctx.restore();

  // Marco rosado
  ctx.strokeStyle = "#ff4da6";
  ctx.lineWidth = 4;
  ctx.beginPath();
  ctx.arc(200, 165, 50, 0, Math.PI * 2);
  ctx.stroke();
}

// Corazones
function drawHeart(x, y, size, color) {
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.moveTo(x, y);
  ctx.bezierCurveTo(x - size, y - size, x - size * 2, y + size / 2, x, y + size * 2);
  ctx.bezierCurveTo(x + size * 2, y + size / 2, x + size, y - size, x, y);
  ctx.fill();
}

// Corazones animados
let hearts = [];

for (let i = 0; i < 160; i++) {
  hearts.push({
    x: 200 + (Math.random() - 0.5) * 210,
    y: 220 + (Math.random() - 0.5) * 170,
    size: Math.random() * 6 + 4,
    pulse: Math.random() * Math.PI * 2,
    speed: Math.random() * 0.03 + 0.01,
    color: ["#ff1a75", "#ff4da6", "#ff66b2", "#ff3385"][
      Math.floor(Math.random() * 4)
    ]
  });
}

// Animación
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  hearts.forEach(h => {
    h.pulse += h.speed;
    drawHeart(h.x, h.y, h.size + Math.sin(h.pulse) * 1.5, h.color);
  });

  drawPhoto();
  drawTrunk();

  requestAnimationFrame(animate);
}
