<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sattvaa Launch — 3,2,1</title>
<style>
  :root{
    --bg:#eaf6ff;
    --deep:#073a63;
    --accent:#28a0ff;
    --glass:#e8f9ff;
    --muted:#0b597f;
  }
  html,body{height:100%;margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:linear-gradient(180deg,var(--bg),#f6fbff);color:var(--deep);-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;}
  .stage{height:100%;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden;padding:30px;}

  /* main layout */
  .card{width:100%;max-width:1100px;display:grid;grid-template-columns:1fr 420px;gap:28px;align-items:center;z-index:3;}
  @media(max-width:980px){ .card{grid-template-columns:1fr; } }

  /* left visual area */
  .visual{position:relative;height:520px;display:flex;align-items:center;justify-content:center;overflow:hidden;border-radius:14px;padding:12px;}
  .visual .bg-glow{position:absolute;inset:-20% -10% auto -10%;filter:blur(36px) saturate(120%);background:radial-gradient(circle at 40% 20%, rgba(40,160,255,0.18), transparent 12%),radial-gradient(circle at 80% 60%, rgba(4,120,220,0.12), transparent 20%);z-index:0}

  /* Countdown big number */
  .countdown{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;z-index:6;pointer-events:none;}
  .countdown .num{font-weight:900;font-size:220px;line-height:1;color:rgba(3,30,60,0.06);-webkit-text-stroke:2px rgba(3,30,60,0.06);opacity:0;transform-origin:center;letter-spacing:6px;text-shadow:0 8px 40px rgba(0,120,200,0.06)}
  .countdown .num.show{animation:countScale 920ms cubic-bezier(.22,.9,.3,1) forwards}

  @keyframes countScale{
    0%{opacity:0;transform:scale(.6) translateY(28px) rotate(-4deg);filter:blur(6px)}
    30%{opacity:1;transform:scale(1.02) translateY(0) rotate(0);filter:blur(0)}
    100%{opacity:0;transform:scale(1.6) translateY(-40px) rotate(2deg)}
  }

  /* bottles area */
  .bottles{position:relative;z-index:5;display:flex;align-items:flex-end;justify-content:space-between;height:400px;padding:20px;transform:translateY(80px)}
  .bottle-wrap{width:140px;height:320px;display:flex;flex-direction:column;align-items:center;justify-content:flex-end;opacity:0;transform:translateY(40px) rotate(-6deg) scale(.96)}
  .bottle-wrap.revealed{animation:popIn .9s cubic-bezier(.2,.9,.25,1) forwards}
  @keyframes popIn{from{opacity:0;transform:translateY(40px) scale(.96) rotate(-6deg)} to{opacity:1;transform:translateY(0) scale(1) rotate(0)}}

  .label{margin-top:10px;font-weight:700;color:var(--muted);font-size:14px}

  /* subtle float */
  .float{animation:float 6s ease-in-out infinite}
  @keyframes float{0%{transform:translateY(0) }50%{transform:translateY(-8px)}100%{transform:translateY(0)}}

  /* shine effect for bottles */
  .bottle-shine{position:absolute;left:0;top:0;width:100%;height:100%;pointer-events:none;mix-blend-mode:screen;opacity:0}
  .bottle-wrap.revealed .bottle-shine{animation:shine 1.1s ease-out 0.18s forwards}
  @keyframes shine{0%{opacity:0;transform:translateX(-60%)}50%{opacity:0.95;transform:translateX(20%)}100%{opacity:0;transform:translateX(120%)}}

  /* wave at bottom */
  .wave-wrap{position:absolute;bottom:0;left:0;right:0;height:36%;pointer-events:none;z-index:2;overflow:hidden}
  .wave{position:absolute;left:0;top:0;width:200%;height:120%;background:linear-gradient(180deg, rgba(40,150,255,0.18), rgba(40,150,255,0.06));transform:translateY(10%);opacity:0.9;animation:wave 6s linear infinite}
  @keyframes wave{0%{transform:translateX(0)}100%{transform:translateX(-50%)}}

  /* right copy */
  .copy{padding:14px 18px}
  .headline{font-size:32px;margin:0 0 8px 0;font-weight:900;color:var(--deep)}
  .sub{margin:0;color:#0e5c86;line-height:1.45}
  .launch-btn{margin-top:14px;background:linear-gradient(90deg,var(--accent),#0c89ff);color:#fff;padding:12px 18px;border-radius:10px;border:0;font-weight:800;cursor:pointer;box-shadow:0 12px 30px rgba(2,80,140,0.12)}
  .footer-note{font-size:13px;color:#0b4f79;opacity:0.9;margin-top:10px}

  /* final banner */
  .final-banner{position:absolute;top:8%;left:50%;transform:translateX(-50%) translateY(-40px);background:rgba(255,255,255,0.95);border-radius:12px;padding:12px 18px;box-shadow:0 10px 30px rgba(2,40,80,0.06);opacity:0;z-index:8;display:flex;gap:12px;align-items:center;pointer-events:none}
  .final-banner.show{animation:popIn .6s ease forwards;opacity:1;transform:translateX(-50%) translateY(0)}
  .final-banner h2{margin:0;font-size:18px;color:var(--deep);font-weight:900}
  .final-banner p{margin:0;color:#0b496f;opacity:0.9;font-size:13px}

  /* small particles svg */
  svg.splash{position:absolute;inset:0;z-index:4;pointer-events:none;overflow:visible}

  /* accessibility */
  [aria-hidden="true"]{pointer-events:none}
</style>
</head>
<body>
  <div class="stage" role="main">
    <div class="card" aria-labelledby="launchTitle">
      <!-- VISUAL -->
      <div class="visual" aria-hidden="true">
        <div class="bg-glow" aria-hidden="true"></div>

        <!-- Countdown big number -->
        <div class="countdown" id="countdown" aria-hidden="true">
          <div class="num" id="num">3</div>
        </div>

        <!-- particles svg -->
        <svg class="splash" id="particles" width="100%" height="100%" viewBox="0 0 1000 700" preserveAspectRatio="none"></svg>

        <!-- Bottles -->
        <div class="bottles" id="bottles" aria-hidden="true">
          <div class="bottle-wrap" data-type="prod1">
            <div class="bottle-shine"></div>
            <!-- simple bottle SVG -->
            <svg viewBox="0 0 200 520" class="bottle" width="140" height="320" aria-hidden="true">
              <defs>
                <linearGradient id="g1" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#f8ffff"/><stop offset="1" stop-color="#dff6ff"/></linearGradient>
                <linearGradient id="w1" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#9fe6ff"/><stop offset="1" stop-color="#57c2ff"/></linearGradient>
              </defs>
              <g transform="translate(20,10)">
                <path class="glass" d="M60 0 C58 0 55 6 55 12 L55 50 C55 60 45 68 45 80 L45 420 C45 452 155 452 155 420 L155 80 C155 68 145 60 145 50 L145 12 C145 6 142 0 140 0 Z" fill="url(#g1)"/>
                <rect x="72" y="110" width="56" height="320" rx="6" fill="url(#w1)" />
                <rect x="72" y="420" width="56" height="64" rx="10" fill="#e6f8ff" opacity="0.8"/>
              </g>
            </svg>
            <div class="label">1L — Pure</div>
          </div>

          <div class="bottle-wrap" data-type="prod2">
            <div class="bottle-shine"></div>
            <svg viewBox="0 0 200 520" class="bottle" width="140" height="320" aria-hidden="true">
              <defs>
                <linearGradient id="g2" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#fbffff"/><stop offset="1" stop-color="#e0f6ff"/></linearGradient>
                <linearGradient id="w2" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#bff1ff"/><stop offset="1" stop-color="#5fc7ff"/></linearGradient>
              </defs>
              <g transform="translate(20,10)">
                <path fill="url(#g2)" d="M60 0 C58 0 55 6 55 12 L55 50 C55 60 45 68 45 80 L45 420 C45 452 155 452 155 420 L155 80 C155 68 145 60 145 50 L145 12 C145 6 142 0 140 0 Z"/>
                <rect x="72" y="150" width="56" height="280" rx="6" fill="url(#w2)"/>
              </g>
            </svg>
            <div class="label">20L — Can</div>
          </div>

          <div class="bottle-wrap" data-type="prod3">
            <div class="bottle-shine"></div>
            <svg viewBox="0 0 200 520" class="bottle" width="140" height="320" aria-hidden="true">
              <defs>
                <linearGradient id="g3" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#fbffff"/><stop offset="1" stop-color="#d6f2ff"/></linearGradient>
                <linearGradient id="w3" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#bfefff"/><stop offset="1" stop-color="#60c6ff"/></linearGradient>
              </defs>
              <g transform="translate(20,10)">
                <path fill="url(#g3)" d="M60 0 C58 0 55 6 55 12 L55 50 C55 60 45 68 45 80 L45 420 C45 452 155 452 155 420 L155 80 C155 68 145 60 145 50 L145 12 C145 6 142 0 140 0 Z"/>
                <rect x="72" y="180" width="56" height="240" rx="6" fill="url(#w3)"/>
              </g>
            </svg>
            <div class="label">Packaged</div>
          </div>
        </div>

        <div class="wave-wrap" aria-hidden="true"><div class="wave" id="wave"></div></div>
      </div>

      <!-- COPY / ACTION -->
      <div class="copy">
        <h1 id="launchTitle" class="headline">Sattvaa — Purified Water</h1>
        <p class="sub">Clean. Pure. Fresh. Launching now with crystal-clear water and premium packaging.</p>
        <button class="launch-btn" id="playBtn" aria-label="Start launch">Play Launch Animation</button>
        <p class="footer-note">Bengaluru • Contact: 8074590321</p>
      </div>
    </div>

    <!-- final banner -->
    <div class="final-banner" id="finalBanner" role="status" aria-live="polite" aria-hidden="true">
      <svg width="36" height="36" viewBox="0 0 24 24" fill="none" aria-hidden="true"><circle cx="12" cy="12" r="10" fill="#28a0ff"/><path d="M7 13l3 3 7-7" stroke="#fff" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
      <div><h2>We proudly announce</h2><p>Sattvaa is now live — Pure water for better life.</p></div>
    </div>
  </div>

<script>
/* Orchestration: countdown -> water rush -> reveal bottles -> final banner */

// elements
const playBtn = document.getElementById('playBtn');
const numEl = document.getElementById('num');
const countdownEl = document.getElementById('countdown');
const bottles = Array.from(document.querySelectorAll('.bottle-wrap'));
const finalBanner = document.getElementById('finalBanner');
const particlesSVG = document.getElementById('particles');
const wave = document.getElementById('wave');

function rand(min,max){ return Math.random()*(max-min)+min; }

/* create decorative particles in SVG */
function spawnParticles(){
  particlesSVG.innerHTML = '';
  for(let i=0;i<22;i++){
    const cx = rand(60,940);
    const cy = rand(80,520);
    const r = rand(2,8);
    const c = document.createElementNS('http://www.w3.org/2000/svg','circle');
    c.setAttribute('cx', cx);
    c.setAttribute('cy', cy);
    c.setAttribute('r', r);
    c.setAttribute('fill', `rgba(40,160,255,${(rand(0.06,0.25)).toFixed(2)})`);
    c.style.opacity = 0;
    c.style.transform = 'translateY(20px)';
    c.style.transition = `${700+Math.random()*900}ms cubic-bezier(.22,.9,.3,1)`;
    particlesSVG.appendChild(c);
  }
}

/* show one countdown number */
function showNumber(n){
  return new Promise(resolve=>{
    numEl.textContent = n;
    numEl.classList.remove('show');
    void numEl.offsetWidth; // reflow
    numEl.classList.add('show');

    // particle burst
    const circles = particlesSVG.querySelectorAll('circle');
    circles.forEach((c,idx)=>{
      setTimeout(()=>{ c.style.opacity = 1; c.style.transform = `translateY(${ - (rand(40,160)) }px) translateX(${ (Math.random()-0.5)*40 }px)`; }, idx*18 + Math.random()*120);
    });

    setTimeout(()=>{ resolve(); }, 950);
  });
}

/* reveal bottles with stagger */
function revealBottles(){
  bottles.forEach((b, i)=>{
    setTimeout(()=>{
      b.classList.add('revealed');
      b.style.opacity = '1';
      b.style.transform = 'translateY(0) rotate(0) scale(1)';
      // floating after reveal
      setTimeout(()=> b.classList.add('float'), 400);
      // show bottle shine element
      const shine = b.querySelector('.bottle-shine');
      if(shine){
        shine.style.background = 'linear-gradient(120deg, rgba(255,255,255,0.28), rgba(255,255,255,0))';
        shine.style.opacity = 1;
      }
    }, i*300 + 200);
  });
}

/* animate big wave "rush" */
function waveRush(){
  wave.style.transition = 'transform 600ms cubic-bezier(.22,.9,.3,1), opacity 700ms';
  wave.style.transform = 'translateX(-40%) translateY(-12%)';
  wave.style.opacity = 1;
}

/* full sequence */
async function playSequence(){
  spawnParticles();
  finalBanner.classList.remove('show');
  // reset bottles
  bottles.forEach(b=>{ b.classList.remove('revealed','float'); b.style.opacity=0; b.style.transform='translateY(40px) rotate(-6deg) scale(.96)'; });

  await showNumber('3');
  await showNumber('2');
  await showNumber('1');

  waveRush();
  revealBottles();
  setTimeout(()=> finalBanner.classList.add('show'), 1400);
}

/* Auto-play on load lightly, and wire button */
window.addEventListener('load', ()=>{ spawnParticles(); setTimeout(()=>playSequence(), 800); });
playBtn.addEventListener('click', playSequence);
</script>
</body>
</html>
