<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Лабораторная №7 — Выталкивающая сила</title>
<link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#06101a;--bg2:#0a1828;--bg3:#0e2035;
  --blue:#00b4d8;--blue2:#0077b6;--teal:#48cae4;
  --orange:#fb8500;--yellow:#ffb703;
  --green:#52b788;--red:#e63946;
  --text:#e0f4ff;--text-dim:#5a8aaa;--text-mid:#8ab4cc;
  --border:rgba(0,180,216,0.18);
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html{scroll-behavior:smooth;}
body{background:var(--bg);color:var(--text);font-family:'Nunito',sans-serif;min-height:100vh;overflow-x:hidden;}
::-webkit-scrollbar{width:4px;}
::-webkit-scrollbar-track{background:var(--bg);}
::-webkit-scrollbar-thumb{background:var(--blue2);border-radius:4px;}

.header{background:rgba(6,16,26,0.97);border-bottom:1px solid var(--border);padding:10px 14px;position:sticky;top:0;z-index:100;backdrop-filter:blur(12px);display:flex;align-items:center;justify-content:space-between;gap:8px;}
.header-left{display:flex;flex-direction:column;gap:1px;flex:1;min-width:0;}
.h-sub{font-size:8px;color:var(--text-dim);text-transform:uppercase;letter-spacing:.12em;font-family:'JetBrains Mono',monospace;}
.h-title{font-family:'Unbounded',sans-serif;font-size:12px;font-weight:900;color:var(--blue);}
.header-right{display:flex;flex-direction:column;align-items:flex-end;gap:4px;flex-shrink:0;}
.student-chip{background:rgba(0,180,216,0.1);border:1px solid rgba(0,180,216,0.2);border-radius:8px;padding:3px 8px;font-size:8px;color:var(--blue);font-family:'JetBrains Mono',monospace;max-width:130px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;display:none;}
.timer-wrap{display:flex;align-items:center;gap:5px;background:rgba(0,180,216,0.08);border:1px solid rgba(0,180,216,0.2);border-radius:10px;padding:5px 10px;transition:all .3s;}
.timer-wrap.warn{background:rgba(251,133,0,0.12);border-color:rgba(251,133,0,0.4);animation:blink .8s ease-in-out infinite;}
.timer-wrap.danger{background:rgba(230,57,70,0.15);border-color:rgba(230,57,70,0.5);animation:blink .5s ease-in-out infinite;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.55}}
.timer-val{font-family:'JetBrains Mono',monospace;font-size:15px;font-weight:700;color:var(--blue);transition:color .3s;}
.timer-wrap.warn .timer-val{color:var(--orange);}
.timer-wrap.danger .timer-val{color:var(--red);}
.timer-lbl{font-size:8px;color:var(--text-dim);font-family:'JetBrains Mono',monospace;}

.nav-dots{display:flex;justify-content:center;gap:8px;padding:8px 14px;background:var(--bg2);border-bottom:1px solid var(--border);overflow-x:auto;scrollbar-width:none;}
.nav-dots::-webkit-scrollbar{display:none;}
.dot{width:26px;height:26px;border-radius:50%;border:2px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;color:var(--text-dim);cursor:pointer;transition:all .2s;flex-shrink:0;font-family:'JetBrains Mono',monospace;}
.dot.active{border-color:var(--blue);background:var(--blue);color:#000;}
.dot.done{border-color:var(--green);background:rgba(82,183,136,0.15);color:var(--green);}

.slide{display:none;padding:14px;flex-direction:column;gap:13px;}
.slide.active{display:flex;animation:fadeUp .3s ease;}
@keyframes fadeUp{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}

.section-badge{display:inline-flex;align-items:center;gap:6px;background:rgba(0,180,216,0.1);border:1px solid rgba(0,180,216,0.25);border-radius:20px;padding:4px 12px;font-size:10px;font-weight:700;color:var(--blue);text-transform:uppercase;letter-spacing:.1em;font-family:'JetBrains Mono',monospace;width:fit-content;}
.card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:14px;position:relative;overflow:hidden;}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,transparent,var(--blue),transparent);}
.card-title{font-family:'Unbounded',sans-serif;font-size:12px;font-weight:700;color:var(--text);margin-bottom:9px;}
.card-text{font-size:12px;line-height:1.7;color:var(--text-mid);}
.card-text b{color:var(--text);}

.warn-box{background:rgba(251,133,0,0.08);border:1px solid rgba(251,133,0,0.3);border-radius:13px;padding:12px;display:flex;gap:10px;align-items:flex-start;}
.warn-box.red{background:rgba(230,57,70,0.08);border-color:rgba(230,57,70,0.3);}
.warn-box.green{background:rgba(82,183,136,0.06);border-color:rgba(82,183,136,0.25);}
.warn-box.blue{background:rgba(0,180,216,0.06);border-color:rgba(0,180,216,0.25);}
.warn-icon{font-size:20px;flex-shrink:0;}
.warn-text{font-size:11px;line-height:1.7;color:var(--text-mid);}
.warn-text b{color:var(--orange);}
.warn-box.red .warn-text b{color:var(--red);}
.warn-box.green .warn-text b{color:var(--green);}
.warn-box.blue .warn-text b{color:var(--blue);}

