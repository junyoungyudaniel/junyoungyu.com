[index.html](https://github.com/user-attachments/files/33288142/index.html)
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Junyoung Yu · A Journey Through Space</title>
<meta name="description" content="Junyoung Yu, Business Analytics at HKU Business School. Walk across a mirror sea of stars where every planet is a chapter: education, awards, experience, leadership.">
<meta property="og:title" content="Junyoung Yu · A Journey Through Space">
<meta property="og:description" content="An interactive CV: walk across a sea of stars and visit a planet for every chapter.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#04050d;
    --glass:rgba(12,16,34,.74);
    --glass-strong:rgba(10,13,28,.9);
    --line:rgba(150,175,255,.16);
    --line-strong:rgba(150,175,255,.32);
    --text:#eceffa;
    --muted:#9aa3c7;
    --soft:#c6cce6;
    --cyan:#6fe7ff;
    --amber:#ffbe7a;
    --violet:#b39bff;
    --display:'Space Grotesk',system-ui,sans-serif;
    --body:'Inter',system-ui,sans-serif;
    --mono:'JetBrains Mono',ui-monospace,monospace;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{margin:0;background:var(--bg);color:var(--text);font-family:var(--body);overflow:hidden;-webkit-font-smoothing:antialiased}
  a{color:inherit}
  button{font:inherit;color:inherit;cursor:pointer}
  #scene{position:fixed;inset:0;width:100%;height:100%;display:block;touch-action:none;outline:none}

  /* ---------- HUD ---------- */
  .hud{position:fixed;top:20px;left:24px;z-index:5;pointer-events:none;transition:opacity .5s}
  .hud *{pointer-events:auto}
  .hud .tag,.intro-card .tag{font:500 10.5px/1 var(--mono);letter-spacing:.18em;text-transform:uppercase;color:var(--cyan);display:flex;align-items:center;gap:8px}
  .hud .tag i,.intro-card .tag i{width:6px;height:6px;border-radius:50%;background:var(--cyan);box-shadow:0 0 10px var(--cyan);animation:blink 2.4s infinite}
  .intro-card .version{margin:0 0 18px}
  .hud .version,.intro-card .version{display:inline-block;margin-top:10px;padding:4px 9px;border-radius:999px;border:1px solid rgba(255,190,122,.3);background:rgba(255,190,122,.08);font:500 10.5px/1 var(--mono);letter-spacing:.1em;text-transform:uppercase;color:var(--amber)}
  .hud h1{font:600 22px/1.1 var(--display);margin:8px 0 4px;letter-spacing:-.01em}
  .hud p{margin:0;color:var(--muted);font-size:13px}
  .hud-right{position:fixed;top:18px;right:20px;z-index:5;display:flex;gap:8px;transition:opacity .4s}
  .pill{display:inline-flex;align-items:center;gap:7px;padding:8px 13px;border-radius:999px;background:var(--glass);border:1px solid var(--line);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);font:500 12.5px/1 var(--body);text-decoration:none;color:var(--soft);transition:border-color .2s,color .2s,background .2s}
  .pill:hover{border-color:var(--line-strong);color:var(--text)}
  .pill svg{width:14px;height:14px}

  /* ---------- Dock ---------- */
  .dock{position:fixed;bottom:18px;left:50%;transform:translateX(-50%);z-index:6;display:flex;gap:4px;padding:6px;background:var(--glass);backdrop-filter:blur(14px);-webkit-backdrop-filter:blur(14px);border:1px solid var(--line);border-radius:999px;max-width:calc(100vw - 32px);overflow-x:auto;scrollbar-width:none;transition:opacity .4s,transform .4s,left .6s cubic-bezier(.2,.8,.2,1)}
  .dock::-webkit-scrollbar{display:none}
  .dock button{border:0;background:transparent;padding:9px 14px;border-radius:999px;font:500 13px/1 var(--body);color:var(--muted);white-space:nowrap;transition:background .2s,color .2s}
  .dock button:hover{color:var(--text);background:rgba(255,255,255,.05)}
  .dock button.active{color:var(--cyan);background:rgba(111,231,255,.12)}
  @media (min-width:861px){ body.panel-open .dock{left:calc((100vw - 472px) / 2);max-width:calc(100vw - 520px)} body.panel-open .hint{opacity:0} }
  .hint{position:fixed;bottom:74px;left:50%;transform:translateX(-50%);z-index:5;font:500 11px/1 var(--mono);letter-spacing:.12em;text-transform:uppercase;color:var(--muted);opacity:.8;white-space:nowrap;transition:opacity .6s}

  /* ---------- 3D markers ---------- */
  #markers{position:fixed;inset:0;pointer-events:none;z-index:4;transition:opacity .4s}
  .marker{position:absolute;left:0;top:0;display:flex;align-items:center;gap:8px;border:0;background:none;padding:6px;pointer-events:auto;will-change:transform;transition:opacity .3s}
  .marker .dot{position:relative;width:10px;height:10px;border-radius:50%;background:var(--cyan);box-shadow:0 0 14px var(--cyan),0 0 2px #fff inset;flex:none}
  .marker .dot::after{content:"";position:absolute;inset:-7px;border-radius:50%;border:1px solid var(--cyan);opacity:0;animation:ping 2.6s infinite}
  .marker .label{padding:5px 10px;border-radius:999px;background:rgba(6,8,20,.62);border:1px solid var(--line);backdrop-filter:blur(6px);-webkit-backdrop-filter:blur(6px);font:500 11px/1 var(--mono);letter-spacing:.08em;text-transform:uppercase;color:var(--soft);white-space:nowrap;transition:color .2s,border-color .2s}
  .marker:hover .label,.marker.hot .label{color:var(--cyan);border-color:rgba(111,231,255,.5)}
  .marker.hot .dot{background:#fff}
  body.panel-open #markers,body.intro #markers{opacity:0}
  body.intro .hud,body.intro .hud-right,body.intro .dock,body.intro .hint{opacity:0;pointer-events:none}
  body.panel-open #markers .marker{pointer-events:none}

  /* ---------- Tooltip ---------- */
  #tooltip{position:fixed;left:0;top:0;z-index:7;pointer-events:none;padding:7px 11px;border-radius:10px;background:var(--glass-strong);border:1px solid var(--line-strong);font:500 12px/1.2 var(--body);color:var(--text);opacity:0;transition:opacity .15s;white-space:nowrap}
  #tooltip small{display:block;font:500 10px/1.4 var(--mono);color:var(--cyan);letter-spacing:.1em;text-transform:uppercase}

  /* ---------- Panel ---------- */
  #panel{position:fixed;top:16px;right:16px;bottom:16px;width:440px;max-width:calc(100vw - 32px);z-index:20;display:flex;flex-direction:column;overflow:hidden;background:var(--glass);backdrop-filter:blur(20px) saturate(1.25);-webkit-backdrop-filter:blur(20px) saturate(1.25);border:1px solid var(--line);border-radius:24px;box-shadow:0 30px 80px -20px rgba(0,0,0,.6);transform:translateX(calc(100% + 40px));transition:transform .6s cubic-bezier(.2,.8,.2,1)}
  #panel.open{transform:none}
  #panel::before{content:"";position:absolute;inset:0 0 auto 0;height:1px;background:linear-gradient(90deg,transparent,rgba(111,231,255,.7),transparent)}
  .p-head{position:relative;padding:24px 26px 18px;border-bottom:1px solid var(--line)}
  .p-kicker{font:500 10.5px/1 var(--mono);letter-spacing:.18em;text-transform:uppercase;color:var(--cyan)}
  .p-head h2{font:600 30px/1.1 var(--display);letter-spacing:-.02em;margin:10px 0 0}
  .p-close{position:absolute;top:18px;right:18px;width:34px;height:34px;border-radius:50%;border:1px solid var(--line);background:rgba(255,255,255,.03);display:grid;place-items:center;color:var(--muted);transition:color .2s,border-color .2s}
  .p-close:hover{color:var(--text);border-color:var(--line-strong)}
  .p-body{flex:1;overflow-y:auto;padding:6px 26px 26px;scrollbar-width:thin;scrollbar-color:rgba(150,175,255,.25) transparent}
  .p-body > *{animation:rise .5s both}
  .p-body > *:nth-child(2){animation-delay:.05s}.p-body > *:nth-child(3){animation-delay:.1s}.p-body > *:nth-child(4){animation-delay:.15s}.p-body > *:nth-child(n+5){animation-delay:.2s}
  .p-foot{display:flex;align-items:center;justify-content:space-between;gap:8px;padding:12px 14px;border-top:1px solid var(--line)}
  .p-foot button{border:1px solid var(--line);background:rgba(255,255,255,.03);padding:9px 14px;border-radius:999px;font:500 12.5px/1 var(--body);color:var(--soft);display:inline-flex;align-items:center;gap:6px;max-width:44%;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .p-foot button:hover{border-color:var(--line-strong);color:var(--text)}
  .p-count{font:500 11px/1 var(--mono);color:var(--muted);letter-spacing:.1em}

  .lead{font-size:15px;line-height:1.65;color:var(--soft);margin:18px 0 6px}
  .group-label{font:500 10.5px/1 var(--mono);letter-spacing:.16em;text-transform:uppercase;color:var(--muted);margin:24px 0 2px;display:flex;align-items:center;gap:10px}
  .group-label::after{content:"";flex:1;height:1px;background:var(--line)}
  .item{padding:16px 0;border-bottom:1px dashed var(--line)}
  .item:last-child{border-bottom:0}
  .item .date{font:500 11px/1 var(--mono);color:var(--muted);letter-spacing:.04em}
  .item h3{font:600 16.5px/1.3 var(--display);margin:8px 0 3px;letter-spacing:-.005em}
  .item .org{font-size:13px;color:var(--amber)}
  .item ul{margin:9px 0 0;padding-left:17px;color:var(--soft);font-size:14px;line-height:1.6}
  .item li{margin:3px 0}
  .item li::marker{color:rgba(111,231,255,.6)}
  .badge{display:inline-block;margin-left:6px;padding:3px 7px;border-radius:6px;font:500 10px/1 var(--mono);letter-spacing:.06em;text-transform:uppercase;vertical-align:2px;background:rgba(255,190,122,.12);color:var(--amber);border:1px solid rgba(255,190,122,.25)}
  .badge.live{background:rgba(111,231,255,.1);color:var(--cyan);border-color:rgba(111,231,255,.3)}

  .stats{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin:18px 0 4px}
  .stat{padding:14px;border-radius:14px;background:rgba(255,255,255,.03);border:1px solid var(--line)}
  .stat b{display:block;font:600 22px/1 var(--display);color:var(--cyan);letter-spacing:-.01em}
  .stat span{display:block;margin-top:7px;font-size:12px;line-height:1.4;color:var(--muted)}
  .links{display:flex;flex-wrap:wrap;gap:8px;margin-top:16px}
  .chips{display:flex;flex-wrap:wrap;gap:8px;margin-top:14px}
  .chip{padding:9px 12px;border-radius:12px;background:rgba(255,255,255,.03);border:1px solid var(--line);font-size:13.5px;color:var(--soft);display:flex;align-items:center;gap:8px}
  .chip::before{content:"";width:6px;height:6px;border-radius:50%;background:var(--cyan);box-shadow:0 0 8px var(--cyan);animation:blink 2s infinite}
  .lang{display:grid;grid-template-columns:110px 1fr auto;align-items:center;gap:14px;padding:14px 0;border-bottom:1px dashed var(--line)}
  .lang:last-child{border-bottom:0}
  .lang b{font:600 15px/1 var(--display)}
  .lang .bar{height:6px;border-radius:99px;background:rgba(255,255,255,.06);overflow:hidden}
  .lang .bar i{display:block;height:100%;border-radius:inherit;background:linear-gradient(90deg,var(--violet),var(--cyan));transform-origin:left;animation:grow 1.1s .2s cubic-bezier(.2,.8,.2,1) both}
  .lang small{font:500 11px/1 var(--mono);color:var(--muted)}
  .hello{display:flex;flex-wrap:wrap;gap:8px;margin-top:18px}
  .hello span{padding:8px 12px;border-radius:10px;border:1px solid var(--line);font:500 14px/1 var(--display);color:var(--soft)}

  /* ---------- Intro ---------- */
  #intro{position:fixed;inset:0;z-index:30;display:grid;place-items:center;padding:24px;background:radial-gradient(ellipse at center,rgba(4,5,13,.35) 0%,rgba(4,5,13,.85) 70%);transition:opacity .9s,visibility .9s}
  #intro.gone{opacity:0;visibility:hidden}
  .intro-card{text-align:center;max-width:620px}
  .intro-card .tag{justify-content:center;display:flex}
  .intro-card h1{font:600 clamp(44px,9vw,84px)/0.95 var(--display);letter-spacing:-.035em;margin:20px 0 16px;background:linear-gradient(180deg,#fff 30%,#9fb3ff);-webkit-background-clip:text;background-clip:text;color:transparent}
  .intro-card p{color:var(--soft);font-size:16px;line-height:1.6;margin:0 auto 30px;max-width:460px}
  .enter{border:1px solid rgba(111,231,255,.5);background:rgba(111,231,255,.1);color:var(--text);padding:15px 26px;border-radius:999px;font:600 15px/1 var(--display);letter-spacing:.01em;display:inline-flex;align-items:center;gap:10px;box-shadow:0 0 40px -6px rgba(111,231,255,.45);transition:transform .2s,background .2s}
  .enter:hover{transform:translateY(-2px);background:rgba(111,231,255,.18)}
  .enter[disabled]{opacity:.5;cursor:progress}
  .intro-card small{display:block;margin-top:22px;font:500 11px/1.6 var(--mono);letter-spacing:.12em;text-transform:uppercase;color:var(--muted)}

  #cities{position:fixed;inset:0;pointer-events:none;z-index:4}
  .city{position:absolute;left:0;top:0;padding:5px 9px;border-radius:9px;background:rgba(6,8,20,.75);border:1px solid rgba(255,122,138,.5);font:600 12px/1.2 var(--display);color:#fff;white-space:nowrap;opacity:0;transition:opacity .5s;will-change:transform}
  .city small{display:block;font:500 9.5px/1.3 var(--mono);color:var(--amber);letter-spacing:.1em;text-transform:uppercase}
  .journey{position:relative;margin:14px 0 4px;padding-left:24px}
  .journey::before{content:"";position:absolute;left:6px;top:8px;bottom:22px;width:1px;background:linear-gradient(#ff7a8a,var(--violet),var(--cyan))}
  .stop{position:relative;padding:0 0 18px;animation:rise .6s both}
  .stop:nth-child(2){animation-delay:.9s}.stop:nth-child(3){animation-delay:2.2s}
  .stop::before{content:"";position:absolute;left:-23px;top:3px;width:11px;height:11px;border-radius:50%;background:var(--bg);border:2px solid #ff7a8a;box-shadow:0 0 10px rgba(255,122,138,.6)}
  .stop .date{font:500 11px/1 var(--mono);color:var(--muted)}
  .stop b{display:block;margin-top:6px;font:600 16px/1.2 var(--display)}
  .stop p{margin:4px 0 0;color:var(--soft);font-size:13.5px;line-height:1.55}
  .pill[aria-pressed="true"]{color:var(--cyan);border-color:rgba(111,231,255,.45)}
  /* ---------- Let's connect ---------- */
  .cta{margin-top:12px;color:var(--text);border-color:rgba(236,72,153,.45);background:linear-gradient(90deg,rgba(236,72,153,.18),rgba(74,163,255,.18));box-shadow:0 0 26px -6px rgba(236,72,153,.55)}
  .cta:hover{border-color:rgba(236,72,153,.8)}
  .cta i{width:7px;height:7px;border-radius:50%;background:#ff7ab8;box-shadow:0 0 10px #ff7ab8;animation:blink 1.6s infinite}
  .radar-wrap{display:flex;gap:18px;align-items:center;margin-top:18px}
  .radar-wrap .lead{margin:0;font-size:14.5px}
  .radar{position:relative;flex:none;width:96px;height:96px;border-radius:50%;border:1px solid rgba(111,231,255,.35);background:repeating-radial-gradient(circle,transparent 0 15px,rgba(111,231,255,.14) 15px 16px);overflow:hidden}
  .radar::before,.radar::after{content:"";position:absolute;background:rgba(111,231,255,.14)}
  .radar::before{left:50%;top:0;bottom:0;width:1px}.radar::after{top:50%;left:0;right:0;height:1px}
  .radar .sweep{position:absolute;inset:0;border-radius:50%;background:conic-gradient(from 0deg,rgba(111,231,255,.5),rgba(111,231,255,0) 75deg,transparent);animation:spin 3s linear infinite}
  .radar .blip{position:absolute;width:7px;height:7px;border-radius:50%;animation:blink 3s infinite}
  .radar .b1{top:24%;left:64%;background:#4aa3ff;box-shadow:0 0 10px #4aa3ff}
  .radar .b2{top:58%;left:24%;background:#ff7ab8;box-shadow:0 0 10px #ff7ab8;animation-delay:1s}
  .radar .b3{top:70%;left:66%;background:var(--cyan);box-shadow:0 0 10px var(--cyan);animation-delay:2s}
  .ccards{display:grid;gap:10px;margin:22px 0 6px}
  .crow{position:relative}
  .ccard{--mx:50%;--my:50%;--c:111,231,255;position:relative;display:flex;align-items:center;gap:14px;padding:16px;border-radius:16px;text-decoration:none;border:1px solid rgba(var(--c),.28);background:radial-gradient(circle at var(--mx) var(--my),rgba(var(--c),.22),transparent 62%),rgba(255,255,255,.025);transition:transform .3s cubic-bezier(.2,.8,.2,1),border-color .2s,box-shadow .3s;will-change:transform}
  .ccard:hover{border-color:rgba(var(--c),.65);box-shadow:0 20px 44px -18px rgba(var(--c),.65)}
  .ccard.li{--c:74,163,255}.ccard.ig{--c:236,72,153}.ccard.em{--c:111,231,255}
  .crow.has-copy .ccard{padding-right:118px}
  .cicon{flex:none;width:46px;height:46px;border-radius:13px;display:grid;place-items:center;color:#fff;font:700 19px/1 var(--display)}
  .cicon svg{width:24px;height:24px}
  .li .cicon{background:linear-gradient(135deg,#0a66c2,#4aa3ff)}
  .ig .cicon{background:radial-gradient(circle at 30% 110%,#fdf497 0%,#fd5949 40%,#d6249f 65%,#285aeb 100%)}
  .em .cicon{background:linear-gradient(135deg,#145a72,#6fe7ff)}
  .ctext{display:flex;flex-direction:column;gap:4px;min-width:0}
  .ctext small{font:500 10.5px/1 var(--mono);letter-spacing:.14em;text-transform:uppercase;color:var(--muted)}
  .ctext b{font:600 16px/1.2 var(--display);color:var(--text);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .cgo{margin-left:auto;color:rgb(var(--c));font-size:18px;transition:transform .25s}
  .ccard:hover .cgo{transform:translate(3px,-3px)}
  .ccopy{position:absolute;right:44px;top:50%;transform:translateY(-50%);padding:7px 11px;border-radius:999px;border:1px solid rgba(236,72,153,.4);background:rgba(236,72,153,.1);color:#ffc2e0;font:500 11.5px/1 var(--mono);letter-spacing:.04em;transition:background .2s}
  .ccopy:hover{background:rgba(236,72,153,.22)}
  .tx{display:grid;gap:8px;margin-top:12px}
  .tx input,.tx textarea{width:100%;font:400 14px/1.5 var(--body);color:var(--text);background:rgba(255,255,255,.03);border:1px solid var(--line);border-radius:12px;padding:11px 13px;resize:vertical;outline:none;transition:border-color .2s,box-shadow .2s}
  .tx input::placeholder,.tx textarea::placeholder{color:#6f789c}
  .tx input:focus,.tx textarea:focus{border-color:rgba(111,231,255,.5);box-shadow:0 0 0 3px rgba(111,231,255,.1)}
  .tx-row{display:flex;align-items:center;gap:10px;margin-top:4px}
  .sig{display:flex;align-items:flex-end;gap:3px;height:18px}
  .sig i{width:4px;border-radius:2px;background:rgba(255,255,255,.12);transition:background .3s,box-shadow .3s}
  .sig i:nth-child(1){height:28%}.sig i:nth-child(2){height:46%}.sig i:nth-child(3){height:64%}.sig i:nth-child(4){height:82%}.sig i:nth-child(5){height:100%}
  .sig i.on{background:var(--cyan);box-shadow:0 0 6px var(--cyan)}
  .tx-hint{font:500 11px/1 var(--mono);color:var(--muted);letter-spacing:.06em}
  .tx-send{margin-left:auto;padding:11px 18px;font-size:13.5px}
  .tx-note{color:var(--muted);font-size:11.5px}

  /* ---------- Cinematic intro ---------- */
  .cbar{position:fixed;left:0;right:0;height:12vh;background:#000;z-index:28;transform:scaleY(0);transition:transform 1.4s cubic-bezier(.7,0,.2,1);pointer-events:none}
  .cbar.top{top:0;transform-origin:top}.cbar.bot{bottom:0;transform-origin:bottom}
  body.cine .cbar{transform:scaleY(1)}
  .cap{position:fixed;left:0;right:0;bottom:calc(12vh + 30px);z-index:29;margin:0;padding:0 24px;text-align:center;font:500 clamp(17px,2.3vw,26px)/1.4 var(--display);letter-spacing:.01em;color:#f5efff;text-shadow:0 2px 24px rgba(0,0,0,.85);opacity:0;transform:translateY(10px);transition:opacity 1.1s,transform 1.4s;pointer-events:none}
  .cap small{display:block;margin-bottom:8px;font:500 10.5px/1 var(--mono);letter-spacing:.24em;text-transform:uppercase;color:var(--amber)}
  .cap.show{opacity:1;transform:none}
  #skip{position:fixed;right:22px;bottom:calc(6vh - 14px);z-index:30;border:1px solid rgba(255,255,255,.2);background:rgba(255,255,255,.05);color:#cfd5ee;padding:8px 14px;border-radius:999px;font:500 11px/1 var(--mono);letter-spacing:.12em;text-transform:uppercase;opacity:0;pointer-events:none;transition:opacity .6s}
  body.cine #skip{opacity:1;pointer-events:auto}
  #loader{position:fixed;inset:0;z-index:27;display:grid;place-items:center;font:500 11px/1 var(--mono);letter-spacing:.2em;text-transform:uppercase;color:var(--muted);transition:opacity .8s,visibility .8s;pointer-events:none}
  body.ready #loader{opacity:0;visibility:hidden}
  .intro-card{transition:opacity 1.3s,transform 1.3s}
  #intro.wait{background:transparent;pointer-events:none}
  #intro.wait .intro-card{opacity:0;transform:translateY(14px)}
  @keyframes spin{to{transform:rotate(360deg)}}
  @keyframes ping{0%{transform:scale(.6);opacity:.9}100%{transform:scale(2.1);opacity:0}}
  @keyframes blink{0%,100%{opacity:1}50%{opacity:.35}}
  @keyframes rise{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:none}}
  @keyframes grow{from{transform:scaleX(0)}}

  @media (max-width:860px){
    #panel{top:auto;left:8px;right:8px;bottom:8px;width:auto;max-width:none;height:60vh;height:60dvh;border-radius:22px;transform:translateY(calc(100% + 24px))}
    .p-head{padding:20px 20px 14px}.p-head h2{font-size:25px}
    .p-body{padding:4px 20px 20px}
    body.panel-open .dock,body.panel-open .hint{opacity:0;pointer-events:none}
    body.panel-open .hud,body.panel-open .hud-right{opacity:0;pointer-events:none}
  }
  @media (max-width:640px){
    .hud{top:16px;left:16px}.hud h1{font-size:19px}
    .hud-right{top:auto;bottom:76px;right:16px}
    .hud-right .pill span{display:none}
    .marker .label{display:none}
    .hint{display:none}
    .dock{bottom:14px}
    .lang{grid-template-columns:90px 1fr auto}
  }
  @media (prefers-reduced-motion:reduce){
    *,*::before,*::after{animation-duration:.01ms!important;animation-iteration-count:1!important;transition-duration:.01ms!important}
  }

  /* ---------- Universe ---------- */
  .marker .dot{background:#ffd9a8;box-shadow:0 0 14px #ffc98f,0 0 2px #fff inset}
  .marker .dot::after{border-color:#ffd9a8}
  .marker.near .label{color:#ffe2bd;border-color:rgba(255,217,168,.6)}
  #hero{position:fixed;inset:0;z-index:4;display:grid;place-items:center;padding:24px;text-align:center;pointer-events:none;transition:opacity .9s,transform .9s}
  #hero .tag{justify-content:center;display:flex;align-items:center;gap:8px;font:500 10.5px/1 var(--mono);letter-spacing:.2em;text-transform:uppercase;color:#ffd9a8}
  #hero .tag i{width:6px;height:6px;border-radius:50%;background:#ffd9a8;box-shadow:0 0 10px #ffd9a8;animation:blink 2.4s infinite}
  #hero h1{font:600 clamp(48px,9vw,100px)/.95 var(--display);letter-spacing:-.04em;margin:20px 0 16px;background:linear-gradient(180deg,#fff 25%,#ffd9a8 90%);-webkit-background-clip:text;background-clip:text;color:transparent;filter:drop-shadow(0 6px 30px rgba(255,170,110,.25))}
  #hero p{color:var(--soft);font-size:16.5px;line-height:1.6;max-width:470px;margin:0 auto}
  .cue{margin-top:38px;display:inline-flex;flex-direction:column;align-items:center;gap:12px;font:500 10.5px/1 var(--mono);letter-spacing:.2em;text-transform:uppercase;color:var(--muted)}
  .motto{position:absolute;left:0;right:0;bottom:88px;padding:0 24px;font:500 clamp(16px,1.8vw,20px)/1.4 var(--display);letter-spacing:.01em;color:#ffe2bd;text-shadow:0 2px 16px rgba(0,0,0,.9),0 0 30px rgba(0,0,0,.6)}
  .mouse{width:24px;height:38px;border:1.5px solid rgba(255,255,255,.45);border-radius:14px;position:relative}
  .mouse::after{content:"";position:absolute;left:50%;top:7px;width:3px;height:7px;margin-left:-1.5px;border-radius:2px;background:#ffd9a8;animation:wheel 1.8s infinite}
  @keyframes wheel{0%{opacity:0;transform:translateY(-3px)}40%{opacity:1}100%{opacity:0;transform:translateY(11px)}}
  body.walking #hero{opacity:0;transform:translateY(-24px)}
  body:not(.walking) .hud,body:not(.walking) #markers{opacity:0;pointer-events:none}
  #chapter{position:fixed;left:50%;bottom:84px;z-index:5;display:flex;align-items:center;gap:18px;padding:12px 12px 12px 20px;border-radius:18px;background:var(--glass);border:1px solid rgba(255,217,168,.25);backdrop-filter:blur(14px);-webkit-backdrop-filter:blur(14px);box-shadow:0 20px 50px -20px rgba(255,150,90,.35);opacity:0;transform:translate(-50%,16px);pointer-events:none;transition:opacity .5s,transform .6s cubic-bezier(.2,.8,.2,1);max-width:calc(100vw - 32px)}
  #chapter.show{opacity:1;transform:translate(-50%,0);pointer-events:auto}
  #chapter .ck{font:500 10px/1 var(--mono);letter-spacing:.16em;text-transform:uppercase;color:#ffd9a8}
  #chapter b{display:block;margin-top:7px;font:600 18px/1.2 var(--display);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  #chapter .enter{padding:11px 16px;font-size:13.5px;border-color:rgba(255,217,168,.5);background:rgba(255,217,168,.1);box-shadow:0 0 30px -8px rgba(255,190,120,.5);flex:none}
  body.panel-open #chapter{opacity:0;pointer-events:none}
  #whiteout{position:fixed;inset:0;z-index:40;pointer-events:none;opacity:0;background:radial-gradient(ellipse at 50% 55%,#fffdf8 0%,#fff6ea 45%,#f4e8d6 100%)}
  #beyond{position:fixed;inset:0;z-index:41;overflow-y:auto;display:grid;place-items:center;padding:56px 20px;opacity:0;visibility:hidden;transition:opacity 1.3s,visibility 1.3s;
    --text:#251d33;--soft:#4b4462;--muted:#867e9a;--line:rgba(60,40,100,.14);--line-strong:rgba(60,40,100,.3);--cyan:#3a8fb0;--glass:rgba(255,255,255,.75);--bg:#fffaf2;
    color:var(--text);background:radial-gradient(ellipse at 50% 28%,#fffefb 0%,#fff6ea 55%,#f2e4d0 100%)}
  #beyond.show{opacity:1;visibility:visible}
  #beyond.show .bw{animation:rise 1.2s .5s both}
  #beyond .bw{width:min(560px,100%)}
  #beyond .bk{font:500 11px/1 var(--mono);letter-spacing:.24em;text-transform:uppercase;color:#b98a4e}
  #beyond h2{font:600 clamp(42px,7vw,68px)/1 var(--display);letter-spacing:-.035em;margin:14px 0 4px;color:#1d1530}
  #beyond .lead{color:var(--soft)}
  #beyond .ccard{background:radial-gradient(circle at var(--mx) var(--my),rgba(var(--c),.16),transparent 62%),#fff;box-shadow:0 12px 30px -20px rgba(60,40,100,.45)}
  #beyond .ctext b{color:#1d1530}
  #beyond .radar{border-color:rgba(58,143,176,.35);background:repeating-radial-gradient(circle,transparent 0 15px,rgba(58,143,176,.16) 15px 16px)}
  #beyond .tx input,#beyond .tx textarea{background:#fff}
  #beyond .enter{color:#1d1530;border-color:rgba(185,138,78,.55);background:rgba(255,214,160,.25);box-shadow:0 0 30px -10px rgba(220,160,90,.6)}
  #beyond .sig i{background:rgba(60,40,100,.12)}
  #beyond .back{margin-top:28px;border:1px solid var(--line);background:rgba(255,255,255,.6);padding:10px 16px;border-radius:999px;font:500 13px/1 var(--body);color:var(--soft)}
  #beyond .back:hover{border-color:var(--line-strong);color:var(--text)}
  body.through .hud,body.through .hud-right,body.through .dock,body.through #rail,body.through #chapter,body.through #markers,body.through #cities{opacity:0!important;pointer-events:none!important}
  #scene{cursor:grab}
  #rail{position:fixed;right:24px;top:50%;transform:translateY(-50%);z-index:5;height:min(56vh,460px);width:20px;transition:opacity .4s}
  #rail .track{position:absolute;left:9px;top:0;bottom:0;width:2px;background:rgba(255,255,255,.1);border-radius:2px}
  #rail .fill{position:absolute;left:9px;top:0;width:2px;height:0;border-radius:2px;background:linear-gradient(#ffd9a8,#b39bff);box-shadow:0 0 8px rgba(255,217,168,.6)}
  #rail button{position:absolute;left:3px;width:14px;height:14px;padding:0;border-radius:50%;border:2px solid rgba(255,255,255,.35);background:var(--bg);transform:translateY(-50%);transition:border-color .3s,background .3s,box-shadow .3s}
  #rail button.done{border-color:#ffd9a8;background:#ffd9a8;box-shadow:0 0 10px #ffd9a8}
  #rail button span{position:absolute;right:22px;top:50%;transform:translateY(-50%);padding:4px 8px;border-radius:8px;background:var(--glass);border:1px solid var(--line);white-space:nowrap;font:500 10.5px/1 var(--mono);letter-spacing:.08em;text-transform:uppercase;color:var(--soft);opacity:0;transition:opacity .2s;pointer-events:none}
  #rail button:hover span,#rail button.here span{opacity:1}
  body.panel-open #rail,body:not(.walking) #rail{opacity:0;pointer-events:none}
  @media (max-width:860px){ #rail{display:none} #chapter{bottom:78px} #chapter b{font-size:16px} }
</style>
</head>
<body>
<canvas id="scene" aria-label="A walk across a mirror sea through Junyoung Yu's achievements. Use the chapter bar to jump between chapters."></canvas>
<div id="markers" aria-hidden="true"></div>
<div id="cities" aria-hidden="true"></div>
<div id="tooltip"></div>

<header class="hud">
  <div class="tag"><i></i>A journey through space</div>
  <h1>Junyoung Yu</h1>
  <p>Business Analytics · HKU Business School</p>
  <button class="pill cta" id="connectCta"><i></i>Let's connect</button>
</header>

<div class="hud-right">
  <button class="pill" id="soundBtn" aria-pressed="false" title="Toggle sound"></button>
  <button class="pill" id="nightBtn" aria-pressed="false" title="Toggle night mode">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.8A9 9 0 1 1 11.2 3a7 7 0 0 0 9.8 9.8z"/></svg><span>Night mode</span></button>
  <a class="pill" href="cv.pdf" target="_blank" rel="noopener" title="Download CV">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3v12m0 0-4-4m4 4 4-4M5 21h14"/></svg><span>CV</span></a>
</div>

<section id="hero" aria-label="Introduction">
  <div>
    <div class="tag"><i></i>HKU Business School Freshman</div>
    <h1>Junyoung Yu</h1>
    <p>Walk across a sea of stars. Every light on the way is a chapter of my story.</p>
    <div class="cue"><span class="mouse"></span><span id="cueText">Scroll or press W to begin</span></div>
    <div class="motto">I'm just getting started.</div>
  </div>
</section>

<div id="chapter" role="status"><div style="min-width:0"><div class="ck" id="chK"></div><b id="chT"></b></div><button class="enter" id="chGo">Explore →</button></div>
<nav id="rail" aria-label="Journey progress"><div class="track"></div><div class="fill" id="railFill"></div></nav>
<nav class="dock" id="dock" aria-label="Chapters"></nav>

<aside id="panel" aria-live="polite" aria-hidden="true">
  <div class="p-head">
    <div class="p-kicker" id="pKicker"></div>
    <h2 id="pTitle"></h2>
    <button class="p-close" id="pClose" aria-label="Close">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M6 6l12 12M18 6 6 18"/></svg>
    </button>
  </div>
  <div class="p-body" id="pBody"></div>
  <div class="p-foot">
    <button id="pPrev">← <span></span></button>
    <span class="p-count" id="pCount"></span>
    <button id="pNext"><span></span> →</button>
  </div>
</aside>

<div id="whiteout" aria-hidden="true"></div>
<section id="beyond" aria-hidden="true" aria-label="Let's connect">
  <div class="bw">
    <div class="bk">Beyond the door</div>
    <h2>Let's connect.</h2>
    <div id="beyondBody"></div>
    <button class="back" id="beyondBack">← Back to the sea</button>
  </div>
</section>
<div id="loader">Loading the universe…</div>
<script>if (matchMedia('(pointer: coarse)').matches) document.getElementById('cueText').textContent = 'Swipe up to begin';</script>

<script>
/* =========================================================
   CONTENT — edit this to update the site.
   ========================================================= */
window.CV = {
  about: {
    label: "About", object: "The Origin",
    title: "Hi, I'm Junyoung.",
    lead: "First-year Business Analytics student at HKU Business School. I'm drawn to markets, data and models — and to building things with people, from a 50-member boxing club to a recycling system that won a global UNESCO prize.",
    stats: [
      ["34,000+", "participants — placed 1st at UNESCO & Prada Sea Beyond"],
      ["9,000+", "plastic units collected by a recycling system I scaled"],
      ["25,000 RMB", "raised for Fudan University Shanghai Cancer Center"],
      ["120+", "tutors led in the Peer Advisor Program"]
    ],
    links: [
      ["Email", "mailto:junyoungyu@connect.hku.hk"],
      ["LinkedIn", "https://www.linkedin.com/in/junyoungyu07"],
      ["Download CV", "cv.pdf"]
    ]
  },
  education: {
    label: "Education", object: "The Library",
    title: "Education & coursework",
    groups: [
      { label: "Education", items: [
        { title: "BBA in Business Analytics", org: "The University of Hong Kong Business School", date: "2026 Sept – Present", tag: "Now",
          bullets: ["First-year student focusing on Probability & Statistics, Business Programming and Data-Driven Optimization"] },
        { title: "IB Diploma Programme", org: "Shanghai High School International Division", date: "2022 Sept – 2026 Jun",
          bullets: ["Mathematics: Analysis & Approaches HL", "Economics HL"] }
      ]},
      { label: "Technical coursework", items: [
        { title: "Finance & Quantitative Modeling for Analysts", org: "The Wharton School Online", date: "2026 Jun – Aug",
          bullets: ["Coursework under Prof. Michael R. Roberts on quantitative finance, statistical modeling and data-driven valuation"] },
        { title: "Becoming an Effective Leader", org: "Harvard Business School Online", date: "2025 Jun – Jul",
          bullets: ["Prof. Joshua D. Margolis's executive leadership curriculum: leaders' decision-making and organizational strategy"] }
      ]}
    ]
  },
  awards: {
    label: "Awards", object: "The Golden Arch",
    title: "Awards",
    groups: [{ items: [
      { title: "1st Place — out of 34,000+ participants", org: "UNESCO & Prada · Sea Beyond", date: "2024 Feb – 2025 Jun", tag: "Global",
        bullets: ["Won international 1st place for designing a predictive ecological model and scaling a community recycling system that collected 9,000+ plastic units"] },
      { title: "Honorable Mention — Top 10%", org: "Cambridge Centre for International Research", date: "2025 May",
        bullets: ["Awarded for my research paper evaluating globalization's impact on GDP growth and inequality"] },
      { title: "Highest Distinction — Top 5%", org: "FBLA · Economics & Business Management", date: "2023 Jan",
        bullets: ["Ranked top 5% globally in standardized exams on micro/macroeconomics, market mechanics and business management analytics"] },
      { title: "Rock Paper Scissors Champion — 1st of 100+", org: "SHSID", date: "2022 Oct",
        bullets: ["Took 1st place by predicting opponents' behavior patterns."] }
    ]}]
  },
  projects: {
    label: "Trading", object: "The Market Skyline",
    title: "Projects & trading",
    groups: [
      { label: "Projects", items: [
        { title: "Lead Portfolio Manager", org: "SHSID Investment Club", date: "2024 Sept – 2026 Jun",
          bullets: ["Managed a simulated cryptocurrency portfolio, making decisions across high-volatility digital assets",
                    "Applied position sizing and predefined risk-reward parameters to manage downside risk"] }
      ]},
      { label: "Skill", items: [
        { title: "Trading", org: "Personal practice",
          bullets: ["Experienced in personal trading; keep detailed performance journals for simulated portfolios to track metrics and refine execution"] }
      ]}
    ]
  },
  experience: {
    label: "Experience", object: "The Constellation Spire",
    title: "Experience",
    groups: [
      { label: "Industry", items: [
        { title: "Private Equity Investment Intern", org: "LH Ventures", date: "2026 Apr – May",
          bullets: ["Evaluated portfolio companies alongside senior partners, analyzing operational data to assess business milestones",
                    "Conducted industry analysis to produce reports informing follow-on investment decisions"] },
        { title: "Student Journalist Intern", org: "Maeil Business Newspaper", date: "2025 Feb – Jul",
          bullets: ["Selected as 1 of 23 student journalists in the 28th cohort to cover market trends and financial reporting"] }
      ]},
      { label: "Ambassador roles", items: [
        { title: "Campus Lead — IBM Z Student Ambassador", org: "IBM", date: "2026 Sept – Present", tag: "Now",
          bullets: ["Represent IBM Z on campus: directing technical workshops, organizing developer hackathons and driving engagement in emerging developer technologies"] },
        { title: "Campus Lead — Tencent Cloud Student Ambassador", org: "Tencent Cloud", date: "2026 Sept – Present", tag: "Now",
          bullets: ["Selected to represent Tencent Cloud on the HKU campus"] },
        { title: "Green Citizens Youth Ambassador", org: "UNESCO Hong Kong Association", date: "2026 Jul – Present", tag: "Now",
          bullets: ["Advocate regional environmental sustainability by leading community outreach, facilitating policy discussions and driving local climate action programs in Hong Kong"] },
        { title: "Learning & Development Ambassador", org: "HKU Business School", date: "2026 Sept – Present", tag: "Now",
          bullets: ["Promote professional development opportunities and career resources to business students"] },
        { title: "Common Core Student Consultant", org: "HKU", date: "2026 Sept – Present", tag: "Now",
          bullets: ["Contribute a student perspective to Common Core recommendations on the HKU student experience"] },
        { title: "Sustainability Leadership Ambassador", org: "HKU", date: "2026 Sept – Present", tag: "Now",
          bullets: ["Support campus sustainability initiatives and engage students in environmental activities"] }
      ]}
    ]
  },
  leadership: {
    label: "Leadership", object: "The Flame",
    title: "Leadership",
    groups: [{ items: [
      { title: "Founder & Co-Leader", org: "SHSID Boxing Club", date: "2024 Sept – 2026 Jun",
        bullets: ["Built and scaled the club from 3 to 50+ members, gaining sponsorship from a local gym to fund the club"] },
      { title: "President", org: "Korean Student Association", date: "2025 Sept – 2026 Jun",
        bullets: ["Directed Korean community engagement, organizing alumni networking sessions and structured tutorials to share resources among Korean students"] },
      { title: "Operations Lead", org: "Charity Running Club", date: "2024 Sept – 2026 Jun",
        bullets: ["Tracked and contributed to a collective 1,000 km running goal, driving fundraising that yielded a 25,000 RMB donation to the Fudan University Shanghai Cancer Center"] },
      { title: "Founder & President", org: "Elevate Together Association", date: "2023 Sept – 2026 Jun",
        bullets: ["Founded and scaled a peer-led habit-building organization, designing fitness challenges across multiple international schools in Shanghai"] },
      { title: "Leader of AP Department", org: "Peer Advisor Program", date: "2022 Sept – 2026 Jun",
        bullets: ["Directed operations for a 120+ tutor network, leading 10+ academic workshops and overseeing standardized midterm/final study guides"] }
    ]}]
  },
  missions: {
    label: "Missions", object: "The Beacon",
    title: "Current missions",
    lead: "Competitions I'm taking part in right now. The beacon is sweeping the sky for what's next.",
    chips: ["Bloomberg Global Trading Challenge", "Inter-University GenAI Hackathon for SDGs", "Hong Kong AI Summit BUILD ACROSS Hackathon",
            "HIV Hackathon", "Global Entrepreneurship Challenge", "HKU Social Venture Champions Challenge", "Solve MIT Challenge"]
  },
  languages: {
    label: "Languages", object: "The Home Globe",
    title: "Languages & journey",
    lead: "Korea → Shanghai → Hong Kong. Watch the globe trace the route.",
    journey: [
      { place: "Korea", when: "Roots", text: "Korean, fluent." },
      { place: "Shanghai", when: "2022 – 2026", text: "Shanghai High School International Division: IB Diploma. President of the Korean Student Association." },
      { place: "Hong Kong", when: "2026 – now", text: "HKU Business School, Business Analytics. Learning Cantonese." }
    ],
    langs: [["English", 100, "Fluent"], ["Korean", 100, "Fluent"], ["Mandarin", 100, "Fluent"], ["Cantonese", 35, "Developing"]],
    hello: ["Hello", "안녕하세요", "你好", "你好呀 (Cantonese, working on it)"]
  },
  connect: {
    label: "Connect", object: "The door",
    title: "Let's connect.",
    lead: "A project, a competition team, or just a good conversation: send a signal.",
    email: "junyoungyu@connect.hku.hk",
    links: [
      { cls: "li", name: "LinkedIn", handle: "in/junyoungyu07", url: "https://www.linkedin.com/in/junyoungyu07", icon: "in" },
      { cls: "ig", name: "Instagram", handle: "@danielyu_07", url: "https://www.instagram.com/danielyu_07/", copy: "@danielyu_07", icon: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="5"/><circle cx="12" cy="12" r="4.2"/><circle cx="17.4" cy="6.6" r="1" fill="currentColor" stroke="none"/></svg>' },
      { cls: "em", name: "Email", handle: "junyoungyu@connect.hku.hk", url: "mailto:junyoungyu@connect.hku.hk", copy: "junyoungyu@connect.hku.hk", icon: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="5" width="18" height="14" rx="3"/><path d="m4 7 8 6 8-6"/></svg>' }
    ]
  }
};
</script>

<script>
/* =========================================================
   SOUND — everything is synthesized, no audio files needed.
   Off by default; the visitor turns it on.
   ========================================================= */
window.Sound = (() => {
  let ctx, master, verb, amb, nb, on = false, twTimer;
  let pref = false; try { pref = localStorage.getItem('room-sound') === 'on'; } catch (e) {}
  const btn = document.getElementById('soundBtn');
  const ICON_ON = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H3v6h3l5 4z"/><path d="M15.5 8.5a5 5 0 0 1 0 7M18.5 5.5a9 9 0 0 1 0 13"/></svg>';
  const ICON_OFF = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H3v6h3l5 4z"/><path d="m16 9 5 6M21 9l-5 6"/></svg>';
  const paint = () => { btn.innerHTML = (on ? ICON_ON : ICON_OFF) + `<span>${on ? 'Sound on' : 'Sound off'}</span>`; btn.setAttribute('aria-pressed', on); };
  paint();

  function build(){
    ctx = new (window.AudioContext || window.webkitAudioContext)();
    master = ctx.createGain(); master.gain.value = 0; master.connect(ctx.destination);
    const len = ctx.sampleRate * 3.4, ir = ctx.createBuffer(2, len, ctx.sampleRate);
    for (let c = 0; c < 2; c++) { const d = ir.getChannelData(c); for (let i = 0; i < len; i++) d[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / len, 2.6); }
    verb = ctx.createConvolver(); verb.buffer = ir; const vg = ctx.createGain(); vg.gain.value = .6; verb.connect(vg).connect(master);
    nb = ctx.createBuffer(1, ctx.sampleRate * 2, ctx.sampleRate); const nd = nb.getChannelData(0); for (let i = 0; i < nd.length; i++) nd[i] = Math.random() * 2 - 1;
    // ambient pad: slow, detuned, filtered
    amb = ctx.createGain(); amb.gain.value = 0;
    const lp = ctx.createBiquadFilter(); lp.type = 'lowpass'; lp.frequency.value = 650; lp.Q.value = .8;
    amb.connect(lp); lp.connect(master); lp.connect(verb);
    [[110, .16], [110.7, .12], [164.8, .08], [220.4, .06], [246.9, .04], [329.6, .025]].forEach(([f, v], i) => {
      const o = ctx.createOscillator(), g = ctx.createGain(); o.type = i % 2 ? 'triangle' : 'sine'; o.frequency.value = f; g.gain.value = v;
      const l = ctx.createOscillator(), lg = ctx.createGain(); l.frequency.value = .04 + i * .027; lg.gain.value = v * .7; l.connect(lg).connect(g.gain); l.start();
      o.connect(g).connect(amb); o.start();
    });
    const l = ctx.createOscillator(), lg = ctx.createGain(); l.frequency.value = .06; lg.gain.value = 320; l.connect(lg).connect(lp.frequency); l.start();
    const n = ctx.createBufferSource(); n.buffer = nb; n.loop = true;
    const nf = ctx.createBiquadFilter(); nf.type = 'bandpass'; nf.frequency.value = 380; nf.Q.value = .6;
    const ng = ctx.createGain(); ng.gain.value = .025; n.connect(nf).connect(ng).connect(amb); n.start();
  }
  const T = () => ctx.currentTime;
  function env(g, s, a, peak, d){ g.gain.setValueAtTime(.0001, s); g.gain.exponentialRampToValueAtTime(peak, s + a); g.gain.exponentialRampToValueAtTime(.0001, s + a + d); }
  function tone(f, { type = 'sine', t = 0, a = .01, d = .6, v = .2, to = null, wet = true } = {}){
    const s = T() + t, o = ctx.createOscillator(), g = ctx.createGain(); o.type = type;
    o.frequency.setValueAtTime(f, s); if (to) o.frequency.exponentialRampToValueAtTime(to, s + a + d * .6);
    env(g, s, a, v, d); o.connect(g); g.connect(master); if (wet) g.connect(verb); o.start(s); o.stop(s + a + d + .1);
  }
  function noise({ t = 0, a = .01, d = .3, v = .2, type = 'bandpass', f = 1000, to = null, q = 1, wet = true } = {}){
    const s = T() + t, src = ctx.createBufferSource(), fl = ctx.createBiquadFilter(), g = ctx.createGain();
    src.buffer = nb; fl.type = type; fl.Q.value = q; fl.frequency.setValueAtTime(f, s); if (to) fl.frequency.exponentialRampToValueAtTime(to, s + a + d);
    env(g, s, a, v, d); src.connect(fl).connect(g); g.connect(master); if (wet) g.connect(verb); src.start(s, Math.random()); src.stop(s + a + d + .1);
  }
  const PENTA = [523.3, 587.3, 659.3, 784, 880, 1046.5, 1174.7];
  function twinkle(){ if (!on) return; tone(PENTA[Math.random() * PENTA.length | 0] * (Math.random() < .35 ? .5 : 1), { a: .02, d: 2.8, v: .03 }); twTimer = setTimeout(twinkle, 3500 + Math.random() * 6500); }
  const FX = {
    thud(){ tone(120, { to: 40, a: .004, d: .35, v: .75, wet: false }); noise({ type: 'lowpass', f: 650, d: .14, v: .4, wet: false }); tone(65, { a: .005, d: .5, v: .25 }); },
    clink(){ [1568, 2349, 3136, 2093].forEach((f, i) => tone(f, { t: i * .09, a: .003, d: 1.5, v: .07 })); },
    blip(){ tone(440, { to: 1320, a: .01, d: .35, v: .12, type: 'triangle' }); tone(1320, { t: .12, a: .01, d: .9, v: .04 }); },
    swish(){ noise({ f: 900, to: 3600, a: .05, d: .3, v: .2, q: 1.2 }); noise({ t: .9, f: 3000, to: 800, a: .05, d: .25, v: .12, q: 1.2 }); },
    keys(){ for (let i = 0; i < 8; i++) noise({ t: i * .055 + Math.random() * .02, type: 'highpass', f: 3500, a: .002, d: .03, v: .14, wet: false }); tone(880, { t: .48, a: .005, d: .3, v: .05, type: 'square' }); },
    chime(){ [523.3, 659.3, 784, 987.8].forEach((f, i) => tone(f, { t: i * .07, a: .01, d: 1.7, v: .06, type: 'triangle' })); },
    sweep(){ const s = T(), o = ctx.createOscillator(), f = ctx.createBiquadFilter(), g = ctx.createGain();
      o.type = 'sawtooth'; o.frequency.value = 110; f.type = 'lowpass'; f.Q.value = 9;
      f.frequency.setValueAtTime(200, s); f.frequency.exponentialRampToValueAtTime(3200, s + 1.2); f.frequency.exponentialRampToValueAtTime(260, s + 2.6);
      env(g, s, .15, .05, 2.5); o.connect(f).connect(g); g.connect(master); g.connect(verb); o.start(s); o.stop(s + 2.8);
      tone(1760, { t: 1.4, a: .01, d: 1.6, v: .05 }); tone(2637, { t: 1.5, a: .01, d: 1.4, v: .025 }); },
    whoosh(){ noise({ f: 300, to: 1700, a: .45, d: .9, v: .15, q: .8 }); tone(392, { t: .9, a: .03, d: 1.2, v: .045, type: 'triangle' }); tone(587.3, { t: 2.3, a: .03, d: 1.5, v: .045, type: 'triangle' }); },
    step(){ const f = 520 + Math.random() * 260; tone(f, { to: f * .45, a: .004, d: .16, v: .045 }); noise({ type: 'lowpass', f: 900, a: .004, d: .07, v: .025, wet: false }); },
    door(){ noise({ f: 180, to: 1400, a: .9, d: 1.8, v: .09, q: .6 }); [392, 523.3, 659.3, 784, 1046.5].forEach((f, i) => tone(f, { t: .5 + i * .14, a: .4, d: 3.2, v: .04 })); },
    transmit(){ [1200, 1600, 2000].forEach((f, i) => tone(f, { t: i * .09, a: .005, d: .12, v: .05, type: 'square', wet: false })); tone(880, { t: .32, to: 1760, a: .01, d: .5, v: .06, type: 'triangle' }); noise({ t: .3, f: 2000, to: 5200, a: .05, d: .5, v: .05, q: 2 }); },
    close(){ tone(660, { to: 330, a: .01, d: .25, v: .05, type: 'triangle' }); }
  };
  function set(v){
    on = v; paint(); try { localStorage.setItem('room-sound', v ? 'on' : 'off'); } catch (e) {}
    if (v) { if (!ctx) build(); ctx.resume(); master.gain.cancelScheduledValues(T()); master.gain.setTargetAtTime(.9, T(), .3); amb.gain.setTargetAtTime(.5, T(), 2.5); clearTimeout(twTimer); twTimer = setTimeout(twinkle, 2500); }
    else if (ctx) { master.gain.setTargetAtTime(0, T(), .2); clearTimeout(twTimer); }
  }
  btn.addEventListener('click', () => set(!on));
  return { toggle: () => set(!on), autoStart(){ if (pref && !on) set(true); }, play(n){ if (on && ctx && FX[n]) FX[n](); } };
})();
</script>

<script>
/* =========================================================
   PANEL / UI  (works even if WebGL is unavailable)
   ========================================================= */
(function(){
  const CV = window.CV, ORDER = Object.keys(CV);
  const $ = s => document.querySelector(s);
  const panel = $('#panel'), dock = $('#dock');
  let current = null;

  ORDER.forEach(id => {
    const b = document.createElement('button');
    b.textContent = CV[id].label; b.dataset.id = id;
    b.addEventListener('click', () => current === id ? close() : open(id));
    dock.appendChild(b);
  });

  function itemHTML(it){
    return `<div class="item">
      ${it.date ? `<div class="date">${it.date}</div>` : ''}
      <h3>${it.title}${it.tag ? `<span class="badge ${it.tag==='Now'?'live':''}">${it.tag}</span>` : ''}</h3>
      ${it.org ? `<div class="org">${it.org}</div>` : ''}
      ${it.bullets ? `<ul>${it.bullets.map(b=>`<li>${b}</li>`).join('')}</ul>` : ''}
    </div>`;
  }

  function render(id){
    const s = CV[id];
    $('#pKicker').textContent = `${s.object} · ${String(ORDER.indexOf(id)+1).padStart(2,'0')}`;
    $('#pTitle').textContent = s.title;
    let h = '';
    if (s.lead) h += `<p class="lead">${s.lead}</p>`;
    if (s.stats) h += `<div class="stats">${s.stats.map(([b,t])=>`<div class="stat"><b>${b}</b><span>${t}</span></div>`).join('')}</div>`;
    if (s.links && Array.isArray(s.links[0])) h += `<div class="links">${s.links.map(([t,u])=>`<a class="pill" href="${u}" ${u.startsWith('http')?'target="_blank" rel="noopener"':''}>${t} ↗</a>`).join('')}</div>`;
    if (s.groups) s.groups.forEach(g => {
      h += `<div>${g.label ? `<div class="group-label">${g.label}</div>` : ''}${g.items.map(itemHTML).join('')}</div>`;
    });
    if (s.chips) h += `<div class="chips">${s.chips.map(c=>`<div class="chip">${c}</div>`).join('')}</div>`;
    if (s.journey) h += `<div class="group-label">Journey</div><div class="journey">${s.journey.map(j=>`<div class="stop"><div class="date">${j.when}</div><b>${j.place}</b><p>${j.text}</p></div>`).join('')}</div><div class="group-label">Languages</div>`;
    if (s.langs) h += `<div style="margin-top:8px">${s.langs.map(([n,v,l])=>`<div class="lang"><b>${n}</b><div class="bar"><i style="width:${v}%"></i></div><small>${l}</small></div>`).join('')}</div>`;
    if (s.hello) h += `<div class="hello">${s.hello.map(x=>`<span>${x}</span>`).join('')}</div>`;
    if (s.email && s.links && s.links[0].cls) h = connectHTML(s);
    const body = $('#pBody'); body.innerHTML = h; body.scrollTop = 0;
    if (s.email && s.links && s.links[0].cls) wireConnect(s, body);
    const i = ORDER.indexOf(id), prev = ORDER[(i-1+ORDER.length)%ORDER.length], next = ORDER[(i+1)%ORDER.length];
    $('#pPrev span').textContent = CV[prev].label; $('#pPrev').onclick = () => open(prev);
    $('#pNext span').textContent = CV[next].label; $('#pNext').onclick = () => open(next);
    $('#pCount').textContent = `${i+1} / ${ORDER.length}`;
  }

  function connectHTML(s){
    return `<div class="radar-wrap"><div class="radar"><i class="sweep"></i><b class="blip b1"></b><b class="blip b2"></b><b class="blip b3"></b></div><p class="lead">${s.lead}</p></div>
    <div class="ccards">${s.links.map(l => `<div class="crow ${l.copy ? 'has-copy' : ''}"><a class="ccard ${l.cls}" href="${l.url}" ${l.url.startsWith('http') ? 'target="_blank" rel="noopener"' : ''}><span class="cicon">${l.icon}</span><span class="ctext"><small>${l.name}</small><b>${l.handle}</b></span><span class="cgo">↗</span></a>${l.copy ? `<button class="ccopy" data-copy="${l.copy}">Copy</button>` : ''}</div>`).join('')}</div>
    <div class="group-label">Leave a transmission</div>
    <div class="tx">
      <input id="txName" placeholder="Your name" autocomplete="name">
      <textarea id="txMsg" rows="3" placeholder="Say hi, pitch a project, ask about a competition…"></textarea>
      <div class="tx-row"><span class="sig" aria-hidden="true"><i></i><i></i><i></i><i></i><i></i></span><span class="tx-hint" id="txHint">No signal yet</span><button class="enter tx-send" id="txSend" disabled>Transmit →</button></div>
      <small class="tx-note">Opens your email app with the message ready to send.</small>
    </div>`;
  }
  function wireConnect(s, body){
    const ping = () => { window.Room && window.Room.signal(); window.Sound && window.Sound.play('transmit'); };
    body.querySelectorAll('.ccard').forEach(c => {
      c.addEventListener('pointermove', e => {
        const r = c.getBoundingClientRect(), x = (e.clientX - r.left) / r.width, y = (e.clientY - r.top) / r.height;
        c.style.setProperty('--mx', x * 100 + '%'); c.style.setProperty('--my', y * 100 + '%');
        c.style.transform = `perspective(700px) rotateX(${(.5 - y) * 8}deg) rotateY(${(x - .5) * 10}deg) translateY(-2px)`;
      });
      c.addEventListener('pointerleave', () => { c.style.transform = ''; });
      c.addEventListener('click', ping);
    });
    body.querySelectorAll('.ccopy').forEach(b => b.addEventListener('click', () => {
      const t = b.dataset.copy, done = () => { b.textContent = 'Copied ✓'; setTimeout(() => b.textContent = 'Copy', 1600); ping(); };
      const fallback = () => { const ta = document.createElement('textarea'); ta.value = t; document.body.appendChild(ta); ta.select(); try { document.execCommand('copy'); } catch (e) {} ta.remove(); done(); };
      navigator.clipboard ? navigator.clipboard.writeText(t).then(done, fallback) : fallback();
    }));
    const name = body.querySelector('#txName'), msg = body.querySelector('#txMsg'), send = body.querySelector('#txSend'), hint = body.querySelector('#txHint'), bars = body.querySelectorAll('.sig i');
    const LEVELS = ['No signal yet', 'Signal forming', 'Getting clearer', 'Strong signal', 'Crystal clear', 'Ready to transmit'];
    const update = () => {
      const n = msg.value.trim().length + (name.value.trim() ? 10 : 0), lv = Math.min(5, Math.ceil(n / 22));
      bars.forEach((b, i) => b.classList.toggle('on', i < lv)); hint.textContent = LEVELS[lv]; send.disabled = msg.value.trim().length < 2;
    };
    name.addEventListener('input', update); msg.addEventListener('input', update);
    send.addEventListener('click', () => {
      const who = name.value.trim() || 'a visitor';
      const url = `mailto:${s.email}?subject=${encodeURIComponent(`Hello from ${who}, via your universe`)}&body=${encodeURIComponent(msg.value.trim() + `\n\n— ${who}`)}`;
      ping(); hint.textContent = 'Transmitting…'; setTimeout(() => { location.href = url; }, 450);
    });
  }

  function open(id){
    if (id === 'connect' && window.Room && window.Room.openDoor) { if (current) close(); window.Room.openDoor(); return; }
    current = id; render(id);
    panel.classList.add('open'); panel.setAttribute('aria-hidden','false');
    document.body.classList.add('panel-open');
    dock.querySelectorAll('button').forEach(b => b.classList.toggle('active', b.dataset.id === id));
    dock.querySelector(`[data-id="${id}"]`).scrollIntoView({block:'nearest', inline:'center', behavior:'smooth'});
    window.Room && window.Room.focus(id);
  }
  function close(){
    if (!current) return;
    current = null;
    window.Sound && Sound.play('close');
    panel.classList.remove('open'); panel.setAttribute('aria-hidden','true');
    document.body.classList.remove('panel-open');
    dock.querySelectorAll('button').forEach(b => b.classList.remove('active'));
    window.Room && window.Room.reset();
  }

  $('#pClose').addEventListener('click', close);
  $('#connectCta').addEventListener('click', () => open('connect'));
  addEventListener('keydown', e => {
    if (e.target.closest && e.target.closest('input, textarea')) return;
    if (e.key === 'Escape') { if ($('#beyond').classList.contains('show')) window.Room && window.Room.leaveDoor(); else close(); }
    if (current && (e.key === 'ArrowRight' || e.key === 'ArrowLeft')) {
      const i = ORDER.indexOf(current), d = e.key === 'ArrowRight' ? 1 : -1;
      open(ORDER[(i+d+ORDER.length)%ORDER.length]);
    }
  });

  function beyond(show){
    const el = $('#beyond');
    if (show) { const s = CV.connect, b = $('#beyondBody'); b.innerHTML = connectHTML(s); wireConnect(s, b); el.classList.add('show'); el.setAttribute('aria-hidden', 'false'); }
    else { el.classList.remove('show'); el.setAttribute('aria-hidden', 'true'); }
  }
  $('#beyondBack').addEventListener('click', () => window.Room && window.Room.leaveDoor());

  window.UI = { open, close, beyond, get current(){ return current; } };

  // if 3D can't load, the chapters still work from the bottom bar
  setTimeout(() => { if (!window.Room) document.body.classList.add('ready', 'walking'); }, 8000);
})();
</script>

<script type="importmap">
{ "imports": {
  "three": "https://cdn.jsdelivr.net/npm/three@0.169.0/build/three.module.js",
  "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.169.0/examples/jsm/"
}}
</script>

<script type="module">
/* =========================================================
   THE JOURNEY — a walk across a mirror sea
   ========================================================= */
import * as THREE from 'three';
import { RoomEnvironment } from 'three/addons/environments/RoomEnvironment.js';
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';
import { OutputPass } from 'three/addons/postprocessing/OutputPass.js';
import { Reflector } from 'three/addons/objects/Reflector.js';

const V = (x,y,z) => new THREE.Vector3(x,y,z);
const reduceMotion = matchMedia('(prefers-reduced-motion: reduce)').matches;
const canvas = document.getElementById('scene');
const LOWQ = matchMedia('(max-width: 860px)').matches;
const ease = x => x < .5 ? 4*x*x*x : 1 - Math.pow(-2*x + 2, 3) / 2;
const clamp = THREE.MathUtils.clamp, L = THREE.MathUtils.lerp, smooth = THREE.MathUtils.smoothstep;

const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, powerPreference: 'high-performance' });
renderer.setPixelRatio(Math.min(devicePixelRatio, LOWQ ? 1.5 : 1.75));
renderer.setSize(innerWidth, innerHeight);
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 1.05;

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x06040f);
scene.fog = new THREE.FogExp2(0x24142e, .0085);
const fogDay = new THREE.Color(0x24142e), fogNight = new THREE.Color(0x07050d);
const pmrem = new THREE.PMREMGenerator(renderer);
scene.environment = pmrem.fromScene(new RoomEnvironment(), 0.04).texture;
scene.environmentIntensity = 0.3;

const camera = new THREE.PerspectiveCamera(LOWQ ? 62 : 50, innerWidth / innerHeight, 0.1, 1400);

const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));
const bloom = new UnrealBloomPass(new THREE.Vector2(innerWidth, innerHeight), 0.45, 0.6, 0.9);
composer.addPass(bloom);
composer.addPass(new OutputPass());

/* ---------- helpers ---------- */
const world = new THREE.Group(); scene.add(world);
const B = (w,h,d) => new THREE.BoxGeometry(w,h,d);
function mesh(geo, mat, x=0, y=0, z=0, parent=world){ const m = new THREE.Mesh(geo, mat); m.position.set(x,y,z); parent.add(m); return m; }
function tex(w, h, draw){
  const c = document.createElement('canvas'); c.width = w; c.height = h;
  draw(c.getContext('2d'), w, h);
  const t = new THREE.CanvasTexture(c); t.colorSpace = THREE.SRGBColorSpace;
  t.anisotropy = renderer.capabilities.getMaxAnisotropy(); return t;
}
const rnd = (a=0, b=1) => a + Math.random() * (b - a);
const std = (color, o={}) => new THREE.MeshStandardMaterial({ color, roughness: .8, metalness: 0, ...o });
const glowMat = (color, i=2) => new THREE.MeshStandardMaterial({ color: 0x000000, emissive: color, emissiveIntensity: i });
const M = {
  gold: std(0xf3c35a, { metalness: 1, roughness: .22, side: THREE.DoubleSide }),
  silver: std(0xdfe4ee, { metalness: 1, roughness: .2, side: THREE.DoubleSide }),
  bronze: std(0xc77b45, { metalness: 1, roughness: .3, side: THREE.DoubleSide }),
  base: std(0x2a2030, { metalness: .6, roughness: .4 }),
  chrome: std(0xd8dde8, { metalness: 1, roughness: .18 }),
};
const softDot = tex(128, 128, (g, w) => {
  const rg = g.createRadialGradient(w/2, w/2, 0, w/2, w/2, w/2);
  rg.addColorStop(0, 'rgba(255,255,255,1)'); rg.addColorStop(.25, 'rgba(255,255,255,.5)'); rg.addColorStop(1, 'rgba(255,255,255,0)');
  g.fillStyle = rg; g.fillRect(0,0,w,w);
});
const BADGES = [
  ['HKU','L&D','#6fe7ff'], ['HKU','COMMON CORE','#b39bff'], ['HKU','SUSTAIN','#3ff2a4'], ['TENCENT','CLOUD','#4f8bff'],
  ['IBM Z','CAMPUS LEAD','#7aa2ff'], ['UNESCO','GREEN CITIZEN','#5fd3a0'], ['LH','VENTURES','#ffbe7a'], ['MAEIL','JOURNALIST','#ff7a8a']
];
const globeTex = tex(1024, 512, (g, w, h) => {
  const bg = g.createLinearGradient(0,0,0,h); bg.addColorStop(0,'#2a4f8f'); bg.addColorStop(.5,'#1f6aa8'); bg.addColorStop(1,'#2a4f8f');
  g.fillStyle = bg; g.fillRect(0,0,w,h);
  const land = ['#e9d8a6', '#94c9a9', '#d9b48f'];
  [[860,175,45,45],[180,150,90,70],[230,300,60,110],[520,140,70,60],[540,280,70,90],[700,160,170,80],[820,330,60,40],[560,90,40,25],[300,100,50,30]].forEach(([x,y,rx,ry]) => {
    for (let k = 0; k < 18; k++) { g.fillStyle = land[k % 3]; g.beginPath(); g.ellipse(x + rnd(-rx,rx)*.7, y + rnd(-ry,ry)*.7, rx*rnd(.25,.6), ry*rnd(.25,.6), rnd(0,3), 0, Math.PI*2); g.fill(); }
  });
  g.strokeStyle = 'rgba(255,255,255,.18)'; g.lineWidth = 1.5;
  for (let x = 0; x <= w; x += w/12) { g.beginPath(); g.moveTo(x,0); g.lineTo(x,h); g.stroke(); }
  for (let y = h/6; y < h; y += h/6) { g.beginPath(); g.moveTo(0,y); g.lineTo(w,y); g.stroke(); }
  [[873,149],[857,167],[836,193]].forEach(([x,y]) => { g.beginPath(); g.arc(x,y,8,0,Math.PI*2); g.fillStyle = '#ff5c7a'; g.fill(); g.lineWidth = 3; g.strokeStyle = '#fff'; g.stroke(); });
});
function sprite(color, scale, opacity){
  const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: softDot, color, transparent: true, opacity, depthWrite: false, blending: THREE.AdditiveBlending }));
  s.scale.setScalar(scale); return s;
}
function cup(mat, s){
  const g = new THREE.Group();
  const pts = [[0,0],[.13,0],[.13,.04],[.05,.06],[.035,.16],[.05,.2],[.14,.24],[.16,.42],[.15,.425],[.13,.26],[0,.23]].map(([x,y]) => new THREE.Vector2(x*s, y*s));
  mesh(new THREE.LatheGeometry(pts, 40), mat, 0, .04*s, 0, g);
  [-1, 1].forEach(sd => { const h = mesh(new THREE.TorusGeometry(.065*s, .014*s, 8, 20, Math.PI), mat, sd*.16*s, .36*s, 0, g); h.rotation.z = sd > 0 ? -Math.PI/2 : Math.PI/2; });
  mesh(B(.24*s, .05*s, .24*s), M.base, 0, .02*s, 0, g);
  return g;
}

/* =========================================================
   THE PATH — a soft curve across the sea, ending at a door
   ========================================================= */
const CV = window.CV, ORDER = Object.keys(CV), CHAPTERS = ORDER.filter(id => id !== 'connect');
const GAP = 36;
const STOP = {}; CHAPTERS.forEach((id, i) => STOP[id] = (i + 1) * GAP);
const DOOR_S = (CHAPTERS.length + 1) * GAP + 4;
const END_S = DOOR_S - 8;          // where you stand in front of the door
const ROOM_S = DOOR_S + 6;         // the path runs a little past the door, into the light
STOP.connect = END_S;
const pathX = s => Math.sin(s * .017) * 7 + Math.sin(s * .043 + 1) * 2.2;
const pathAt = (s, o = new THREE.Vector3()) => o.set(pathX(s), 0, -s);
const pathDir = s => pathAt(s + .5).sub(pathAt(s)).normalize();
const pathSide = s => { const d = pathDir(s); return V(-d.z, 0, d.x); };
const invisible = new THREE.MeshBasicMaterial({ transparent: true, opacity: 0, depthWrite: false, colorWrite: false });

/* =========================================================
   SKY — dusk violet and gold
   ========================================================= */
const SKY_GLSL = `
uniform float nightK;
float hash(vec3 p){ p = fract(p * .3183099 + .1); p *= 17.; return fract(p.x * p.y * p.z * (p.x + p.y + p.z)); }
float noise(vec3 x){ vec3 i = floor(x), f = fract(x); f = f * f * (3. - 2. * f);
  return mix(mix(mix(hash(i), hash(i + vec3(1,0,0)), f.x), mix(hash(i + vec3(0,1,0)), hash(i + vec3(1,1,0)), f.x), f.y),
             mix(mix(hash(i + vec3(0,0,1)), hash(i + vec3(1,0,1)), f.x), mix(hash(i + vec3(0,1,1)), hash(i + vec3(1,1,1)), f.x), f.y), f.z); }
float fbm(vec3 p){ float a = .5, s = 0.; for (int i = 0; i < 5; i++){ s += a * noise(p); p = p * 2.03 + vec3(1.7, 9.2, 3.1); a *= .5; } return s; }
vec3 sky(vec3 d){
  float y = d.y, hy = max(y, 0.);
  vec3 zen = mix(vec3(.012, .01, .038), vec3(.004, .004, .014), nightK);
  vec3 mid = mix(vec3(.055, .026, .095), vec3(.016, .012, .04), nightK);
  vec3 hor = mix(vec3(.3, .14, .13), vec3(.06, .03, .06), nightK);
  vec3 col = mix(mid, zen, smoothstep(.06, .65, hy));
  col = mix(hor, col, smoothstep(0., .2, hy));
  float sd = max(dot(d, normalize(vec3(-.05, .06, -1.))), 0.);
  col += vec3(1., .6, .3) * (pow(sd, 22.) * .28 + pow(sd, 4.) * .07) * (1. - nightK * .7);
  float b = dot(d, normalize(vec3(.6, .9, .2)));
  float band = exp(-b * b * 9.), core = exp(-b * b * 38.);
  float n = fbm(d * 3.2), n2 = fbm(d * 7. + n * 1.6);
  float dust = smoothstep(.42, .72, fbm(d * 6. + vec3(3.))) * core * 1.2;
  float vis = smoothstep(0., .25, hy) * mix(.55, 1.2, nightK);
  vec3 gal = band * vec3(.04, .04, .08) * n2 * 1.4 + core * mix(vec3(.2, .13, .22), vec3(.34, .22, .13), n2) * pow(n2, 1.8) * 1.3;
  col += gal * (1. - clamp(dust, 0., .9)) * vis;
  float neb = pow(fbm(d * 2. + vec3(4., 1., 7.)), 3.4) * .85;
  vec3 nebC = mix(vec3(.36, .1, .46), vec3(.06, .26, .4), smoothstep(.3, .7, fbm(d * 1.4 + 2.)));
  nebC = mix(nebC, vec3(.62, .26, .14), smoothstep(.55, .8, fbm(d * 2.6 + 9.)) * .7);
  col += neb * nebC * (.4 + .8 * fbm(d * 9.)) * smoothstep(-.02, .3, y) * mix(1., .8, nightK);
  col *= .42;
  vec3 q = d * 300.; vec3 id = floor(q); float h = hash(id);
  if (h > .982) { float s = pow(max(0., 1. - length(fract(q) - .5) * 2.4), 5.); col += s * mix(vec3(.75, .8, 1.), vec3(1., .82, .6), hash(id + 3.)) * (1. + 2. * band) * smoothstep(.02, .3, hy) * mix(.7, 1.7, nightK); }
  return col;
}`;
const sky = new THREE.Mesh(new THREE.SphereGeometry(500, 64, 32), new THREE.ShaderMaterial({
  side: THREE.BackSide, depthWrite: false, uniforms: { nightK: { value: 0 } },
  vertexShader: `varying vec3 vDir; void main(){ vDir = position; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.); }`,
  fragmentShader: SKY_GLSL + `varying vec3 vDir; void main(){ gl_FragColor = vec4(sky(normalize(vDir)), 1.); }`
}));
sky.renderOrder = -20; sky.frustumCulled = false; scene.add(sky);

// twinkling stars (follow the camera, like the sky)
const stars = (() => {
  const N = LOWQ ? 2200 : 4200, pos = [], col = [], size = [], ph = [], c = new THREE.Color();
  for (let i = 0; i < N; i++) {
    const v = new THREE.Vector3().randomDirection(); v.y = Math.abs(v.y) * .95 + .02; v.normalize().multiplyScalar(rnd(300, 470)); pos.push(v.x, v.y, v.z);
    c.setHSL(rnd(.6, .75), rnd(.2, .5), rnd(.75, 1)); if (Math.random() < .18) c.setHSL(rnd(.06, .11), .8, .82); col.push(c.r, c.g, c.b);
    size.push(Math.random() < .025 ? rnd(4, 7.5) : rnd(1, 2.6)); ph.push(Math.random());
  }
  const g = new THREE.BufferGeometry();
  g.setAttribute('position', new THREE.Float32BufferAttribute(pos, 3)); g.setAttribute('color', new THREE.Float32BufferAttribute(col, 3));
  g.setAttribute('size', new THREE.Float32BufferAttribute(size, 1)); g.setAttribute('phase', new THREE.Float32BufferAttribute(ph, 1));
  const p = new THREE.Points(g, new THREE.ShaderMaterial({
    transparent: true, depthWrite: false, blending: THREE.AdditiveBlending,
    uniforms: { time: { value: 0 }, pr: { value: renderer.getPixelRatio() }, bright: { value: 1 } },
    vertexShader: `attribute float size; attribute float phase; attribute vec3 color; uniform float time, pr, bright; varying vec3 vC; varying float vS;
      void main(){ float tw = .6 + .4 * sin(time * (1. + phase * 2.5) + phase * 40.); vC = color * (.55 + .6 * tw) * bright; vS = size;
        gl_PointSize = size * pr * (.85 + .3 * tw); gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.); }`,
    fragmentShader: `varying vec3 vC; varying float vS;
      void main(){ vec2 c = gl_PointCoord - .5; float d = length(c); float a = pow(max(0., 1. - d * 2.), 2.4);
        if (vS > 3.5) a += max(0., 1. - abs(c.x) * 18.) * max(0., 1. - abs(c.y) * 2.) * .5 + max(0., 1. - abs(c.y) * 18.) * max(0., 1. - abs(c.x) * 2.) * .5;
        gl_FragColor = vec4(vC * a * (vS > 3.5 ? 2.2 : 1.3), a); }`
  }));
  p.renderOrder = -10; p.frustumCulled = false; scene.add(p); return p;
})();



/* =========================================================
   THE MIRROR SEA
   ========================================================= */
const RIPPLES = 10;
const SeaShader = {
  name: 'MirrorSea',
  uniforms: {
    color: { value: null }, tDiffuse: { value: null }, textureMatrix: { value: null },
    time: { value: 0 }, camPos: { value: new THREE.Vector3() }, horizon: { value: new THREE.Color(0x2a1430) },
    ripples: { value: Array.from({ length: RIPPLES }, () => new THREE.Vector4(0, 0, -99, 0)) }
  },
  vertexShader: `uniform mat4 textureMatrix; varying vec4 vUv; varying vec3 vWorld;
    #include <common>
    #include <logdepthbuf_pars_vertex>
    void main(){ vUv = textureMatrix * vec4(position, 1.); vec4 wp = modelMatrix * vec4(position, 1.); vWorld = wp.xyz;
      gl_Position = projectionMatrix * viewMatrix * wp;
      #include <logdepthbuf_vertex>
    }`,
  fragmentShader: `uniform vec3 color; uniform sampler2D tDiffuse; uniform float time; uniform vec3 camPos, horizon; uniform vec4 ripples[${RIPPLES}];
    varying vec4 vUv; varying vec3 vWorld;
    #include <logdepthbuf_pars_fragment>
    float h2(vec2 p){ return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453); }
    float n2(vec2 p){ vec2 i = floor(p), f = fract(p); f = f * f * (3. - 2. * f); return mix(mix(h2(i), h2(i + vec2(1, 0)), f.x), mix(h2(i + vec2(0, 1)), h2(i + vec2(1, 1)), f.x), f.y); }
    void main(){
      #include <logdepthbuf_fragment>
      vec2 p = vWorld.xz;
      vec2 d = (vec2(n2(p * .22 + time * .04), n2(p * .22 + 17. - time * .03)) - .5) * .3
             + (vec2(n2(p * 1.5 + time * .16), n2(p * 1.5 + 5. + time * .13)) - .5) * .1;
      float rip = 0.;
      for (int i = 0; i < ${RIPPLES}; i++) {
        vec4 r = ripples[i]; float age = time - r.z;
        if (age < 0. || age > 5.) continue;
        vec2 dv = p - r.xy; float dist = length(dv), front = age * 1.5;
        float w = exp(-pow((dist - front) * 1.8, 2.)) * exp(-age * .8) * r.w;
        d += dv / max(dist, .001) * sin((dist - front) * 12.) * w * 1.5; rip += w;
      }
      float dist = length(vWorld - camPos);
      vec4 uv = vUv; uv.xy += d * uv.w * .035 / (1. + dist * .015);
      vec3 refl = texture2DProj(tDiffuse, uv).rgb;
      vec3 vd = normalize(camPos - vWorld);
      float fres = .58 + .42 * pow(1. - max(vd.y, 0.), 3.);
      vec3 col = mix(color, refl, fres);
      col += vec3(1., .82, .58) * rip * .05;
      float graze = 1. - smoothstep(0., .07, vd.y);
      col = mix(col, horizon * 1.1, max(smoothstep(140., 650., dist) * .95, graze * .85));
      gl_FragColor = vec4(col, 1.);
      #include <tonemapping_fragment>
      #include <colorspace_fragment>
    }`
};
const rtScale = LOWQ ? .4 : .55;
const water = new Reflector(new THREE.PlaneGeometry(1800, 1800), {
  shader: SeaShader, clipBias: .003, color: 0x0a0614, multisample: LOWQ ? 0 : 4,
  textureWidth: Math.max(2, innerWidth * renderer.getPixelRatio() * rtScale), textureHeight: Math.max(2, innerHeight * renderer.getPixelRatio() * rtScale)
});
water.rotation.x = -Math.PI / 2; scene.add(water);
const WU = water.material.uniforms;
let ripI = 0;
function ripple(x, z, strength = 1){ WU.ripples.value[ripI].set(x, z, WU.time.value, strength); ripI = (ripI + 1) % RIPPLES; }

/* ---------- the glowing path ---------- */
const PATH_W = 2.4;
const pathGeo = (() => {
  const SEG = 900, pos = [], uv = [], idx = [], p = new THREE.Vector3();
  for (let i = 0; i <= SEG; i++) {
    const s = -14 + (ROOM_S + 4) * i / SEG; pathAt(s, p); const n = pathSide(s);
    pos.push(p.x - n.x * PATH_W / 2, .03, p.z - n.z * PATH_W / 2, p.x + n.x * PATH_W / 2, .03, p.z + n.z * PATH_W / 2);
    uv.push(0, s, 1, s);
    if (i < SEG) { const a = i * 2; idx.push(a, a + 2, a + 1, a + 1, a + 2, a + 3); }
  }
  const g = new THREE.BufferGeometry();
  g.setAttribute('position', new THREE.Float32BufferAttribute(pos, 3)); g.setAttribute('uv', new THREE.Float32BufferAttribute(uv, 2)); g.setIndex(idx);
  return g;
})();
const pathMat = new THREE.ShaderMaterial({
  transparent: true, depthWrite: false, blending: THREE.AdditiveBlending, side: THREE.DoubleSide,
  uniforms: { time: { value: 0 }, walkS: { value: 0 }, nightK: { value: 0 } },
  vertexShader: `varying vec2 vUv; void main(){ vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.); }`,
  fragmentShader: `uniform float time, walkS, nightK; varying vec2 vUv;
    float h(vec2 p){ return fract(sin(dot(p, vec2(41.3, 289.1))) * 43758.5); }
    void main(){
      float x = vUv.x * 2. - 1., s = vUv.y;
      float edge = pow(max(0., 1. - abs(x)), 1.6);
      float core = exp(-x * x * 26.);
      float walked = smoothstep(walkS + 3., walkS - 3., s);
      float ahead = exp(-max(0., s - walkS - 30.) / 90.);
      float near = exp(-pow((s - walkS) / 7., 2.));
      vec2 q = vec2(vUv.x * 12., s * 2.2); vec2 cell = floor(q); float r = h(cell);
      float tw = step(.86, r) * pow(max(0., 1. - length(fract(q) - .5) * 2.6), 2.) * (.5 + .5 * sin(time * 2.5 + r * 40.));
      float I = (edge * .06 + core * .17) * mix(.45, 1., walked) * ahead + near * core * .28 + tw * edge * .7 * ahead;
      vec3 c = mix(vec3(1., .78, .5), vec3(.78, .7, 1.), smoothstep(-.5, 1., x) * .4);
      gl_FragColor = vec4(c * I * mix(1., 1.25, nightK), 1.);
    }`
});
const pathMesh = new THREE.Mesh(pathGeo, pathMat); pathMesh.frustumCulled = false; scene.add(pathMesh);

// floating light motes along the way
const MOTES = LOWQ ? 260 : 520;
const moteGeo = new THREE.BufferGeometry();
{
  const a = new Float32Array(MOTES * 3), p = new THREE.Vector3();
  for (let i = 0; i < MOTES; i++) { const s = rnd(-10, ROOM_S); pathAt(s, p).addScaledVector(pathSide(s), rnd(-16, 16)); a.set([p.x, rnd(.3, 7), p.z], i * 3); }
  moteGeo.setAttribute('position', new THREE.BufferAttribute(a, 3));
}
const motes = new THREE.Points(moteGeo, new THREE.PointsMaterial({ size: .16, map: softDot, color: 0xffd7a0, transparent: true, opacity: .8, depthWrite: false, blending: THREE.AdditiveBlending }));
motes.frustumCulled = false; scene.add(motes);
const moteSpeed = Array.from({ length: MOTES }, () => rnd(.05, .22));

/* ---------- you: a small glowing figure ---------- */
const walker = new THREE.Group(); scene.add(walker);
const wMat = new THREE.MeshStandardMaterial({ color: 0x241a3a, emissive: 0xffd2a0, emissiveIntensity: .95, roughness: .4 });
function limb(len, r, x, y){ const pv = new THREE.Group(); pv.position.set(x, y, 0); const m = new THREE.Mesh(new THREE.CapsuleGeometry(r, len, 4, 10), wMat); m.position.y = -len / 2 - r; pv.add(m); walker.add(pv); return pv; }
{ const b = new THREE.Mesh(new THREE.CapsuleGeometry(.16, .5, 6, 14), wMat); b.position.y = 1.06; walker.add(b);
  const h = new THREE.Mesh(new THREE.SphereGeometry(.135, 20, 16), wMat); h.position.y = 1.6; walker.add(h); }
const legL = limb(.5, .065, -.09, .74), legR = limb(.5, .065, .09, .74), armL = limb(.42, .05, -.23, 1.38), armR = limb(.42, .05, .23, 1.38);
const halo = new THREE.Sprite(new THREE.SpriteMaterial({ map: softDot, color: 0xffc98f, transparent: true, opacity: .22, depthWrite: false, blending: THREE.AdditiveBlending }));
halo.position.y = 1.15; halo.scale.setScalar(2.6); walker.add(halo);



/* =========================================================
   LIGHT MONUMENTS — one per chapter, rising out of the sea
   ========================================================= */
const monuments = {};
const columnTex = tex(4, 256, (g, w, h) => { const gr = g.createLinearGradient(0, 0, 0, h); gr.addColorStop(0, 'rgba(255,255,255,0)'); gr.addColorStop(.6, 'rgba(255,255,255,.3)'); gr.addColorStop(1, 'rgba(255,255,255,1)'); g.fillStyle = gr; g.fillRect(0, 0, w, h); });
const poolTex = tex(256, 256, (g, w) => {
  const rg = g.createRadialGradient(w/2, w/2, 0, w/2, w/2, w/2);
  rg.addColorStop(0, 'rgba(255,255,255,.9)'); rg.addColorStop(.5, 'rgba(255,255,255,.25)'); rg.addColorStop(.78, 'rgba(255,255,255,.08)'); rg.addColorStop(.8, 'rgba(255,255,255,.5)'); rg.addColorStop(.83, 'rgba(255,255,255,.05)'); rg.addColorStop(1, 'rgba(255,255,255,0)');
  g.fillStyle = rg; g.fillRect(0, 0, w, w);
});
function makeMonument(id, cfg){
  const i = CHAPTERS.indexOf(id), sideSign = i % 2 ? 1 : -1, s = STOP[id];
  const base = pathAt(s + 8).add(pathSide(s + 8).multiplyScalar(sideSign * (cfg.off || 11)));
  const group = new THREE.Group(); group.position.copy(base); scene.add(group);
  group.lookAt(pathAt(s - 2));
  const color = new THREE.Color(cfg.color);
  const pool = new THREE.Mesh(new THREE.CircleGeometry(cfg.pool || 5, 64), new THREE.MeshBasicMaterial({ map: poolTex, color, transparent: true, opacity: .5, blending: THREE.AdditiveBlending, depthWrite: false }));
  pool.rotation.x = -Math.PI / 2; pool.position.y = .05; group.add(pool);
  const col = new THREE.Mesh(new THREE.CylinderGeometry(cfg.colR || 2, cfg.colR || 2, cfg.h * 1.25, 48, 1, true),
    new THREE.MeshBasicMaterial({ color, alphaMap: columnTex, transparent: true, opacity: .12, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide }));
  col.position.y = cfg.h * .625; group.add(col);
  const hit = new THREE.Mesh(B(cfg.w || 8, cfg.h, cfg.w || 8), invisible); hit.position.y = cfg.h / 2; hit.userData.id = id; group.add(hit);
  return monuments[id] = { id, s, group, pos: base.clone().setY(cfg.h * .55), top: cfg.h + .8, color, pool, col, hit, glows: [], heat: 0, pulse: 0, update: null, react: null };
}
function addGlow(Mo, mat){ mat.userData.i0 = mat.emissiveIntensity; Mo.glows.push(mat); return mat; }
function makeBurst(parent, color, N, r0, size = .3){
  const a = new Float32Array(N * 3), vel = Array.from({ length: N }, () => V(0, 0, 0));
  const g = new THREE.BufferGeometry().setAttribute('position', new THREE.BufferAttribute(a, 3));
  const pts = new THREE.Points(g, new THREE.PointsMaterial({ size, map: softDot, color, transparent: true, opacity: 0, depthWrite: false, blending: THREE.AdditiveBlending }));
  pts.frustumCulled = false; parent.add(pts);
  let t = 9;
  return {
    fire(center, speed = 6, up = 0){ t = 0; for (let k = 0; k < N; k++) { const d = new THREE.Vector3().randomDirection(); a.set([center.x + d.x * r0, center.y + d.y * r0, center.z + d.z * r0], k * 3); vel[k].copy(d).multiplyScalar(rnd(.4, 1) * speed); vel[k].y += up; } g.attributes.position.needsUpdate = true; },
    update(dt){ t += dt; if (t < 2.6) { for (let k = 0; k < N; k++) { vel[k].multiplyScalar(1 - dt * 1.1); vel[k].y -= dt * 1.2; a[k*3] += vel[k].x * dt; a[k*3+1] += vel[k].y * dt; a[k*3+2] += vel[k].z * dt; } g.attributes.position.needsUpdate = true; pts.material.opacity = 1 - t / 2.6; } else pts.material.opacity = 0; }
  };
}

/* 01 · ABOUT — The Origin: a ring of light around a small sun */
{
  const Mo = makeMonument('about', { color: 0xffc98a, h: 11, w: 9 });
  const ringMat = addGlow(Mo, glowMat(0xffc27a, 2.2));
  const big = mesh(new THREE.TorusGeometry(4.2, .09, 16, 180), ringMat, 0, 5.8, 0, Mo.group);
  const thin = [0, 1].map(k => mesh(new THREE.TorusGeometry(3.2 - k * .7, .03, 8, 160), ringMat, 0, 5.8, 0, Mo.group));
  const orb = mesh(new THREE.SphereGeometry(.7, 32, 24), new THREE.MeshBasicMaterial({ color: new THREE.Color(2.2, 1.6, 1) }), 0, 5.8, 0, Mo.group);
  const halo = sprite(0xffb35a, 8, .4); halo.position.y = 5.8; Mo.group.add(halo);
  Mo.update = (t) => {
    const y = 5.8 + Math.sin(t * .8) * .15;
    thin.forEach((r, k) => { r.rotation.y = t * (.3 + k * .25) * (1 + Mo.pulse * 5); r.rotation.x = Math.sin(t * .4 + k) * .6; r.position.y = y; });
    big.rotation.z = t * .05; orb.position.y = halo.position.y = y;
    orb.scale.setScalar(1 + Math.sin(t * 1.5) * .05 + Mo.pulse * .8); halo.scale.setScalar(8 * (1 + Mo.pulse * .9));
  };
}

/* 02 · EDUCATION — The Library: a helix of glowing pages rising from an open book */
{
  const Mo = makeMonument('education', { color: 0x9fc4ff, h: 12, w: 6 });
  const pageMat = addGlow(Mo, new THREE.MeshStandardMaterial({ color: 0xf4efe4, emissive: 0xfff1d6, emissiveIntensity: .9, side: THREE.DoubleSide, roughness: .6 }));
  const bookMat = addGlow(Mo, new THREE.MeshStandardMaterial({ color: 0xece6da, emissive: 0xffe9c4, emissiveIntensity: .6, side: THREE.DoubleSide }));
  [-1, 1].forEach(sd => { const p = mesh(B(1.7, .06, 2.3), bookMat, sd * .85, .45, 0, Mo.group); p.rotation.z = sd * .18; });
  const N = LOWQ ? 46 : 70;
  const pages = Array.from({ length: N }, (_, k) => { const m = mesh(new THREE.PlaneGeometry(.7, .5), pageMat, 0, 0, 0, Mo.group); m.userData = { a: k * .42, y: .9 + k / N * 10.5, r: 1.6 + Math.sin(k * .7) * .3, sp: rnd(-1, 1) }; return m; });
  let speed = 1;
  Mo.react = () => { speed = 6; };
  Mo.update = (t, dt) => {
    speed += (1 - speed) * dt * .7;
    pages.forEach(p => { const u = p.userData, a = u.a + t * .25 * speed, r = u.r + Math.sin(t * .5 + u.a) * .15;
      p.position.set(Math.cos(a) * r, u.y + Math.sin(t * .6 + u.a) * .12, Math.sin(a) * r); p.rotation.set(Math.sin(t * .7 + u.sp) * .6, -a, Math.cos(t * .5 + u.a) * .3 * speed); });
  };
}

/* 03 · AWARDS — The Golden Arch, with trophies floating in its light */
{
  const Mo = makeMonument('awards', { color: 0xffd27a, h: 12, w: 9 });
  const archMat = addGlow(Mo, std(0xf3c35a, { metalness: 1, roughness: .25, emissive: 0x6a4210, emissiveIntensity: .8 }));
  const edgeMat = addGlow(Mo, glowMat(0xffd88a, 2.4));
  mesh(new THREE.TorusGeometry(3.6, .3, 20, 100, Math.PI), archMat, 0, 7, 0, Mo.group);
  mesh(new THREE.TorusGeometry(3.22, .04, 8, 100, Math.PI), edgeMat, 0, 7, 0, Mo.group);
  [-1, 1].forEach(sd => { mesh(new THREE.CylinderGeometry(.3, .38, 7, 24), archMat, sd * 3.6, 3.5, 0, Mo.group); mesh(B(.06, 7, .06), edgeMat, sd * 3.22, 3.5, 0, Mo.group); });
  const cups = [[M.gold, 2.4, 6.4], [M.silver, 1.9, 4.6], [M.bronze, 1.8, 3.1], [M.gold, 1.6, 1.8]].map(([m, sc, y], k) => {
    const c = cup(m, sc); c.position.set((k % 2 ? 1 : -1) * .9, y, 0); c.userData = { y, ry: 0 }; Mo.group.add(c);
    const sp = sprite(0xffe2a0, 2.2, .25); sp.position.y = .5; c.add(sp); return c;
  });
  const burst = makeBurst(Mo.group, 0xffe08a, 150, .6);
  let spin = 9;
  Mo.react = () => { burst.fire(V(0, 5, 0), 7, 2); spin = 0; };
  Mo.update = (t, dt) => {
    spin += dt;
    cups.forEach((c, k) => { c.position.y = c.userData.y + Math.sin(t * .9 + k) * .15; c.rotation.y = t * .4 + k + (spin < 1.6 ? ease(spin / 1.6) * Math.PI * 2 : 0); });
    burst.update(dt);
  };
}

/* 04 · TRADING — The Market Skyline: pillars of light shaped like candlesticks */
{
  const Mo = makeMonument('projects', { color: 0x3ff2a4, h: 11, w: 12, pool: 7, colR: 3 });
  const upM = addGlow(Mo, glowMat(0x3ff2a4, 1.8)), dnM = addGlow(Mo, glowMat(0xff5c7a, 1.8));
  const N = 15, bars = [];
  for (let k = 0; k < N; k++) {
    const x = (k - (N - 1) / 2) * .82, body = mesh(B(.46, 1, .46), upM, x, 0, 0, Mo.group), wick = mesh(B(.06, 1, .06), upM, x, 0, 0, Mo.group);
    bars.push({ body, wick, cur: { y: 4, h: 1, wy: 4, wh: 2 }, tgt: { y: 4, h: 1, wy: 4, wh: 2 } });
  }
  function newChart(){
    let p = rnd(3, 5);
    bars.forEach(b => {
      const o = p; p = clamp(p + rnd(-1.4, 1.5), 1.5, 9.5);
      const up = p >= o, lo = Math.min(o, p), hi = Math.max(o, p), wl = lo - rnd(.2, .9), wh = hi + rnd(.2, 1.1);
      b.body.material = b.wick.material = up ? upM : dnM;
      Object.assign(b.tgt, { y: (lo + hi) / 2, h: Math.max(.25, hi - lo), wy: (wl + wh) / 2, wh: wh - wl });
    });
  }
  newChart();
  Mo.react = newChart;
  Mo.update = (t, dt) => {
    const k = Math.min(1, dt * 3);
    bars.forEach(b => { for (const key in b.cur) b.cur[key] += (b.tgt[key] - b.cur[key]) * k;
      b.body.position.y = b.cur.y; b.body.scale.y = b.cur.h; b.wick.position.y = b.cur.wy; b.wick.scale.y = b.cur.wh; });
  };
}

/* 05 · EXPERIENCE — The Constellation Spire: badges held in a constellation */
{
  const Mo = makeMonument('experience', { color: 0xb39bff, h: 12.5, w: 9 });
  const crystal = mesh(new THREE.OctahedronGeometry(1, 0), addGlow(Mo, std(0x2a1d55, { metalness: .3, roughness: .15, emissive: 0x8a6cff, emissiveIntensity: 1.1, flatShading: true })), 0, 5.2, 0, Mo.group);
  crystal.scale.set(.75, 5.2, .75);
  const shape = [[-3.2, 9.6], [-1.6, 11.2], [.4, 10.4], [2.8, 11.4], [3.6, 8.4], [2.2, 6.4], [-.6, 7.2], [-3, 6.8]];
  const holder = new THREE.Group(); Mo.group.add(holder);
  const badges = BADGES.map(([a, b, c], k) => {
    const bt = tex(256, 256, (g) => {
      g.beginPath(); g.arc(128, 128, 112, 0, Math.PI * 2); g.fillStyle = '#0c1226'; g.fill(); g.lineWidth = 12; g.strokeStyle = c; g.stroke();
      g.textAlign = 'center'; g.fillStyle = '#fff'; g.font = `700 ${a.length > 5 ? 40 : 52}px Space Grotesk, sans-serif`; g.fillText(a, 128, 136);
      g.fillStyle = c; g.font = '500 20px JetBrains Mono, monospace'; g.fillText(b.slice(0, 13), 128, 172);
    });
    const sp = new THREE.Sprite(new THREE.SpriteMaterial({ map: bt, transparent: true, depthWrite: false }));
    sp.position.set(shape[k][0], shape[k][1] - 2, Math.sin(k * 2.1) * .8); sp.scale.setScalar(1.25); holder.add(sp);
    const star = sprite(new THREE.Color(c), 2.4, .35); star.position.copy(sp.position); holder.add(star);
    return sp;
  });
  const lp = new Float32Array((badges.length + 1) * 3);
  badges.forEach((b, k) => lp.set([b.position.x, b.position.y, b.position.z], k * 3)); lp.set([badges[0].position.x, badges[0].position.y, badges[0].position.z], badges.length * 3);
  const lines = new THREE.Line(new THREE.BufferGeometry().setAttribute('position', new THREE.BufferAttribute(lp, 3)), new THREE.LineBasicMaterial({ color: 0xd9ccff, transparent: true, opacity: .35, blending: THREE.AdditiveBlending, depthWrite: false }));
  holder.add(lines);
  let boost = 0;
  Mo.react = () => { boost = 1; };
  Mo.update = (t, dt) => {
    boost = Math.max(0, boost - dt * .45);
    holder.rotation.y = Math.sin(t * .15) * .35; holder.position.y = Math.sin(t * .5) * .2;
    crystal.rotation.y = t * .2;
    badges.forEach((b, k) => b.scale.setScalar(1.25 * (1 + Math.sin(t * 1.3 + k) * .04 + boost * .35)));
    lines.material.opacity = .3 + boost * .6 + Math.sin(t * 2) * .05;
  };
}

/* 06 · LEADERSHIP — The Flame: a spiral of embers above a brazier */
{
  const Mo = makeMonument('leadership', { color: 0xff7a3a, h: 12, w: 7 });
  const bowlPts = [[0, 0], [.6, .05], [1.2, .5], [1.5, 1.1], [1.45, 1.15], [1.1, .6], [0, .45]].map(([x, y]) => new THREE.Vector2(x, y));
  mesh(new THREE.LatheGeometry(bowlPts, 48), std(0x2a1a18, { metalness: .8, roughness: .35, side: THREE.DoubleSide }), 0, .1, 0, Mo.group);
  mesh(new THREE.TorusGeometry(1.48, .04, 8, 64), addGlow(Mo, glowMat(0xffa050, 2.6)), 0, 1.22, 0, Mo.group).rotation.x = Math.PI / 2;
  const core = sprite(0xff8a3a, 4.5, .55); core.position.y = 1.8; Mo.group.add(core);
  const N = LOWQ ? 220 : 380, fa = new Float32Array(N * 3), ph = Float32Array.from({ length: N }, () => Math.random());
  const flameGeo = new THREE.BufferGeometry().setAttribute('position', new THREE.BufferAttribute(fa, 3));
  const flame = new THREE.Points(flameGeo, new THREE.PointsMaterial({ size: .32, map: softDot, color: 0xffa060, transparent: true, opacity: .9, depthWrite: false, blending: THREE.AdditiveBlending }));
  flame.frustumCulled = false; Mo.group.add(flame);
  // a crown of 50 embers: the boxing club's 50+ members
  const cr = new Float32Array(50 * 3);
  for (let k = 0; k < 50; k++) { const a = k / 50 * Math.PI * 2; cr.set([Math.cos(a) * 2.6, 0, Math.sin(a) * 2.6], k * 3); }
  const crown = new THREE.Points(new THREE.BufferGeometry().setAttribute('position', new THREE.BufferAttribute(cr, 3)), new THREE.PointsMaterial({ size: .4, map: softDot, color: 0xffd08a, transparent: true, opacity: .95, depthWrite: false, blending: THREE.AdditiveBlending }));
  crown.position.y = 11; Mo.group.add(crown);
  const burst = makeBurst(Mo.group, 0xff8a4a, 160, .8, .38);
  let heat = 1;
  Mo.react = () => { burst.fire(V(0, 6, 0), 6, 3); heat = 3; };
  Mo.update = (t, dt) => {
    heat += (1 - heat) * dt * .6;
    for (let k = 0; k < N; k++) {
      const p = (ph[k] + t * .09 * heat) % 1, y = 1.4 + p * 9.8, a = p * 11 + k * 2.4 + t * .8, r = (1.15 - p * .75) * (1 + Math.sin(k * 7.1 + t) * .2);
      fa.set([Math.cos(a) * r, y, Math.sin(a) * r], k * 3);
    }
    flameGeo.attributes.position.needsUpdate = true;
    flame.material.size = .32 * (1 + (heat - 1) * .25);
    crown.rotation.y += dt * .25 * heat; crown.position.y = 11 + Math.sin(t * .7) * .2;
    core.scale.setScalar(4.5 * (1 + Math.sin(t * 6) * .06) * (1 + (heat - 1) * .3));
    burst.update(dt);
  };
}

/* 07 · MISSIONS — The Beacon: a lighthouse sweeping the sky for what's next */
{
  const Mo = makeMonument('missions', { color: 0xc9a0ff, h: 13.5, w: 6 });
  const towerMat = addGlow(Mo, std(0xe9e4f4, { roughness: .35, metalness: .2, emissive: 0x6a54a8, emissiveIntensity: .35 }));
  mesh(new THREE.CylinderGeometry(.55, 1.1, 11, 32), towerMat, 0, 5.5, 0, Mo.group);
  [3, 6, 9].forEach(y => mesh(new THREE.TorusGeometry(.95 - y * .045, .05, 8, 48), addGlow(Mo, glowMat(0xc9a0ff, 2)), 0, y, 0, Mo.group).rotation.x = Math.PI / 2);
  mesh(new THREE.CylinderGeometry(.75, .75, .15, 32), M.chrome, 0, 11.1, 0, Mo.group);
  const lamp = mesh(new THREE.SphereGeometry(.55, 24, 16), new THREE.MeshBasicMaterial({ color: new THREE.Color(2, 1.8, 2.4) }), 0, 11.8, 0, Mo.group);
  const lampGlow = sprite(0xd9c2ff, 6, .5); lampGlow.position.y = 11.8; Mo.group.add(lampGlow);
  const beamPivot = new THREE.Group(); beamPivot.position.y = 11.8; Mo.group.add(beamPivot);
  const beamGeo = new THREE.ConeGeometry(4, 60, 32, 1, true); beamGeo.translate(0, -30, 0); beamGeo.rotateZ(Math.PI / 2 - .12);
  const beamMat = new THREE.MeshBasicMaterial({ color: 0xd9c2ff, alphaMap: columnTex, transparent: true, opacity: .1, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide });
  mesh(beamGeo, beamMat, 0, 0, 0, beamPivot);
  const comet = new THREE.Group(); Mo.group.add(comet);
  const head = sprite(0xcfe9ff, 2.4, 0); comet.add(head);
  const tail = mesh(new THREE.ConeGeometry(.45, 9, 16, 1, true), new THREE.MeshBasicMaterial({ color: 0x9fd8ff, transparent: true, opacity: 0, depthWrite: false, blending: THREE.AdditiveBlending, side: THREE.DoubleSide }), 0, 0, 4.5, comet);
  tail.rotation.x = -Math.PI / 2;
  let ct = 9, flash = 0;
  Mo.react = () => { ct = 0; flash = 1; };
  Mo.update = (t, dt) => {
    flash = Math.max(0, flash - dt * .7);
    beamPivot.rotation.y = t * .5; beamMat.opacity = .1 + flash * .25;
    lampGlow.scale.setScalar(6 * (1 + flash));
    ct += dt;
    if (ct < 3) { const k = ct / 3, a = V(0, 12, 0), b = V(30, 60, -80); comet.position.lerpVectors(a, b, ease(k)); comet.lookAt(Mo.group.localToWorld(a.clone())); const o = Math.sin(k * Math.PI); head.material.opacity = o; tail.material.opacity = o * .35; }
    else { head.material.opacity = 0; tail.material.opacity = 0; }
  };
}

/* 08 · LANGUAGES — The Home Globe, tracing Korea → Shanghai → Hong Kong */
let earthJourney = -1;
const CITIES = [], cityEls = [];
{
  const Mo = makeMonument('languages', { color: 0x7fc8ff, h: 12, w: 8 });
  const GR = 3;
  const tilt = new THREE.Group(); tilt.position.y = 7; tilt.rotation.z = .41; Mo.group.add(tilt);
  const globe = mesh(new THREE.SphereGeometry(GR, 64, 48), new THREE.MeshStandardMaterial({ map: globeTex, roughness: .5, emissive: 0xffffff, emissiveMap: globeTex, emissiveIntensity: .35 }), 0, 0, 0, tilt);
  addGlow(Mo, globe.material);
  const grid = mesh(new THREE.SphereGeometry(GR * 1.12, 24, 16), new THREE.MeshBasicMaterial({ color: 0x8fd0ff, wireframe: true, transparent: true, opacity: .12, blending: THREE.AdditiveBlending, depthWrite: false }), 0, 0, 0, tilt);
  const ringMat = addGlow(Mo, glowMat(0x9fd8ff, 1.8));
  const meridian = mesh(new THREE.TorusGeometry(GR * 1.25, .04, 8, 120), ringMat, 0, 7, 0, Mo.group);
  mesh(new THREE.CylinderGeometry(.08, .3, 3, 16), M.chrome, 0, 1.5, 0, Mo.group);
  const cityPos = (px, py) => { const phi = px / 1024 * Math.PI * 2, th = py / 512 * Math.PI; return V(-Math.cos(phi) * Math.sin(th), Math.cos(th), Math.sin(phi) * Math.sin(th)).multiplyScalar(GR); };
  CITIES.push(cityPos(873, 149), cityPos(857, 167), cityPos(836, 193));
  const arcMat = new THREE.MeshBasicMaterial({ color: new THREE.Color(1.6, .7, .8) });
  const arcs = [[0, 1], [1, 2]].map(([i, j]) => {
    const A = CITIES[i].clone().normalize(), Bv = CITIES[j].clone().normalize(), ang = A.angleTo(Bv), axis = new THREE.Vector3().crossVectors(A, Bv).normalize(), pts = [];
    for (let k = 0; k <= 32; k++) { const t = k / 32; pts.push(A.clone().applyAxisAngle(axis, ang * t).multiplyScalar(GR * (1.01 + .45 * Math.sin(Math.PI * t)))); }
    const curve = new THREE.CatmullRomCurve3(pts), geo = new THREE.TubeGeometry(curve, 64, .04, 6, false);
    const m = mesh(geo, arcMat, 0, 0, 0, globe); m.visible = false;
    return { m, geo, curve, total: geo.index.count };
  });
  const pins = CITIES.map(c => { const s = mesh(new THREE.SphereGeometry(.09, 12, 12), new THREE.MeshBasicMaterial({ color: 0xffffff }), 0, 0, 0, globe); s.position.copy(c).multiplyScalar(1.01); s.visible = false; return s; });
  const jet = mesh(new THREE.SphereGeometry(.08, 8, 8), new THREE.MeshBasicMaterial({ color: new THREE.Color(2, 1.8, 1.2) }), 0, 0, 0, globe); jet.visible = false;
  [['Korea', 'Roots'], ['Shanghai', '2022–26'], ['Hong Kong', 'Now']].forEach(([n, sub]) => {
    const d = document.createElement('div'); d.className = 'city'; d.innerHTML = `<small>${sub}</small>${n}`; document.getElementById('cities').appendChild(d); cityEls.push(d);
  });
  Mo.react = () => { earthJourney = 0; };
  Mo.update = (t, dt) => {
    grid.rotation.y -= dt * .03; meridian.rotation.x = Math.sin(t * .3) * .3; meridian.rotation.y = t * .1;
    if (window.UI.current !== 'languages') earthJourney = -1;
    if (earthJourney >= 0) {
      earthJourney += dt;
      const d = tilt.worldToLocal(camera.position.clone());
      const want = Math.atan2(d.x, d.z) - Math.atan2(CITIES[1].x, CITIES[1].z);
      const diff = ((want - globe.rotation.y) % (Math.PI * 2) + Math.PI * 3) % (Math.PI * 2) - Math.PI;
      globe.rotation.y += diff * Math.min(1, dt * 2.5);
      const ks = [clamp((earthJourney - .9) / 1.2, 0, 1), clamp((earthJourney - 2.3) / 1.2, 0, 1)];
      ks.forEach((k, i) => { const a = arcs[i]; a.m.visible = k > 0; a.geo.setDrawRange(0, Math.floor(a.total * k / 36) * 36); });
      pins.forEach((p, i) => p.visible = earthJourney > .7 + i * 1.4);
      const live = ks[0] < 1 ? 0 : 1; jet.visible = ks[live] > 0 && ks[live] < 1;
      if (jet.visible) jet.position.copy(arcs[live].curve.getPoint(ks[live]));
    } else { globe.rotation.y += dt * .08; arcs.forEach(a => a.m.visible = false); pins.forEach(p => p.visible = false); jet.visible = false; }
    const gc = tilt.getWorldPosition(new THREE.Vector3());
    cityEls.forEach((el, i) => {
      const wp = globe.localToWorld(CITIES[i].clone().multiplyScalar(1.03));
      const facing = wp.clone().sub(gc).dot(camera.position.clone().sub(wp)) > 0;
      wp.project(camera);
      el.style.opacity = earthJourney > .7 + i * 1.4 && facing ? 1 : 0;
      el.style.transform = `translate(${(wp.x * .5 + .5) * innerWidth + [16, 30, 16][i]}px, ${(-wp.y * .5 + .5) * innerHeight + [-40, -12, 16][i]}px)`;
    });
  };
}

/* =========================================================
   THE DOOR — big, white, standing on the water
   ========================================================= */
const DW = 3.4, DH = 7;
const door = (() => {
  const g = new THREE.Group(); g.position.copy(pathAt(DOOR_S)); scene.add(g);
  const d = pathDir(DOOR_S); g.rotation.y = Math.atan2(-d.x, -d.z);
  const white = std(0xf2eee6, { roughness: .35, emissive: 0xfff3e2, emissiveIntensity: .12 });
  const leafMat = std(0xf6f3ec, { roughness: .45, emissive: 0xfff6ea, emissiveIntensity: .08 });
  const lightMat = new THREE.MeshBasicMaterial({ color: new THREE.Color(3.2, 3, 2.7) });
  // threshold step and frame
  mesh(B(DW + 2.2, .22, 2.6), white, 0, .11, 0, g);
  mesh(B(.38, DH + .4, .6), white, -DW / 2 - .19, (DH + .4) / 2 + .2, 0, g);
  mesh(B(.38, DH + .4, .6), white, DW / 2 + .19, (DH + .4) / 2 + .2, 0, g);
  mesh(B(DW + .76, .38, .6), white, 0, DH + .41, 0, g);
  // the light behind the door, and light leaking round its edges
  mesh(new THREE.PlaneGeometry(DW, DH), lightMat, 0, DH / 2 + .22, -.22, g);
  // the door itself, hinged on the left
  const hinge = new THREE.Group(); hinge.position.set(-DW / 2, .22, .05); g.add(hinge);
  const leaf = mesh(B(DW - .06, DH - .05, .14), leafMat, DW / 2, DH / 2, 0, hinge);
  [[.65, 1.9], [.65, 4.6]].forEach(([w, y]) => mesh(B(DW - .9, w * 2.4, .02), std(0xf1ece3, { roughness: .5, emissive: 0xfff3e2, emissiveIntensity: .05 }), DW / 2, y, .08, hinge));
  mesh(new THREE.SphereGeometry(.09, 16, 12), M.gold, DW - .35, DH * .47, .14, hinge);
  // glow on the water in front, and a halo around the frame
  const spillTex = tex(64, 256, (c, w, h) => { const gr = c.createLinearGradient(0, 0, 0, h); gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); c.fillStyle = gr; c.fillRect(0, 0, w, h); });
  const spill = mesh(new THREE.PlaneGeometry(DW * 1.1, 14), new THREE.MeshBasicMaterial({ color: 0xfff1dc, map: spillTex, transparent: true, opacity: .18, blending: THREE.AdditiveBlending, depthWrite: false }), 0, .06, 7.4, g);
  spill.rotation.x = -Math.PI / 2;
  const halo = sprite(0xffe9cc, 14, .18); halo.position.y = DH / 2 + .5; halo.position.z = -.6; g.add(halo);
  const flood = mesh(new THREE.BoxGeometry(DW * .96, DH * .96, 26), new THREE.MeshBasicMaterial({ color: 0xfff2de, transparent: true, opacity: 0, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide }), 0, DH / 2 + .2, 13, g);
  const glow = new THREE.PointLight(0xfff0dc, 2, 30, 1.6); glow.position.set(0, DH / 2, 4); g.add(glow);
  const hit = mesh(B(DW + 1.4, DH + 1, 2), invisible, 0, DH / 2, 0, g); hit.userData.id = 'connect';
  return { g, hinge, leaf, white, leafMat, spill, halo, flood, glow, hit, open: 0, phase: 'closed', t: 0, pending: false, camFrom: new THREE.Vector3(), lookFrom: new THREE.Vector3() };
})();
monuments.connect = { id: 'connect', s: END_S, group: door.g, pos: door.g.position.clone().setY(3.6), top: DH + 1.4, hit: door.hit, glows: [door.white, door.leafMat], heat: 0, pulse: 0, ahead: true };
[door.white, door.leafMat].forEach(m => m.userData.i0 = m.emissiveIntensity);

/* ---------- lights ---------- */
const hemi = new THREE.HemisphereLight(0x9a86ff, 0x120a1e, .6); scene.add(hemi);
const sunset = new THREE.DirectionalLight(0xffb27a, 2.1); sunset.position.set(-20, 30, -220); scene.add(sunset);
const fill = new THREE.DirectionalLight(0x6f8cff, .7); fill.position.set(40, 25, 90); scene.add(fill);

// shooting stars
const shoot = new THREE.Mesh(new THREE.CylinderGeometry(0, .14, 16, 6, 1, true), new THREE.MeshBasicMaterial({ color: 0xffe2c4, transparent: true, opacity: 0, blending: THREE.AdditiveBlending, depthWrite: false }));
shoot.frustumCulled = false; scene.add(shoot); let shootT = 99; const shootDir = V(0, 0, 0);

/* =========================================================
   MOVING — scroll, swipe, W/S or ↑/↓ to walk · A/D, ←/→ or drag to look
   ========================================================= */
let walkS = 0, walkTarget = 0, speedK = 0, autopilot = false, lastStep = 0, stepSound = 0, started = false;
let yaw = 0, pitch = 0, yawS = 0, pitchS = 0, lookIdle = 0;
const keys = new Set();
const mobile = () => innerWidth < 860;
const maxWalk = () => door.phase === 'closed' ? END_S : DOOR_S + 2;
const inField = e => e.target.closest && e.target.closest('input, textarea, #beyond');
function nudge(d){
  if (door.phase !== 'closed') return;
  if (window.UI.current && Math.abs(d) > .4) window.UI.close();
  autopilot = false; door.pending = false; walkTarget = clamp(walkTarget + d, 0, maxWalk());
}
function turn(dy, dp = 0){ yaw = clamp(yaw + dy, -Math.PI, Math.PI); pitch = clamp(pitch + dp, -.45, .5); lookIdle = 0; }
addEventListener('wheel', e => {
  if (e.target.closest && e.target.closest('#panel, .dock, #rail, #beyond')) return;
  e.preventDefault();
  const k = e.deltaMode === 1 ? 16 : e.deltaMode === 2 ? innerHeight : 1;
  nudge(clamp(e.deltaY * k, -120, 120) * .05);
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) turn(e.deltaX * k * .004);
}, { passive: false });
const WALK_KEYS = { w: 1, arrowup: 1, s: -1, arrowdown: -1 }, TURN_KEYS = { a: 1, arrowleft: 1, d: -1, arrowright: -1 };
addEventListener('keydown', e => {
  if (inField(e)) return;
  const k = e.key.toLowerCase();
  if (k in WALK_KEYS || (k in TURN_KEYS && !window.UI.current)) { e.preventDefault(); keys.add(k); if (k in WALK_KEYS && window.UI.current) window.UI.close(); }
  if (k === 'shift') keys.add('shift');
  if (e.key === ' ' || e.key === 'PageDown') { e.preventDefault(); nudge(GAP / 3); }
  if (e.key === 'PageUp') { e.preventDefault(); nudge(-GAP / 3); }
  if (e.key === 'Enter' && !window.UI.current) { const n = nearest(); if (n.d < 12) window.UI.open(n.id); }
});
addEventListener('keyup', e => keys.delete(e.key.toLowerCase()));
addEventListener('blur', () => keys.clear());

// drag (mouse) or swipe sideways (touch) to look around; swipe up/down to walk
let drag = null;
canvas.addEventListener('pointerdown', e => { drag = { x: e.clientX, y: e.clientY, sx: e.clientX, sy: e.clientY, axis: null, type: e.pointerType }; canvas.setPointerCapture(e.pointerId); });
canvas.addEventListener('pointermove', e => {
  if (!drag) return;
  const dx = e.clientX - drag.x, dy = e.clientY - drag.y; drag.x = e.clientX; drag.y = e.clientY;
  if (drag.type === 'mouse') { if (Math.hypot(e.clientX - drag.sx, e.clientY - drag.sy) > 4) { turn(dx * .0045, dy * .003); canvas.style.cursor = 'grabbing'; } return; }
  if (!drag.axis && Math.hypot(e.clientX - drag.sx, e.clientY - drag.sy) > 8) drag.axis = Math.abs(e.clientX - drag.sx) > Math.abs(e.clientY - drag.sy) ? 'x' : 'y';
  if (drag.axis === 'x') turn(dx * .006); else if (drag.axis === 'y') nudge(-dy * .12);
});
const endDrag = e => {
  if (!drag) return;
  const moved = Math.hypot(e.clientX - drag.sx, e.clientY - drag.sy); drag = null; canvas.style.cursor = '';
  if (moved < 6) { const id = pick(e); if (id) window.UI.open(id); }
};
canvas.addEventListener('pointerup', endDrag); canvas.addEventListener('pointercancel', () => { drag = null; });
function nearest(){ let id = null, d = 1e9; for (const k of ORDER) { const x = Math.abs(walkS - STOP[k]); if (x < d) { d = x; id = k; } } return { id, d }; }
const wake = () => { window.Sound && window.Sound.autoStart(); };
addEventListener('pointerdown', wake, { once: true }); addEventListener('keydown', wake, { once: true });

/* ---------- panel offset (keeps the monument clear of the panel) ---------- */
let offX = 0, offY = 0, tOffX = 0, tOffY = 0;
function setOffsets(open){ tOffX = open && !mobile() ? 236 : 0; tOffY = open && mobile() ? innerHeight * .27 : 0; }

const SOUNDS = { about: 'blip', education: 'swish', awards: 'clink', projects: 'keys', experience: 'chime', leadership: 'thud', missions: 'sweep', languages: 'whoosh' };
function react(id){
  const Mo = monuments[id]; if (!Mo || id === 'connect') return;
  Mo.pulse = 1; Mo.react && Mo.react();
  window.Sound && window.Sound.play(SOUNDS[id]);
}

/* ---------- opening the door ---------- */
const whiteout = document.getElementById('whiteout');
function startDoor(){
  if (door.phase !== 'closed') return;
  door.phase = 'opening'; door.t = 0; door.pending = false; autopilot = false; yaw = 0; pitch = 0;
  door.camFrom.copy(camera.position); door.lookFrom.copy(look);
  window.UI.close(); window.Sound && window.Sound.play('door');
  document.body.classList.add('through');
}
function leaveDoor(){
  if (door.phase === 'closed' || door.phase === 'leaving') return;
  door.phase = 'leaving'; door.t = 0;
  walkS = walkTarget = END_S - 2;
  window.UI.beyond(false);
}
window.Room = {
  focus(id){ walkTarget = STOP[id]; autopilot = true; setOffsets(true); react(id); },
  walkTo(id){ if (door.phase !== 'closed') return; walkTarget = STOP[id]; autopilot = true; },
  reset(){ setOffsets(false); },
  openDoor(){ if (door.phase !== 'closed') return; if (Math.abs(walkS - END_S) < 1.5) startDoor(); else { walkTarget = END_S; autopilot = true; door.pending = true; } },
  leaveDoor,
  signal(){}
};

/* ---------- markers, chapter card, progress rail ---------- */
const markersEl = document.getElementById('markers'), tip = document.getElementById('tooltip');
for (const id of ORDER) {
  const b = document.createElement('button'); b.className = 'marker';
  b.innerHTML = `<span class="dot"></span><span class="label">${id === 'connect' ? "Let's connect" : CV[id].label}</span>`;
  b.addEventListener('click', () => window.UI.open(id));
  b.addEventListener('mouseenter', () => hovered = id); b.addEventListener('mouseleave', () => hovered = null);
  markersEl.appendChild(b); monuments[id].el = b;
}
const chapterEl = document.getElementById('chapter'), chK = document.getElementById('chK'), chT = document.getElementById('chT'), chGo = document.getElementById('chGo');
let chapterId = null;
chGo.addEventListener('click', () => chapterId && window.UI.open(chapterId));
const rail = document.getElementById('rail'), railFill = document.getElementById('railFill'), railBtns = {};
for (const id of ORDER) {
  const b = document.createElement('button'); b.style.top = (STOP[id] / END_S * 100) + '%'; b.setAttribute('aria-label', CV[id].label);
  b.innerHTML = `<span>${id === 'connect' ? 'The door' : CV[id].label}</span>`; b.addEventListener('click', () => window.Room.walkTo(id));
  rail.appendChild(b); railBtns[id] = b;
}

const ray = new THREE.Raycaster(), ptr = new THREE.Vector2();
const hitList = ORDER.map(id => monuments[id].hit);
let hovered = null;
function pick(e){
  ptr.set(e.clientX / innerWidth * 2 - 1, -(e.clientY / innerHeight) * 2 + 1);
  ray.setFromCamera(ptr, camera);
  const h = ray.intersectObjects(hitList, false)[0];
  return h ? h.object.userData.id : null;
}
canvas.addEventListener('pointermove', e => {
  if (e.pointerType !== 'mouse' || drag) { tip.style.opacity = 0; return; }
  hovered = pick(e); canvas.style.cursor = hovered ? 'pointer' : 'grab';
  if (hovered) { tip.innerHTML = `<small>${CV[hovered].object}</small>${hovered === 'connect' ? 'Open the door' : CV[hovered].title}`; tip.style.transform = `translate(${e.clientX + 16}px, ${e.clientY + 14}px)`; tip.style.opacity = 1; }
  else tip.style.opacity = 0;
});
canvas.addEventListener('pointerleave', () => { hovered = null; tip.style.opacity = 0; });

/* ---------- night mode ---------- */
const nightBtn = document.getElementById('nightBtn');
let night = false; try { night = localStorage.getItem('room-night') === 'on'; } catch (e) {}
let nightK = night ? 1 : 0;
const paintNight = () => { nightBtn.setAttribute('aria-pressed', night); nightBtn.querySelector('span').textContent = night ? 'Night on' : 'Night mode'; };
paintNight();
nightBtn.addEventListener('click', () => { night = !night; paintNight(); try { localStorage.setItem('room-night', night ? 'on' : 'off'); } catch (e) {} window.Sound && window.Sound.play(night ? 'sweep' : 'blip'); });
const horDay = new THREE.Color().setRGB(.126, .059, .055), horNight = new THREE.Color().setRGB(.025, .013, .025);

/* =========================================================
   LOOP
   ========================================================= */
let lastW = 0, lastH = 0;
function fitSize(){
  const w = Math.max(1, innerWidth), h = Math.max(1, innerHeight); lastW = innerWidth; lastH = innerHeight;
  camera.aspect = w / h; camera.updateProjectionMatrix();
  renderer.setSize(w, h); composer.setSize(w, h);
  water.getRenderTarget().setSize(Math.max(2, Math.floor(w * renderer.getPixelRatio() * rtScale)), Math.max(2, Math.floor(h * renderer.getPixelRatio() * rtScale)));
  setOffsets(!!window.UI.current);
}
addEventListener('resize', fitSize);

const perf = { n: 0, sum: 0, done: false };
const clock = new THREE.Clock(), tmp = new THREE.Vector3(), tmp2 = new THREE.Vector3(), camGoal = new THREE.Vector3(), lookGoal = new THREE.Vector3(), look = new THREE.Vector3();
let dropT = 0;
pathAt(-10, camera.position).add(V(0, 3.4, 0)); look.copy(pathAt(14)).add(V(0, 2.6, 0)); camera.lookAt(look);

function tick(){
  if (innerWidth !== lastW || innerHeight !== lastH) fitSize();
  if (!innerWidth || !innerHeight) { requestAnimationFrame(tick); return; }
  const dt = Math.min(clock.getDelta(), .05), t = clock.elapsedTime, m = reduceMotion ? 0 : 1;

  // keyboard: hold to walk / turn
  const kw = (keys.has('w') || keys.has('arrowup') ? 1 : 0) - (keys.has('s') || keys.has('arrowdown') ? 1 : 0);
  const kt = (keys.has('a') || keys.has('arrowleft') ? 1 : 0) - (keys.has('d') || keys.has('arrowright') ? 1 : 0);
  if (kw && door.phase === 'closed') { autopilot = false; door.pending = false; walkTarget = clamp(clamp(walkTarget, walkS - 1.5, walkS + 1.5) + kw * dt * (keys.has('shift') ? 30 : 15), 0, maxWalk()); }
  if (kt) turn(kt * dt * 1.9);
  lookIdle += dt;
  if (lookIdle > 1.1 && !drag) { const k = Math.min(1, dt * 1.6); yaw -= yaw * k; pitch -= pitch * k; }
  yawS += (yaw - yawS) * Math.min(1, dt * 12); pitchS += (pitch - pitchS) * Math.min(1, dt * 12);

  // walking
  const maxStep = dt * (autopilot ? 75 : 48);
  const step = clamp((walkTarget - walkS) * Math.min(1, dt * (kw ? 8 : 2.6)), -maxStep, maxStep);
  walkS += step;
  speedK += (Math.min(1, Math.abs(step) / Math.max(dt, 1e-4) / 5) - speedK) * Math.min(1, dt * 6);
  if (walkS > 2 && !started) { started = true; document.body.classList.add('walking'); }
  if (walkS < 1 && started && walkTarget < 1) { started = false; document.body.classList.remove('walking'); }
  if (door.pending && Math.abs(walkS - END_S) < 1) startDoor();

  // the figure
  pathAt(walkS, walker.position);
  const dir = pathDir(walkS), side = pathSide(walkS);
  walker.rotation.y = Math.atan2(dir.x, dir.z);
  const phase = walkS * 2.2, swing = Math.sin(phase) * .55 * speedK;
  legL.rotation.x = swing; legR.rotation.x = -swing; armL.rotation.x = -swing * .8; armR.rotation.x = swing * .8;
  walker.position.y = Math.abs(Math.sin(phase)) * .05 * speedK;
  halo.material.opacity = .2 + Math.sin(t * 2) * .04;
  const stepIdx = Math.floor(phase / Math.PI);
  if (stepIdx !== lastStep) {
    lastStep = stepIdx;
    if (speedK > .05) {
      const f = pathAt(walkS).addScaledVector(side, stepIdx % 2 ? .12 : -.12);
      ripple(f.x, f.z, .9);
      if (t - stepSound > .22) { stepSound = t; window.Sound && window.Sound.play('step'); }
    }
  }
  dropT -= dt;
  if (dropT < 0 && m) { dropT = rnd(1.2, 2.8); const p = pathAt(walkS + rnd(5, 45)).addScaledVector(side, rnd(-16, 16)); ripple(p.x, p.z, .45); }

  // camera: follows you, turns toward the nearest monument, plus your own look-around
  const n = nearest();
  const fp = monuments[window.UI.current] || (n.d < 15 ? monuments[n.id] : null);
  const wF = window.UI.current ? 1 : fp ? Math.exp(-Math.pow(n.d / 9, 2)) : 0;
  pathAt(walkS - 10, camGoal).y = 3.4;
  lookGoal.copy(pathAt(walkS + 14)).setY(2.6);
  if (fp) {
    if (!fp.ahead) { const sign = Math.sign(tmp.copy(fp.pos).sub(walker.position).dot(side)) || 1; camGoal.addScaledVector(side, -sign * 2.4 * wF).y += 1.3 * wF; }
    lookGoal.lerp(fp.pos, (window.UI.current ? .8 : .55) * wF);
  }
  camera.position.lerp(camGoal, Math.min(1, dt * 4.5));
  look.lerp(lookGoal, Math.min(1, dt * 6));

  // walking through the door
  if (door.phase !== 'closed') {
    door.t += dt;
    const dp = door.g.position, fwd = pathDir(DOOR_S);
    if (door.phase === 'opening') {
      const k = smooth(door.t, 1.2, 4.2), e = k * k;
      walkTarget = walkS = L(END_S, DOOR_S + 1.5, smooth(door.t, 1.4, 3.8));
      camera.position.lerpVectors(door.camFrom, tmp.copy(dp).addScaledVector(fwd, 2.5).setY(3.4), e);
      look.lerpVectors(door.lookFrom, tmp2.copy(dp).addScaledVector(fwd, 14).setY(3.4), smooth(door.t, .6, 2.6));
      if (door.t > 4.3) { door.phase = 'beyond'; window.UI.beyond(true); }
    }
    if (door.phase === 'beyond') { camera.position.copy(tmp.copy(dp).addScaledVector(fwd, 2.5).setY(3.4)); look.copy(tmp2.copy(dp).addScaledVector(fwd, 14).setY(3.4)); }
    if (door.phase === 'leaving' && door.t > 1.6) { door.phase = 'closed'; document.body.classList.remove('through'); }
  }
  const opening = door.phase === 'opening' ? smooth(door.t, 0, 1.8) : door.phase === 'beyond' ? 1 : door.phase === 'leaving' ? 1 - smooth(door.t, .4, 1.6) : 0;
  door.open += (opening - door.open) * Math.min(1, dt * 6);
  door.hinge.rotation.y = -ease(door.open) * 1.85;
  const flood = door.phase === 'opening' ? smooth(door.t, .8, 3.6) : door.phase === 'beyond' ? 1 : door.phase === 'leaving' ? 1 - smooth(door.t, 0, 1.4) : 0;
  door.flood.material.opacity = flood * .22; door.spill.material.opacity = .18 + flood * .6;
  door.glow.intensity = 2 + flood * 60; door.halo.material.opacity = .18 + flood * .4 + Math.sin(t * 1.5) * .03;
  whiteout.style.opacity = door.phase === 'opening' ? smooth(door.t, 2.2, 4.2) : door.phase === 'beyond' ? 1 : door.phase === 'leaving' ? 1 - smooth(door.t, 0, 1.2) : 0;
  bloom.strength = .45 + flood * .9;

  // look-around on top of everything
  tmp.copy(look).sub(camera.position);
  const len = tmp.length();
  tmp.applyAxisAngle(V(0, 1, 0), yawS); tmp.y += Math.tan(-pitchS) * len;
  camera.lookAt(tmp.add(camera.position));

  offX += (tOffX - offX) * Math.min(1, dt * 4); offY += (tOffY - offY) * Math.min(1, dt * 4);
  if (Math.abs(offX) < .5 && Math.abs(offY) < .5 && !tOffX && !tOffY) camera.clearViewOffset();
  else camera.setViewOffset(innerWidth, innerHeight, offX, offY, innerWidth, innerHeight);

  sky.position.copy(camera.position); stars.position.copy(camera.position);
  water.position.x = camera.position.x; water.position.z = camera.position.z;

  // uniforms + dusk ↔ night
  nightK += ((night ? 1 : 0) - nightK) * Math.min(1, dt * 1.8);
  WU.time.value = t; WU.camPos.value.copy(camera.position); WU.horizon.value.lerpColors(horDay, horNight, nightK);
  pathMat.uniforms.time.value = t; pathMat.uniforms.walkS.value = walkS; pathMat.uniforms.nightK.value = nightK;
  sky.material.uniforms.nightK.value = nightK;
  stars.material.uniforms.time.value = t; stars.material.uniforms.bright.value = L(.75, 1.35, nightK);
  scene.fog.color.lerpColors(fogDay, fogNight, nightK);
  hemi.intensity = L(.6, .18, nightK); sunset.intensity = L(2.1, .65, nightK); fill.intensity = L(.7, .28, nightK);
  motes.material.opacity = L(.75, 1, nightK);
  { const a = moteGeo.attributes.position.array;
    for (let i = 0; i < MOTES; i++) { a[i * 3 + 1] += dt * moteSpeed[i] * m; if (a[i * 3 + 1] > 7.5) a[i * 3 + 1] = .3; }
    moteGeo.attributes.position.needsUpdate = true; }

  shootT += dt;
  if (shootT > 6 + Math.random() * 6 && m) {
    shootT = 0; shootDir.set(rnd(-1, -.4), rnd(-.4, -.15), rnd(-.2, .2)).normalize();
    shoot.position.copy(camera.position).add(V(rnd(-120, 160), rnd(70, 140), -rnd(260, 340)));
    shoot.quaternion.setFromUnitVectors(V(0, -1, 0), shootDir);
  }
  shoot.material.opacity = shootT < 1.2 ? Math.sin(shootT / 1.2 * Math.PI) * .8 : 0;
  shoot.position.addScaledVector(shootDir, dt * 80);

  // monuments, markers, chapter card, rail
  const w = innerWidth, h = innerHeight, nightBoost = 1 + nightK * .35;
  for (const id of ORDER) {
    const Mo = monuments[id];
    const target = hovered === id ? 1 : window.UI.current === id ? .5 : 0;
    Mo.heat += (target - Mo.heat) * Math.min(1, dt * 6);
    Mo.pulse = Math.max(0, Mo.pulse - dt * .9);
    const near = Math.exp(-Math.pow((walkS - Mo.s) / 14, 2));
    Mo.glows.forEach(mt => mt.emissiveIntensity = mt.userData.i0 * (1 + Mo.heat * .6 + Mo.pulse * 1.2 + near * .25) * nightBoost);
    if (Mo.col) { Mo.col.material.opacity = (.035 + near * .05 + Mo.heat * .06 + Mo.pulse * .15) * nightBoost; Mo.pool.material.opacity = .35 + near * .35 + Mo.heat * .3; }
    Mo.update && Mo.update(t, dt * (m || .2));
    tmp.copy(Mo.group.position).y = Mo.top;
    const dist = tmp.distanceTo(camera.position);
    tmp.project(camera);
    const vis = tmp.z < 1 && Math.abs(tmp.x) < 1.1 && Math.abs(tmp.y) < 1.1 && dist < 260 && door.phase === 'closed';
    Mo.el.style.opacity = vis ? clamp(1 - (dist - 140) / 120, 0, 1) : 0;
    Mo.el.style.pointerEvents = vis ? 'auto' : 'none';
    Mo.el.style.transform = `translate(${(tmp.x * .5 + .5) * w - 11}px, ${(-tmp.y * .5 + .5) * h - 11}px)`;
    Mo.el.classList.toggle('near', n.id === id && n.d < 12);
    Mo.el.classList.toggle('hot', hovered === id);
    railBtns[id].classList.toggle('done', walkS >= STOP[id] - 1);
    railBtns[id].classList.toggle('here', n.id === id && n.d < 12);
  }
  railFill.style.height = clamp(walkS / END_S * 100, 0, 100) + '%';
  const showCh = n.d < 10 && !window.UI.current && door.phase === 'closed' ? n.id : null;
  if (showCh !== chapterId) {
    chapterId = showCh;
    if (showCh) {
      const i = ORDER.indexOf(showCh);
      chK.textContent = showCh === 'connect' ? 'Final stop · The door' : `Chapter ${String(i + 1).padStart(2, '0')} · ${CV[showCh].object}`;
      chT.textContent = CV[showCh].title;
      chGo.textContent = showCh === 'connect' ? 'Open the door →' : 'Explore →';
      chapterEl.classList.add('show');
    } else chapterEl.classList.remove('show');
  }

  composer.render();
  perf.n++; perf.sum += dt;
  if (perf.n === 90 && !perf.done) { perf.done = true; if (perf.sum / 90 > 1 / 40) { renderer.setPixelRatio(1); composer.setPixelRatio(1); water.getRenderTarget().setSize(Math.floor(innerWidth * rtScale * .8), Math.floor(innerHeight * rtScale * .8)); } }
  requestAnimationFrame(tick);
}
tick();
document.body.classList.add('ready');
</script>
</body>
</html>
