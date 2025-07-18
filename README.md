<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Hacker Birthday for 9ARMZ</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: black;
    }
    canvas {
      position: absolute;
      top: 0;
      left: 0;
      display: block;
    }
  </style>
</head>
<body>

  <!-- 🔊 เพลง Happy Birthday India Version -->
  <audio id="hbd-audio" autoplay loop hidden>
    <source src="Happy birthday India Version 1.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <!-- พื้นหลัง Matrix -->
  <canvas id="background"></canvas>
  <canvas id="canvas"></canvas>

  <script>
    // พื้นหลัง Matrix rain สีฟ้า
    const bgCanvas = document.getElementById('background');
    const bgCtx = bgCanvas.getContext('2d');
    bgCanvas.width = window.innerWidth;
    bgCanvas.height = window.innerHeight;

    const chars = "ｱｲｳｴｵｶｷｸｹｺｻｼｽ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const fontSize = 16;
    const columns = Math.floor(bgCanvas.width / fontSize);
    const drops = Array(columns).fill(1);
    let matrixSpeed = 0.25;
    let frameCounter = 0;

    function drawMatrixRain() {
      frameCounter += matrixSpeed;
      if (frameCounter < 1) {
        requestAnimationFrame(drawMatrixRain);
        return;
      }
      frameCounter = 0;

      bgCtx.fillStyle = "rgba(0, 0, 0, 0.2)";
      bgCtx.fillRect(0, 0, bgCanvas.width, bgCanvas.height);

      bgCtx.fillStyle = "#00ccff";
      bgCtx.font = fontSize + "px monospace";

      for(let i = 0; i < drops.length; i++) {
        const text = chars[Math.floor(Math.random() * chars.length)];
        bgCtx.fillText(text, i * fontSize, drops[i] * fontSize);
        if(drops[i] * fontSize > bgCanvas.height || Math.random() > 0.975) {
          drops[i] = 0;
        }
        drops[i]++;
      }

      requestAnimationFrame(drawMatrixRain);
    }
    drawMatrixRain();

    // จุดข้อความ
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const words = ['3', '2', '1', 'HAPPY', 'BIRTHDAY', 'TO', 'YOU', '9ARMZ'];
    let currentWordIndex = -1;
    let particles = [];
    const density = 4;

    function createParticlesFromText(text) {
      const offCanvas = document.createElement('canvas');
      const offCtx = offCanvas.getContext('2d');
      offCanvas.width = canvas.width;
      offCanvas.height = canvas.height;

      offCtx.fillStyle = 'white';
      offCtx.font = 'bold 160px monospace';
      offCtx.textAlign = 'center';
      offCtx.textBaseline = 'middle';
      offCtx.clearRect(0, 0, offCanvas.width, offCanvas.height);
      offCtx.fillText(text, canvas.width / 2, canvas.height / 2);

      const imgData = offCtx.getImageData(0, 0, canvas.width, canvas.height);
      const newParticles = [];

      for(let y = 0; y < canvas.height; y += density) {
        for(let x = 0; x < canvas.width; x += density) {
          const index = (y * canvas.width + x) * 4;
          const alpha = imgData.data[index + 3];
          if(alpha > 128) {
            newParticles.push({
              x: Math.random() * canvas.width,
              y: Math.random() * canvas.height,
              tx: x,
              ty: y,
              vx: 0,
              vy: 0
            });
          }
        }
      }
      return newParticles;
    }

    function updateParticles() {
      for(let p of particles) {
        const dx = p.tx - p.x;
        const dy = p.ty - p.y;
        p.vx += dx * 0.01;
        p.vy += dy * 0.01;
        p.vx *= 0.9;
        p.vy *= 0.9;
        p.x += p.vx;
        p.y += p.vy;
      }
    }

    function drawParticles() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = '#00ccff';
      for(let p of particles) {
        ctx.beginPath();
        ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
        ctx.fill();
      }
    }

    function disperseParticles() {
      for(let p of particles) {
        p.tx = Math.random() * canvas.width;
        p.ty = Math.random() * canvas.height;
      }
    }

    function loop() {
      updateParticles();
      drawParticles();
      requestAnimationFrame(loop);
    }

    function changeToNextWord() {
      currentWordIndex = (currentWordIndex + 1) % words.length;
      particles = createParticlesFromText(words[currentWordIndex]);
    }

    function nextWord() {
      changeToNextWord();
      setTimeout(() => {
        disperseParticles();
      }, 3000); // แสดงคำ 3 วิ ก่อนกระจาย
    }

    changeToNextWord();
    loop();
    setInterval(nextWord, 4000); // เปลี่ยนคำใหม่ทุก 4 วินาที

    window.addEventListener('resize', () => {
      bgCanvas.width = window.innerWidth;
      bgCanvas.height = window.innerHeight;
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });
  </script>
</body>
</html># Happy-birthday-