/* NOTEBOOK */
.notebook{background:#fffdf5;border:2px solid #c8a84b;border-radius:14px;padding:16px 16px 16px 32px;color:#2d2d2d;font-family:'JetBrains Mono',monospace;font-size:11px;line-height:2.1;position:relative;overflow:hidden;}
.notebook::before{content:'';position:absolute;left:26px;top:0;bottom:0;width:1px;background:#e0c070;opacity:.6;}
.nb-title{font-size:12px;font-weight:700;text-align:center;margin-bottom:8px;color:#1a1a1a;text-decoration:underline;font-family:'Nunito',sans-serif;}
.nb-line{border-bottom:1px solid #ddd09a;padding:1px 2px;min-height:22px;color:#222;}
.nb-line.bold{font-weight:700;}
.nb-blank{border-bottom:1px solid #ddd09a;height:22px;}
.nb-sub{color:#555;font-size:10px;}
.nb-table{width:100%;border-collapse:collapse;margin:8px 0;}
.nb-table th{background:#e8dfc0;border:1px solid #b8a870;padding:5px 4px;font-size:9px;text-align:center;color:#3d3520;}
.nb-table td{border:1px solid #b8a870;padding:4px;text-align:center;background:#fffdf5;height:28px;font-size:10px;}
.nb-calc{border-bottom:1px solid #ddd09a;padding:1px 2px;min-height:22px;color:#333;}

/* VARIANT */
.variant-card{background:linear-gradient(135deg,rgba(0,119,182,0.2),rgba(0,180,216,0.1));border:1px solid rgba(0,180,216,0.4);border-radius:16px;padding:16px;text-align:center;}
.variant-num{font-family:'JetBrains Mono',monospace;font-size:10px;color:var(--text-dim);margin-bottom:4px;text-transform:uppercase;letter-spacing:.1em;}
.variant-body{font-family:'Unbounded',sans-serif;font-size:18px;font-weight:900;color:var(--yellow);}
.variant-params{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px;}
.vp{background:rgba(0,0,0,0.3);border:1px solid var(--border);border-radius:10px;padding:9px;text-align:center;}
.vp-val{font-family:'JetBrains Mono',monospace;font-size:16px;font-weight:700;color:var(--blue);}
.vp-lbl{font-size:8px;color:var(--text-dim);text-transform:uppercase;letter-spacing:.06em;margin-top:2px;}

/* STAND */
.stand-container{background:var(--bg2);border:1px solid var(--border);border-radius:16px;overflow:hidden;}
.stand-top{padding:10px 14px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;}
.stand-title-txt{font-family:'Unbounded',sans-serif;font-size:11px;font-weight:700;}
.step-badge{font-size:9px;padding:3px 10px;border-radius:20px;background:rgba(0,180,216,0.12);border:1px solid rgba(0,180,216,0.3);color:var(--blue);font-family:'JetBrains Mono',monospace;transition:all .3s;}
.step-badge.done{background:rgba(82,183,136,0.15);border-color:rgba(82,183,136,0.3);color:var(--green);}
.step-hint-bar{margin:0;padding:9px 14px;background:rgba(255,183,3,0.07);border-bottom:1px solid rgba(255,183,3,0.12);font-size:11px;color:var(--yellow);display:flex;align-items:center;gap:8px;min-height:36px;}
.hint-pulse{font-size:15px;animation:hpulse .7s ease-in-out infinite;}
@keyframes hpulse{0%,100%{transform:scale(1)}50%{transform:scale(1.3)}}
.stand-canvas-wrap{display:flex;justify-content:center;padding:6px 8px;background:#070f1c;cursor:pointer;user-select:none;}
canvas{touch-action:none;display:block;}
.reading-panel{padding:10px 14px;border-top:1px solid var(--border);display:none;}
.reading-panel.show{display:block;}
.reading-row{background:var(--bg3);border:1px solid var(--border);border-radius:11px;padding:11px 14px;display:flex;align-items:center;justify-content:space-between;}
.reading-lbl{font-size:11px;color:var(--text-mid);}
.reading-hint{font-size:10px;color:var(--green);margin-top:3px;}
.reading-num{font-family:'JetBrains Mono',monospace;font-size:26px;font-weight:700;color:var(--yellow);}
.reading-unit{font-size:11px;color:var(--text-dim);margin-left:3px;}

/* INPUTS */
.input-group{display:flex;flex-direction:column;gap:5px;}
.input-label{font-size:10px;color:var(--text-dim);text-transform:uppercase;letter-spacing:.1em;font-family:'JetBrains Mono',monospace;}
.input-row{display:flex;align-items:center;gap:8px;}
.lab-input{flex:1;background:var(--bg3);border:1.5px solid var(--border);border-radius:11px;padding:11px 12px;font-size:16px;color:var(--text);font-family:'JetBrains Mono',monospace;outline:none;transition:border .2s;-webkit-appearance:none;}
.lab-input:focus{border-color:var(--blue);}
.input-unit{font-size:11px;color:var(--text-dim);font-family:'JetBrains Mono',monospace;width:20px;}

/* TABLE */
.lab-table{width:100%;border-collapse:collapse;font-size:11px;}
.lab-table th{background:rgba(0,180,216,0.1);border:1px solid var(--border);padding:7px 5px;text-align:center;color:var(--blue);font-family:'JetBrains Mono',monospace;font-size:8px;text-transform:uppercase;letter-spacing:.04em;}
.lab-table td{border:1px solid var(--border);padding:7px 5px;text-align:center;color:var(--text-mid);font-family:'JetBrains Mono',monospace;}
.lab-table td.hl{color:var(--yellow);font-weight:700;}
.lab-table td.ok{color:var(--green);font-weight:700;}
.lab-table td.err{color:var(--red);font-weight:700;}

/* FORMULA */
.formula-box{background:linear-gradient(135deg,rgba(0,119,182,0.15),rgba(0,180,216,0.08));border:1px solid rgba(0,180,216,0.3);border-radius:13px;padding:13px;text-align:center;}
.formula{font-family:'JetBrains Mono',monospace;font-size:19px;font-weight:700;color:var(--blue);}
.formula-sub{font-size:10px;color:var(--text-dim);margin-top:5px;line-height:1.6;}

/* BUTTONS */
.nav-btns{display:flex;gap:10px;margin-top:2px;}
.nav-btn{flex:1;padding:13px;border:none;border-radius:13px;font-family:'Unbounded',sans-serif;font-size:10px;font-weight:700;cursor:pointer;transition:all .25s;letter-spacing:.04em;}
.btn-next{background:linear-gradient(135deg,var(--blue),var(--blue2));color:#fff;box-shadow:0 5px 18px rgba(0,180,216,0.25);}
.btn-next:hover{transform:translateY(-2px);}
.btn-next:disabled{opacity:.4;cursor:not-allowed;transform:none;}
.btn-back{background:var(--bg3);border:1px solid var(--border);color:var(--text-dim);}
.btn-sample{background:rgba(255,183,3,0.1);border:1px solid rgba(255,183,3,0.3);color:var(--yellow);font-size:10px;padding:11px;border-radius:13px;font-family:'Unbounded',sans-serif;font-weight:700;cursor:pointer;width:100%;margin-bottom:4px;transition:all .2s;}
.btn-sample:hover{background:rgba(255,183,3,0.15);}

/* QUIZ */
.quiz-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:14px;}
.quiz-num{font-size:9px;color:var(--text-dim);font-family:'JetBrains Mono',monospace;text-transform:uppercase;letter-spacing:.12em;margin-bottom:7px;}
.quiz-q{font-size:13px;font-weight:700;line-height:1.5;margin-bottom:12px;}
.quiz-opts{display:flex;flex-direction:column;gap:7px;}
.quiz-opt{background:var(--bg3);border:2px solid var(--border);border-radius:11px;padding:11px 13px;cursor:pointer;font-size:12px;line-height:1.4;transition:all .2s;display:flex;align-items:center;gap:9px;}
.quiz-opt:hover{border-color:var(--blue);background:rgba(0,180,216,0.06);}
.quiz-opt.correct{border-color:var(--green)!important;background:rgba(82,183,136,0.1)!important;color:var(--green);}
.quiz-opt.wrong{border-color:var(--red)!important;background:rgba(230,57,70,0.1)!important;color:var(--red);}
.quiz-opt.disabled{pointer-events:none;}
.opt-num{width:22px;height:22px;border-radius:50%;border:1px solid currentColor;display:flex;align-items:center;justify-content:center;font-size:9px;font-weight:700;flex-shrink:0;}
.quiz-explain{margin-top:10px;padding:10px;border-radius:10px;font-size:11px;line-height:1.6;display:none;}
.quiz-explain.show{display:block;}
.quiz-explain.ok{background:rgba(82,183,136,0.1);border:1px solid rgba(82,183,136,0.25);color:var(--green);}
.quiz-explain.fail{background:rgba(230,57,70,0.1);border:1px solid rgba(230,57,70,0.25);color:var(--red);}

/* SEND */
.send-status{padding:11px 13px;border-radius:11px;font-size:11px;text-align:center;line-height:1.5;display:none;}
.send-status.sending{display:block;background:rgba(0,180,216,0.08);border:1px solid rgba(0,180,216,0.2);color:var(--blue);}
.send-status.sent{display:block;background:rgba(82,183,136,0.1);border:1px solid rgba(82,183,136,0.25);color:var(--green);}
.send-status.error{display:block;background:rgba(230,57,70,0.08);border:1px solid rgba(230,57,70,0.2);color:var(--red);}

/* SUMMARY */
.summary-score{text-align:center;padding:22px;background:linear-gradient(135deg,rgba(0,119,182,0.15),rgba(0,180,216,0.08));border:1px solid rgba(0,180,216,0.25);border-radius:16px;}
.score-emoji{font-size:44px;margin-bottom:10px;}
.score-val{font-family:'Unbounded',sans-serif;font-size:32px;font-weight:900;color:var(--blue);}
.score-lbl{font-size:12px;color:var(--text-dim);margin-top:5px;}
.score-msg{font-size:12px;color:var(--text-mid);margin-top:9px;line-height:1.6;}

/* REG */
#regScreen{position:fixed;inset:0;z-index:200;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;}
.reg-logo{font-size:42px;margin-bottom:12px;}
.reg-title{font-family:'Unbounded',sans-serif;font-size:15px;font-weight:900;color:var(--blue);text-align:center;margin-bottom:4px;}
.reg-sub{font-size:11px;color:var(--text-dim);text-align:center;margin-bottom:18px;line-height:1.6;}
.reg-form{width:100%;max-width:340px;display:flex;flex-direction:column;gap:11px;}
.reg-field{display:flex;flex-direction:column;gap:5px;}
.reg-lbl{font-size:10px;color:var(--text-dim);text-transform:uppercase;letter-spacing:.12em;font-family:'JetBrains Mono',monospace;}
.reg-input{background:var(--bg2);border:1.5px solid var(--border);border-radius:11px;padding:12px 13px;font-size:15px;color:var(--text);font-family:'Nunito',sans-serif;outline:none;transition:border .2s;width:100%;}
.reg-input:focus{border-color:var(--blue);}
.reg-input::placeholder{color:var(--text-dim);}
.reg-warnbox{background:rgba(230,57,70,0.06);border:1px solid rgba(230,57,70,0.25);border-radius:11px;padding:11px 13px;font-size:11px;line-height:1.7;color:var(--text-mid);}
.reg-warnbox b{color:var(--red);}
.reg-error{color:var(--red);font-size:11px;text-align:center;background:rgba(230,57,70,0.08);border:1px solid rgba(230,57,70,0.2);border-radius:9px;padding:7px 11px;display:none;}
.reg-error.show{display:block;}
.reg-btn{background:linear-gradient(135deg,var(--blue),var(--blue2));color:#fff;border:none;border-radius:13px;padding:15px;font-family:'Unbounded',sans-serif;font-size:11px;font-weight:700;cursor:pointer;transition:all .25s;box-shadow:0 5px 18px rgba(0,180,216,0.3);margin-top:3px;}
.reg-btn:hover{transform:translateY(-2px);}

/* OVERLAYS */
#warnOverlay{position:fixed;bottom:0;left:0;right:0;z-index:150;background:linear-gradient(135deg,rgba(251,133,0,0.97),rgba(200,100,0,0.99));padding:14px 16px;display:none;align-items:center;gap:12px;border-top:2px solid var(--orange);}
#warnOverlay.show{display:flex;}
.warn-ov-btn{background:rgba(0,0,0,0.3);color:#fff;border:none;border-radius:9px;padding:8px 14px;font-family:'Unbounded',sans-serif;font-size:9px;font-weight:700;cursor:pointer;white-space:nowrap;}
#timeupOverlay{position:fixed;inset:0;z-index:300;background:rgba(6,16,26,0.97);backdrop-filter:blur(8px);display:none;flex-direction:column;align-items:center;justify-content:center;padding:24px;text-align:center;gap:16px;}
#timeupOverlay.show{display:flex;}
</style>
</head>
<body>

<!-- REG -->
<div id="regScreen">
  <div class="reg-logo">🔬</div>
  <div class="reg-title">Лабораторная работа №7</div>
  <div class="reg-sub">Определение выталкивающей силы<br>Физика 7 класс • Пёрышкин</div>
  <div class="reg-form">
    <div class="reg-field">
      <div class="reg-lbl">Фамилия и Имя</div>
      <input class="reg-input" id="inputName" type="text" placeholder="Например: Иванов Иван" maxlength="60" autocomplete="off">
    </div>
    <div class="reg-field">
      <div class="reg-lbl">Класс</div>
      <input class="reg-input" id="inputClass" type="text" placeholder="Например: 7А" maxlength="10" autocomplete="off">
    </div>
    <div class="reg-warnbox">⚠️ <b>Работа выполняется один раз!</b> После нажатия кнопки запустится таймер на <b>40 минут</b>. Повторное прохождение невозможно.</div>
    <div class="reg-error" id="regErr"></div>
    <button class="reg-btn" onclick="startLab()">Начать работу →</button>
  </div>
</div>

<div id="warnOverlay">
  <div style="flex:1;font-size:12px;font-weight:700;color:#fff;line-height:1.5;">⏰ Осталось 5 минут! Успей завершить работу.</div>
  <button class="warn-ov-btn" onclick="document.getElementById('warnOverlay').classList.remove('show')">OK</button>
</div>
<div id="timeupOverlay">
  <div style="font-size:56px;">⏱️</div>
  <div style="font-family:'Unbounded',sans-serif;font-size:20px;font-weight:900;color:var(--red);">Время вышло!</div>
  <div style="font-size:13px;color:var(--text-mid);line-height:1.7;max-width:320px;">40 минут истекло. Результаты отправлены учителю. Покажи тетрадь учителю.</div>
</div>

<!-- HEADER -->
<div class="header">
  <div class="header-left">
    <span class="h-sub">Лабораторная №7 • Физика 7 класс</span>
    <span class="h-title">Выталкивающая сила</span>
  </div>
  <div class="header-right">
    <div class="student-chip" id="studentChip"></div>
    <div class="timer-wrap" id="timerWrap">
      <span style="font-size:12px;">⏱</span>
      <span class="timer-val" id="timerVal">40:00</span>
      <span class="timer-lbl">мин</span>
    </div>
  </div>
</div>

<div class="nav-dots">
  <div class="dot active" id="dot1" onclick="goTo(1)">1</div>
  <div class="dot" id="dot2" onclick="goTo(2)">2</div>
  <div class="dot" id="dot3" onclick="goTo(3)">3</div>
  <div class="dot" id="dot4" onclick="goTo(4)">4</div>
  <div class="dot" id="dot5" onclick="goTo(5)">5</div>
</div>

<!-- ===== СЛАЙД 1: ОФОРМЛЕНИЕ ТЕТРАДИ ===== -->
<div class="slide active" id="slide1">
  <div class="section-badge">📓 Шаг 1 — Оформление тетради</div>

  <div class="variant-card">
    <div class="variant-num">Вариант <span id="varNum">—</span> из 10</div>
    <div class="variant-body" id="varBody">—</div>
    <div class="variant-params">
      <div class="vp"><div class="vp-val" id="varVol">—</div><div class="vp-lbl">Объём, см³</div></div>
      <div class="vp"><div class="vp-val" id="varRho">—</div><div class="vp-lbl">Плотность, кг/м³</div></div>
    </div>
  </div>

  <div class="warn-box red">
    <div class="warn-icon">🔒</div>
    <div class="warn-text"><b>Работа выполняется один раз!</b> Таймер уже запущен — у тебя <b>40 минут</b>.</div>
  </div>

  <div class="warn-box green">
    <div class="warn-icon">📓</div>
    <div class="warn-text"><b>Сначала оформи тетрадь!</b> Перепиши название, цель, оборудование, начерти таблицу и оставь строки для вычислений.<br><br>✏️ <b>Все показания со стенда сразу записывай в тетрадь</b> — они понадобятся для расчётов и вывода!</div>
  </div>

  <!-- ШАБЛОН ТЕТРАДИ — только таблица и вычисления -->
  <div class="notebook">
    <div class="nb-title">Лабораторная работа № 7</div>
    <div class="nb-line bold">Тема: Определение выталкивающей силы,</div>
    <div class="nb-line bold">действующей на погружённое в жидкость тело</div>
    <div class="nb-blank"></div>
    <div class="nb-line"><b>Цель:</b> научиться обнаруживать и</div>
    <div class="nb-line">рассчитывать выталкивающую силу.</div>
    <div class="nb-blank"></div>
    <div class="nb-line"><b>Оборудование:</b> динамометр, штатив,</div>
    <div class="nb-line">тело — <span id="nb_body" style="color:#8b6914;font-weight:700;">—</span>,</div>
    <div class="nb-line">стакан с водой, стакан с солёной водой.</div>
    <div class="nb-blank"></div>
    <div class="nb-line bold">Таблица результатов:</div>
    <table class="nb-table">
      <thead>
        <tr>
          <th style="width:18px;">№</th>
          <th>Жидкость</th>
          <th>P, Н<br>(воздух)</th>
          <th>P₁, Н<br>(жидкость)</th>
          <th>F = P−P₁<br>Н</th>
        </tr>
      </thead>
      <tbody>
        <tr><td>1</td><td style="font-size:9px;">Вода</td><td></td><td></td><td></td></tr>
        <tr><td>2</td><td style="font-size:9px;">Солёная вода</td><td></td><td></td><td></td></tr>
      </tbody>
    </table>
    <div class="nb-blank"></div>
    <div class="nb-line bold">Вычисления:</div>
    <div class="nb-line nb-sub">Опыт 1 (вода):</div>
    <div class="nb-line">F₁ = P − P₁ = ___ − ___ = ___ Н</div>
    <div class="nb-blank"></div>
    <div class="nb-line nb-sub">Опыт 2 (солёная вода):</div>
    <div class="nb-line">F₂ = P − P₁ = ___ − ___ = ___ Н</div>
    <div class="nb-blank"></div>
    <div class="nb-line bold">Вывод:</div>
    <div class="nb-line">Выталкивающая сила зависит от _____</div>
    <div class="nb-line">________________________________.</div>
    <div class="nb-line">В солёной воде F _____, потому что её</div>
    <div class="nb-line">плотность _____ плотности чистой воды.</div>
  </div>

  <div class="formula-box">
    <div class="formula">F = P − P₁</div>
    <div class="formula-sub">Считай в тетради! P — вес в воздухе, P₁ — вес в жидкости</div>
  </div>

  <div class="nav-btns">
    <button class="nav-btn btn-next" onclick="goTo(2)">Оформил(а) → к опыту 1</button>
  </div>
</div>

<!-- ===== СЛАЙД 2: СТЕНД — ВОДА ===== -->
<div class="slide" id="slide2">
  <div class="section-badge">💧 Опыт 1 — Вода</div>

  <div class="stand-container">
    <div class="stand-top">
      <span class="stand-title-txt">🔬 Виртуальный стенд</span>
      <span class="step-badge" id="sb1">ШАГ 1 из 3</span>
    </div>
    <div class="step-hint-bar" id="sh1">
      <span class="hint-pulse">👆</span>
      <span id="sh1t">Тапни на тело чтобы повесить на динамометр</span>
    </div>
    <div class="stand-canvas-wrap">
      <canvas id="cv1" width="320" height="320"></canvas>
    </div>
    <div class="reading-panel" id="rp1">
      <div class="reading-row">
        <div>
          <div class="reading-lbl" id="rl1">—</div>
          <div class="reading-hint">✏️ Запиши это значение в тетрадь!</div>
        </div>
        <div style="display:flex;align-items:baseline;">
          <div class="reading-num" id="rn1">—</div>
          <div class="reading-unit">Н</div>
        </div>
      </div>
    </div>
  </div>

  <div class="warn-box blue">
    <div class="warn-icon">✏️</div>
    <div class="warn-text"><b>Смотри на показание</b> и записывай в тетрадь. После двух шагов введи данные ниже и посчитай F в тетради.</div>
  </div>

  <div class="card">
    <div class="card-title">Введи данные из тетради</div>
    <div style="display:flex;flex-direction:column;gap:12px;margin-top:4px;">
      <div class="input-group">
        <div class="input-label">P — вес в воздухе (со стенда)</div>
        <div class="input-row"><input class="lab-input" id="pw_air" type="number" step="0.01" min="0" max="999" placeholder="0.00" oninput="chk1()"><span class="input-unit">Н</span></div>
      </div>
      <div class="input-group">
        <div class="input-label">P₁ — вес в воде (со стенда)</div>
        <div class="input-row"><input class="lab-input" id="pw_p1" type="number" step="0.01" min="0" max="999" placeholder="0.00" oninput="chk1()"><span class="input-unit">Н</span></div>
      </div>
      <div class="input-group">
        <div class="input-label">F = P − P₁ (посчитай в тетради!)</div>
        <div class="input-row"><input class="lab-input" id="pw_f" type="number" step="0.01" min="-999" max="999" placeholder="0.00" oninput="chk1()"><span class="input-unit">Н</span></div>
      </div>
    </div>
  </div>

  <div class="nav-btns">
    <button class="nav-btn btn-back" onclick="goTo(1)">← Назад</button>
    <button class="nav-btn btn-next" id="nx2" onclick="goTo(3)" disabled>К опыту 2 →</button>
  </div>
</div>

<!-- ===== СЛАЙД 3: СТЕНД — СОЛЬ ===== -->
<div class="slide" id="slide3">
  <div class="section-badge">🧂 Опыт 2 — Солёная вода</div>

  <div class="stand-container">
    <div class="stand-top">
      <span class="stand-title-txt">🔬 Виртуальный стенд</span>
      <span class="step-badge" id="sb2">ШАГ 1 из 3</span>
    </div>
    <div class="step-hint-bar" id="sh2">
      <span class="hint-pulse">👆</span>
      <span id="sh2t">Тапни на тело чтобы повесить на динамометр</span>
    </div>
    <div class="stand-canvas-wrap">
      <canvas id="cv2" width="320" height="320"></canvas>
    </div>
    <div class="reading-panel" id="rp2">
      <div class="reading-row">
        <div>
          <div class="reading-lbl" id="rl2">—</div>
          <div class="reading-hint">✏️ Запиши это значение в тетрадь!</div>
        </div>
        <div style="display:flex;align-items:baseline;">
          <div class="reading-num" id="rn2">—</div>
          <div class="reading-unit">Н</div>
        </div>
      </div>
    </div>
  </div>

  <div class="warn-box blue">
    <div class="warn-icon">✏️</div>
    <div class="warn-text"><b>Вес в воздухе тот же</b> что в опыте 1. Запиши P₁ и посчитай F в тетради.</div>
  </div>

  <div class="card">
    <div class="card-title">Введи данные из тетради</div>
    <div style="display:flex;flex-direction:column;gap:12px;margin-top:4px;">
      <div class="input-group">
        <div class="input-label">P — вес в воздухе (тот же)</div>
        <div class="input-row"><input class="lab-input" id="ps_air" type="number" step="0.01" min="0" max="999" placeholder="0.00" oninput="chk2()"><span class="input-unit">Н</span></div>
      </div>
      <div class="input-group">
        <div class="input-label">P₁ — вес в солёной воде (со стенда)</div>
        <div class="input-row"><input class="lab-input" id="ps_p1" type="number" step="0.01" min="0" max="999" placeholder="0.00" oninput="chk2()"><span class="input-unit">Н</span></div>
      </div>
      <div class="input-group">
        <div class="input-label">F = P − P₁ (посчитай в тетради!)</div>
        <div class="input-row"><input class="lab-input" id="ps_f" type="number" step="0.01" min="-999" max="999" placeholder="0.00" oninput="chk2()"><span class="input-unit">Н</span></div>
      </div>
    </div>
  </div>

  <div class="nav-btns">
    <button class="nav-btn btn-back" onclick="goTo(2)">← Назад</button>
    <button class="nav-btn btn-next" id="nx3" onclick="goTo(4)" disabled>К выводу →</button>
  </div>
</div>

<!-- ===== СЛАЙД 4: ВЫВОД ===== -->
<div class="slide" id="slide4">
  <div class="section-badge">📊 Итоги опытов</div>

  <!-- Кнопка посмотреть образец -->
  <button class="btn-sample" onclick="goTo(1)">📓 Посмотреть образец оформления (слайд 1)</button>

  <div class="card">
    <div class="card-title">Твои результаты</div>
    <div style="overflow-x:auto;margin-top:8px;">
      <table class="lab-table">
        <thead><tr><th>№</th><th>Жидкость</th><th>P, Н</th><th>P₁, Н</th><th>F (твой), Н</th></tr></thead>
        <tbody>
          <tr><td>1</td><td>Вода</td><td id="t_p1">—</td><td id="t_p1w">—</td><td class="hl" id="t_f1">—</td></tr>
          <tr><td>2</td><td>Солёная вода</td><td id="t_p2">—</td><td id="t_p2s">—</td><td class="hl" id="t_f2">—</td></tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="card">
    <div class="card-title">Допиши вывод в тетради</div>
    <div class="card-text">
      <b>1.</b> В какой жидкости выталкивающая сила больше и почему?<br>
      <b>2.</b> От каких величин зависит выталкивающая сила?<br>
      <b>3.</b> Какова природа выталкивающей силы?
    </div>
    <div style="margin-top:10px;padding:9px 12px;background:rgba(255,183,3,0.08);border:1px solid rgba(255,183,3,0.25);border-radius:10px;font-size:11px;color:var(--yellow);display:flex;align-items:center;gap:8px;">
      <span>💡</span>
      <span>Образец оформления вывода — на <b>странице 1</b> (кнопка «←» или точка «1» вверху)</span>
    </div>
  </div>

  <div class="warn-box">
    <div class="warn-icon">📓</div>
    <div class="warn-text"><b>Напоминание:</b> без оформления в тетради оценка на <b>1 балл ниже</b>. Учитель проверит тетрадь.</div>
  </div>

  <div class="nav-btns">
    <button class="nav-btn btn-back" onclick="goTo(3)">← Назад</button>
    <button class="nav-btn btn-next" onclick="goTo(5)">К тесту →</button>
  </div>
</div>

<!-- ===== СЛАЙД 5: ТЕСТ ===== -->
<div class="slide" id="slide5">
  <div class="section-badge">📝 Проверочный тест</div>
  <div class="warn-box red">
    <div class="warn-icon">⚠️</div>
    <div class="warn-text"><b>1 попытка!</b> После выбора ответ изменить нельзя. Результат отправляется учителю.</div>
  </div>
  <div class="quiz-card">
    <div class="quiz-num">Вопрос 1 из 4</div>
    <div class="quiz-q">Как называется сила, действующая на тело в жидкости и направленная вертикально вверх?</div>
    <div class="quiz-opts">
      <div class="quiz-opt" onclick="qa(1,this,false)"><div class="opt-num">А</div>Сила тяжести</div>
      <div class="quiz-opt" onclick="qa(1,this,false)"><div class="opt-num">Б</div>Сила трения</div>
      <div class="quiz-opt" onclick="qa(1,this,true)"><div class="opt-num">В</div>Выталкивающая сила (сила Архимеда)</div>
      <div class="quiz-opt" onclick="qa(1,this,false)"><div class="opt-num">Г</div>Сила упругости</div>
    </div>
    <div class="quiz-explain" id="qe1"></div>
  </div>
  <div class="quiz-card">
    <div class="quiz-num">Вопрос 2 из 4</div>
    <div class="quiz-q">В каком опыте выталкивающая сила оказалась больше?</div>
    <div class="quiz-opts">
      <div class="quiz-opt" onclick="qa(2,this,false)"><div class="opt-num">А</div>В опыте с чистой водой</div>
      <div class="quiz-opt" onclick="qa(2,this,true)"><div class="opt-num">Б</div>В опыте с солёной водой — она плотнее</div>
      <div class="quiz-opt" onclick="qa(2,this,false)"><div class="opt-num">В</div>Одинакова в обоих опытах</div>
      <div class="quiz-opt" onclick="qa(2,this,false)"><div class="opt-num">Г</div>Зависит от формы тела</div>
    </div>
    <div class="quiz-explain" id="qe2"></div>
  </div>
  <div class="quiz-card">
    <div class="quiz-num">Вопрос 3 из 4</div>
    <div class="quiz-q">Как вычислить выталкивающую силу с помощью динамометра?</div>
    <div class="quiz-opts">
      <div class="quiz-opt" onclick="qa(3,this,false)"><div class="opt-num">А</div>F = P · P₁</div>
      <div class="quiz-opt" onclick="qa(3,this,true)"><div class="opt-num">Б</div>F = P − P₁</div>
      <div class="quiz-opt" onclick="qa(3,this,false)"><div class="opt-num">В</div>F = P + P₁</div>
      <div class="quiz-opt" onclick="qa(3,this,false)"><div class="opt-num">Г</div>F = P₁ − P</div>
    </div>
    <div class="quiz-explain" id="qe3"></div>
  </div>
  <div class="quiz-card">
    <div class="quiz-num">Вопрос 4 из 4</div>
    <div class="quiz-q">От чего зависит выталкивающая сила?</div>
    <div class="quiz-opts">
      <div class="quiz-opt" onclick="qa(4,this,false)"><div class="opt-num">А</div>От массы тела и температуры</div>
      <div class="quiz-opt" onclick="qa(4,this,false)"><div class="opt-num">Б</div>От формы тела и цвета жидкости</div>
      <div class="quiz-opt" onclick="qa(4,this,true)"><div class="opt-num">В</div>От объёма тела и плотности жидкости</div>
      <div class="quiz-opt" onclick="qa(4,this,false)"><div class="opt-num">Г</div>Только от глубины погружения</div>
    </div>
    <div class="quiz-explain" id="qe4"></div>
  </div>
  <button class="nav-btn btn-next" id="finBtn" onclick="goTo(6)" disabled style="margin-top:2px;">Завершить работу →</button>
  <div class="nav-btns" style="margin-top:0;"><button class="nav-btn btn-back" onclick="goTo(4)">← Назад</button></div>
</div>

<!-- ===== СЛАЙД 6: ИТОГ ===== -->
<div class="slide" id="slide6">
  <div class="section-badge">🏆 Работа завершена</div>
  <div class="summary-score">
    <div class="score-emoji" id="sumEmoji">🏆</div>
    <div class="score-val" id="sumScore">—</div>
    <div class="score-lbl">баллов за тест</div>
    <div class="score-msg" id="sumMsg"></div>
    <div style="margin-top:9px;font-size:10px;color:var(--text-dim);font-family:'JetBrains Mono',monospace;" id="sumInfo"></div>
  </div>
  <div class="send-status" id="sendSt"></div>
  <div class="card">
    <div class="card-title">Проверка расчётов F = P − P₁</div>
    <div style="overflow-x:auto;margin-top:8px;">
      <table class="lab-table">
        <thead><tr><th>Жидкость</th><th>P, Н</th><th>P₁, Н</th><th>Твой F</th><th>Верно?</th></tr></thead>
        <tbody>
          <tr><td>Вода</td><td id="r_p1">—</td><td id="r_p1w">—</td><td id="r_f1">—</td><td id="r_c1">—</td></tr>
          <tr><td>Солёная вода</td><td id="r_p2">—</td><td id="r_p2s">—</td><td id="r_f2">—</td><td id="r_c2">—</td></tr>
        </tbody>
      </table>
    </div>
    <div style="font-size:10px;color:var(--text-dim);margin-top:8px;">Допустимая погрешность ±0.05 Н</div>
  </div>
  <div style="background:rgba(0,180,216,0.06);border:1px solid var(--border);border-radius:13px;padding:13px;text-align:center;font-size:12px;color:var(--text-dim);line-height:1.7;">
    ✅ Результаты отправлены учителю.<br><span style="color:var(--text-mid);">Покажи тетрадь с оформлением!</span>
  </div>
</div>

<script>
var SCRIPT_URL='https://script.google.com/macros/s/AKfycbwpIYjJo9wN3foVCW6f93FWduBZyHrf16waIcnOW-wAkqNU7mRdI2k3kvzZayReMJNN/exec';
var TOTAL=40*60, G=10, RHO_W=1000, RHO_S=1200;

var BODIES=[
  {n:1,name:'Алюминиевый цилиндр',emoji:'🔘',vol:120,rho:2700,color:'#a8b8d8'},
  {n:2,name:'Деревянный брусок',emoji:'🪵',vol:200,rho:650,color:'#c8843c'},
  {n:3,name:'Медный кубик',emoji:'🟫',vol:80,rho:8900,color:'#d4721a'},
  {n:4,name:'Стеклянный шар',emoji:'⚪',vol:150,rho:2500,color:'#d0e8f8'},
  {n:5,name:'Пластиковый цилиндр',emoji:'🔷',vol:180,rho:1100,color:'#3090d0'},
  {n:6,name:'Железный шар',emoji:'⚫',vol:100,rho:7800,color:'#606880'},
  {n:7,name:'Восковой кубик',emoji:'🟡',vol:160,rho:900,color:'#e8c840'},
  {n:8,name:'Бронзовый цилиндр',emoji:'🔶',vol:90,rho:8800,color:'#c87820'},
  {n:9,name:'Каменный кубик',emoji:'🪨',vol:130,rho:2600,color:'#8890a0'},
  {n:10,name:'Свинцовый шар',emoji:'🔵',vol:70,rho:11300,color:'#5870a0'},
];

var curSlide=1,body=null,sName='',sClass='';
var physP=0,physP1w=0,physP1s=0;
var exp1={p:null,p1:null,f:null},exp2={p:null,p1:null,f:null};
var qAns={1:null,2:null,3:null,4:null},qScore=0;
var sent=false,timerInt=null,secsLeft=TOTAL,warned=false;

// Stand animation state
var ST=[null,null]; // ST[0]=water, ST[1]=salt
// step: 0=idle(body on table), 1=animating to hook, 2=on hook(show P), 3=animating to liquid, 4=in liquid(show P1)
// bodyX,bodyY: current position
// animId: current animation interval
// ptrFrac: dynamometer pointer 0..1

function hashName(n){var h=0;for(var i=0;i<n.length;i++){h=((h<<5)-h)+n.charCodeAt(i);h=h&h;}return Math.abs(h)%BODIES.length;}

function startLab(){
  var n=document.getElementById('inputName').value.trim();
  var c=document.getElementById('inputClass').value.trim();
  var e=document.getElementById('regErr');
  if(!n||n.length<3){e.textContent='Введи полное имя (минимум 3 символа)';e.classList.add('show');return;}
  if(!c){e.textContent='Введи класс';e.classList.add('show');return;}
  e.classList.remove('show');
  sName=n;sClass=c;body=BODIES[hashName(n.toLowerCase())];

  var V=body.vol/1e6;
  physP=Math.round(body.rho*G*V*100)/100;
  physP1w=Math.round((physP-RHO_W*G*V)*100)/100;
  physP1s=Math.round((physP-RHO_S*G*V)*100)/100;
  if(physP1w<0)physP1w=0;
  if(physP1s<0)physP1s=0;

  document.getElementById('regScreen').style.display='none';
  var ch=document.getElementById('studentChip');
  ch.textContent=sName+' · '+sClass;ch.style.display='block';
  document.getElementById('varNum').textContent=body.n;
  document.getElementById('varBody').textContent=body.emoji+' '+body.name;
  document.getElementById('varVol').textContent=body.vol;
  document.getElementById('varRho').textContent=body.rho;
  document.getElementById('nb_body').textContent=body.emoji+' '+body.name;

  initST(0,'water');
  initST(1,'salt');
  startTimer();
}

document.getElementById('inputName').addEventListener('keydown',function(e){if(e.key==='Enter')document.getElementById('inputClass').focus();});
document.getElementById('inputClass').addEventListener('keydown',function(e){if(e.key==='Enter')startLab();});

function startTimer(){
  timerInt=setInterval(function(){
    secsLeft--;
    var m=Math.floor(secsLeft/60),s=secsLeft%60;
    document.getElementById('timerVal').textContent=(m<10?'0':'')+m+':'+(s<10?'0':'')+s;
    var w=document.getElementById('timerWrap');
    w.className='timer-wrap';
    if(secsLeft<=10*60)w.classList.add('danger');
    else if(secsLeft<=15*60)w.classList.add('warn');
    if(secsLeft===5*60&&!warned){warned=true;document.getElementById('warnOverlay').classList.add('show');}
    if(secsLeft<=0){
      clearInterval(timerInt);
      if(!sent){sent=true;sendData();}
      document.querySelectorAll('.lab-input,.quiz-opt,.nav-btn,.dot').forEach(function(el){el.disabled=true;el.style.pointerEvents='none';el.style.opacity='.4';});
      document.getElementById('timeupOverlay').classList.add('show');
    }
  },1000);
}

function goTo(n){
  var cur=document.getElementById('slide'+curSlide);
  if(cur){cur.classList.remove('active');cur.style.display='none';}
  var dc=document.getElementById('dot'+curSlide);
  if(dc){dc.classList.remove('active');if(curSlide<n&&curSlide<=5)dc.classList.add('done');}
  curSlide=n;
  var nx=document.getElementById('slide'+n);
  if(nx){nx.style.display='flex';nx.classList.add('active');}
  var dn=document.getElementById('dot'+n);
  if(dn){dn.classList.add('active');dn.classList.remove('done');}
  window.scrollTo(0,0);
  if(n===2)setTimeout(function(){renderST(0);},80);
  if(n===3)setTimeout(function(){renderST(1);},80);
  if(n===4)fillTable();
  if(n===6)showResult();
}

// ===== STAND ENGINE =====
var W=320,H=320;

// Key positions in canvas coordinates
var P_TABLE={x:70,y:255};   // body resting on table
var P_HOOK={x:210,y:148};   // hook below dynamometer
var P_WATER_W={x:175,y:222};// center of water body position
var P_WATER_S={x:175,y:222};

// Dynamometer position (wider for readable scale)
var DYN={x:188,y:42,w:30,h:100};

function initST(idx,liq){
  ST[idx]={
    step:0, liq:liq,
    bx:P_TABLE.x, by:P_TABLE.y,
    ptrFrac:0, animId:null,
    wavePhase:0
  };
  var cid=idx===0?'cv1':'cv2';
  var c=document.getElementById(cid);
  if(!c)return;
  // Remove old listeners by replacing element
  var clone=c.cloneNode(true);
  c.parentNode.replaceChild(clone,c);
  var nc=document.getElementById(cid);

  nc.addEventListener('click',function(e){
    var r=nc.getBoundingClientRect();
    var sx=W/r.width, sy=H/r.height;
    handleTap(idx,(e.clientX-r.left)*sx,(e.clientY-r.top)*sy);
  });
  nc.addEventListener('touchend',function(e){
    e.preventDefault();
    var r=nc.getBoundingClientRect();
    var t=e.changedTouches[0];
    var sx=W/r.width, sy=H/r.height;
    handleTap(idx,(t.clientX-r.left)*sx,(t.clientY-r.top)*sy);
  },{passive:false});

  renderST(idx);
}

function handleTap(idx,tx,ty){
  var st=ST[idx];
  if(st.animId)return;

  if(st.step===0){
    // Check body hit
    if(dist(tx,ty,st.bx,st.by)<30){
      st.step=1;
      setHint(idx,1);
      animMove(idx,P_HOOK.x,P_HOOK.y+20,physP/5,function(){
        st.step=2;
        setHint(idx,2);
        showReading(idx,'air');
      });
    }
  } else if(st.step===2){
    // Tap on beaker area
    var bkX=95,bkY=168,bkW=150,bkH=80;
    if(tx>bkX&&tx<bkX+bkW&&ty>bkY&&ty<bkY+bkH){
      st.step=3;
      setHint(idx,3);
      var liqP1=idx===0?physP1w:physP1s;
      animMove(idx,P_HOOK.x,P_WATER_W.y,liqP1/5,function(){
        st.step=4;
        setHint(idx,4);
        showReading(idx,'liq');
        startWave(idx);
      });
    }
  }
}

function dist(x1,y1,x2,y2){return Math.sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));}

function animMove(idx,tx,ty,targetPtr,cb){
  var st=ST[idx];
  var sx=st.bx,sy=st.by,sp=st.ptrFrac;
  var isEntry=(st.step===3);
  var total=isEntry?72:42;
  var f=0;
  st.splashFrame=-1;
  var liquidSurfaceY=182;
  st.animId=setInterval(function(){
    f++;
    var prog=easeInOut(f/total);
    st.bx=sx+(tx-sx)*prog;
    st.by=sy+(ty-sy)*prog;
    if(isEntry){
      var depth=Math.max(0,st.by-liquidSurfaceY);
      var maxDepth=ty-liquidSurfaceY;
      var entryProg=maxDepth>0?Math.min(1,depth/maxDepth):0;
      st.ptrFrac=sp+(targetPtr-sp)*easeInOut(entryProg);
      if(st.by>=liquidSurfaceY-1&&st.splashFrame<0) st.splashFrame=0;
    } else {
      st.ptrFrac=sp+(targetPtr-sp)*prog;
    }
    if(st.splashFrame>=0&&st.splashFrame<18) st.splashFrame++;
    renderST(idx);
    if(f>=total){
      clearInterval(st.animId);st.animId=null;
      st.bx=tx;st.by=ty;st.ptrFrac=targetPtr;
      renderST(idx);
      if(cb)cb();
    }
  },16);
}

function easeInOut(t){return t<.5?2*t*t:1-Math.pow(-2*t+2,2)/2;}

function startWave(idx){
  var st=ST[idx];
  var wid=setInterval(function(){
    if(st.step!==4){clearInterval(wid);return;}
    st.wavePhase+=0.08;
    renderST(idx);
  },30);
}

function setHint(idx,step){
  var sfx=idx===0?'1':'2';
  var liq=ST[idx].liq;
  var hints={
    0:'Тапни на тело чтобы повесить на динамометр',
    1:'Поднимаем тело...',
    2:'Тело на динамометре. '+( liq==='water'?'Тапни на стакан с водой':'Тапни на стакан с солёной водой'),
    3:'Опускаем тело в жидкость...',
    4:'Готово! Запиши показание в тетрадь',
  };
  var badges={0:'ШАГ 1 из 3',1:'...',2:'ШАГ 2 из 3',3:'...',4:'✅ ГОТОВО'};
  document.getElementById('sh'+sfx+'t').textContent=hints[step]||'';
  var b=document.getElementById('sb'+sfx);
  b.textContent=badges[step]||'';
  b.className='step-badge'+(step===4?' done':'');
}

function showReading(idx,type){
  var sfx=idx===0?'1':'2';
  var liq=ST[idx].liq;
  document.getElementById('rp'+sfx).classList.add('show');
  if(type==='air'){
    document.getElementById('rl'+sfx).textContent='⬆ Вес тела в воздухе P:';
    document.getElementById('rn'+sfx).textContent=physP.toFixed(2);
  } else {
    var val=liq==='water'?physP1w:physP1s;
    document.getElementById('rl'+sfx).textContent=liq==='water'?'💧 Вес в воде P₁:':'🧂 Вес в солёной воде P₁:';
    document.getElementById('rn'+sfx).textContent=val.toFixed(2);
  }
}

// ===== RENDER =====
function renderST(idx){
  var cid=idx===0?'cv1':'cv2';
  var c=document.getElementById(cid);
  if(!c)return;
  var ctx=c.getContext('2d');
  var st=ST[idx];
  var liq=st.liq;
  ctx.clearRect(0,0,W,H);

  // BG gradient
  var bg=ctx.createLinearGradient(0,0,0,H);
  bg.addColorStop(0,'#07111e');bg.addColorStop(1,'#04090f');
  ctx.fillStyle=bg;ctx.fillRect(0,0,W,H);

  // ===== WORKBENCH TABLE =====
  // Table top
  var tg=ctx.createLinearGradient(0,268,0,285);
  tg.addColorStop(0,'#8b5e1a');tg.addColorStop(1,'#5a3a0a');
  ctx.fillStyle=tg;ctx.fillRect(15,268,290,18);
  // Table edge highlight
  ctx.fillStyle='rgba(255,200,80,0.15)';ctx.fillRect(15,268,290,2);
  // Table legs
  ctx.fillStyle='#4a2a08';
  ctx.fillRect(25,286,16,32);ctx.fillRect(279,286,16,32);

  // ===== TRIPOD ====
  // Base plate
  var bpg=ctx.createLinearGradient(0,258,0,268);
  bpg.addColorStop(0,'#505068');bpg.addColorStop(1,'#303048');
  ctx.fillStyle=bpg;
  ctx.beginPath();ctx.ellipse(175,265,38,7,0,0,Math.PI*2);ctx.fill();
  ctx.strokeStyle='rgba(100,100,160,0.4)';ctx.lineWidth=1;
  ctx.beginPath();ctx.ellipse(175,265,38,7,0,0,Math.PI*2);ctx.stroke();

  // Vertical rod — thick with shine
  var rg=ctx.createLinearGradient(170,0,180,0);
  rg.addColorStop(0,'#909ab8');rg.addColorStop(.4,'#c8d0e8');rg.addColorStop(1,'#5a6080');
  ctx.fillStyle=rg;ctx.fillRect(171,38,8,228);
  ctx.fillStyle='rgba(255,255,255,0.2)';ctx.fillRect(173,38,2,228);

  // Horizontal arm
  var ag=ctx.createLinearGradient(0,50,0,58);
  ag.addColorStop(0,'#c0c8e0');ag.addColorStop(1,'#6878a0');
  ctx.fillStyle=ag;ctx.fillRect(171,50,52,8);
  ctx.fillStyle='rgba(255,255,255,0.15)';ctx.fillRect(171,50,52,2);

  // Clamp (муфта) — cylinder shape
  ctx.fillStyle='#585870';
  ctx.beginPath();ctx.roundRect(167,44,20,20,4);ctx.fill();
  ctx.strokeStyle='rgba(150,150,200,0.5)';ctx.lineWidth=1;
  ctx.beginPath();ctx.roundRect(167,44,20,20,4);ctx.stroke();
  // Clamp screw
  ctx.fillStyle='#888890';
  ctx.beginPath();ctx.arc(177,54,4,0,Math.PI*2);ctx.fill();
  ctx.fillStyle='rgba(255,255,255,0.3)';
  ctx.beginPath();ctx.arc(176,53,1.5,0,Math.PI*2);ctx.fill();

  // Lapka (arm clamp)
  ctx.fillStyle='#484860';
  ctx.beginPath();ctx.roundRect(218,52,8,18,2);ctx.fill();

  // ===== DYNAMOMETER =====
  // Wider body so scale is readable
  var dx = 188, dy = 42, dw = 30, dh = 100;

  // Outer metal casing
  var dg = ctx.createLinearGradient(dx, 0, dx+dw, 0);
  dg.addColorStop(0,'#b0b8d0'); dg.addColorStop(.35,'#e8ecf8');
  dg.addColorStop(.65,'#d8dff0'); dg.addColorStop(1,'#8890b0');
  ctx.fillStyle = dg;
  ctx.beginPath(); ctx.roundRect(dx, dy, dw, dh, 5); ctx.fill();
  ctx.strokeStyle = '#7880a8'; ctx.lineWidth = 1.5;
  ctx.beginPath(); ctx.roundRect(dx, dy, dw, dh, 5); ctx.stroke();

  // White scale window — full height minus caps
  var winX = dx+3, winY = dy+8, winW = dw-6, winH = dh-16;
  ctx.fillStyle = '#f8f8f4';
  ctx.beginPath(); ctx.roundRect(winX, winY, winW, winH, 2); ctx.fill();
  ctx.strokeStyle = '#ccccd8'; ctx.lineWidth = 0.8;
  ctx.beginPath(); ctx.roundRect(winX, winY, winW, winH, 2); ctx.stroke();

  // Scale: 0 at top, 5 at bottom (5 major divisions, 10 minor)
  var scTop = winY + 4, scBot = winY + winH - 4, scH = scBot - scTop;
  var maxN = 5; // Newtons shown on scale
  for (var si = 0; si <= 20; si++) {
    var sy2 = scTop + si * (scH / 20);
    var isMajor = (si % 4 === 0);
    var isMid   = (si % 2 === 0);
    ctx.strokeStyle = isMajor ? '#333' : (isMid ? '#888' : '#bbb');
    ctx.lineWidth   = isMajor ? 1.2 : 0.7;
    var tickLen = isMajor ? winW-2 : (isMid ? 7 : 4);
    ctx.beginPath();
    ctx.moveTo(winX+1, sy2);
    ctx.lineTo(winX+1+tickLen, sy2);
    ctx.stroke();
    // Numbers on major ticks — right side, clear and big
    if (isMajor) {
      var val = (si / 20) * maxN;
      ctx.fillStyle = '#111';
      ctx.font = 'bold 8px JetBrains Mono, monospace';
      ctx.textAlign = 'right';
      ctx.fillText(val.toFixed(0), winX+winW-1, sy2+3);
      ctx.textAlign = 'left';
    }
  }

  // "Н" unit label at bottom of window
  ctx.fillStyle = '#555566';
  ctx.font = '7px JetBrains Mono, monospace';
  ctx.textAlign = 'center';
  ctx.fillText('Н', winX+winW/2, winY+winH-1);
  ctx.textAlign = 'left';

  // Pointer — red line + triangle, highly visible
  var ptrFrac = st.ptrFrac; // 0=top(0N) → 1=bottom(maxN)
  var ptrY = scTop + ptrFrac * scH;
  ptrY = Math.max(scTop, Math.min(scBot, ptrY));

  // Glow behind pointer
  ctx.shadowColor = 'rgba(230,57,70,0.6)'; ctx.shadowBlur = 6;
  ctx.fillStyle = '#e63946';
  ctx.fillRect(winX+1, ptrY-2, winW-2, 4);
  // Left-pointing arrow on pointer
  ctx.beginPath();
  ctx.moveTo(winX+1, ptrY);
  ctx.lineTo(winX+8, ptrY-5);
  ctx.lineTo(winX+8, ptrY+5);
  ctx.closePath(); ctx.fill();
  ctx.shadowBlur = 0;

  // Current reading bubble — shown outside casing on the right
  if (st.ptrFrac > 0) {
    var readingN = (st.ptrFrac * maxN).toFixed(2);
    var bblX = dx + dw + 4, bblY = ptrY;
    ctx.fillStyle = 'rgba(0,180,216,0.18)';
    ctx.strokeStyle = 'rgba(0,180,216,0.7)'; ctx.lineWidth = 1.2;
    ctx.beginPath(); ctx.roundRect(bblX, bblY-9, 34, 18, 5); ctx.fill(); ctx.stroke();
    ctx.fillStyle = '#00d4ff';
    ctx.font = 'bold 9px JetBrains Mono, monospace';
    ctx.textAlign = 'center';
    ctx.fillText(readingN+' Н', bblX+17, bblY+3);
    ctx.textAlign = 'left';
    // connecting line
    ctx.strokeStyle = 'rgba(0,180,216,0.4)'; ctx.lineWidth = 1;
    ctx.setLineDash([2,2]);
    ctx.beginPath(); ctx.moveTo(dx+dw, ptrY); ctx.lineTo(bblX, ptrY); ctx.stroke();
    ctx.setLineDash([]);
  }

  // Ring at top
  ctx.fillStyle = '#6878a0';
  ctx.beginPath(); ctx.arc(dx+dw/2, dy+3, 4, 0, Math.PI*2); ctx.fill();
  ctx.strokeStyle = '#9090b8'; ctx.lineWidth=1;
  ctx.beginPath(); ctx.arc(dx+dw/2, dy+3, 4, 0, Math.PI*2); ctx.stroke();

  // Hook at bottom
  ctx.strokeStyle = '#7878a0'; ctx.lineWidth = 2; ctx.setLineDash([]);
  ctx.beginPath(); ctx.moveTo(dx+dw/2, dy+dh); ctx.lineTo(dx+dw/2, dy+dh+8); ctx.stroke();
  ctx.beginPath(); ctx.arc(dx+dw/2, dy+dh+12, 5, Math.PI, Math.PI*2); ctx.stroke();

  // ===== BEAKER =====
  var bkX=95,bkY=168,bkW=150,bkH=82;

  // Beaker glass (slightly trapezoidal)
  ctx.strokeStyle='rgba(180,220,255,0.6)';ctx.lineWidth=2;
  ctx.beginPath();
  ctx.moveTo(bkX+8,bkY);
  ctx.lineTo(bkX,bkY+bkH);
  ctx.lineTo(bkX+bkW,bkY+bkH);
  ctx.lineTo(bkX+bkW-8,bkY);
  ctx.stroke();

  // Liquid fill
  var liqTop=bkY+14;
  if(liq==='water'){
    var wg2=ctx.createLinearGradient(bkX,liqTop,bkX,bkY+bkH);
    wg2.addColorStop(0,'rgba(0,140,200,0.55)');
    wg2.addColorStop(1,'rgba(0,60,120,0.75)');
    ctx.fillStyle=wg2;
  } else {
    var sg=ctx.createLinearGradient(bkX,liqTop,bkX,bkY+bkH);
    sg.addColorStop(0,'rgba(170,220,255,0.5)');
    sg.addColorStop(1,'rgba(80,160,220,0.7)');
    ctx.fillStyle=sg;
  }
  ctx.fillRect(bkX+2,liqTop,bkW-4,bkH-liqTop+bkY-2);

  // Wave on liquid surface
  ctx.fillStyle=liq==='water'?'rgba(80,180,255,0.35)':'rgba(200,235,255,0.35)';
  ctx.beginPath();
  ctx.moveTo(bkX+2,liqTop);
  for(var wx=0;wx<=bkW-4;wx+=4){
    var wy=liqTop+Math.sin((wx*0.12)+st.wavePhase)*2.5;
    ctx.lineTo(bkX+2+wx,wy);
  }
  ctx.lineTo(bkX+bkW-2,liqTop);
  ctx.closePath();ctx.fill();

  // Glass shine on beaker left
  ctx.strokeStyle='rgba(255,255,255,0.2)';ctx.lineWidth=3;
  ctx.beginPath();ctx.moveTo(bkX+15,bkY+10);ctx.lineTo(bkX+10,bkY+bkH-10);ctx.stroke();

  // Beaker label
  ctx.fillStyle=liq==='water'?'rgba(100,200,255,0.8)':'rgba(180,230,255,0.8)';
  ctx.font='bold 8px JetBrains Mono,monospace';ctx.textAlign='center';
  ctx.fillText(liq==='water'?'H₂O  ρ=1000 кг/м³':'СОЛЬ  ρ=1200 кг/м³',bkX+bkW/2,bkY+bkH-4);
  ctx.textAlign='left';

  // ===== STRING from hook to body (when hanging) =====
  if(st.step>=1){
    ctx.strokeStyle='rgba(220,220,235,0.75)';ctx.lineWidth=1.5;ctx.setLineDash([]);
    var hookBotX=DYN.x+DYN.w/2, hookBotY=DYN.y+DYN.h+17;
    ctx.beginPath();ctx.moveTo(hookBotX,hookBotY);ctx.lineTo(st.bx,st.by-getBodyH()/2);ctx.stroke();
  }

  // ===== TAP HINTS =====
  if(st.step===0){
    // Pulsing circle around body
    var pulse=0.5+0.5*Math.sin(Date.now()*0.004);
    ctx.strokeStyle='rgba(255,183,3,'+(0.4+0.3*pulse)+')';ctx.lineWidth=2;
    ctx.setLineDash([4,4]);
    ctx.beginPath();ctx.arc(st.bx,st.by,28+4*pulse,0,Math.PI*2);ctx.stroke();
    ctx.setLineDash([]);
    ctx.fillStyle='#ffb703';ctx.font='bold 10px Nunito,sans-serif';ctx.textAlign='center';
    ctx.fillText('👆 Тапни!',st.bx,st.by+28);ctx.textAlign='left';
    // Repaint loop for pulse
    requestAnimationFrame(function(){if(ST[idx]&&ST[idx].step===0)renderST(idx);});
  }
  if(st.step===2){
    // Beaker highlight
    ctx.strokeStyle='rgba(0,180,216,0.7)';ctx.lineWidth=2;ctx.setLineDash([5,5]);
    ctx.beginPath();ctx.roundRect(bkX-6,bkY-6,bkW+12,bkH+12,8);ctx.stroke();
    ctx.setLineDash([]);
    ctx.fillStyle='#00b4d8';ctx.font='bold 9px Nunito,sans-serif';ctx.textAlign='center';
    ctx.fillText('👆 Тапни на стакан',bkX+bkW/2,bkY+bkH+18);ctx.textAlign='left';
  }

  // ===== BODY =====
  drawBody(ctx,idx,st.bx,st.by,st.step);

  // ===== SHELF line =====
  ctx.strokeStyle='rgba(180,140,60,0.5)';ctx.lineWidth=3;ctx.setLineDash([]);
  ctx.beginPath();ctx.moveTo(15,270);ctx.lineTo(105,270);ctx.stroke();

  // ===== SPLASH EFFECT =====
  if(st.splashFrame!==undefined&&st.splashFrame>=0&&st.splashFrame<18){
    var sf=st.splashFrame/18;
    var splashAlpha=1-sf;
    var splashX=st.bx, splashY=182;
    var drops=[[-14,-1],[14,-1],[-8,-3],[8,-3],[0,-4],[-18,1],[18,1]];
    drops.forEach(function(d){
      var rx=splashX+d[0]*(sf*2.2);
      var ry=splashY+d[1]*(sf*18);
      ctx.globalAlpha=splashAlpha*0.7;
      ctx.fillStyle='rgba(120,200,255,0.9)';
      ctx.beginPath();ctx.ellipse(rx,ry,2.5,3.5,0,0,Math.PI*2);ctx.fill();
    });
    ctx.globalAlpha=1;
    // Ripple
    var rr=(sf)*22;
    ctx.strokeStyle='rgba(100,200,255,'+(splashAlpha*0.5)+')';
    ctx.lineWidth=1.5;
    ctx.beginPath();ctx.ellipse(splashX,splashY,rr,rr*0.35,0,0,Math.PI*2);ctx.stroke();
  }

  // Force arrows when in liquid
  if(st.step===4){
    var bcy=st.by;
    // F_arch up (green)
    var archLen=Math.min(55,(idx===0?physP1w:physP1s)>0?(physP-physP1w)*18:0);
    if(archLen>5){
      drawArrow(ctx,st.bx+30,bcy,st.bx+30,bcy-archLen,'#52b788');
      ctx.fillStyle='#52b788';ctx.font='bold 7px JetBrains Mono,monospace';ctx.textAlign='left';
      ctx.fillText('F꜀',st.bx+35,bcy-archLen/2+3);ctx.textAlign='left';
    }
    // Weight down (red)
    var wLen=Math.min(55,physP*8);
    drawArrow(ctx,st.bx-30,bcy,st.bx-30,bcy+wLen,'#e63946');
    ctx.fillStyle='#e63946';ctx.font='bold 7px JetBrains Mono,monospace';ctx.textAlign='right';
    ctx.fillText('P',st.bx-35,bcy+wLen/2+3);ctx.textAlign='left';
  }
}

function getBodyH(){return 20;}

function drawBody(ctx,idx,bx,by,step){
  if(!body)return;
  var bw=22,bh=20;
  var col=body.color||'#e07030';

  // Shadow
  if(step>=1&&step<=2){
    ctx.shadowColor='rgba(0,0,0,0.5)';ctx.shadowBlur=8;ctx.shadowOffsetY=3;
  }

  // Body shape
  ctx.fillStyle=col;
  ctx.strokeStyle='rgba(255,255,255,0.25)';ctx.lineWidth=1.5;
  ctx.beginPath();ctx.roundRect(bx-bw/2,by-bh,bw,bh,4);ctx.fill();ctx.stroke();
  ctx.shadowBlur=0;ctx.shadowOffsetY=0;

  // Shine
  ctx.fillStyle='rgba(255,255,255,0.2)';
  ctx.beginPath();ctx.roundRect(bx-bw/2+3,by-bh+3,bw-6,5,2);ctx.fill();

  // Emoji
  ctx.font='10px serif';ctx.textAlign='center';
  ctx.fillText(body.emoji,bx,by-5);
  ctx.textAlign='left';
}

function drawArrow(ctx,x1,y1,x2,y2,col){
  ctx.strokeStyle=col;ctx.lineWidth=2;ctx.setLineDash([]);
  ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
  var a=Math.atan2(y2-y1,x2-x1);
  ctx.fillStyle=col;
  ctx.beginPath();ctx.moveTo(x2,y2);
  ctx.lineTo(x2-8*Math.cos(a-0.45),y2-8*Math.sin(a-0.45));
  ctx.lineTo(x2-8*Math.cos(a+0.45),y2-8*Math.sin(a+0.45));
  ctx.closePath();ctx.fill();
}

function chk1(){
  var p=parseFloat(document.getElementById('pw_air').value);
  var p1=parseFloat(document.getElementById('pw_p1').value);
  var f=parseFloat(document.getElementById('pw_f').value);
  var ok=!isNaN(p)&&!isNaN(p1)&&!isNaN(f);
  if(ok){exp1.p=p;exp1.p1=p1;exp1.f=f;}
  document.getElementById('nx2').disabled=!ok;
}
function chk2(){
  var p=parseFloat(document.getElementById('ps_air').value);
  var p1=parseFloat(document.getElementById('ps_p1').value);
  var f=parseFloat(document.getElementById('ps_f').value);
  var ok=!isNaN(p)&&!isNaN(p1)&&!isNaN(f);
  if(ok){exp2.p=p;exp2.p1=p1;exp2.f=f;}
  document.getElementById('nx3').disabled=!ok;
}

function fillTable(){
  document.getElementById('t_p1').textContent=exp1.p!==null?exp1.p.toFixed(2):'—';
  document.getElementById('t_p1w').textContent=exp1.p1!==null?exp1.p1.toFixed(2):'—';
  document.getElementById('t_f1').textContent=exp1.f!==null?exp1.f.toFixed(2):'—';
  document.getElementById('t_p2').textContent=exp2.p!==null?exp2.p.toFixed(2):'—';
  document.getElementById('t_p2s').textContent=exp2.p1!==null?exp2.p1.toFixed(2):'—';
  document.getElementById('t_f2').textContent=exp2.f!==null?exp2.f.toFixed(2):'—';
}

function qa(n,el,correct){
  if(qAns[n]!==null)return;
  qAns[n]=correct;if(correct)qScore++;
  el.closest('.quiz-opts').querySelectorAll('.quiz-opt').forEach(function(o){o.classList.add('disabled');});
  el.classList.add(correct?'correct':'wrong');
  el.querySelector('.opt-num').textContent=correct?'✓':'✗';
  var ex=document.getElementById('qe'+n);
  var QEXP={
    1:{ok:'✅ Выталкивающая сила (сила Архимеда) — направлена вертикально вверх.',fail:'❌ Это выталкивающая сила (сила Архимеда) — направлена вертикально вверх.'},
    2:{ok:'✅ Верно! Солёная вода плотнее → F больше.',fail:'❌ В солёной воде F больше, так как ρ солёной > ρ чистой воды.'},
    3:{ok:'✅ Верно! F = P − P₁.',fail:'❌ Правильная формула: F = P − P₁.'},
    4:{ok:'✅ F = ρ·g·V — зависит от плотности жидкости и объёма тела.',fail:'❌ F зависит от плотности жидкости и объёма тела.'},
  };
  ex.textContent=QEXP[n][correct?'ok':'fail'];
  ex.className='quiz-explain show '+(correct?'ok':'fail');
  if(Object.values(qAns).every(function(v){return v!==null;}))
    document.getElementById('finBtn').disabled=false;
}

function showResult(){
  var msgs=['Нужно повторить тему 📚','Неплохо, есть ошибки','Хорошо! 👍','Отлично! 🎉','Превосходно! 🏆'];
  var emojis=['😔','🤔','😊','😄','🏆'];
  var idx=Math.min(qScore,4);
  document.getElementById('sumScore').textContent=qScore+'/4';
  document.getElementById('sumEmoji').textContent=emojis[idx];
  document.getElementById('sumMsg').textContent=msgs[idx];
  document.getElementById('sumInfo').textContent=sName+' · '+sClass+' · Вариант '+(body?body.n:'—');

  var eps=0.05;
  function chkF(p,p1,f){
    if(p===null||p1===null||f===null)return{f:'—',t:'—',cls:''};
    var c=p-p1,d=Math.abs(f-c);
    return{f:f.toFixed(2),t:d<=eps?'✅ Верно':'❌ '+c.toFixed(2),cls:d<=eps?'ok':'err'};
  }
  var r1=chkF(exp1.p,exp1.p1,exp1.f);
  var r2=chkF(exp2.p,exp2.p1,exp2.f);
  document.getElementById('r_p1').textContent=exp1.p!==null?exp1.p.toFixed(2):'—';
  document.getElementById('r_p1w').textContent=exp1.p1!==null?exp1.p1.toFixed(2):'—';
  document.getElementById('r_f1').textContent=r1.f;
  var c1=document.getElementById('r_c1');c1.textContent=r1.t;c1.className=r1.cls;
  document.getElementById('r_p2').textContent=exp2.p!==null?exp2.p.toFixed(2):'—';
  document.getElementById('r_p2s').textContent=exp2.p1!==null?exp2.p1.toFixed(2):'—';
  document.getElementById('r_f2').textContent=r2.f;
  var c2=document.getElementById('r_c2');c2.textContent=r2.t;c2.className=r2.cls;

  if(!sent){sent=true;sendData();}
}

function sendData(){
  setSt('sending','📤 Отправляю результаты учителю...');
  var now=new Date();
  var ts=now.toLocaleString('ru-RU',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});
  var spent=TOTAL-secsLeft;
  var spentStr=Math.floor(spent/60)+' мин '+(spent%60)+' сек';
  var fc=function(a,b){return a!==null&&b!==null?(a-b).toFixed(2):'—';};
  var fo=function(f,a,b){return f!==null&&a!==null&&b!==null?(Math.abs(f-(a-b))<=0.05?'✅ верно':'❌ ошибка'):'—';};
  fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'application/json'},
    body:JSON.stringify({
      timestamp:ts, name:sName, className:sClass,
      variant:body?body.n:'—', body:body?body.name:'—',
      time_spent:spentStr,
      p_water:exp1.p!==null?exp1.p.toFixed(2):'—',
      p1_water:exp1.p1!==null?exp1.p1.toFixed(2):'—',
      f_water_stu:exp1.f!==null?exp1.f.toFixed(2):'—',
      f_water_correct:fc(exp1.p,exp1.p1),
      f_water_ok:fo(exp1.f,exp1.p,exp1.p1),
      p_salt:exp2.p!==null?exp2.p.toFixed(2):'—',
      p1_salt:exp2.p1!==null?exp2.p1.toFixed(2):'—',
      f_salt_stu:exp2.f!==null?exp2.f.toFixed(2):'—',
      f_salt_correct:fc(exp2.p,exp2.p1),
      f_salt_ok:fo(exp2.f,exp2.p,exp2.p1),
      quiz_score:qScore, quiz_total:4
    })})
  .then(function(){setSt('sent','✅ Результаты записаны в журнал учителя!');})
  .catch(function(){setSt('error','❌ Ошибка отправки. Покажи экран учителю.');sent=false;});
}
function setSt(t,m){var e=document.getElementById('sendSt');e.textContent=m;e.className='send-status '+t;}
</script>
</body>
</html>
