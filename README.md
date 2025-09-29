<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>un espacio para los dos — queridisima liz</title>
  <style>
    :root{
      --bg:#0b0f1a;
      --neon:#ff6b9a;
      --soft:#ffd6e0;
      --sparkle-size:28px;
      --font: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    }
    html,body{height:100%;margin:0}
    body{
      background: radial-gradient(circle at 20% 10%, rgba(255,102,153,0.08), transparent 10%),
                  linear-gradient(180deg,#081022 0%, var(--bg) 60%);
      color:var(--soft);
      font-family:var(--font);
      overflow:hidden;
      display:flex;align-items:center;justify-content:center;
      user-select:none;
    }

    /* Contenedor principal */
    .stage{
      position:relative;width:100%;height:100%;
      display:block;overflow:hidden;
    }

    /* Mensaje central bonito */
    .center-card{
      position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);
      background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.015));
      padding:18px 28px;border-radius:16px;backdrop-filter: blur(6px);
      box-shadow:0 6px 30px rgba(0,0,0,0.6);
      text-align:center;max-width:92%;
    }
    h1{margin:0 0 6px 0;font-size:1.6rem;color:var(--neon);text-shadow:0 2px 18px rgba(255,107,154,0.15)}
    p{margin:0;font-size:1rem;opacity:0.85}

    /* Palabras que caen */
    .raindrop{
      position:absolute;left:0;top:-10%;white-space:nowrap;
      font-size:18px;font-weight:600;pointer-events:none;
      transform:translateY(-20px);
      color:var(--soft);opacity:0.95;
      text-shadow:0 2px 14px rgba(255,107,154,0.06);
      animation-name:fall;animation-timing-function:linear;animation-iteration-count:1;
    }
    @keyframes fall{
      0%{transform: translateY(-10vh) rotate(-6deg); opacity:0}
      10%{opacity:1}
      100%{transform: translateY(110vh) rotate(6deg); opacity:0.95}
    }

    /* Efecto de brillo al tocar */
    .sparkle{
      position:absolute;transform:translate(-50%,-50%);pointer-events:none;
      font-weight:800;font-size:var(--sparkle-size);mix-blend-mode:screen;
      animation:pop 900ms ease-out forwards;
      text-shadow: 0 0 8px rgba(255,107,154,0.9), 0 0 20px rgba(255,107,154,0.6);
      color:var(--neon);
    }
    @keyframes pop{
      0%{opacity:0;transform:translate(-50%,-50%) scale(0.6) rotate(-15deg)}
      30%{opacity:1;transform:translate(-50%,-50%) scale(1.1) rotate(6deg)}
      100%{opacity:0;transform:translate(-50%,-50%) scale(1.6) rotate(25deg)}
    }

    /* pequeÃ±o pulso cuando aparece */
    .pulse{animation: pulseGlow 1.4s ease-out both}
    @keyframes pulseGlow{
      0%{box-shadow:0 0 0 rgba(255,107,154,0.0)}
      50%{box-shadow:0 0 40px rgba(255,107,154,0.12)}
      100%{box-shadow:0 0 0 rgba(255,107,154,0)}
    }

    /* Botoncito para pausar */
    .controls{position:fixed; right:12px; bottom:12px; z-index:9999}
    .btn{
      background:rgba(255,255,255,0.04); border:1px solid rgba(255,255,255,0.06);
      color:var(--soft); padding:8px 12px; border-radius:999px;font-weight:600;cursor:pointer;
      backdrop-filter: blur(6px);
    }

    /* Responsive tweaks */
    @media (max-width:420px){:root{--sparkle-size:22px} h1{font-size:1.3rem} .raindrop{font-size:16px}}
  </style>
