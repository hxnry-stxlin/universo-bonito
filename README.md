<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Universo de Palabras Bonitas 💖</title>
  <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Poppins:wght@300;400&display=swap" rel="stylesheet">
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      height: 100%;
      background: radial-gradient(circle at center, #1b0034 0%, #000010 100%);
      font-family: 'Poppins', sans-serif;
      color: white;
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }

    .overlay {
      position: absolute;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      text-align: center;
    }

    h1 {
      font-family: 'Great Vibes', cursive;
      font-size: 48px;
      color: #ffb6c1;
      text-shadow: 0 0 10px #ff80a0;
    }

    .button {
      margin-top: 10px;
      padding: 8px 16px;
      background: #ff5f91;
      border: none;
      border-radius: 20px;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s;
    }

    .button:hover {
      background: #ff2e75;
    }

    .music-control {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.5);
      padding: 6px 12px;
      border-radius: 12px;
      color: white;
      font-size: 14px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <canvas id="canvas"></canvas>
  <div class="overlay">
    <h1>Universo de Palabras Bonitas</h1>
    <button class="button" id="addWord">Agregar palabra ✨</button>
  </div>

  <!-- Música lo-fi suave (de fondo con volumen bajo) -->
  <audio id="bgMusic" loop volume="0.4">
    <source src="https://cdn.pixabay.com/audio/2022/07/30/audio_bfc9bfe9c1.mp3" type="audio/mpeg">
    Tu navegador no soporta audio.
  </audio>

  <div class="music-control" id="musicControl">🔈 Pausar música</div>

  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const words = ['Te amo', 'Mi vida', 'Eres mi todo', 'Mi corazón', 'Siempre tú', 'Mi cielo'];
    let particles = [];

    function resize(){
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resize);
    resize();

    class Particle {
      constructor(text){
        this.text = text;
        this.x = Math.random() * canvas.width;
        this.y = Math.random() * canvas.height;
        this.vx = (Math.random() - 0.5) * 1.5;
        this.vy = (Math.random() - 0.5) * 1.5;
        this.size = 24 + Math.random() * 16;
        this.alpha = 0.7 + Math.random() * 0.3;
        this.color = `hsl(${Math.random() * 360}, 80%, 70%)`;
      }
      draw(){
        ctx.font = `${this.size}px 'Great Vibes'`;
        ctx.fillStyle = this.color;
        ctx.globalAlpha = this.alpha;
        ctx.fillText(this.text, this.x, this.y);
        ctx.globalAlpha = 1;
      }
      update(){
        this.x += this.vx;
        this.y += this.vy;
        if (this.x < 0 || this.x > canvas.width) this.vx *= -1;
        if (this.y < 0 || this.y > canvas.height) this.vy *= -1;
      }
    }

    function drawHeart(x, y, size, color){
      ctx.save();
      ctx.translate(x, y);
      ctx.scale(size, size);
      ctx.beginPath();
      ctx.moveTo(0, 0);
      ctx.bezierCurveTo(0, -3, -5, -15, -25, -15);
      ctx.bezierCurveTo(-55, -15, -55, 25, -55, 25);
      ctx.bezierCurveTo(-55, 55, 0, 70, 0, 95);
      ctx.bezierCurveTo(0, 70, 55, 55, 55, 25);
      ctx.bezierCurveTo(55, 25, 55, -15, 25, -15);
      ctx.bezierCurveTo(5, -15, 0, -3, 0, 0);
      ctx.fillStyle = color;
      ctx.fill();
      ctx.restore();
    }

    let hearts = Array.from({length: 20}, () => ({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      vy: 0.3 + Math.random() * 0.5,
      size: 0.08 + Math.random() * 0.05,
      color: `rgba(255,100,150,${0.5 + Math.random() * 0.5})`
    }));

    function animate(){
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      hearts.forEach(h => {
        drawHeart(h.x, h.y, h.size, h.color);
        h.y -= h.vy;
        if (h.y < -100) {
          h.y = canvas.height + 50;
          h.x = Math.random() * canvas.width;
        }
      });
      particles.forEach(p => {
        p.update();
        p.draw();
      });
      requestAnimationFrame(animate);
    }

    document.getElementById('addWord').addEventListener('click', () => {
      const text = prompt('Escribe una palabra bonita 💖');
      if (text) particles.push(new Particle(text));
    });

    for (let w of words) particles.push(new Particle(w));
    animate();

    // 🎵 Control de música
    const bgMusic = document.getElementById('bgMusic');
    const musicControl = document.getElementById('musicControl');
    let playing = false;

    function toggleMusic(){
      if (playing) {
        bgMusic.pause();
        musicControl.textContent = '🔇 Reproducir música';
      } else {
        bgMusic.play().catch(err => console.log('No se pudo iniciar automáticamente', err));
        musicControl.textContent = '🔈 Pausar música';
      }
      playing = !playing;
    }

    musicControl.addEventListener('click', toggleMusic);
    window.addEventListener('load', toggleMusic);
  </script>
</body>
</html>
