[ai_writer_picker.html](https://github.com/user-attachments/files/23195988/ai_writer_picker.html)
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>AI Writer Picker — Классика с характером</title>
<style>
  :root{
    --bg:#0b0b10;
    --panel:#0f1724;
    --accent1:#ffb84c;
    --accent2:#7c5cff;
    --btn:#6a5cff;
    --muted:#9aa0b4;
    --glass: rgba(255,255,255,0.04);
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    background: linear-gradient(180deg,#050507 0%, #0b0b10 60%);
    color:#fff;
    font-family: Inter, "Helvetica Neue", Arial, sans-serif;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:20px;
  }
  .card{
    width:100%;
    max-width:420px;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    border-radius:18px;
    padding:18px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.6);
    text-align:center;
    border:1px solid rgba(255,255,255,0.03);
  }
  h1{
    margin:6px 0 8px;
    font-size:18px;
    color:var(--accent1);
  }
  p.lead{
    margin:0 0 14px;
    color:var(--muted);
    font-size:13px;
  }
  .picker{
    height:260px;
    display:flex;
    align-items:center;
    justify-content:center;
    position:relative;
    margin-bottom:14px;
  }
  .avatar{
    width:220px;
    height:220px;
    border-radius:18px;
    background:linear-gradient(135deg,#0f1724,#11121a);
    display:flex;
    align-items:center;
    justify-content:center;
    flex-direction:column;
    padding:12px;
    transition:transform .45s cubic-bezier(.2,.9,.3,1), box-shadow .3s;
    box-shadow: 0 6px 18px rgba(0,0,0,0.6);
  }
  .avatar.show{
    transform: translateY(-6px) scale(1.02);
    box-shadow: 0 18px 40px rgba(106,92,255,0.16);
  }
  .avatar svg{width:120px;height:120px}
  .name{
    margin-top:10px;
    font-weight:700;
    font-size:16px;
    color:#fff;
  }
  .meme{
    margin-top:6px;
    font-size:13px;
    color:var(--muted);
    padding:8px 12px;
    border-radius:12px;
    background:var(--glass);
  }
  .controls{display:flex;gap:10px;justify-content:center;margin-top:12px}
  .btn{
    flex:1;
    padding:12px 10px;
    border-radius:12px;
    background:linear-gradient(90deg,var(--btn), #8e6cff);
    color:white;
    text-decoration:none;
    font-weight:700;
    border:none;
    cursor:pointer;
    box-shadow: 0 8px 20px rgba(106,92,255,0.12);
  }
  .btn.secondary{
    background:transparent;
    border:1px solid rgba(255,255,255,0.06);
    color:var(--muted);
    font-weight:600;
  }
  .thinking{
    position:absolute;
    top:12px;
    left:50%;
    transform:translateX(-50%);
    font-size:12px;
    color:var(--muted);
    display:flex;
    gap:8px;
    align-items:center;
    opacity:0;
    transition:opacity .18s;
  }
  .thinking.show{opacity:1}
  .dot{width:8px;height:8px;border-radius:50%;background:#7c5cff;animation:blink 1s infinite;}
  .dot.d2{animation-delay:.15s}
  .dot.d3{animation-delay:.3s}
  @keyframes blink{0%{transform:translateY(0)}50%{transform:translateY(-6px)}100%{transform:translateY(0)}}
  footer{margin-top:14px;color:var(--muted);font-size:12px}
  /* small tweak for "tap-like" feel */
  .btn:active{transform:translateY(2px) scale(.998)}
  @media (max-width:360px){
    .avatar{width:200px;height:200px}
    .avatar svg{width:100px;height:100px}
  }
</style>
</head>
<body>
<div class="card" role="application" aria-label="AI Writer Picker">
  <h1>AI Group Generator</h1>
  <p class="lead">Классика с характером — тапни и узнай, кем ты стал(а) сегодня</p>

  <div class="picker">
    <div class="thinking" id="thinking"><span>ИИ думает</span><span style="opacity:.6">.</span>
      <div style="display:flex;align-items:center;gap:6px;margin-left:8px">
        <div class="dot"></div><div class="dot d2"></div><div class="dot d3"></div>
      </div>
    </div>

    <div class="avatar" id="avatar">
      <!-- default neutral face (SVG) -->
      <svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <rect x="0" y="0" width="120" height="120" rx="14" fill="#0f1724"/>
        <g transform="translate(10,10)">
          <circle cx="50" cy="40" r="26" fill="#1f2937"/>
          <rect x="20" y="70" width="60" height="14" rx="6" fill="#11121a"/>
          <circle cx="36" cy="36" r="4" fill="#fff"/>
          <circle cx="64" cy="36" r="4" fill="#fff"/>
          <path d="M30 50 q20 12 40 0" stroke="#9aa0b4" stroke-width="3" fill="none" stroke-linecap="round"/>
        </g>
      </svg>
      <div class="name" id="name">Нажми "Выбрать писателя"</div>
      <div class="meme" id="meme">Здесь появится твой писатель и подпись</div>
    </div>
  </div>

  <div class="controls">
    <button class="btn" id="pickBtn">Выбрать писателя</button>
    <button class="btn secondary" id="nextBtn">Далее</button>
  </div>

  <footer>Покажи коллегам — и смотри реакцию. © LIT.AI</footer>
</div>

<script>
(function(){
  const writers = [
    {
      id:'tolstoy',
      name:'Лев Толстой',
      meme:'Пишет долго. Проверяет — ещё дольше.',
      svg: `<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <rect x="0" y="0" width="120" height="120" rx="14" fill="#1a1320"/>
        <g transform="translate(10,10)">
          <rect x="6" y="6" width="98" height="98" rx="12" fill="#171923"/>
          <circle cx="50" cy="36" r="26" fill="#2b2f3a"/>
          <path d="M30 28 q20 -34 40 0" stroke="#f4d6a0" stroke-width="6" fill="none" stroke-linecap="round"/>
          <rect x="20" y="68" width="60" height="16" rx="6" fill="#14151a" />
          <path d="M28 42 q10 6 44 0" stroke="#fff" stroke-width="2" fill="none" stroke-linecap="round"/>
          <!-- beard -->
          <path d="M26 46 q24 30 48 0" fill="#e6c9a4" stroke="#e6c9a4" stroke-width="0"/>
          <!-- glasses hint -->
          <circle cx="36" cy="36" r="4" fill="#fff"/>
          <circle cx="64" cy="36" r="4" fill="#fff"/>
        </g>
      </svg>`
    },
    {
      id:'chekhov',
      name:'Антон Чехов',
      meme:'Коротко, но с сарказмом.',
      svg:`<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <rect x="0" y="0" width="120" height="120" rx="14" fill="#08121a"/>
        <g transform="translate(10,10)">
          <rect x="6" y="6" width="98" height="98" rx="12" fill="#0c1720"/>
          <circle cx="50" cy="36" r="26" fill="#23303b"/>
          <path d="M30 46 q20 -16 40 0" stroke="#c8d8ff" stroke-width="3" fill="none" stroke-linecap="round"/>
          <rect x="22" y="68" width="56" height="14" rx="6" fill="#0d1116" />
          <circle cx="36" cy="36" r="3.6" fill="#fff"/>
          <circle cx="64" cy="36" r="3.6" fill="#fff"/>
          <!-- hint of mustache -->
          <path d="M36 48 q14 6 28 0" stroke="#9aa0b4" stroke-width="2" fill="none" stroke-linecap="round"/>
        </g>
      </svg>`
    },
    {
      id:'dost',
      name:'Фёдор Достоевский',
      meme:'Когда проверяешь тетради и задумываешься о смысле жизни.',
      svg:`<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <rect x="0" y="0" width="120" height="120" rx="14" fill="#0b0810"/>
        <g transform="translate(10,10)">
          <rect x="6" y="6" width="98" height="98" rx="12" fill="#0f0d16"/>
          <circle cx="50" cy="36" r="26" fill="#201920"/>
          <path d="M28 34 q22 -28 44 0" stroke="#d7c3b0" stroke-width="5" fill="none" stroke-linecap="round"/>
          <rect x="20" y="68" width="60" height="16" rx="6" fill="#0b0b0e" />
          <circle cx="36" cy="36" r="3.8" fill="#fff"/>
          <circle cx="64" cy="36" r="3.8" fill="#fff"/>
          <path d="M30 48 q20 12 40 0" stroke="#9aa0b4" stroke-width="2" fill="none" stroke-linecap="round"/>
        </g>
      </svg>`
    }
  ];

  const avatar = document.getElementById('avatar');
  const nameEl = document.getElementById('name');
  const memeEl = document.getElementById('meme');
  const thinking = document.getElementById('thinking');
  const pickBtn = document.getElementById('pickBtn');
  const nextBtn = document.getElementById('nextBtn');

  function showNeutral(){
    nameEl.textContent = 'Нажми "Выбрать писателя"';
    memeEl.textContent = 'Здесь появится твой писатель и подпись';
    avatar.classList.remove('show');
    // reset default svg (first child)
    avatar.querySelector('svg').outerHTML = `<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <rect x="0" y="0" width="120" height="120" rx="14" fill="#0f1724"/>
        <g transform="translate(10,10)">
          <circle cx="50" cy="40" r="26" fill="#1f2937"/>
          <rect x="20" y="70" width="60" height="14" rx="6" fill="#11121a"/>
          <circle cx="36" cy="36" r="4" fill="#fff"/>
          <circle cx="64" cy="36" r="4" fill="#fff"/>
          <path d="M30 50 q20 12 40 0" stroke="#9aa0b4" stroke-width="3" fill="none" stroke-linecap="round"/>
        </g>
      </svg>`;
  }

  function pickRandom(){
    const idx = Math.floor(Math.random()*writers.length);
    return writers[idx];
  }

  function reveal(writer){
    // show thinking
    thinking.classList.add('show');
    avatar.classList.remove('show');
    nameEl.textContent = '...';
    memeEl.textContent = '';
    // short delay for effect
    setTimeout(()=>{
      thinking.classList.remove('show');
      // set SVG
      const svgWrap = writer.svg;
      avatar.querySelector('svg').outerHTML = svgWrap;
      nameEl.textContent = writer.name;
      memeEl.textContent = writer.meme;
      avatar.classList.add('show');
    }, 800 + Math.random()*700);
  }

  pickBtn.addEventListener('click', ()=>{
    const w = pickRandom();
    reveal(w);
  });
  nextBtn.addEventListener('click', ()=>{
    const w = pickRandom();
    reveal(w);
  });

  // keyboard accessibility
  document.addEventListener('keydown', (e)=>{
    if(e.key===' ' || e.key==='Enter') {
      e.preventDefault();
      const w = pickRandom();
      reveal(w);
    }
  });

  // initial
  showNeutral();
})();
</script>
</body>
</html>
