إليك الكود كاملاً مع تعديل الكتابة حرفاً حرفاً وسطراً سطراً، وتأكيد ظهور شاشة الخطأ المزيف (Fake Bug) كأول مرحلة بعد القلادة مباشرة.

```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes, viewport-fit=cover">
  <title>🎂 عامك الـ 18 | رحلة Reem 💫</title>
  <link href="https://fonts.googleapis.com/css2?family=El+Messiri:wght@400;600;700&family=Tajawal:wght@400;500;700;800&family=Marhey:wght@500;700;800&family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
  <style>
    * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; -webkit-user-select:none; user-select:none; }
    body { font-family:'Tajawal','Cairo','El Messiri',sans-serif; overflow:hidden; height:100dvh; width:100dvw; background:#000; position:fixed; top:0; left:0; touch-action:manipulation; }
    .stage-indicator { position:fixed; right:8px; top:50%; transform:translateY(-50%); z-index:250; display:flex; flex-direction:column; align-items:center; gap:3px; background:rgba(10,5,25,0.75); border-radius:20px; padding:10px 6px; backdrop-filter:blur(8px); -webkit-backdrop-filter:blur(8px); border:1px solid rgba(255,215,0,0.4); box-shadow:0 0 20px rgba(0,0,0,0.5); }
    .stage-dot { width:8px; height:8px; border-radius:50%; background:rgba(255,255,255,0.2); transition:all 0.4s; border:1px solid rgba(255,215,0,0.3); flex-shrink:0; }
    .stage-dot.active { background:#ffd700; box-shadow:0 0 10px #ffd700,0 0 18px #ffaa00; width:11px; height:11px; }
    .stage-dot.done { background:#f39c12; box-shadow:0 0 5px #f39c12; }
    .stage-num { color:#f7e05e; font-size:0.5rem; font-weight:700; text-align:center; line-height:1; margin-top:2px; }
    .top-instruction { position:fixed; top:16px; left:50%; transform:translateX(-50%); z-index:240; background:rgba(15,8,30,0.88); backdrop-filter:blur(16px); -webkit-backdrop-filter:blur(16px); border:1.5px solid rgba(212,175,55,0.5); border-radius:28px; padding:8px 18px; color:#fadf8a; font-size:0.85rem; font-weight:600; text-align:center; letter-spacing:1px; box-shadow:0 0 25px rgba(200,160,30,0.2); pointer-events:none; white-space:nowrap; max-width:85vw; overflow:hidden; text-overflow:ellipsis; }
    .sound-toggle { position:fixed; top:16px; left:16px; z-index:260; font-size:1.8rem; cursor:pointer; filter:drop-shadow(0 0 10px rgba(255,215,0,0.5)); transition:transform 0.2s; background:rgba(10,5,25,0.6); border-radius:50%; width:38px; height:38px; display:flex; align-items:center; justify-content:center; backdrop-filter:blur(8px); -webkit-backdrop-filter:blur(8px); }
    .sound-toggle:active { transform:scale(0.85); }
    .gate-container { position:fixed; top:0; left:0; width:100%; height:100%; background:radial-gradient(ellipse at center,#1a1028 0%,#06040f 100%); display:flex; flex-direction:column; align-items:center; justify-content:center; z-index:1000; cursor:pointer; transition:opacity 0.8s,visibility 0.8s; }
    .gate-container.fade-out { opacity:0; visibility:hidden; pointer-events:none; }
    .gate-glow { position:absolute; width:250px; height:250px; background:radial-gradient(circle,rgba(255,215,0,0.3) 0%,transparent 70%); border-radius:50%; animation:gatePulse 2.5s infinite; pointer-events:none; }
    @keyframes gatePulse { 0%,100%{ transform:scale(0.8); opacity:0.5; } 50%{ transform:scale(1.3); opacity:1; } }
    .gate-text { font-family:'Marhey',serif; font-size:clamp(3rem,11vw,7rem); font-weight:800; color:#f7e9c3; text-shadow:0 0 35px #d4af37,0 0 70px #b88a1e; letter-spacing:10px; margin-bottom:0.8rem; position:relative; z-index:2; animation:gentleGlow 2s infinite alternate; }
    .gate-sub { font-size:1.8rem; color:#e0b86e; letter-spacing:8px; font-weight:300; text-align:center; z-index:2; }
    .gate-date { font-size:1.2rem; color:rgba(255,215,140,0.8); margin-top:25px; letter-spacing:6px; z-index:2; }
    .gate-hint { position:absolute; bottom:40px; font-size:1rem; color:rgba(255,255,255,0.5); animation:fadeHint 2s infinite; letter-spacing:3px; z-index:2; }
    @keyframes gentleGlow { 0%{ text-shadow:0 0 25px #d4af37,0 0 50px #b8960c; } 100%{ text-shadow:0 0 50px #f1d9a7,0 0 100px #d4af37; } }
    @keyframes fadeHint { 0%,100%{ opacity:0.3; } 50%{ opacity:0.9; } }
    .countdown-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.96); display:flex; align-items:center; justify-content:center; z-index:999; opacity:0; visibility:hidden; transition:0.4s; }
    .countdown-number { font-size:10rem; font-weight:900; color:#f7e382; text-shadow:0 0 70px #f39c12; font-family:'Marhey',serif; animation:popIn 0.6s; }
    @keyframes popIn { 0%{ transform:scale(0.05); opacity:0; } 70%{ transform:scale(1.15); } 100%{ transform:scale(1); opacity:1; } }
    .instructions-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(5,2,15,0.97); display:flex; flex-direction:column; align-items:center; justify-content:center; z-index:998; opacity:0; visibility:hidden; transition:0.5s; padding:15px; }
    .instructions-card { background:rgba(20,10,40,0.85); backdrop-filter:blur(20px); -webkit-backdrop-filter:blur(20px); border:2px solid rgba(212,175,55,0.5); border-radius:25px; padding:20px; max-width:450px; width:100%; text-align:center; box-shadow:0 0 50px rgba(212,175,55,0.2); max-height:80vh; display:flex; flex-direction:column; }
    .instructions-card h3 { font-family:'Marhey',serif; font-size:1.4rem; color:#f7e05e; margin-bottom:10px; }
    .instructions-scroll { overflow-y:auto; max-height:42vh; padding:5px 10px; text-align:right; color:#e8dbb8; font-size:0.85rem; line-height:2rem; margin-bottom:8px; scroll-behavior:smooth; -webkit-overflow-scrolling:touch; }
    .instructions-scroll::-webkit-scrollbar { width:3px; }
    .instructions-scroll::-webkit-scrollbar-thumb { background:#d4af37; border-radius:6px; }
    .instructions-scroll ol { padding-right:15px; }
    .instructions-scroll ol li { margin-bottom:4px; }
    .btn-gold { background:linear-gradient(135deg,#d4af37,#f7e05e,#d4af37); border:none; padding:12px 38px; border-radius:35px; font-size:1.2rem; font-weight:700; cursor:pointer; font-family:'Marhey',serif; letter-spacing:2px; box-shadow:0 0 30px rgba(212,175,55,0.45); color:#1a0a00; transition:all 0.3s; display:none; margin-top:5px; }
    .btn-gold.show { display:inline-block; }
    .btn-gold:active { transform:scale(0.94); }
    .swipe-hint { color:#ffd966; font-size:0.7rem; margin-top:3px; animation:fadeHint 2s infinite; }
    .main-stage { position:fixed; top:0; left:0; width:100%; height:100%; background:radial-gradient(ellipse at 50% 40%,#2a1e42 0%,#0f0a1c 50%,#070311 100%); overflow:hidden; display:flex; flex-direction:column; align-items:center; justify-content:center; opacity:0; visibility:hidden; transition:opacity 0.7s,visibility 0.7s; z-index:10; }
    .main-stage.active { opacity:1; visibility:visible; }
    .stars-bg { position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:0; }
    .star-dot { position:absolute; background:white; border-radius:50%; animation:twinkleStar 3s infinite alternate; }
    @keyframes twinkleStar { 0%{ opacity:0.2; transform:scale(0.5); } 100%{ opacity:1; transform:scale(1.2); } }
    .balloons-layer { position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:1; }
    .balloon { position:absolute; bottom:-200px; width:65px; height:85px; border-radius:50% 50% 45% 45%; animation:floatBalloon linear infinite; box-shadow:0 15px 25px rgba(0,0,0,0.4); opacity:0.7; }
    .balloon::after { content:''; position:absolute; bottom:-14px; left:50%; transform:translateX(-50%); width:2px; height:22px; background:#888; border-radius:2px; }
    .balloon:nth-child(1){ left:8%; background:radial-gradient(circle at 35% 30%,#ffb8d0,#e84393); animation-duration:14s; }
    .balloon:nth-child(2){ left:28%; background:radial-gradient(circle at 35% 30%,#ffe082,#f39c12); animation-duration:16s; animation-delay:2s; }
    .balloon:nth-child(3){ left:52%; background:radial-gradient(circle at 35% 30%,#ff8080,#c0392b); animation-duration:13s; animation-delay:1s; }
    .balloon:nth-child(4){ left:74%; background:radial-gradient(circle at 35% 30%,#80e5ff,#0984e3); animation-duration:15s; animation-delay:3s; }
    .balloon:nth-child(5){ left:88%; background:radial-gradient(circle at 35% 30%,#f8a5e8,#6c5ce7); animation-duration:17s; animation-delay:0.5s; }
    @keyframes floatBalloon { 0%{ transform:translateY(0) rotate(0deg); opacity:0.6; } 30%{ transform:translateY(-30vh) rotate(6deg); opacity:0.8; } 70%{ transform:translateY(-65vh) rotate(-4deg); opacity:0.4; } 100%{ transform:translateY(-115vh) rotate(12deg); opacity:0; } }
    .confetti-layer { position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:2; }
    .butterfly { position:absolute; font-size:2.5rem; z-index:15; pointer-events:none; }
    @keyframes flyUp { 0%{ bottom:-50px; opacity:1; transform:scale(0.5); } 50%{ opacity:1; transform:scale(1); } 100%{ bottom:110vh; opacity:0; transform:scale(0.8); } }
    .greeting-card { position:relative; z-index:10; background:rgba(18,12,28,0.7); backdrop-filter:blur(25px); -webkit-backdrop-filter:blur(25px); border:2px solid rgba(212,175,55,0.5); border-radius:28px; padding:1.2rem 1.5rem; text-align:center; box-shadow:0 30px 55px rgba(0,0,0,0.75); max-width:480px; width:90%; color:#f3e7c4; transition:all 0.5s; }
    .greeting-card.hidden { opacity:0; transform:scale(0.5); pointer-events:none; }
    .greeting-card h2 { font-family:'Marhey',serif; font-size:1.6rem; color:#f7e05e; text-shadow:0 0 18px #d4af37; margin-bottom:0.3rem; }
    .greeting-card .name { font-size:2.3rem; font-weight:900; background:linear-gradient(to left,#ffe484,#f7d44a,#ffeb8a); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; margin:5px 0; }
    .greeting-card .msg { font-size:0.95rem; line-height:1.8rem; white-space:pre-line; max-height:110px; overflow-y:auto; color:#ede2c6; margin-bottom:12px; -webkit-overflow-scrolling:touch; }
    .msg::-webkit-scrollbar { width:3px; } .msg::-webkit-scrollbar-thumb { background:#d4af37; border-radius:8px; }
    .btn-read-done { background:linear-gradient(135deg,#d4af37,#f7e05e); border:none; padding:9px 26px; border-radius:28px; font-size:0.95rem; font-weight:700; cursor:pointer; font-family:'Marhey',serif; letter-spacing:2px; box-shadow:0 0 18px rgba(212,175,55,0.35); color:#1a0a00; }
    .btn-read-done:active { transform:scale(0.93); }
    .heart-overlay { position:fixed; top:0; left:0; width:100%; height:100%; z-index:90; display:flex; flex-direction:column; align-items:center; justify-content:center; opacity:0; pointer-events:none; transition:opacity 0.5s; }
    .heart-overlay.active { opacity:1; pointer-events:auto; }
    .giant-heart { font-size:10rem; animation:heartBeat 0.9s infinite; filter:drop-shadow(0 0 45px #ff3366); cursor:pointer; }
    @keyframes heartBeat { 0%,100%{ transform:scale(1); } 25%{ transform:scale(1.16); } 50%{ transform:scale(0.94); } 75%{ transform:scale(1.1); } }
    .heart-hint { color:#ffe0e0; font-size:1.2rem; margin-top:12px; animation:fadeHint 2s infinite; text-shadow:0 0 18px #ff6b9d; font-weight:600; }
    .heart-particles-container { position:fixed; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:91; }
    .name-reveal { position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); font-size:4rem; font-weight:900; color:#ffe484; z-index:95; pointer-events:none; opacity:0; transition:opacity 1s; text-shadow:0 0 50px #ffd700; font-family:'Marhey',serif; letter-spacing:12px; }
    .love-envelope { position:absolute; font-size:2.5rem; z-index:30; cursor:pointer; filter:drop-shadow(0 0 15px #ff9ff3); animation:envelopeFloat 3s infinite alternate; }
    .love-envelope:active { transform:scale(1.25); }
    @keyframes envelopeFloat { 0%{ transform:translateY(0px); } 100%{ transform:translateY(-18px); } }
    .envelope-open-msg { position:fixed; top:25%; left:50%; transform:translateX(-50%); background:rgba(15,5,25,0.93); border:2px solid #ff9ff3; padding:16px 28px; border-radius:22px; color:#ffd6ea; font-size:1.3rem; z-index:100; text-align:center; backdrop-filter:blur(16px); -webkit-backdrop-filter:blur(16px); box-shadow:0 0 40px rgba(255,105,180,0.45); display:none; white-space:nowrap; animation:popMsg 0.4s; max-width:90vw; overflow:hidden; text-overflow:ellipsis; }
    @keyframes popMsg { 0%{ transform:translateX(-50%) scale(0.2); opacity:0; } 100%{ transform:translateX(-50%) scale(1); opacity:1; } }
    .skip-btn { position:fixed; bottom:25px; right:60px; z-index:110; background:rgba(255,255,255,0.08); border:1.5px solid rgba(255,215,0,0.45); color:#ffd966; padding:7px 16px; border-radius:22px; cursor:pointer; font-family:'Tajawal',sans-serif; font-weight:600; font-size:0.85rem; backdrop-filter:blur(8px); -webkit-backdrop-filter:blur(8px); }
    .skip-btn:active { background:rgba(255,215,0,0.18); }
    .cake-emoji { font-size:8rem; cursor:pointer; z-index:10; filter:drop-shadow(0 0 30px rgba(255,200,0,0.7)); animation:cakeBounce 1.5s infinite; }
    .cake-emoji:active { transform:scale(1.08); }
    @keyframes cakeBounce { 0%,100%{ transform:translateY(0); } 50%{ transform:translateY(-14px); } }
    .hidden-rose-el { position:absolute; font-size:2.8rem; z-index:35; cursor:pointer; filter:drop-shadow(0 0 18px #ff6b9d); animation:gentlePulse 1.5s infinite; }
    @keyframes gentlePulse { 0%{ transform:scale(1); } 50%{ transform:scale(1.25); } 100%{ transform:scale(1); } }
    .secret-box { position:absolute; font-size:4rem; cursor:pointer; z-index:30; filter:drop-shadow(0 0 20px #d4af37); animation:boxGlow 2s infinite alternate; }
    @keyframes boxGlow { 0%{ filter:drop-shadow(0 0 15px #d4af37); } 100%{ filter:drop-shadow(0 0 35px #ffd700); } }
    .teddy-bear { position:absolute; font-size:5rem; cursor:pointer; z-index:30; filter:drop-shadow(0 0 20px #ff9f70); animation:teddyWiggle 3s infinite; }
    @keyframes teddyWiggle { 0%,100%{ transform:rotate(0deg); } 25%{ transform:rotate(5deg); } 75%{ transform:rotate(-5deg); } }
    .floating-heart-msg { position:absolute; font-size:2.5rem; cursor:pointer; z-index:30; animation:heartFloat 3s infinite alternate; filter:drop-shadow(0 0 12px #ff6b9d); transition:transform 0.2s; }
    .floating-heart-msg:active { transform:scale(1.3); }
    @keyframes heartFloat { 0%{ transform:translateY(0px); } 100%{ transform:translateY(-15px); } }
    .candle-light-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index:85; display:flex; flex-direction:column; align-items:center; justify-content:center; opacity:0; pointer-events:none; transition:opacity 0.8s; }
    .candle-icon { font-size:5rem; filter:drop-shadow(0 0 35px #ffaa00); animation:candleFlicker 1.5s infinite alternate; }
    @keyframes candleFlicker { 0%{ filter:drop-shadow(0 0 25px #ffaa00); transform:scale(1); } 100%{ filter:drop-shadow(0 0 55px #ffdd55); transform:scale(1.05); } }
    .light-reveal-text { color:#fff5d1; font-size:1.4rem; margin-top:20px; text-shadow:0 0 40px #ffd700; font-family:'Marhey',serif; text-align:center; animation:fadeInText 2s; padding:0 10px; }
    @keyframes fadeInText { 0%{ opacity:0; } 100%{ opacity:1; } }
    .ribbon-bow { position:absolute; font-size:5rem; cursor:pointer; z-index:30; filter:drop-shadow(0 0 20px #ff9ff3); animation:ribbonWiggle 2s infinite; }
    @keyframes ribbonWiggle { 0%,100%{ transform:rotate(0deg); } 25%{ transform:rotate(8deg); } 75%{ transform:rotate(-8deg); } }
    .wish-candle { position:absolute; font-size:2rem; cursor:pointer; z-index:25; filter:drop-shadow(0 0 10px #ffaa00); animation:candleGlow 1.5s infinite alternate; transition:all 0.3s; }
    .wish-candle.extinguished { filter:grayscale(1); opacity:0.45; animation:none; }
    @keyframes candleGlow { 0%{ filter:drop-shadow(0 0 8px #ffaa00); } 100%{ filter:drop-shadow(0 0 22px #ff6600); } }
    .wish-message { position:fixed; top:18%; left:50%; transform:translateX(-50%); background:rgba(15,5,25,0.93); border:2px solid #ffd700; padding:16px 28px; border-radius:22px; color:#ffe484; font-size:1.2rem; z-index:100; text-align:center; backdrop-filter:blur(16px); -webkit-backdrop-filter:blur(16px); box-shadow:0 0 40px rgba(255,200,0,0.45); display:none; white-space:nowrap; animation:popMsg 0.4s; max-width:90vw; overflow:hidden; text-overflow:ellipsis; }
    .lock-container { position:fixed; top:0; left:0; width:100%; height:100%; z-index:83; display:flex; flex-direction:column; align-items:center; justify-content:center; opacity:0; pointer-events:none; transition:opacity 0.5s; background:rgba(0,0,0,0.8); }
    .lock-icon { font-size:6rem; filter:drop-shadow(0 0 25px #ffd700); }
    .lock-input { background:rgba(255,255,255,0.1); border:2px solid #ffd700; color:#ffd700; font-size:1.5rem; padding:8px 16px; border-radius:15px; text-align:center; margin:15px 0; width:140px; font-family:'Marhey',serif; outline:none; }
    .lock-hint { color:#ffd966; font-size:0.9rem; margin-bottom:5px; }
    .ring-box { position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); font-size:7rem; cursor:pointer; z-index:84; filter:drop-shadow(0 0 30px #d4af37); animation:boxBounce 0.8s infinite alternate; }
    @keyframes boxBounce { 0%{ transform:translate(-50%,-50%) scale(1); } 100%{ transform:translate(-50%,-50%) scale(1.08); } }
    .necklace-container { position:fixed; top:0; left:0; width:100%; height:100%; z-index:85; display:flex; flex-direction:column; align-items:center; justify-content:center; opacity:0; pointer-events:none; transition:opacity 0.5s; background:rgba(0,0,0,0.85); }
    .necklace-icon { font-size:6rem; cursor:pointer; filter:drop-shadow(0 0 30px #ffd700); animation:boxBounce 0.8s infinite alternate; }
    .necklace-message { color:#ffe484; font-size:1.1rem; text-align:center; margin-top:20px; line-height:2.2rem; font-family:'Cairo',serif; text-shadow:0 0 20px #ffd700; animation:fadeInText 2s; max-width:90vw; white-space:pre-line; display:none; }
    .final-screen { position:fixed; top:0; left:0; width:100%; height:100%; display:flex; flex-direction:column; align-items:center; justify-content:center; z-index:300; background:rgba(0,0,0,0.92); opacity:0; visibility:hidden; transition:1s; }
    .final-heart { font-size:9rem; animation:finalBeat 1.5s infinite; filter:drop-shadow(0 0 60px #ff3366); }
    @keyframes finalBeat { 0%,100%{ transform:scale(1); } 50%{ transform:scale(1.2); } }
    .final-text { font-size:2rem; color:#ffe484; font-family:'Marhey',serif; margin-top:20px; text-shadow:0 0 40px gold; text-align:center; padding:0 15px; }
    .floating-msg { position:fixed; top:22%; left:50%; transform:translateX(-50%); background:rgba(15,5,25,0.92); border:2px solid #ff9ff3; padding:18px 28px; border-radius:24px; color:#ffd6ea; font-size:1.2rem; z-index:100; text-align:center; backdrop-filter:blur(16px); -webkit-backdrop-filter:blur(16px); box-shadow:0 0 40px rgba(255,105,180,0.45); display:none; white-space:nowrap; animation:popMsg 0.4s; max-width:90vw; overflow:hidden; text-overflow:ellipsis; pointer-events:none; }
    .time-counter { position:fixed; bottom:10px; left:10px; color:#ffd966; font-size:0.8rem; z-index:300; font-family:'Cairo'; }
    @media (max-width:480px) { .gate-text{font-size:3rem;} .gate-sub{font-size:1.3rem;} .countdown-number{font-size:7rem;} .giant-heart{font-size:7rem;} .cake-emoji{font-size:6rem;} .name-reveal{font-size:2.8rem;} .final-heart{font-size:6rem;} .final-text{font-size:1.6rem;} .ring-box{font-size:5rem;} .lock-icon{font-size:4rem;} .teddy-bear{font-size:3.5rem;} .secret-box{font-size:3rem;} .ribbon-bow{font-size:3.5rem;} .necklace-icon{font-size:4rem;} .candle-icon{font-size:3.5rem;} .light-reveal-text{font-size:1.2rem;} .wish-candle{font-size:1.5rem;} }
  </style>
</head>
<body>

<div id="stageIndicator" class="stage-indicator" style="opacity:0"></div>
<div id="topInstruction" class="top-instruction" style="opacity:0"></div>
<div id="soundToggle" class="sound-toggle" style="display:none">🔊</div>
<div id="gateScreen" class="gate-container"><div class="gate-glow"></div><div class="gate-text">✨</div><div class="gate-sub">ليلة Reem</div><div class="gate-date">🌟 3 / 5 / 2026 🌟</div><div class="gate-hint">اضغطي مرتين للدخول 💫</div></div>
<div id="countdownOverlay" class="countdown-overlay"><div id="countdownNumber" class="countdown-number">3</div></div>
<div id="instructionsOverlay" class="instructions-overlay"><div class="instructions-card"><h3>📜 تعليمات الرحلة</h3><div class="instructions-scroll" id="instructionsScroll"><ol><li>🦋 الفراشات تحمل البطاقة</li><li>💌 اقرئي الرسالة</li><li>💓 قلب نابض.. اضغطي مرتين</li><li>💥 انفجار القلب ورسائل حب</li><li>🎂 الكيكة العملاقة</li><li>💌 أظرف الحب المتناثرة</li><li>🌹 الوردة المخفية</li><li>🎀 علبة الأسرار</li><li>🧸 دمية الذكريات</li><li>💌 رسائل من القلب</li><li>🕯️ ضو شمعة</li><li>🎀 فيونكة الحب</li><li>🕯️ 18 شمعة أمنيات</li><li>🔐 قفل الحب</li><li>💍 صندوق الخاتم</li><li>🗝️ قلادة القفل الذهبية</li><li>😈 خطأ مزيف</li><li>🧩 لغز الاسم</li><li>💖 النهاية الكبرى</li></ol></div><span class="swipe-hint">👆 اسحبي للأعلى للمزيد</span><button class="btn-gold" id="btnStartJourney">🌟 هيا نبدأ 🌟</button></div></div>
<div id="mainStage" class="main-stage"><div class="stars-bg" id="starsBg"></div><div class="balloons-layer"><div class="balloon"></div><div class="balloon"></div><div class="balloon"></div><div class="balloon"></div><div class="balloon"></div></div><div class="confetti-layer" id="confettiLayer"></div><div id="greetingCard" class="greeting-card hidden"><h2>🎀 عيد ميلاد سعيد 🎀</h2><div class="name">يا Reem</div><div class="msg" id="personalMessage"></div><button class="btn-read-done" id="btnReadDone">أتممت القراءة 💛</button></div></div>
<div id="heartOverlay" class="heart-overlay"><div id="giantHeart" class="giant-heart">❤️</div><div id="heartHint" class="heart-hint">اضغطي مرتين على القلب 💕</div></div>
<div id="heartParticlesContainer" class="heart-particles-container"></div>
<div id="nameReveal" class="name-reveal">Reem</div>
<div id="envelopeOpenMsg" class="envelope-open-msg"></div>
<button id="skipEnvelopesBtn" class="skip-btn" style="display:none">تخطي ⏭️</button>
<div id="candleLightOverlay" class="candle-light-overlay"><div class="candle-icon">🕯️</div><div class="light-reveal-text" id="lightRevealText"></div></div>
<div id="wishMessage" class="wish-message"></div>
<button id="skipCandlesBtn" class="skip-btn" style="display:none">تخطي ⏭️</button>
<div id="lockContainer" class="lock-container"><div class="lock-icon">🔐</div><div class="lock-hint">🔑 ضع تاريخ ميلاد العسل</div><input type="text" id="lockInput" class="lock-input" placeholder="3/5" maxlength="3"><button class="btn-gold" id="btnUnlock" style="display:inline-block;margin-top:8px;">🔓 افتح</button></div>
<div id="necklaceContainer" class="necklace-container"><div class="necklace-icon" id="necklaceIcon">🗝️</div><div class="necklace-message" id="necklaceMessage"></div></div>
<div id="floatingMsg" class="floating-msg"></div>
<div id="finalScreen" class="final-screen"><div class="final-heart">💖</div><div class="final-text"></div></div>
<div id="timeCounter" class="time-counter"></div>

<script>
(function() {
  const gate=document.getElementById('gateScreen'),countdownOverlay=document.getElementById('countdownOverlay'),countdownNumber=document.getElementById('countdownNumber'),instructionsOverlay=document.getElementById('instructionsOverlay'),instructionsScroll=document.getElementById('instructionsScroll'),btnStartJourney=document.getElementById('btnStartJourney'),mainStage=document.getElementById('mainStage'),greetingCard=document.getElementById('greetingCard'),btnReadDone=document.getElementById('btnReadDone'),personalMsgEl=document.getElementById('personalMessage'),heartOverlay=document.getElementById('heartOverlay'),giantHeart=document.getElementById('giantHeart'),heartHint=document.getElementById('heartHint'),heartParticlesContainer=document.getElementById('heartParticlesContainer'),nameReveal=document.getElementById('nameReveal'),envelopeOpenMsg=document.getElementById('envelopeOpenMsg'),skipEnvelopesBtn=document.getElementById('skipEnvelopesBtn'),confettiLayer=document.getElementById('confettiLayer'),starsBg=document.getElementById('starsBg'),floatingMsg=document.getElementById('floatingMsg'),topInstruction=document.getElementById('topInstruction'),stageIndicator=document.getElementById('stageIndicator'),finalScreen=document.getElementById('finalScreen'),soundToggle=document.getElementById('soundToggle'),candleLightOverlay=document.getElementById('candleLightOverlay'),lightRevealText=document.getElementById('lightRevealText'),wishMessage=document.getElementById('wishMessage'),skipCandlesBtn=document.getElementById('skipCandlesBtn'),lockContainer=document.getElementById('lockContainer'),lockInput=document.getElementById('lockInput'),btnUnlock=document.getElementById('btnUnlock'),necklaceContainer=document.getElementById('necklaceContainer'),necklaceIcon=document.getElementById('necklaceIcon'),necklaceMessage=document.getElementById('necklaceMessage'),timeCounter=document.getElementById('timeCounter');

  let currentStage=0,totalStages=19,bgMusic=null,musicPlaying=!1,userClicks=0,idleTimer;

  // Typewriter حرف حرف وسطر سطر
  function typeWriter(text, element, speed = 40, callback = null) {
    element.textContent = "";
    const lines = text.split('\n');
    let lineIndex = 0;
    let charIndex = 0;
    function typeLine() {
      if (lineIndex < lines.length) {
        if (charIndex < lines[lineIndex].length) {
          element.textContent += lines[lineIndex].charAt(charIndex);
          charIndex++;
          element.scrollTop = element.scrollHeight;
          setTimeout(typeLine, speed);
        } else {
          if (lineIndex < lines.length - 1) element.textContent += '\n';
          lineIndex++;
          charIndex = 0;
          element.scrollTop = element.scrollHeight;
          setTimeout(typeLine, speed * 2);
        }
      } else { if (callback) callback(); }
    }
    typeLine();
  }

  const fullMessage="أتممتي اليوم يا زهرتي عامك الثامن عشر\nكل عام وانت قلبي وحياتي وعمري كله ورغم انني لم اعرفك سوى منذ بضعة اشهر الا انك منذ اللحظة الاولى لم تفارقي ذهني ولا قلبي لحظة واحدة احببتك وكانني اعرفك منذ سنين احببتك بعينين مغمضتين وقلب مفتوح لك وحدك اتمنى لك عاما مليئا بالخير والحب والنجاح عاما يليق بقلبك النقي وروحك الجميلة واتمنى ان تبقي دائما سعيدة ومطمئنة وقريبة مني كما انت الان يا قمري و اجمل صدفه في حياتي يا احلى شيء مر في دنيتي كل عام وانت النور الذي اختاره قلبي وكل عام وانت معي وكل عام وانا احبك يا كتكوتي";

  function fakeBugEffect(n){const g=document.createElement("div");g.style.cssText="position:fixed;top:0;left:0;width:100%;height:100%;background:black;color:red;display:flex;align-items:center;justify-content:center;font-size:1.5rem;z-index:999;text-align:center;";g.textContent="ERROR... SYSTEM FAILED 💔";document.body.appendChild(g);let s=setInterval(()=>{document.body.style.transform=`translate(${Math.random()*10-5}px, ${Math.random()*10-5}px)`},50);setTimeout(()=>{clearInterval(s);document.body.style.transform="none";g.textContent="خفتِ؟ 😏 مستحيل أخرب شي حلو زيك ❤️"},2000);setTimeout(()=>{g.remove();n&&n()},4000)}

  function namePuzzle(n){const l=["ب","ح","ب","ك"];let c=[];const b=document.createElement("div");b.style.cssText="position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:black;color:white;padding:20px;z-index:999;border-radius:20px;text-align:center;";b.innerHTML="<p>رتبي الكلمة 😏</p>";[...l].sort(()=>Math.random()-.5).forEach(t=>{const x=document.createElement("button");x.textContent=t;x.style.cssText="margin:5px;padding:10px;font-size:1.2rem;";x.onclick=()=>{c.push(t);x.disabled=!0;c.join("")==="بحبك"&&(b.innerHTML="<p>صح ❤️ بحبك</p>",setTimeout(()=>{b.remove(),n()},1e3))};b.appendChild(x)});document.body.appendChild(b)}

  function resetIdle(){clearTimeout(idleTimer);idleTimer=setTimeout(()=>{const m=document.createElement("div");m.textContent="ليش سكتي؟ 😢";m.style.cssText="position:fixed;bottom:50px;left:50%;transform:translateX(-50%);background:black;color:white;padding:10px 20px;border-radius:20px;z-index:999;";document.body.appendChild(m);setTimeout(()=>m.remove(),2000)},5000)}

  function dynamicEnding(){const e=["💖 أنتِ الحكاية اللي ما بدي تخلص... والقلب اللي ما بدو يرتاح إلا معكِ","💖 من يوم ما عرفتك وأنا عرفت إنو الحياة ممكن تكون حلوة... لأنكِ فيها","💖 أنتِ مش بس حب... أنتِ الروح اللي ساكنة فيّ... والقلب اللي بينبض باسمك","💖 لو يرجع فيي الزمن... برجع وقع في حبكِ من أول نظرة... ومن غير تفكير","💖 ما في كلام بيوصف شو أنتِ بالنسبة إلي... أنتِ كل شي حلو في دنيتي"];return e[Math.floor(Math.random()*e.length)]}

  document.addEventListener("mousemove",resetIdle);
  document.addEventListener("click",()=>{userClicks++;resetIdle()});
  document.addEventListener("touchstart",()=>{userClicks++;resetIdle()});
  resetIdle();

  const loveStartDate=new Date("2025-11-23");
  setInterval(()=>{const n=new Date(),d=Math.floor((n-loveStartDate)/864e5);timeCounter.textContent=`مع بعض من ${d} يوم ❤️`},1000);

  for(let i=0;i<totalStages;i++){const d=document.createElement('div');d.className='stage-dot';d.id='sdot-'+i;stageIndicator.appendChild(d)}
  const stageNumLabel=document.createElement('div');stageNumLabel.className='stage-num';stageNumLabel.id='stageNumLabel';stageNumLabel.textContent='1';stageIndicator.appendChild(stageNumLabel);

  function updateProgress(s){for(let i=0;i<totalStages;i++){const d=document.getElementById('sdot-'+i);d&&(d.classList.remove('active','done'),i<s?d.classList.add('done'):i===s&&d.classList.add('active'))}document.getElementById('stageNumLabel').textContent=s+1;stageIndicator.style.opacity=s>=0?'1':'0'}
  function setInstruction(t){topInstruction.textContent=t;topInstruction.style.opacity='1'}
  function hideInstruction(){topInstruction.style.opacity='0'}
  function playSound(s){try{const a=new Audio(s);a.volume=0.4;a.play().catch(()=>{})}catch(e){}}
  function reanim(e,a){e.style.animation='none';void e.offsetWidth;e.style.animation=a}

  function initBgMusic(){bgMusic||(bgMusic=new Audio("Floating in Reverie.mp3"),bgMusic.loop=!0,bgMusic.volume=0.3);bgMusic.play().catch(()=>{});musicPlaying=!0;soundToggle.textContent='🔊'}
  function toggleMusic(){bgMusic&&(musicPlaying?(bgMusic.pause(),musicPlaying=!1,soundToggle.textContent='🔇'):(bgMusic.play().catch(()=>{}),musicPlaying=!0,soundToggle.textContent='🔊'))}
  soundToggle.addEventListener('click',toggleMusic);

  for(let i=0;i<80;i++){const s=document.createElement('div');s.className='star-dot';s.style.left=Math.random()*100+'%';s.style.top=Math.random()*100+'%';s.style.width=s.style.height=Math.random()*3+1+'px';s.style.animationDelay=Math.random()*3+'s';starsBg.appendChild(s)}

  function spawnConfetti(){const c=document.createElement('div');c.style.cssText=`position:absolute;top:-25px;left:${Math.random()*100}%;width:${Math.random()*12+6}px;height:${Math.random()*12+6}px;background:${['#f1c40f','#e74c3c','#3498db','#9b59b6','#f39c12','#ff6b9d'][Math.floor(Math.random()*6)]};border-radius:${Math.random()>.5?'50%':'3px'};animation:confettiDrop ${Math.random()*2.5+2.5}s linear forwards;pointer-events:none;`;confettiLayer.appendChild(c);setTimeout(()=>c.remove(),3500)}
  const cfStyle=document.createElement('style');cfStyle.textContent='@keyframes confettiDrop{0%{transform:translateY(-25px) rotate(0deg);opacity:1}100%{transform:translateY(105vh) rotate(700deg);opacity:0}}';document.head.appendChild(cfStyle);
  setInterval(()=>{mainStage.classList.contains('active')&&currentStage>=0&&(()=>{for(let i=0;i<2;i++)spawnConfetti()})()},350);

  setInstruction('اضغطي مرتين على الشاشة للدخول ✨');updateProgress(0);
  gate.addEventListener('dblclick',()=>{gate.classList.add('fade-out');setInstruction('استعدي للعد التنازلي ⏳');startCountdown()});

  function startCountdown(){countdownOverlay.style.opacity='1';countdownOverlay.style.visibility='visible';let c=3;countdownNumber.textContent=c;reanim(countdownNumber,'popIn 0.6s');const i=setInterval(()=>{c--;c>0?(countdownNumber.textContent=c,reanim(countdownNumber,'popIn 0.6s')):(clearInterval(i),countdownOverlay.style.opacity='0',countdownOverlay.style.visibility='hidden',setInstruction('اقرئي التعليمات واسحبي للأعلى 📜'),showInstructions())},1000)}

  function showInstructions(){instructionsOverlay.style.opacity='1';instructionsOverlay.style.visibility='visible';btnStartJourney.classList.remove('show');instructionsScroll.scrollTop=0;instructionsScroll.addEventListener('scroll',function cs(){instructionsScroll.scrollTop+instructionsScroll.clientHeight>=instructionsScroll.scrollHeight-15&&(btnStartJourney.classList.add('show'),instructionsScroll.removeEventListener('scroll',cs))})}
  btnStartJourney.addEventListener('click',()=>{instructionsOverlay.style.opacity='0';instructionsOverlay.style.visibility='hidden';startMainScene()});

  function startMainScene(){mainStage.classList.add('active');soundToggle.style.display='flex';initBgMusic();currentStage=0;updateProgress(0);setInstruction('شاهدي الفراشات تحمل البطاقة 🦋');spawnButterflies()}

  function spawnButterflies(){
    ['🦋','🦋','🦋','🦋','🦋'].forEach((b,i)=>{
      setTimeout(()=>{
        const d=document.createElement('div');
        d.className='butterfly'; d.textContent=b;
        d.style.left=(15+i*16)+'%'; d.style.bottom='-60px';
        d.style.animation='flyUp 3s ease-out forwards';
        mainStage.appendChild(d);
        setTimeout(()=>d.remove(),3500);
      },i*300);
    });
    setTimeout(()=>{
      greetingCard.classList.remove('hidden');
      currentStage=1;updateProgress(1);
      setInstruction('اقرئي رسالتك واضغطي "أتممت القراءة" 💌');
      btnReadDone.style.display='none';
      typeWriter(fullMessage, personalMsgEl, 35, function() {
        btnReadDone.style.display='inline-block';
      });
    },2200);
  }

  btnReadDone.addEventListener('click',()=>{greetingCard.style.display='none';greetingCard.classList.add('hidden');currentStage=2;updateProgress(2);setInstruction('اضغطي مرتين على القلب 💓');showHeart()});

  function showHeart(){heartOverlay.classList.add('active');heartHint.style.display='block';let c=0,t;giantHeart.addEventListener('click',()=>{c++;c===1?t=setTimeout(()=>{c=0},450):c===2&&(clearTimeout(t),c=0,burstHeart())})}

  function burstHeart(){heartOverlay.classList.remove('active');heartHint.style.display='none';currentStage=3;updateProgress(3);setInstruction('القلب انفجر! 10 رسائل حب 💌');const r=giantHeart.getBoundingClientRect(),cx=r.left+r.width/2,cy=r.top+r.height/2;for(let i=0;i<40;i++){const p=document.createElement('div');p.style.cssText=`position:absolute;font-size:1.5rem;pointer-events:none;left:${cx}px;top:${cy}px;animation:pf 2.6s forwards;filter:drop-shadow(0 0 12px hotpink);--px:${Math.random()*600-300}px;--py:${Math.random()*600-300}px;`;p.textContent=['❤️','💕','💖','💗','✨','💫'][Math.floor(Math.random()*6)];heartParticlesContainer.appendChild(p);setTimeout(()=>p.remove(),2700)}const pfStyle=document.createElement('style');pfStyle.textContent='@keyframes pf{0%{transform:translate(0,0) scale(1);opacity:1}100%{transform:translate(var(--px),var(--py)) scale(0);opacity:0}}';document.head.appendChild(pfStyle);nameReveal.style.opacity='1';setTimeout(()=>{nameReveal.style.opacity='0'},3200);playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');setTimeout(()=>spawnLoveEnvelopes(),2000)}

  const loveMsgs=['💖  أنتِ أجمل صدفة في حياتي','💖 عيناكِ عالمي','💖 ابتسامتك بلدنيا وما فيها','💖 أحبك يا روحي','💖  أنتِ حلمي وهدفي','💖 وجودك نعمة في حياتي','💖 قلبي اختار ولم يختر الا ريمي','💖 كل عام وأنتِ عمري','💖 أنتِ نجمتي بسماء عيني','💖 أحبك يسكنني'];
  let envelopesOpened=0;
  function spawnLoveEnvelopes(){skipEnvelopesBtn.style.display='block';const z=[[5,15,10,28],[25,38,12,30],[55,68,10,28],[78,92,14,32],[5,15,42,58],[30,44,48,65],[58,74,46,62],[80,93,48,65],[15,32,72,88],[62,80,70,86]];for(let i=0;i<10;i++){const e=document.createElement('div');e.className='love-envelope';e.textContent='💌';e.style.left=(z[i][0]+Math.random()*(z[i][1]-z[i][0]))+'%';e.style.top=(z[i][2]+Math.random()*(z[i][3]-z[i][2]))+'%';e.style.animationDelay=Math.random()*2+'s';const m=loveMsgs[i];e.addEventListener('click',function h(){envelopeOpenMsg.textContent=m;envelopeOpenMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');e.style.opacity='0';e.style.pointerEvents='none';setTimeout(()=>{envelopeOpenMsg.style.display='none';e.remove()},10000);envelopesOpened++;envelopesOpened>=10&&setTimeout(()=>goToCake(),2000);e.removeEventListener('click',h)});document.body.appendChild(e)}}
  skipEnvelopesBtn.addEventListener('click',()=>{document.querySelectorAll('.love-envelope').forEach(e=>e.remove());envelopeOpenMsg.style.display='none';skipEnvelopesBtn.style.display='none';envelopesOpened=10;goToCake()});
  function goToCake(){skipEnvelopesBtn.style.display='none';document.querySelectorAll('.love-envelope').forEach(e=>e.remove());currentStage=4;updateProgress(4);setInstruction('اضغطي على الكيكة 🎂');showCake()}

  function showCake(){const c=document.createElement('div');c.className='cake-emoji';c.textContent='🎂';c.id='cakeEmoji';c.style.position='relative';c.style.zIndex='10';mainStage.appendChild(c);c.addEventListener('click',function h(){floatingMsg.textContent='💖 كل عام وأنتِ قلبي يا ريم 💖';floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');spawnFlying18();c.style.transform='scale(1.2)';setTimeout(()=>{c.style.transform='';floatingMsg.style.display='none'},2800);c.removeEventListener('click',h);setTimeout(()=>{c.remove();goToRose()},3500)})}
  function spawnFlying18(){for(let i=0;i<18;i++){const e=document.createElement('div');e.style.cssText=`position:absolute;font-size:2.5rem;font-weight:900;color:#ffd966;z-index:25;pointer-events:none;animation:f18 2s forwards;left:${Math.random()*80}%;top:${Math.random()*60}%;--dx:${Math.random()*200-100}px;--dy:${Math.random()*-300-80}px;--rot:${Math.random()*600-300}deg;`;e.textContent='18';mainStage.appendChild(e);setTimeout(()=>e.remove(),2100)}const s=document.createElement('style');s.textContent='@keyframes f18{0%{opacity:1;transform:translate(0,0) scale(0.3) rotate(0)}100%{opacity:0;transform:translate(var(--dx),var(--dy)) scale(1.4) rotate(var(--rot))}}';document.head.appendChild(s)}
  function goToRose(){currentStage=5;updateProgress(5);setInstruction('ابحثي عن الوردة المخفية 🌹');spawnRose()}

  function spawnRose(){const r=document.createElement('div');r.className='hidden-rose-el';r.textContent='🌹';r.style.left=(Math.random()*65+15)+'%';r.style.top=(Math.random()*45+25)+'%';r.addEventListener('click',function h(){floatingMsg.textContent='💘 أنتِ أجمل صدفة في حياتي 💘';floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');r.remove();setTimeout(()=>floatingMsg.style.display='none',3200);setTimeout(()=>goToSecretBox(),3800);r.removeEventListener('click',h)});mainStage.appendChild(r)}
  function goToSecretBox(){currentStage=6;updateProgress(6);setInstruction('افتحي علبة الأسرار 🎀');spawnSecretBox()}

  function spawnSecretBox(){const b=document.createElement('div');b.className='secret-box';b.textContent='🎀';b.style.left='50%';b.style.top='40%';b.style.transform='translate(-50%,-50%)';const s=['💖 أنتِ حلمي اللي ما كنت أتخيله','💖 حبكِ أحلى شي صار بحياتي','💖 قلبي دق وقال هيدي اللي بدو ياها','💖 وجودكِ بالدنيا هو السر'];b.addEventListener('click',function h(){b.textContent='✨';floatingMsg.textContent=s[Math.floor(Math.random()*s.length)];floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');setTimeout(()=>floatingMsg.style.display='none',3500);b.removeEventListener('click',h);setTimeout(()=>{b.remove();goToTeddy()},4000)});mainStage.appendChild(b)}
  function goToTeddy(){currentStage=7;updateProgress(7);setInstruction('اضغطي على الدبدوب 🧸');spawnTeddy()}

  function spawnTeddy(){const t=document.createElement('div');t.className='teddy-bear';t.textContent='🧸';t.style.left='50%';t.style.top='45%';t.style.transform='translate(-50%,-50%)';t.addEventListener('click',function h(){t.textContent='🧸💕';floatingMsg.textContent='🧸 أحبكِ من كل قلبي يا Reem 🧸';floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');setTimeout(()=>floatingMsg.style.display='none',3500);t.removeEventListener('click',h);setTimeout(()=>{t.remove();goToFloatingHearts()},4000)});mainStage.appendChild(t)}
  function goToFloatingHearts(){currentStage=8;updateProgress(8);setInstruction('المسي القلوب الطائرة 💌');spawnFloatingHearts()}

  const floatingMessages=['💖 أنتِ حبي','💖 أنتِ حبيبتي','💖 أنتِ كتكوتي','💖 أنتِ حياتي','💖 أنتِ روحي','💖 أنتِ بطتي','💖 أنتِ زوجتي (يا رب)'];
  let floatingMsgIndex=0;
  function spawnFloatingHearts(){
    const shuffled=[...floatingMessages].sort(()=>Math.random()-.5);
    for(let i=0;i<7;i++){
      const h=document.createElement('div');
      h.className='floating-heart-msg';h.textContent='💗';
      h.style.left=(10+i*12)+'%';h.style.top=(30+Math.random()*40)+'%';
      h.style.animationDelay=Math.random()*2+'s';
      const msg=shuffled[i];
      h.addEventListener('click',function x(){
        floatingMsg.textContent=msg;floatingMsg.style.display='block';
        h.style.opacity='0';h.style.pointerEvents='none';
        setTimeout(()=>{floatingMsg.style.display='none';h.remove()},10000);
        floatingMsgIndex++;
        if(floatingMsgIndex>=7) setTimeout(()=>goToCandleLight(),2500);
        h.removeEventListener('click',x);
      });
      mainStage.appendChild(h);
    }
  }
  function goToCandleLight(){currentStage=9;updateProgress(9);setInstruction('شاهدي ضو الشمعة 🕯️');showCandleLight()}

  function showCandleLight(){candleLightOverlay.style.opacity='1';candleLightOverlay.style.pointerEvents='auto';lightRevealText.textContent='قالوا النور بيجي من الشمس... أنا النور جاي من عينيكِ. قالوا الضوء بيفرق الظلام... أنا ضحكتك بتفرق حزني. قالوا في ناس بتنور الحياة... أنا لقيتكِ';candleLightOverlay.addEventListener('click',function h(){candleLightOverlay.style.opacity='0';candleLightOverlay.style.pointerEvents='none';candleLightOverlay.removeEventListener('click',h);setTimeout(()=>goToRibbon(),1000)})}
  function goToRibbon(){currentStage=10;updateProgress(10);setInstruction('فكي الفيونكة 🎀');spawnRibbon()}

  function spawnRibbon(){const r=document.createElement('div');r.className='ribbon-bow';r.textContent='🎀';r.style.left='50%';r.style.top='40%';r.style.transform='translate(-50%,-50%)';r.addEventListener('click',function h(){r.textContent='💝';floatingMsg.textContent='💝 فكيتِ الفيونكة... وطلع قلبي أنا ملككِ 💝';floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/birthday-01.mp3');setTimeout(()=>floatingMsg.style.display='none',3500);r.removeEventListener('click',h);setTimeout(()=>{r.style.display='none';r.remove();goToWishCandles()},4000)});mainStage.appendChild(r)}

  function goToWishCandles(){currentStage=11;updateProgress(11);setInstruction('أطفئي 18 شمعة أمنيات 🕯️');spawnWishCandles()}

  const wishesList=['🤍 راحة البال','✨ تحقيق الأحلام','💪 القوة والعزيمة','💖 حب يدوم','🌈 أيام مليانة فرح','😊 ضحكة متواصلة','💰 رزق واسع','🕰️ عمر مديد','👭 أصدقاء أوفياء','☮️ طمأنينة القلب','🌍 سفر ومغامرات','📸 ذكريات جميلة','📚 توفيق ونجاح','👨‍👩‍👧 لمّة العيلة','☀️ صباحات مشرقة','💓 قلب سليم','🌅 ليالي هادئة','👑 عز وفخر'];
  let wishCandlesExtinguished=0,wishMessageTimeout=null;
  function spawnWishCandles(){skipCandlesBtn.style.display='block';for(let i=0;i<18;i++){const c=document.createElement('div');c.className='wish-candle';c.textContent='🕯️';c.style.left=(4+(i%9)*10.5)+'%';c.style.top=(18+Math.floor(i/9)*28)+'%';const w=wishesList[i];c.addEventListener('click',function h(){if(c.classList.contains('extinguished'))return;c.classList.add('extinguished');wishMessage.textContent='✨ '+w+' ✨';wishMessage.style.display='block';playSound('https://www.soundjay.com/misc/sounds/small-bell-ring-01.mp3');wishMessageTimeout&&clearTimeout(wishMessageTimeout);wishMessageTimeout=setTimeout(()=>{wishMessage.style.display='none'},30000);wishCandlesExtinguished++;wishCandlesExtinguished>=18&&setTimeout(()=>{wishMessage.style.display='none';skipCandlesBtn.style.display='none';goToLock()},2000);c.removeEventListener('click',h)});mainStage.appendChild(c)}}
  skipCandlesBtn.addEventListener('click',()=>{document.querySelectorAll('.wish-candle').forEach(c=>c.remove());wishMessage.style.display='none';skipCandlesBtn.style.display='none';goToLock()});
  function goToLock(){currentStage=12;updateProgress(12);setInstruction('افتحي قفل الحب 🔐');showLock()}

  function showLock(){lockContainer.style.opacity='1';lockContainer.style.pointerEvents='auto';lockInput.value='';lockInput.focus();btnUnlock.addEventListener('click',function h(){lockInput.value==='35'||lockInput.value==='3/5'||lockInput.value==='٣/٥'?(lockContainer.style.opacity='0',lockContainer.style.pointerEvents='none',floatingMsg.textContent='🔓 القفل فُتح! حبكِ مفتاح قلبي 🔓',floatingMsg.style.display='block',playSound('https://www.soundjay.com/human/sounds/applause-01.mp3'),setTimeout(()=>floatingMsg.style.display='none',3500),btnUnlock.removeEventListener('click',h),setTimeout(()=>goToRingBox(),4000)):(lockInput.style.borderColor='#ff4444',setTimeout(()=>lockInput.style.borderColor='#ffd700',800))})}
  function goToRingBox(){currentStage=13;updateProgress(13);setInstruction('افتحي صندوق الخاتم 💍');spawnRingBox()}

  function spawnRingBox(){const r=document.createElement('div');r.className='ring-box';r.textContent='🎁';document.body.appendChild(r);r.addEventListener('click',function h(){r.textContent='💍';const bc=document.createElement('div');bc.style.cssText="position:fixed;bottom:25%;left:50%;transform:translateX(-50%);display:flex;gap:20px;z-index:200;";const by=document.createElement('button');by.textContent='💖 طبعاً أقبل';by.style.cssText="padding:12px 28px;border-radius:25px;border:none;background:linear-gradient(135deg,#4CAF50,#8BC34A);color:white;font-size:1.1rem;font-weight:700;cursor:pointer;box-shadow:0 0 20px rgba(76,175,80,0.5);font-family:'Cairo',sans-serif;";const bn=document.createElement('button');bn.textContent='💔 مش متأكدة';bn.style.cssText="padding:12px 28px;border-radius:25px;border:none;background:linear-gradient(135deg,#f44336,#e91e63);color:white;font-size:1.1rem;font-weight:700;cursor:pointer;box-shadow:0 0 20px rgba(244,67,54,0.5);font-family:'Cairo',sans-serif;";by.addEventListener('click',()=>{bc.remove();floatingMsg.textContent='💍 الحمد لله قبلتي! من اليوم قلبي ملككِ 💍';floatingMsg.style.display='block';playSound('https://www.soundjay.com/human/sounds/applause-01.mp3');setTimeout(()=>floatingMsg.style.display='none',4000);r.removeEventListener('click',h);setTimeout(()=>{r.remove();goToNecklace()},4500)});bn.addEventListener('click',()=>{bn.remove();by.style.transform='scale(1.1)';by.style.boxShadow='0 0 35px rgba(76,175,80,0.9)';by.textContent='💖 أكيد أقبل 😏'});bc.appendChild(by);bc.appendChild(bn);document.body.appendChild(bc);r.removeEventListener('click',h)})}
  function goToNecklace(){currentStage=14;updateProgress(14);setInstruction('المسي القلادة الذهبية 🗝️');showNecklace()}

  function showNecklace(){necklaceContainer.style.opacity='1';necklaceContainer.style.pointerEvents='auto';const p=`قلبي صندوق مغلق لا يملك مفتاحه أحد\nحتى جئتِ أنتِ وفتحتِ كل أقفاله\nأدخلتِ النور إلى زواياه المظلمة\nوزرعتِ الورود في أرضه الجدباء\nأنتِ مفتاح روحي وسر سعادتي\nلا أريد أن أهرب من حبكِ أبداً\nفأنا أسير عينيكِ عن طيب خاطر\nأحبكِ فوق ما تتصورين\nوفوق ما أستطيع أن أكتب أو أقول\nأنتِ أجمل فصل في كتاب حياتي`;necklaceIcon.addEventListener('click',function h(){necklaceIcon.textContent='💛';necklaceIcon.style.animation='none';necklaceMessage.textContent=p;necklaceMessage.style.display='block';playSound('https://www.soundjay.com/human/sounds/applause-01.mp3');necklaceIcon.removeEventListener('click',h);setTimeout(()=>{necklaceContainer.style.opacity='0';necklaceContainer.style.pointerEvents='none';goToFakeBug()},6000)})}
  function goToFakeBug(){currentStage=15;updateProgress(15);setInstruction('😈 ماذا يحدث؟ 😈');fakeBugEffect(()=>{currentStage=16;updateProgress(16);setInstruction('🧩 حلّي اللغز 🧩');namePuzzle(goToFinale)})}

  function goToFinale(){currentStage=17;updateProgress(17);setInstruction('💖 النهاية 💖');setTimeout(()=>showFinale(),500)}

  function showFinale(){currentStage=18;updateProgress(18);hideInstruction();stageIndicator.style.opacity='0';mainStage.style.opacity='0';finalScreen.style.opacity='1';finalScreen.style.visibility='visible';document.querySelector('.final-text').textContent=dynamicEnding();playSound('https://www.soundjay.com/human/sounds/applause-01.mp3');const b=document.createElement("button");b.textContent="🔄 إعادة";b.style.cssText="margin-top:20px;padding:10px 20px;border-radius:20px;border:none;background:gold;cursor:pointer;font-size:1rem;";b.onclick=()=>location.reload();finalScreen.appendChild(b)}
})();
</script>
</body>
</html>
```