</head>
<body>
  <div class="stage" id="stage">
    <div class="center-card">
      <h1> mi princesa Liz</h1>
      <p>you are important to me.</p>
    </div>

    <div class="controls">
      <button id="toggle" class="btn">Pausar lluvia</button>
    </div>
  </div>

  <script>
    // Lista de palabras románticas (puedes editarlas)
    const words = [
      'te amo', 'mi vida', 'mi cielo', 'mi reina', 'mi amor', 'princesa', 'mi niña de ojos hermosos', 'mi corazón', 'mi todo', 'PRINCESITA', 'mi LIZ', 'MI VIDA', 'MY LOVE'
    ];

    const stage = document.getElementById('stage');
    let raining = true;
    let spawnInterval = 700; // ms entre palabras
    let spawnTimer = null;

    function rand(min, max){ return Math.random()*(max-min)+min }

    function createDrop(){
      if(!raining) return;
      const el = document.createElement('div');
      el.className = 'raindrop';
      // elegir palabra aleatoria pero con variantes de mayúsculas y emojis suaves
      let w = words[Math.floor(Math.random()*words.length)];
      if(Math.random() < 0.08) w = w.toUpperCase();
      if(Math.random() < 0.12) w += ' 💕';
      el.textContent = w;

      // posición horizontal aleatoria
      const left = rand(2, 96);
      el.style.left = left + '%';

      // duración aleatoria para simular distintas velocidades
      const duration = rand(4200, 9000);
      el.style.animationDuration = duration + 'ms';

      // tamaño ligero aleatorio
      const size = Math.floor(rand(14,22));
      el.style.fontSize = size + 'px';

      // rotación inicial
      el.style.transform = `translateY(-10vh) rotate(${rand(-8,8)}deg)`;

      stage.appendChild(el);

      // remover después de la animación para no acumular DOM
      setTimeout(()=>{ el.remove() }, duration + 200);
    }

    function startRain(){
      if(spawnTimer) clearInterval(spawnTimer);
      spawnTimer = setInterval(createDrop, spawnInterval);
    }
    function stopRain(){ if(spawnTimer) clearInterval(spawnTimer); spawnTimer = null }

    // Inicializar lluvia
    startRain();

    // Toggle botón
    const btn = document.getElementById('toggle');
    btn.addEventListener('click', ()=>{
      raining = !raining;
      if(raining){ startRain(); btn.textContent = 'Pausar lluvia' } else { stopRain(); btn.textContent = 'Reanudar lluvia' }
    });

    // Evento táctil / clic para crear destellos con "te amo"
    function makeSparkle(x,y){
      const s = document.createElement('div');
      s.className = 'sparkle pulse';
      s.textContent = 'te amo';
      // posicionar en coordenadas del viewport
      s.style.left = x + 'px';
      s.style.top = y + 'px';
      // tamaño aleatorio
      const size = Math.floor(rand(18, 36));
      s.style.fontSize = size + 'px';
      stage.appendChild(s);

      // desaparecer después de la anim
      setTimeout(()=> s.remove(), 1100);

      // además, crear pequeñas partículas de palabra que caen desde el punto
      for(let i=0;i<3;i++){
        const small = document.createElement('div');
        small.className = 'raindrop';
        small.textContent = (Math.random()<0.5)? '♥' : 'te amo';
        small.style.left = (x / window.innerWidth * 100 + rand(-6,6)) + '%';
        small.style.fontSize = Math.floor(rand(12,16)) + 'px';
        const dur = rand(500,1300);
        small.style.animationDuration = dur + 'ms';
        stage.appendChild(small);
        setTimeout(()=> small.remove(), dur + 200);
      }
    }

    // Soporta toque y clic
    function handleTap(e){
      e.preventDefault();
      let x, y;
      if(e.touches && e.touches[0]){ x = e.touches[0].clientX; y = e.touches[0].clientY }
      else { x = e.clientX; y = e.clientY }
      makeSparkle(x,y);
    }

    // Añadir listeners para tocar y clic
    window.addEventListener('touchstart', handleTap, {passive:false});
    window.addEventListener('mousedown', handleTap);

    // Mejorar rendimiento: pausamos cuando no está visible
    document.addEventListener('visibilitychange', ()=>{
      if(document.hidden){ stopRain() } else { if(raining) startRain() }
    });

    // Ajuste opcional: cambia la densidad según ancho de pantalla
    function adjustDensity(){
      const w = window.innerWidth;
      if(w < 420) spawnInterval = 900;
      else if(w < 900) spawnInterval = 700;
      else spawnInterval = 450;
      if(raining){ startRain() }
    }
    window.addEventListener('resize', adjustDensity);
    adjustDensity();

    // Toque romántico automático si el usuario no toca: pequeño guiño
    setTimeout(()=>{ createDrop(); createDrop(); }, 600);
  </script>
</body>
</html>
