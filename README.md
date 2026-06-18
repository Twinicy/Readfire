<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0a0500">
<title>FireRead</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Serif+Display:ital@0;1&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
/* ═══ VARIABLES ═══ */
:root{
  --bg:#09040a;--bg2:#100810;--sur:#1c101c;--sur2:#281828;--bdr:#3a1f3a;
  --f1:#ff3a00;--f2:#ff6600;--f3:#ff9900;--f4:#ffcc00;
  --txt:#fff5ee;--txt2:#c8a882;--txt3:#7a5050;
  --hl:rgba(255,102,0,.3);--hlw:#ffcc00;
  --r:12px;--r2:20px;--page-w:calc(100vw - 32px);
  --gfire:linear-gradient(135deg,#ff3a00,#ff6600 45%,#ffcc00);
  --gshadow:0 8px 32px rgba(255,60,0,.25);
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;font-family:'Syne',sans-serif;background:var(--bg);color:var(--txt);overflow:hidden;-webkit-font-smoothing:antialiased;text-rendering:optimizeLegibility}

/* ═══ PAGE SYSTEM — instant, no jank ═══ */
.page{
  position:fixed;inset:0;display:flex;flex-direction:column;
  background:var(--bg);will-change:transform;
  transform:translateX(100%);
  transition:transform .22s cubic-bezier(.25,.46,.45,.94);
  z-index:1;overflow:hidden;
}
.page.active{transform:translateX(0)}
.page.behind{transform:translateX(-22%)}
/* Home page starts visible */
#page-home{transform:translateX(0)}

/* ═══ NAV ═══ */
.nav{display:flex;align-items:center;gap:10px;padding:0 14px;height:52px;min-height:52px;
  background:var(--bg2);border-bottom:1px solid var(--bdr);flex-shrink:0;z-index:5}
.nav-back{width:38px;height:38px;border-radius:9px;border:1px solid var(--bdr);
  background:var(--sur);color:var(--txt2);font-size:18px;cursor:pointer;
  display:flex;align-items:center;justify-content:center;flex-shrink:0}
.nav-back:active{background:var(--sur2);color:var(--f2)}
.nav-title{font-family:'DM Serif Display',serif;font-size:15px;color:var(--txt);
  flex:1;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.nav-logo{font-family:'DM Serif Display',serif;font-size:21px;
  background:var(--gfire);-webkit-background-clip:text;-webkit-text-fill-color:transparent;
  background-clip:text;font-weight:900}
.nav-acts{display:flex;gap:7px;margin-left:auto}
.nav-btn{width:36px;height:36px;border-radius:9px;border:1px solid var(--bdr);
  background:var(--sur);color:var(--txt2);font-size:15px;cursor:pointer;
  display:flex;align-items:center;justify-content:center;flex-shrink:0}
.nav-btn:active{color:var(--f2);border-color:var(--f2)}

/* ═══ TABS ═══ */
.tabs{display:flex;background:var(--bg2);border-top:1px solid var(--bdr);
  height:62px;min-height:62px;flex-shrink:0;
  padding-bottom:env(safe-area-inset-bottom,0px)}
.tab{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;
  gap:2px;cursor:pointer;color:var(--txt3);font-size:9px;font-weight:700;
  letter-spacing:.4px;text-transform:uppercase;border:none;background:none;padding:6px 0}
.tab .ti{font-size:20px;line-height:1}
.tab.on{color:var(--f3)}
.tab.on .ti{filter:drop-shadow(0 0 5px var(--f2))}

/* ═══ BUTTONS ═══ */
.fbtn{width:100%;padding:14px;border:none;border-radius:var(--r);
  font-family:'Syne',sans-serif;font-weight:700;font-size:15px;cursor:pointer;
  display:flex;align-items:center;justify-content:center;gap:8px}
.fbtn:active{opacity:.85}
.fbtn.fire{background:var(--gfire);color:#000}
.fbtn.ghost{background:var(--sur);color:var(--txt2);border:1px solid var(--bdr);margin-top:10px}
.sec{font-size:10px;font-weight:700;letter-spacing:2px;color:var(--txt3);
  text-transform:uppercase;margin-bottom:10px}
.pg-body{flex:1;overflow-y:auto;padding:18px 16px;-webkit-overflow-scrolling:touch}

/* ═══ EMBERS ═══ */
.embers{position:fixed;inset:0;pointer-events:none;z-index:0;overflow:hidden}
.ember{position:absolute;bottom:-8px;border-radius:50%;opacity:0;
  animation:rise var(--d,4s) var(--dl,0s) infinite ease-out}
@keyframes rise{
  0%{opacity:.9;transform:translateY(0) scale(1)}
  50%{opacity:.4;transform:translateY(-42vh) translateX(var(--dx,0px)) scale(.6)}
  100%{opacity:0;transform:translateY(-80vh) translateX(calc(var(--dx,0px)*1.7)) scale(.2)}
}

/* ═══ HOME ═══ */
.home-scroll{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch}
.quote-wrap{padding:22px 20px;background:linear-gradient(180deg,#1a0800,#080300);
  border-bottom:1px solid var(--bdr);text-align:center}
.q-flame{font-size:32px;display:block;margin-bottom:8px;
  animation:jig 2s ease-in-out infinite;filter:drop-shadow(0 0 10px var(--f2))}
@keyframes jig{0%,100%{transform:scaleY(1)}50%{transform:scaleY(1.1) scaleX(.95)}}
.q-text{font-family:'DM Serif Display',serif;font-size:15px;line-height:1.65;
  color:var(--txt);font-style:italic;margin-bottom:8px}
.q-auth{font-size:11px;color:var(--f3);font-weight:700;letter-spacing:1px}
.q-dots{display:flex;gap:5px;justify-content:center;margin-top:10px}
.qdot{width:6px;height:6px;border-radius:50%;background:var(--bdr);cursor:pointer;transition:all .3s}
.qdot.on{background:var(--f2);box-shadow:0 0 5px var(--f2)}

/* Fire CTA */
.cta-wrap{padding:14px 16px 0}
.cta-btn{width:100%;height:100px;border:none;border-radius:var(--r2);cursor:pointer;
  position:relative;overflow:hidden;background:transparent;
  display:flex;align-items:center;justify-content:center;
  box-shadow:0 0 30px rgba(255,80,0,.3)}
.cta-bg{position:absolute;inset:0;border-radius:var(--r2);
  background:linear-gradient(135deg,#550e00,#aa2a00,#ff6600,#ffcc00);
  animation:cpulse 2.8s ease-in-out infinite}
@keyframes cpulse{0%,100%{filter:brightness(1)}50%{filter:brightness(1.2)}}
.cta-inner{position:relative;z-index:1;display:flex;flex-direction:column;align-items:center;gap:2px}
.cta-flames{font-size:26px;letter-spacing:-4px;animation:fsway 1.5s ease-in-out infinite alternate}
@keyframes fsway{from{transform:skewX(-3deg)}to{transform:skewX(3deg)}}
.cta-label{font-family:'DM Serif Display',serif;font-size:21px;font-weight:900;color:#fff;
  text-shadow:0 2px 8px rgba(200,40,0,.9);letter-spacing:.5px}
.cta-sub{font-size:10px;color:rgba(255,255,255,.85);font-weight:700;letter-spacing:1px}

/* Library */
.home-lib{padding:16px 16px 0}
.lib-card{display:flex;align-items:center;gap:11px;background:var(--sur);
  border:1px solid var(--bdr);border-radius:var(--r);padding:12px;
  margin-bottom:8px;position:relative}
.lib-card.now{border-color:var(--f2);background:var(--sur2)}
.lci{font-size:24px;flex-shrink:0}
.lcb{flex:1;min-width:0;cursor:pointer}
.lcn{font-size:13px;font-weight:700;white-space:nowrap;overflow:hidden;
  text-overflow:ellipsis;color:var(--txt)}
.lcm{font-size:11px;color:var(--txt3);margin-top:2px}
.lcp{height:3px;background:var(--bdr);border-radius:2px;margin-top:6px;overflow:hidden}
.lcpf{height:100%;background:var(--gfire);border-radius:2px;transition:width .3s}
/* Delete button — always visible, right side */
.lib-del{width:36px;height:36px;border-radius:9px;border:1px solid rgba(255,58,0,.3);
  background:rgba(255,58,0,.08);color:var(--f1);font-size:16px;cursor:pointer;
  display:flex;align-items:center;justify-content:center;flex-shrink:0;
  transition:all .15s}
.lib-del:active{background:rgba(255,58,0,.25);border-color:var(--f1)}
.lib-empty{color:var(--txt3);font-size:13px;text-align:center;padding:24px;
  background:var(--sur);border-radius:var(--r);border:1px dashed var(--bdr);line-height:1.7}

/* Add grid */
.add-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:18px}
.add-card{background:var(--sur);border:1.5px solid var(--bdr);border-radius:var(--r);
  padding:18px 10px;text-align:center;cursor:pointer;
  display:flex;flex-direction:column;align-items:center;gap:6px}
.add-card:active{background:var(--sur2);border-color:var(--f2)}
.add-card .ci{font-size:26px}
.add-card .cl{font-size:13px;font-weight:700;color:var(--txt)}
.add-card .cs{font-size:11px;color:var(--txt3)}

/* ═══ READER — BOOK PAGES ═══ */
.reader-tb{display:flex;align-items:center;gap:8px;padding:8px 14px;
  background:var(--bg2);border-bottom:1px solid var(--bdr);flex-shrink:0}
.font-btns{display:flex;gap:4px;align-items:center}
.font-btn{width:32px;height:32px;border-radius:8px;border:1px solid var(--bdr);
  background:var(--sur);color:var(--txt);cursor:pointer;font-size:13px;
  display:flex;align-items:center;justify-content:center;font-weight:700}
.font-btn:active{border-color:var(--f2)}
.font-val{font-size:12px;color:var(--txt2);min-width:24px;text-align:center;
  font-family:'JetBrains Mono',monospace}
.pg-indicator{font-size:11px;color:var(--txt3);font-family:'JetBrains Mono',monospace;
  margin-left:4px;white-space:nowrap}
.bm-btn{margin-left:auto;background:none;border:1px solid var(--bdr);border-radius:8px;
  padding:6px 10px;color:var(--txt2);font-family:'Syne',sans-serif;font-size:11px;cursor:pointer}
.bm-btn:active{border-color:var(--f2);color:var(--f2)}

/* Book pages scroll area */
.book-scroll{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch;
  background:#0d0408;padding:16px 16px 24px;display:flex;flex-direction:column;gap:0}

/* Individual book page */
.book-page{
  background:#fffdf8;border-radius:4px 4px 2px 2px;
  box-shadow:0 2px 0 #d4c9b0,0 4px 0 #c8bda4,0 6px 0 #bcb198,
             0 8px 20px rgba(0,0,0,.5),var(--gshadow);
  margin-bottom:28px;position:relative;overflow:hidden;
  border:1px solid rgba(255,255,255,.12);
}
/* Page top bar with number */
.page-num-bar{
  display:flex;align-items:center;justify-content:space-between;
  padding:8px 18px 6px;border-bottom:1px solid rgba(0,0,0,.08);
  background:linear-gradient(180deg,#f5f0e8,#fffdf8);
}
.page-num-label{font-family:'JetBrains Mono',monospace;font-size:11px;
  color:#8a7a60;font-weight:500;letter-spacing:.5px}
.page-doc-name{font-size:10px;color:#b0a080;font-style:italic;
  white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:55%}
/* Page content */
.page-content{padding:20px 22px 24px;color:#1a1008;min-height:120px}
.page-content p{font-family:'DM Serif Display',serif;font-size:var(--fs,17px);
  line-height:1.88;margin-bottom:1.2em;color:#1a1008}
/* Page bottom */
.page-foot{
  display:flex;align-items:center;justify-content:center;
  padding:6px 18px 10px;border-top:1px solid rgba(0,0,0,.07);
  background:linear-gradient(180deg,#fffdf8,#f5f0e8);
}
.page-foot-num{font-family:'JetBrains Mono',monospace;font-size:11px;color:#8a7a60}
/* Page spine effect */
.book-page::before{
  content:'';position:absolute;left:0;top:0;bottom:0;width:6px;
  background:linear-gradient(90deg,rgba(0,0,0,.12),transparent);
}

/* Word highlighting — dark text on light bg */
.word{display:inline;cursor:pointer;border-radius:3px;padding:1px 2px;transition:background .06s}
.word:hover{background:rgba(255,102,0,.15)}
.word.speaking{background:rgba(255,180,0,.5);color:#6b3000;font-weight:700;border-radius:3px}

/* ═══ PLAYBAR ═══ */
.playbar{background:var(--bg2);border-top:1px solid var(--bdr);
  padding:8px 14px 10px;flex-shrink:0}
.lock-badge{text-align:center;margin-bottom:5px;display:none}
.lock-badge span{background:rgba(255,102,0,.12);border:1px solid var(--f2);
  border-radius:20px;padding:2px 10px;font-size:10px;color:var(--f3);font-weight:700}
.pb-row{display:flex;align-items:center;gap:8px;margin-bottom:8px}
.pb-time{font-size:10px;color:var(--txt3);font-family:'JetBrains Mono',monospace;min-width:28px}
.pb-track{flex:1;height:4px;background:var(--bdr);border-radius:4px;
  position:relative;cursor:pointer;touch-action:none}
.pb-fill{height:100%;background:var(--gfire);border-radius:4px;pointer-events:none}
.pb-thumb{position:absolute;top:50%;right:0;transform:translate(50%,-50%);
  width:14px;height:14px;background:var(--f3);border-radius:50%;
  pointer-events:none;box-shadow:0 0 8px var(--f2)}
.pb-ctrls{display:flex;align-items:center;justify-content:center;gap:8px}
.pb-btn{width:40px;height:40px;border-radius:11px;border:1px solid var(--bdr);
  background:var(--sur);color:var(--txt2);font-size:17px;cursor:pointer;
  display:flex;align-items:center;justify-content:center}
.pb-btn:active{background:var(--sur2);transform:scale(.9);border-color:var(--f2)}
.pb-play{width:54px;height:54px;border-radius:16px;background:var(--gfire);
  color:#000;font-size:24px;border:none;cursor:pointer;
  display:flex;align-items:center;justify-content:center;
  box-shadow:0 4px 16px rgba(255,80,0,.4);flex-shrink:0}
.pb-play:active{transform:scale(.92)}
.pb-speed-row{display:flex;align-items:center;gap:8px;margin-top:8px;justify-content:center}
.pb-sl{font-size:10px;color:var(--txt3);font-family:'JetBrains Mono',monospace}
.pb-range{-webkit-appearance:none;flex:1;max-width:180px;height:4px;
  background:var(--bdr);border-radius:4px;outline:none}
.pb-range::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;
  border-radius:50%;background:var(--f3);cursor:pointer;
  border:2px solid #000;box-shadow:0 0 5px var(--f2)}
.pb-sv{font-size:13px;color:var(--f3);font-family:'JetBrains Mono',monospace;
  min-width:34px;font-weight:700;text-align:right}

/* ═══ INPUTS ═══ */
.fr-in{width:100%;background:var(--sur);border:1px solid var(--bdr);
  border-radius:var(--r);color:var(--txt);font-family:'Syne',sans-serif;
  font-size:14px;padding:13px 14px;outline:none;margin-bottom:14px}
.fr-in:focus{border-color:var(--f2)}
.fr-ta{width:100%;background:var(--sur);border:1px solid var(--bdr);
  border-radius:var(--r);color:var(--txt);font-family:'DM Serif Display',serif;
  font-size:15px;line-height:1.75;padding:14px;resize:none;outline:none;
  margin-bottom:14px;-webkit-overflow-scrolling:touch}
.fr-ta:focus{border-color:var(--f2)}
.url-tip{background:var(--sur);border:1px solid var(--bdr);border-radius:var(--r);
  padding:13px;font-size:13px;color:var(--txt2);line-height:1.6;margin-bottom:14px}

/* ═══ UPLOAD ═══ */
.fmt-row{display:flex;gap:7px;flex-wrap:wrap;justify-content:center;margin-bottom:16px}
.fmt{background:var(--sur);border:1px solid var(--bdr);border-radius:20px;
  padding:3px 11px;font-size:11px;color:var(--txt3);font-family:'JetBrains Mono',monospace}

/* ═══ DICTATE ═══ */
.mic-orb{width:112px;height:112px;border-radius:50%;background:var(--sur);
  border:2px solid var(--bdr);display:flex;align-items:center;justify-content:center;
  font-size:46px;cursor:pointer}
.mic-orb.rec{border-color:var(--f1);animation:mpulse 1.2s infinite}
@keyframes mpulse{0%{box-shadow:0 0 0 0 rgba(255,58,0,.5)}
  70%{box-shadow:0 0 0 18px rgba(255,58,0,0)}100%{box-shadow:0 0 0 0 rgba(255,58,0,0)}}
.dict-timer{font-family:'JetBrains Mono',monospace;font-size:22px;color:var(--f3);display:none}

/* ═══ VOICE PAGE ═══ */
.v-sel{width:100%;background:var(--sur);border:1px solid var(--bdr);
  border-radius:var(--r);color:var(--txt);font-family:'Syne',sans-serif;
  font-size:14px;padding:13px 14px;outline:none;margin-bottom:14px;cursor:pointer}
.rec-area{background:var(--sur);border:1px solid var(--bdr);border-radius:var(--r2);
  padding:16px;margin-bottom:14px}
.wave-box{height:50px;background:var(--bg);border-radius:9px;
  display:flex;align-items:center;justify-content:center;
  gap:3px;padding:0 10px;overflow:hidden;margin:10px 0}
.wb{width:3px;border-radius:3px;height:5px;flex-shrink:0}
.rec-btn{width:100%;padding:13px;border-radius:var(--r);border:2px solid var(--bdr);
  background:none;color:var(--txt);font-family:'Syne',sans-serif;font-weight:700;
  font-size:14px;cursor:pointer;display:flex;align-items:center;
  justify-content:center;gap:10px;margin-bottom:8px}
.rec-btn.rec{border-color:var(--f1);color:var(--f1)}
.rec-status{font-size:12px;color:var(--txt3);text-align:center}
.vr-item{display:flex;align-items:flex-start;gap:10px;background:var(--sur);
  border:1.5px solid var(--bdr);border-radius:var(--r);padding:12px 14px;
  margin-bottom:8px}
.vr-item.active-v{border-color:var(--f2);background:var(--sur2)}
.vr-body{flex:1;min-width:0}
.vr-name{font-size:13px;font-weight:700;color:var(--txt)}
.vr-date{font-size:11px;color:var(--txt3);margin-top:2px}
.vr-analysis{font-size:11px;color:var(--txt3);margin-top:4px;line-height:1.4}
.vr-acts{display:flex;gap:6px;flex-shrink:0;margin-top:2px}
.vr-play-btn{width:32px;height:32px;border-radius:8px;background:var(--sur2);
  border:1px solid var(--bdr);color:var(--f3);font-size:14px;cursor:pointer;
  display:flex;align-items:center;justify-content:center}
.vr-del-btn{width:30px;height:30px;border-radius:8px;background:none;border:none;
  color:var(--txt3);font-size:14px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.ai-badge{background:var(--sur2);border:1px solid var(--f2);border-radius:6px;
  padding:2px 6px;font-size:9px;color:var(--f3);font-weight:700;margin-left:5px}
.active-v-banner{display:none;background:var(--sur);border:1px solid var(--f2);
  border-radius:var(--r);padding:12px;text-align:center;
  color:var(--f3);font-weight:700;font-size:14px;margin-top:4px}

/* ═══ SAVES ═══ */
.stabs{display:flex;border-bottom:1px solid var(--bdr);margin:-18px -16px 18px;padding:0 16px}
.stab{flex:1;padding:12px 8px;text-align:center;font-size:11px;font-weight:700;
  color:var(--txt3);cursor:pointer;border-bottom:2px solid transparent;
  letter-spacing:.5px;text-transform:uppercase}
.stab.on{color:var(--f3);border-bottom-color:var(--f2)}
.save-item{background:var(--sur);border-left:3px solid var(--f2);
  border-radius:0 10px 10px 0;padding:10px 12px;margin-bottom:8px;
  display:flex;align-items:flex-start;gap:8px}
.save-item:active{opacity:.8}
.sib{flex:1;min-width:0;cursor:pointer}
.si-text{font-size:13px;color:var(--txt);line-height:1.5}
.si-meta{font-size:11px;color:var(--txt3);margin-top:3px}
.si-del{width:28px;height:28px;border-radius:7px;border:none;background:none;
  color:var(--txt3);font-size:14px;cursor:pointer;flex-shrink:0}
.empty-st{text-align:center;padding:32px 20px;color:var(--txt3);font-size:14px;line-height:1.7}
.notes-ta{width:100%;min-height:250px;background:var(--sur);border:1px solid var(--bdr);
  border-radius:var(--r);color:var(--txt);font-family:'DM Serif Display',serif;
  font-size:15px;line-height:1.8;padding:14px;resize:none;outline:none;
  margin-bottom:12px;-webkit-overflow-scrolling:touch}
.notes-ta:focus{border-color:var(--f2)}

/* ═══ AI ═══ */
.ai-result{background:var(--sur);border:1px solid var(--bdr);border-radius:var(--r);
  padding:14px;font-size:14px;color:var(--txt2);line-height:1.8;
  min-height:70px;margin-bottom:14px;white-space:pre-wrap}

/* ═══ LOADING OVERLAY (full screen) ═══ */
.lov{position:fixed;inset:0;background:rgba(9,4,10,.95);display:flex;
  flex-direction:column;align-items:center;justify-content:center;
  gap:18px;z-index:9000;display:none}
.lov.show{display:flex}
.lov-bar{width:200px;height:3px;background:var(--bdr);border-radius:3px;overflow:hidden}
.lov-fill{height:100%;background:var(--gfire);border-radius:3px;animation:lf 1.3s ease-in-out infinite}
@keyframes lf{0%{width:0;margin-left:0}50%{width:60%;margin-left:20%}100%{width:0;margin-left:100%}}
.lov-msg{font-size:14px;color:var(--txt2)}

/* ═══ TOAST ═══ */
.toast{position:fixed;bottom:76px;left:50%;
  transform:translateX(-50%) translateY(12px);
  background:var(--sur);border:1px solid var(--f2);border-radius:12px;
  padding:10px 20px;font-size:13px;color:var(--txt);z-index:9999;
  opacity:0;transition:all .25s;pointer-events:none;
  white-space:nowrap;max-width:92vw;text-align:center;
  box-shadow:0 4px 18px rgba(255,80,0,.28)}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}

/* ═══ MISC ═══ */
.spin{width:17px;height:17px;border:2px solid rgba(255,102,0,.2);
  border-top-color:var(--f2);border-radius:50%;
  animation:sp .6s linear infinite;display:inline-block}
@keyframes sp{to{transform:rotate(360deg)}}
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--bdr);border-radius:2px}
</style>
</head>
<body>
<div class="embers" id="embers"></div>

<!-- ════════════════════════════════
  HOME
════════════════════════════════ -->
<div class="page active" id="page-home">
  <div class="nav">
    <div class="nav-logo">🔥 FireRead</div>
    <div class="nav-acts">
      <button class="nav-btn" onclick="cycleQuote()">💡</button>
    </div>
  </div>
  <div class="home-scroll">
    <div class="quote-wrap">
      <span class="q-flame">🔥</span>
      <div class="q-text" id="qText"></div>
      <div class="q-auth" id="qAuth"></div>
      <div class="q-dots" id="qDots"></div>
    </div>
    <div class="cta-wrap">
      <button class="cta-btn" onclick="goPage('page-listen')">
        <div class="cta-bg"></div>
        <div class="cta-inner">
          <div class="cta-flames">🔥🔥🔥</div>
          <div class="cta-label">Listen &amp; Learn</div>
          <div class="cta-sub">TAP TO IGNITE YOUR MIND</div>
        </div>
      </button>
    </div>
    <div class="home-lib">
      <div class="sec" style="margin-top:16px">Library (<span id="libCount">0</span>)</div>
      <div id="libraryList"><div class="lib-empty">No files yet — tap Listen &amp; Learn to add content.</div></div>
    </div>
    <div style="height:14px"></div>
  </div>
  <div class="tabs">
    <button class="tab on" id="tab-home" onclick="navTab('page-home',0)"><span class="ti">🏠</span>Home</button>
    <button class="tab" onclick="navTab('page-reader',1)"><span class="ti">📖</span>Reader</button>
    <button class="tab" onclick="navTab('page-voice',2)"><span class="ti">🎙️</span>Voice</button>
    <button class="tab" onclick="navTab('page-saves',3)"><span class="ti">🔖</span>Saves</button>
    <button class="tab" onclick="navTab('page-ai',4)"><span class="ti">✨</span>AI</button>
  </div>
</div>

<!-- ════════════════════════════════
  LISTEN & LEARN
════════════════════════════════ -->
<div class="page" id="page-listen">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Listen &amp; Learn</div>
  </div>
  <div class="pg-body">
    <div style="text-align:center;padding:10px 0 18px">
      <div style="font-family:'DM Serif Display',serif;font-size:25px;margin-bottom:6px">
        Listen &amp; <em style="background:var(--gfire);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text">Learn</em>
      </div>
      <div style="font-size:13px;color:var(--txt2);line-height:1.6">Add content any way you like.<br>FireRead does the rest.</div>
    </div>
    <div class="add-grid">
      <div class="add-card" onclick="goPage('page-upload')"><div class="ci">📂</div><div class="cl">Upload File</div><div class="cs">PDF · TXT · EPUB</div></div>
      <div class="add-card" onclick="goPage('page-paste')"><div class="ci">📋</div><div class="cl">Paste Text</div><div class="cs">Any text</div></div>
      <div class="add-card" onclick="goPage('page-url')"><div class="ci">🔗</div><div class="cl">Add URL</div><div class="cs">Web articles</div></div>
      <div class="add-card" onclick="goPage('page-dictate')"><div class="ci">🎙️</div><div class="cl">Dictate</div><div class="cs">Speak to add</div></div>
    </div>
    <div class="sec">Recent</div>
    <div id="listenRecent"><div class="lib-empty">Nothing added yet.</div></div>
  </div>
</div>

<!-- ════════════════════════════════
  UPLOAD
════════════════════════════════ -->
<div class="page" id="page-upload">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Upload File</div>
  </div>
  <div class="pg-body">
    <!-- THE ONLY CORRECT WAY TO OPEN FILES ON IPHONE SAFARI -->
    <input type="file" id="fileInput"
           accept="application/pdf,.pdf,text/plain,.txt,.epub,.md,.rtf"
           multiple
           onchange="handleFiles(this.files)"
           style="display:none">

    <label for="fileInput" style="display:block;margin-bottom:16px;cursor:pointer">
      <div style="background:var(--gfire);border-radius:var(--r2);padding:40px 20px;text-align:center">
        <div style="font-size:54px;margin-bottom:12px">📂</div>
        <div style="font-family:'DM Serif Display',serif;font-size:22px;color:#000;font-weight:900;margin-bottom:6px">Tap to Browse Files</div>
        <div style="font-size:13px;color:rgba(0,0,0,.65);font-weight:600">Opens iPhone Files app</div>
      </div>
    </label>

    <div class="sec">Supported Formats</div>
    <div class="fmt-row">
      <span class="fmt">PDF</span><span class="fmt">TXT</span>
      <span class="fmt">EPUB</span><span class="fmt">MD</span><span class="fmt">RTF</span>
    </div>
    <p style="color:var(--txt3);font-size:12px;text-align:center;line-height:1.8">
      Unlimited files · Any size<br>Works with iCloud, Downloads, Books &amp; more
    </p>
  </div>
</div>

<!-- ════════════════════════════════
  PASTE
════════════════════════════════ -->
<div class="page" id="page-paste">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Paste Text</div>
  </div>
  <div class="pg-body">
    <div class="sec">Title (optional)</div>
    <input class="fr-in" id="pasteTitle" type="text" placeholder="e.g. Chapter 3 — Human Nature">
    <div class="sec">Your Text</div>
    <textarea class="fr-ta" id="pasteText" placeholder="Paste any text here…" style="min-height:220px"></textarea>
    <button class="fbtn fire" onclick="submitPaste()">🔥 Ignite This Text</button>
    <button class="fbtn ghost" onclick="goBack()">Cancel</button>
  </div>
</div>

<!-- ════════════════════════════════
  URL
════════════════════════════════ -->
<div class="page" id="page-url">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Add URL</div>
  </div>
  <div class="pg-body">
    <div class="sec">Article URL</div>
    <input class="fr-in" id="urlInput" type="url" placeholder="https://example.com/article">
    <div class="url-tip">🔥 FireRead fetches the article text and strips all ads and menus automatically.</div>
    <button class="fbtn fire" id="urlBtn" onclick="submitURL()">🔗 Fetch &amp; Add Article</button>
    <button class="fbtn ghost" onclick="goBack()">Cancel</button>
    <div id="urlLoad" style="display:none;text-align:center;padding:24px 0;color:var(--txt2);font-size:14px">
      <div class="spin" style="margin:0 auto 12px"></div>Fetching article…
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  DICTATE
════════════════════════════════ -->
<div class="page" id="page-dictate">
  <div class="nav">
    <button class="nav-back" onclick="stopDictate();goBack()">←</button>
    <div class="nav-title">Dictate</div>
  </div>
  <div style="flex:1;display:flex;flex-direction:column;overflow:hidden">
    <div style="display:flex;flex-direction:column;align-items:center;gap:16px;padding:24px 20px 14px">
      <div class="mic-orb" id="micOrb" onclick="toggleDictate()">🎙️</div>
      <div class="dict-timer" id="dictTimer">0:00</div>
      <div style="font-size:14px;color:var(--txt2);text-align:center;max-width:260px;line-height:1.6" id="dictStatus">
        Tap the mic to start speaking.
      </div>
    </div>
    <textarea class="fr-ta" id="dictPreview" readonly
      placeholder="Your words appear here…"
      style="margin:0 16px;max-width:calc(100% - 32px);flex:1;min-height:80px"></textarea>
    <div style="padding:12px 16px 16px;display:flex;flex-direction:column;gap:8px">
      <button class="fbtn fire" id="dictSaveBtn" onclick="saveDictation()" style="display:none">💾 Save to Library</button>
      <button class="fbtn ghost" onclick="stopDictate();goBack()">Cancel</button>
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  READER
════════════════════════════════ -->
<div class="page" id="page-reader">
  <div class="nav">
    <button class="nav-back" onclick="goHome()">←</button>
    <div class="nav-title" id="readerTitle">No document</div>
    <div class="nav-acts">
      <button class="nav-btn" onclick="goPage('page-ai')">✨</button>
      <button class="nav-btn" onclick="goPage('page-voice')">🎙️</button>
    </div>
  </div>

  <!-- Empty state -->
  <div id="readerEmpty" style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:16px;padding:32px;text-align:center">
    <div style="font-size:50px">📖</div>
    <div style="font-family:'DM Serif Display',serif;font-size:22px">Nothing loaded yet</div>
    <div style="color:var(--txt2);font-size:14px;line-height:1.6">Go home and pick or add a document.</div>
    <button class="fbtn fire" style="max-width:240px" onclick="goHome()">🔥 Go to Library</button>
  </div>

  <!-- Reader main -->
  <div id="readerMain" style="display:none;flex:1;flex-direction:column;overflow:hidden">
    <div class="reader-tb">
      <div class="font-btns">
        <button class="font-btn" onclick="changeFontSize(-1)">A−</button>
        <span class="font-val" id="fontVal">17</span>
        <button class="font-btn" onclick="changeFontSize(1)">A+</button>
      </div>
      <span class="pg-indicator" id="pgIndicator">p.1</span>
      <button class="bm-btn" onclick="addBookmark()">🔖</button>
    </div>
    <!-- Book pages container -->
    <div class="book-scroll" id="bookScroll"></div>
  </div>

  <!-- Playbar -->
  <div class="playbar" id="playbar" style="display:none">
    <div class="lock-badge" id="lockBadge"><span>🔒 PLAYS ON LOCK SCREEN</span></div>
    <div class="pb-row">
      <span class="pb-time" id="pbEl">0:00</span>
      <div class="pb-track" id="pbTrack">
        <div class="pb-fill" id="pbFill" style="width:0%"></div>
        <div class="pb-thumb" id="pbThumb" style="right:100%"></div>
      </div>
      <span class="pb-time" id="pbTot">0:00</span>
    </div>
    <div class="pb-ctrls">
      <button class="pb-btn" onclick="skipW(-30)">⏮</button>
      <button class="pb-btn" onclick="skipW(-8)">⏪</button>
      <button class="pb-play" id="pbPlay" onclick="togglePlay()">▶</button>
      <button class="pb-btn" onclick="skipW(8)">⏩</button>
      <button class="pb-btn" onclick="skipW(30)">⏭</button>
    </div>
    <div class="pb-speed-row">
      <span class="pb-sl">0.5×</span>
      <input type="range" class="pb-range" id="speedSlider" min="0.5" max="2.5" step="0.1" value="1" oninput="setSpeed(this.value)">
      <span class="pb-sl">2.5×</span>
      <span class="pb-sv" id="speedVal">1.0×</span>
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  VOICE
════════════════════════════════ -->
<div class="page" id="page-voice">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Voice Settings</div>
  </div>
  <div class="pg-body">
    <div class="sec">AI Reading Voice</div>
    <select class="v-sel" id="voiceSel" onchange="pickVoice()"><option value="">Loading…</option></select>
    <button class="fbtn fire" style="margin-bottom:20px" onclick="testVoice()">🔊 Test This Voice</button>
    <div class="sec">Record Your Voice</div>
    <div class="rec-area">
      <div style="font-size:13px;color:var(--txt2);margin-bottom:8px;line-height:1.6">Record up to 60s. AI analyzes and saves your voice for reading.</div>
      <div class="wave-box" id="voiceWave"><span style="font-size:12px;color:var(--txt3)">Record to see waveform</span></div>
      <button class="rec-btn" id="voiceRecBtn" onclick="toggleVoiceRec()">🎙️ <span id="voiceRecLabel">Start Recording</span></button>
      <div class="rec-status" id="voiceRecStatus">Tap above to record</div>
    </div>
    <div id="postRecActions" style="display:none;margin-bottom:14px">
      <input class="fr-in" id="voiceNameIn" type="text" placeholder="Name this voice">
      <button class="fbtn fire" onclick="analyzeAndSave()" id="analyzeBtn">✨ Analyze &amp; Save Voice</button>
      <button class="fbtn ghost" onclick="previewRec()" style="margin-top:8px">▶️ Preview Recording</button>
    </div>
    <div class="sec" id="savedVLabel" style="display:none">My Saved Voices</div>
    <div id="voiceRecList"></div>
    <div class="active-v-banner" id="activeVBanner">🎙️ Custom voice active!</div>
  </div>
</div>

<!-- ════════════════════════════════
  SAVES
════════════════════════════════ -->
<div class="page" id="page-saves">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Saves</div>
  </div>
  <div class="pg-body">
    <div class="stabs">
      <div class="stab on" onclick="switchSavesTab('bm',this)">🔖 Bookmarks</div>
      <div class="stab" onclick="switchSavesTab('notes',this)">✏️ Notes</div>
    </div>
    <div id="savesBM"><div class="empty-st">No bookmarks yet.<br>Use 🔖 in the reader.</div></div>
    <div id="savesNotes" style="display:none">
      <div class="sec">Study Notes</div>
      <textarea class="notes-ta" id="notesTA" placeholder="Write your study notes here…"></textarea>
      <button class="fbtn fire" onclick="saveNotes()">💾 Save Notes</button>
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  AI
════════════════════════════════ -->
<div class="page" id="page-ai">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">AI Study Tools</div>
  </div>
  <div class="pg-body">
    <div class="sec">AI Summary</div>
    <button class="fbtn fire" id="summaryBtn" onclick="genSummary()" style="margin-bottom:12px">✨ Summarize Document</button>
    <div class="ai-result" id="summaryResult">Open a document then tap Summarize.</div>
    <div class="sec" style="margin-top:18px">Ask AI</div>
    <input class="fr-in" id="qaIn" type="text" placeholder="e.g. What are the key takeaways?" onkeydown="if(event.key==='Enter')askAI()">
    <button class="fbtn fire" onclick="askAI()" style="margin-bottom:12px">💬 Ask</button>
    <div class="ai-result" id="qaResult" style="min-height:54px"></div>
  </div>
</div>

<!-- LOADING OVERLAY (shared, full screen) -->
<div class="lov" id="globalLov">
  <div style="font-family:'DM Serif Display',serif;font-size:20px">🔥 Processing…</div>
  <div class="lov-bar"><div class="lov-fill"></div></div>
  <div class="lov-msg" id="lovMsg">Loading…</div>
</div>

<div class="toast" id="toast"></div>

<script>
'use strict';
/* ══════════════════════════════════════
   FIREREAD — PROFESSIONAL BUILD
══════════════════════════════════════ */

// ── STATE ──
let library=[], currentDoc=null;
let words=[], wordIdx=0, pageBreaks=[];
let isPlaying=false, speechRate=1.0, fontSize=17;
let bookmarks=[], savedNotes={};
let voices=[], selVoiceIdx=0;
let savedVoices=[], activeVoiceId=null;
let mediaRec=null, recChunks=[], recBlob=null, recUrl=null;
let audioCtx=null, analyser=null, waveAnimId=null;
let dictRecog=null, dictFinal='', isDictating=false;
let dictTimerInt=null, dictSecs=0;
let playInterval=null, keepAliveTimer=null, utterance=null;
let silentAudio=null, wakeLock=null;
let timerOffset=0, timerStart=0;
let quoteIdx=0;
let pageHistory=['page-home'];
const synth=window.speechSynthesis;

// ── QUOTES ──
const QUOTES=[
  {q:"The mind is not a vessel to be filled but a fire to be kindled.",a:"Plutarch"},
  {q:"An investment in knowledge pays the best interest.",a:"Benjamin Franklin"},
  {q:"The more that you read, the more things you will know.",a:"Dr. Seuss"},
  {q:"Not all readers are leaders, but all leaders are readers.",a:"Harry S. Truman"},
  {q:"The only way to do great work is to love what you do.",a:"Steve Jobs"},
  {q:"Success is the sum of small efforts repeated day in and day out.",a:"Robert Collier"},
  {q:"A reader lives a thousand lives before he dies.",a:"George R.R. Martin"},
  {q:"Education is the most powerful weapon you can use to change the world.",a:"Nelson Mandela"},
  {q:"Knowledge is power. Power is freedom.",a:"Francis Bacon"},
  {q:"It always seems impossible until it's done.",a:"Nelson Mandela"},
  {q:"Ignite your mind. The fire within you never lies.",a:"FireRead"},
  {q:"The secret of getting ahead is getting started.",a:"Mark Twain"},
  {q:"Push yourself, because no one else is going to do it for you.",a:"Anonymous"},
  {q:"Wake up with determination. Go to bed with satisfaction.",a:"Anonymous"},
  {q:"Do something today that your future self will thank you for.",a:"Anonymous"},
];

function initQuotes(){
  const d=document.getElementById('qDots');
  d.innerHTML=QUOTES.map((_,i)=>`<div class="qdot" onclick="setQuote(${i})"></div>`).join('');
  setQuote(Math.floor(Math.random()*QUOTES.length));
  setInterval(cycleQuote,9000);
}
function setQuote(i){
  quoteIdx=i;
  const q=QUOTES[i];
  document.getElementById('qText').textContent=`"${q.q}"`;
  document.getElementById('qAuth').textContent=`— ${q.a}`;
  document.querySelectorAll('.qdot').forEach((d,j)=>d.classList.toggle('on',j===i));
}
function cycleQuote(){setQuote((quoteIdx+1)%QUOTES.length);}

// ── EMBERS ──
function spawnEmbers(){
  const c=document.getElementById('embers');
  const cols=['#ff3a00','#ff6600','#ff9900','#ffcc00','#ff2200'];
  for(let i=0;i<20;i++){
    const e=document.createElement('div');
    e.className='ember';
    const sz=2+Math.random()*4;
    e.style.cssText=`left:${Math.random()*100}%;width:${sz}px;height:${sz}px;`+
      `--d:${3+Math.random()*5}s;--dl:${Math.random()*7}s;`+
      `--dx:${(Math.random()-.5)*80}px;background:${cols[i%5]}`;
    c.appendChild(e);
  }
}

// ── NAVIGATION (instant, no delays) ──
function goPage(id){
  const cur=pageHistory[pageHistory.length-1];
  if(cur===id)return;
  const curEl=document.getElementById(cur);
  const nextEl=document.getElementById(id);
  if(!nextEl)return;
  // Remove active from current, push it behind
  curEl?.classList.add('behind');
  curEl?.classList.remove('active');
  // Activate next — CSS handles the animation
  nextEl.classList.remove('behind');
  nextEl.classList.add('active');
  pageHistory.push(id);
}
function goBack(){
  if(pageHistory.length<=1)return;
  const cur=pageHistory.pop();
  const prev=pageHistory[pageHistory.length-1];
  const curEl=document.getElementById(cur);
  const prevEl=document.getElementById(prev);
  curEl?.classList.remove('active','behind');
  prevEl?.classList.remove('behind');
  prevEl?.classList.add('active');
}
function goHome(){
  // Hard reset to home instantly
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active','behind'));
  document.getElementById('page-home').classList.add('active');
  pageHistory=['page-home'];
  updateTabHighlight(0);
}
function navTab(pageId,idx){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active','behind'));
  const el=document.getElementById(pageId);
  if(el)el.classList.add('active');
  pageHistory=[pageId];
  updateTabHighlight(idx);
}
function updateTabHighlight(idx){
  document.querySelectorAll('.tab').forEach((t,i)=>t.classList.toggle('on',i===idx));
}

// ── LOADING OVERLAY ──
function showLov(msg){
  document.getElementById('lovMsg').textContent=msg||'Loading…';
  document.getElementById('globalLov').classList.add('show');
}
function hideLov(){
  document.getElementById('globalLov').classList.remove('show');
}

// ── VOICES ──
function loadVoices(){
  let all=synth.getVoices();
  const en=all.filter(v=>v.lang.startsWith('en'));
  voices=en.length>0?en:all;
  const sel=document.getElementById('voiceSel');
  if(!voices.length){sel.innerHTML='<option>No voices found</option>';return;}
  sel.innerHTML=voices.map((v,i)=>`<option value="${i}">${v.name} (${v.lang})</option>`).join('');
  const pref=voices.findIndex(v=>/natural|premium|enhanced|samantha|karen|daniel|siri/i.test(v.name));
  selVoiceIdx=pref>=0?pref:0;
  sel.selectedIndex=selVoiceIdx;
}
function pickVoice(){
  selVoiceIdx=parseInt(document.getElementById('voiceSel').value)||0;
  activeVoiceId=null;
  renderVoiceList();
  showToast('Voice updated');
}
function testVoice(){
  synth.cancel();
  const u=new SpeechSynthesisUtterance("This is FireRead. Your knowledge is about to ignite.");
  u.rate=speechRate;
  if(voices[selVoiceIdx])u.voice=voices[selVoiceIdx];
  synth.speak(u);
}

// ── PDF ──
const PDFV='2.16.105';
const PDFBASE=`https://cdnjs.cloudflare.com/ajax/libs/pdf.js/${PDFV}`;

function loadPDFjs(file){
  if(window.pdfjsLib&&window._pdfjsReady){doExtractPDF(file);return;}
  window._pdfjsReady=false;
  document.querySelectorAll('[data-pdfjs]').forEach(s=>s.remove());
  const s=document.createElement('script');
  s.setAttribute('data-pdfjs','1');
  s.src=`${PDFBASE}/pdf.min.js`;
  s.onload=()=>{
    pdfjsLib.GlobalWorkerOptions.workerSrc=`${PDFBASE}/pdf.worker.min.js`;
    window._pdfjsReady=true;
    doExtractPDF(file);
  };
  s.onerror=()=>{hideLov();showToast('Could not load PDF engine. Check internet.');};
  document.head.appendChild(s);
}

async function doExtractPDF(file){
  const reader=new FileReader();
  reader.onerror=()=>{hideLov();showToast('Could not read file from device.');};
  reader.onload=async(ev)=>{
    try{
      // Uint8Array is required — raw ArrayBuffer fails
      const data=new Uint8Array(ev.target.result);
      const pdf=await pdfjsLib.getDocument({data}).promise;
      const n=pdf.numPages;
      let text='';
      for(let i=1;i<=n;i++){
        document.getElementById('lovMsg').textContent=`Page ${i} of ${n}…`;
        const page=await pdf.getPage(i);
        const content=await page.getTextContent();
        // Simple reliable join
        text+=content.items.map(it=>it.str).join(' ').replace(/\s{2,}/g,' ').trim()+'\n\n';
      }
      text=text.trim();
      hideLov();
      if(!text){showToast('No text found. This PDF may be image-only.');return;}
      addToLibrary(file.name,text);
      goPage('page-reader');
    }catch(err){
      hideLov();
      console.error(err);
      if(err.name==='PasswordException')showToast('PDF is password protected.');
      else if(err.name==='InvalidPDFException')showToast('Invalid or corrupted PDF.');
      else showToast('Could not read PDF. Try a different file.');
    }
  };
  reader.readAsArrayBuffer(file);
}

// ── FILE HANDLING ──
function handleFiles(flist){
  if(!flist||!flist.length)return;
  const files=Array.from(flist);
  // Reset input after brief delay (allows re-selecting same file)
  setTimeout(()=>{try{document.getElementById('fileInput').value='';}catch(e){}},300);
  files.forEach(f=>{
    const ext=f.name.split('.').pop().toLowerCase();
    showLov(`Loading ${f.name}…`);
    if(ext==='pdf'){
      loadPDFjs(f);
    } else {
      const r=new FileReader();
      r.onload=e=>{hideLov();addToLibrary(f.name,e.target.result);goPage('page-reader');};
      r.onerror=()=>{hideLov();showToast('Could not read file.');};
      r.readAsText(f);
    }
  });
}

// ── PASTE ──
function submitPaste(){
  const text=document.getElementById('pasteText').value.trim();
  const title=document.getElementById('pasteTitle').value.trim()||'Note — '+new Date().toLocaleTimeString();
  if(!text){showToast('Please paste some text first');return;}
  addToLibrary(title,text);
  document.getElementById('pasteText').value='';
  document.getElementById('pasteTitle').value='';
  goPage('page-reader');
}

// ── URL ──
function submitURL(){
  const url=document.getElementById('urlInput').value.trim();
  if(!url){showToast('Enter a URL');return;}
  document.getElementById('urlBtn').style.display='none';
  document.getElementById('urlLoad').style.display='block';
  fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(url)}`)
    .then(r=>r.json())
    .then(data=>{
      const dp=new DOMParser();
      const doc=dp.parseFromString(data.contents,'text/html');
      ['script','style','nav','footer','header','aside','iframe','noscript'].forEach(t=>
        doc.querySelectorAll(t).forEach(el=>el.remove()));
      const body=doc.querySelector('article')||doc.querySelector('main')||doc.body;
      const text=body?body.innerText.replace(/\n{3,}/g,'\n\n').trim():'';
      document.getElementById('urlBtn').style.display='block';
      document.getElementById('urlLoad').style.display='none';
      if(text.length>50){
        addToLibrary(doc.title||url,text);
        document.getElementById('urlInput').value='';
        goPage('page-reader');
      } else showToast('Could not extract text. Try pasting instead.');
    })
    .catch(()=>{
      document.getElementById('urlBtn').style.display='block';
      document.getElementById('urlLoad').style.display='none';
      showToast('Could not fetch URL. Try pasting the text.');
    });
}

// ── DICTATE ──
function toggleDictate(){isDictating?stopDictate():startDictate();}
function startDictate(){
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(!SR){showToast('Speech recognition not supported');return;}
  dictRecog=new SR();
  dictRecog.continuous=true;dictRecog.interimResults=true;dictRecog.lang='en-US';
  dictFinal='';
  document.getElementById('dictPreview').value='';
  document.getElementById('dictSaveBtn').style.display='none';
  dictRecog.onresult=e=>{
    let interim='';
    for(let i=e.resultIndex;i<e.results.length;i++){
      if(e.results[i].isFinal)dictFinal+=e.results[i][0].transcript+' ';
      else interim=e.results[i][0].transcript;
    }
    document.getElementById('dictPreview').value=dictFinal+interim;
  };
  dictRecog.onerror=err=>{showToast('Mic error: '+err.error);stopDictate();};
  dictRecog.onend=()=>{
    isDictating=false;
    document.getElementById('micOrb').classList.remove('rec');
    document.getElementById('dictTimer').style.display='none';
    clearInterval(dictTimerInt);
    document.getElementById('dictStatus').textContent='Done. Review above.';
    if(dictFinal.trim())document.getElementById('dictSaveBtn').style.display='block';
  };
  dictRecog.start();
  isDictating=true;
  dictSecs=0;
  document.getElementById('micOrb').classList.add('rec');
  document.getElementById('dictStatus').textContent='🔴 Listening… tap to stop.';
  const timerEl=document.getElementById('dictTimer');
  timerEl.style.display='block';timerEl.textContent='0:00';
  dictTimerInt=setInterval(()=>{
    dictSecs++;timerEl.textContent=fmtTime(dictSecs);
    if(dictSecs>=120)stopDictate();
  },1000);
}
function stopDictate(){
  if(dictRecog){try{dictRecog.stop();}catch(e){}dictRecog=null;}
  clearInterval(dictTimerInt);isDictating=false;
  document.getElementById('micOrb').classList.remove('rec');
  document.getElementById('dictTimer').style.display='none';
  document.getElementById('dictStatus').textContent='Tap the mic to start.';
}
function saveDictation(){
  const text=dictFinal.trim();
  if(!text){showToast('Nothing recorded');return;}
  addToLibrary('Dictation — '+new Date().toLocaleTimeString(),text);
  dictFinal='';document.getElementById('dictPreview').value='';
  document.getElementById('dictSaveBtn').style.display='none';
  goPage('page-reader');
}

// ── LIBRARY ──
function addToLibrary(name,text){
  const doc={id:Date.now(),name,text,progress:0};
  library.push(doc);
  loadDoc(doc);
  renderLibrary();
  saveStorage();
  showToast(`🔥 "${name.slice(0,26)}" added`);
}

function renderLibrary(){
  const el=document.getElementById('libraryList');
  const el2=document.getElementById('listenRecent');
  document.getElementById('libCount').textContent=library.length;
  if(!library.length){
    const emp='<div class="lib-empty">No files yet — tap Listen &amp; Learn to add content.</div>';
    el.innerHTML=emp;if(el2)el2.innerHTML='<div class="lib-empty">Nothing added yet.</div>';
    return;
  }
  const html=library.slice().reverse().map(doc=>`
    <div class="lib-card ${currentDoc?.id===doc.id?'now':''}">
      <div class="lci">${fileIcon(doc.name)}</div>
      <div class="lcb" onclick="openDoc(${doc.id})">
        <div class="lcn">${esc(doc.name)}</div>
        <div class="lcm">${cWords(doc.text)} words · ${rTime(doc.text)}</div>
        <div class="lcp"><div class="lcpf" style="width:${doc.progress||0}%"></div></div>
      </div>
      <button class="lib-del" onclick="delDoc(${doc.id})" title="Delete">🗑️</button>
    </div>`).join('');
  el.innerHTML=html;
  if(el2)el2.innerHTML=html;
}

function openDoc(id){
  const doc=library.find(d=>d.id===id);
  if(doc){loadDoc(doc);goPage('page-reader');}
}

function delDoc(id){
  if(!confirm('Delete this document?')) return;
  library=library.filter(d=>d.id!==id);
  if(currentDoc?.id===id){
    currentDoc=null;
    stopSpeech();
    document.getElementById('readerEmpty').style.display='flex';
    document.getElementById('readerMain').style.display='none';
    document.getElementById('playbar').style.display='none';
    document.getElementById('readerTitle').textContent='No document';
  }
  renderLibrary();
  saveStorage();
  showToast('🗑️ Document deleted');
}

function fileIcon(n){
  return {pdf:'📕',epub:'📗',txt:'📄',md:'📝',rtf:'📃'}[(n||'').split('.').pop().toLowerCase()]||'📄';
}
function cWords(t){return (t||'').trim().split(/\s+/).length.toLocaleString();}
function rTime(t){const m=Math.round((t||'').trim().split(/\s+/).length/150);return m<1?'<1 min':m+' min';}
function esc(s){const d=document.createElement('div');d.textContent=s;return d.innerHTML;}

// ── LOAD DOC ──
function loadDoc(doc){
  if(isPlaying)stopSpeech();
  currentDoc=doc;
  document.getElementById('readerTitle').textContent=doc.name.replace(/\.[^.]+$/,'');
  document.getElementById('readerEmpty').style.display='none';
  const rm=document.getElementById('readerMain');
  rm.style.cssText='display:flex;flex:1;flex-direction:column;overflow:hidden';
  document.getElementById('playbar').style.display='block';
  buildBookPages(doc.text);
  if(doc.wordIdx) {
    wordIdx = doc.wordIdx;
    updateProgress();
    updatePageIndicator();
    setTimeout(scrollToCurrentWord, 100);
  }
  renderLibrary();
  document.getElementById('notesTA').value=savedNotes[doc.id]||'';
}

// ── BUILD BOOK PAGES ──
// Split text into pages of ~250 words each, render as physical book pages
const WORDS_PER_PAGE=250;

function buildBookPages(text){
  const scroll=document.getElementById('bookScroll');
  scroll.innerHTML='';
  words=[];pageBreaks=[];

  // Split into paragraphs
  const paras=text.split(/\n{2,}|\r\n\r\n/).map(p=>p.trim()).filter(p=>p);

  // Group paragraphs into pages
  let pages=[];
  let curPage=[];
  let wordCount=0;

  paras.forEach(para=>{
    const paraWords=para.split(/\s+/).filter(w=>w);
    curPage.push({para,wordCount:paraWords.length});
    wordCount+=paraWords.length;
    if(wordCount>=WORDS_PER_PAGE){
      pages.push(curPage);
      curPage=[];wordCount=0;
    }
  });
  if(curPage.length>0)pages.push(curPage);
  if(pages.length===0)pages.push([{para:text.trim(),wordCount:1}]);

  const docName=currentDoc?.name.replace(/\.[^.]+$/,'')||'Document';
  const totalPages=pages.length;

  const fragment = document.createDocumentFragment();
  pages.forEach((pageParas,pageNum)=>{
    pageBreaks.push(words.length);

    const pageEl=document.createElement('div');
    pageEl.className='book-page';
    pageEl.id=`bp_${pageNum}`;

    const topBar=document.createElement('div');
    topBar.className='page-num-bar';
    topBar.innerHTML=`<span class="page-num-label">PAGE ${pageNum+1} OF ${totalPages}</span><span class="page-doc-name">${esc(docName)}</span>`;
    pageEl.appendChild(topBar);

    const content=document.createElement('div');
    content.className='page-content';
    content.style.setProperty('--fs', fontSize+'px');

    pageParas.forEach(({para})=>{
      const p=document.createElement('p');
      const tokens=para.split(/(\s+)/);
      tokens.forEach(tok=>{
        if(/^\s+$/.test(tok)){p.appendChild(document.createTextNode(' '));}
        else if(tok){
          const span=document.createElement('span');
          span.className='word';span.textContent=tok;
          const idx=words.length;
          span.dataset.idx=idx;
          span.addEventListener('click',()=>jumpToWord(idx),{passive:true});
          p.appendChild(span);
          words.push({el:span,text:tok,page:pageNum});
        }
      });
      content.appendChild(p);
    });
    pageEl.appendChild(content);

    const foot=document.createElement('div');
    foot.className='page-foot';
    foot.innerHTML=`<span class="page-foot-num">— ${pageNum+1} —</span>`;
    pageEl.appendChild(foot);

    fragment.appendChild(pageEl);
  });
  scroll.appendChild(fragment);

  wordIdx=0;
  updateProgress();
  updateTotalTime();
  updatePageIndicator();
}

function updatePageIndicator(){
  if(!words.length)return;
  const w=words[wordIdx];
  if(w){
    const pg=w.page+1;
    const total=pageBreaks.length;
    document.getElementById('pgIndicator').textContent=`p.${pg}/${total}`;
  }
}

function scrollToCurrentWord(){
  if(!words[wordIdx])return;
  const el=words[wordIdx].el;
  el.scrollIntoView({behavior:'smooth',block:'center'});
}

// ── SPEECH ──
function togglePlay(){
  if(!currentDoc){showToast('Open a document first');goHome();return;}
  isPlaying?pauseSpeech():resumeOrStart();
}
function resumeOrStart(){
  if(synth.paused){
    synth.resume();isPlaying=true;
    document.getElementById('pbPlay').textContent='⏸';
    startSilentAudio();updateMediaSession();requestWakeLock();startTimer();
    return;
  }
  startSpeechFrom(wordIdx);
}

function startSpeechFrom(idx){
  stopSpeech();
  if(!words.length)return;
  idx=Math.max(0,Math.min(idx,words.length-1));
  wordIdx=idx;isPlaying=true;
  document.getElementById('pbPlay').textContent='⏸';
  document.getElementById('lockBadge').style.display='block';

  const text=words.slice(idx).map(w=>w.text).join(' ');
  utterance=new SpeechSynthesisUtterance(text);
  utterance.rate=speechRate;utterance.pitch=1;utterance.volume=1;
  if(!activeVoiceId&&voices[selVoiceIdx])utterance.voice=voices[selVoiceIdx];

  let li=idx;
  utterance.onboundary=e=>{
    if(e.name!=='word')return;
    if(li>0&&words[li-1])words[li-1].el.classList.remove('speaking');
    if(words[li]){
      words[li].el.classList.add('speaking');
      if(li % 5 === 0) words[li].el.scrollIntoView({behavior:'smooth',block:'center'});
      wordIdx=li;
      updateProgress();
      updatePageIndicator();
      updateMediaSession();
    }
    li++;
  };
  utterance.onend=()=>{
    clearHL();isPlaying=false;
    document.getElementById('pbPlay').textContent='▶';
    document.getElementById('lockBadge').style.display='none';
    stopTimer();stopSilentAudio();updateMediaSession();releaseWakeLock();
    if(currentDoc){currentDoc.progress=100;renderLibrary();saveStorage();}
  };
  utterance.onerror=err=>{
    if(err.error==='interrupted'||err.error==='canceled')return;
    isPlaying=false;
    document.getElementById('pbPlay').textContent='▶';
    document.getElementById('lockBadge').style.display='none';
    stopTimer();stopSilentAudio();updateMediaSession();releaseWakeLock();
  };

  synth.speak(utterance);
  startSilentAudio();setupMediaSession();updateMediaSession();
  requestWakeLock();chromeKeepalive();startTimer();
}

function chromeKeepalive(){
  clearInterval(keepAliveTimer);
  keepAliveTimer=setInterval(()=>{
    if(!isPlaying){clearInterval(keepAliveTimer);return;}
    if(synth.speaking&&!synth.paused){
      synth.pause();setTimeout(()=>{if(isPlaying)synth.resume();},50);
    }
  },12000);
}

function pauseSpeech(){
  synth.pause();isPlaying=false;
  document.getElementById('pbPlay').textContent='▶';
  clearInterval(keepAliveTimer);stopTimer();
  updateMediaSession();releaseWakeLock();
}
function stopSpeech(){
  clearInterval(keepAliveTimer);synth.cancel();isPlaying=false;
  document.getElementById('pbPlay').textContent='▶';
  document.getElementById('lockBadge').style.display='none';
  stopTimer();stopSilentAudio();clearHL();
  updateMediaSession();releaseWakeLock();
}
function clearHL(){words.forEach(w=>w.el.classList.remove('speaking'));}

function jumpToWord(idx){
  stopSpeech();wordIdx=idx;
  updateProgress();updatePageIndicator();
  setTimeout(()=>startSpeechFrom(idx),60);
}
function skipW(delta){
  const ni=Math.max(0,Math.min(words.length-1,wordIdx+delta));
  if(isPlaying)startSpeechFrom(ni);
  else{wordIdx=ni;updateProgress();updatePageIndicator();scrollToCurrentWord();}
}
function setSpeed(v){
  speechRate=parseFloat(v);
  document.getElementById('speedVal').textContent=speechRate.toFixed(1)+'×';
  updateTotalTime();
  if(isPlaying)startSpeechFrom(wordIdx);
}

// ── PROGRESS ──
function updateProgress(){
  if(!words.length)return;
  const pct=(wordIdx/words.length)*100;
  document.getElementById('pbFill').style.width=pct+'%';
  document.getElementById('pbThumb').style.right=(100-pct)+'%';
  if(currentDoc){
    currentDoc.progress=Math.round(pct);
    currentDoc.wordIdx=wordIdx;
  }
}
function updateTotalTime(){
  if(!words.length)return;
  document.getElementById('pbTot').textContent=fmtTime((words.length/Math.max(1,150*speechRate))*60);
}

let timerInt=null;
function startTimer(){
  stopTimer();
  timerOffset=(wordIdx/Math.max(1,words.length))*((words.length/Math.max(1,150*speechRate))*60);
  timerStart=Date.now();
  timerInt=setInterval(()=>{
    if(!isPlaying)return;
    const el=timerOffset+(Date.now()-timerStart)/1000;
    document.getElementById('pbEl').textContent=fmtTime(el);
    updateProgress();
  },400);
}
function stopTimer(){if(timerInt){clearInterval(timerInt);timerInt=null;}}
function fmtTime(s){s=Math.max(0,s);return `${Math.floor(s/60)}:${String(Math.floor(s%60)).padStart(2,'0')}`;}

function initPbTrack(){
  const t=document.getElementById('pbTrack');
  if(!t)return;
  const seek=x=>{
    const r=t.getBoundingClientRect();
    const p=Math.max(0,Math.min(1,(x-r.left)/r.width));
    const ni=Math.floor(p*words.length);
    if(isPlaying)startSpeechFrom(ni);else{wordIdx=ni;updateProgress();updatePageIndicator();}
  };
  t.addEventListener('click',e=>seek(e.clientX));
  t.addEventListener('touchend',e=>{e.preventDefault();seek(e.changedTouches[0].clientX);},{passive:false});
}

// ── FONT ──
function changeFontSize(d){
  fontSize=Math.max(13,Math.min(26,fontSize+d));
  document.getElementById('fontVal').textContent=fontSize;
  // Update all book pages
  document.querySelectorAll('.page-content').forEach(el=>el.style.setProperty('--fs',fontSize+'px'));
}

// ── BOOKMARKS ──
function addBookmark(){
  if(!currentDoc){showToast('Open a document first');return;}
  const snippet=words.slice(wordIdx,wordIdx+10).map(w=>w.text).join(' ');
  const pg=words[wordIdx]?.page||0;
  bookmarks.push({id:Date.now(),docId:currentDoc.id,docName:currentDoc.name,wordIdx,snippet,page:pg+1});
  renderBookmarks();saveStorage();showToast('🔖 Bookmarked!');
}
function renderBookmarks(){
  const el=document.getElementById('savesBM');
  if(!bookmarks.length){
    el.innerHTML='<div class="empty-st">No bookmarks yet.<br>Use 🔖 in the reader.</div>';return;
  }
  el.innerHTML=bookmarks.slice().reverse().map(bm=>`
    <div class="save-item">
      <div class="sib" onclick="gotoBookmark(${bm.id})">
        <div class="si-text">${esc(bm.snippet)}…</div>
        <div class="si-meta">📚 ${esc(bm.docName)}${bm.page?' · Page '+bm.page:''}</div>
      </div>
      <button class="si-del" onclick="delBookmark(${bm.id})">🗑️</button>
    </div>`).join('');
}
function gotoBookmark(id){
  const bm=bookmarks.find(b=>b.id===id);if(!bm)return;
  const doc=library.find(d=>d.id===bm.docId);
  if(doc){loadDoc(doc);goPage('page-reader');setTimeout(()=>jumpToWord(bm.wordIdx),350);}
}
function delBookmark(id){
  bookmarks=bookmarks.filter(b=>b.id!==id);
  renderBookmarks();saveStorage();showToast('Bookmark removed');
}
function switchSavesTab(tab,el){
  document.querySelectorAll('.stab').forEach(t=>t.classList.remove('on'));
  el.classList.add('on');
  document.getElementById('savesBM').style.display=tab==='bm'?'block':'none';
  document.getElementById('savesNotes').style.display=tab==='notes'?'block':'none';
}

// ── NOTES ──
function saveNotes(){
  if(!currentDoc){showToast('Open a document first');return;}
  savedNotes[currentDoc.id]=document.getElementById('notesTA').value;
  saveStorage();showToast('💾 Notes saved!');
}

// ── VOICE RECORDING ──
async function toggleVoiceRec(){
  if(mediaRec&&mediaRec.state==='recording')stopVoiceRec();
  else await startVoiceRec();
}
async function startVoiceRec(){
  try{
    const stream=await navigator.mediaDevices.getUserMedia({audio:true});
    audioCtx=new AudioContext();
    analyser=audioCtx.createAnalyser();analyser.fftSize=64;
    audioCtx.createMediaStreamSource(stream).connect(analyser);
    recChunks=[];recBlob=null;recUrl=null;
    document.getElementById('postRecActions').style.display='none';
    let mime='';
    for(const c of['audio/mp4;codecs=mp4a.40.2','audio/mp4','audio/webm;codecs=opus','audio/webm']){
      if(MediaRecorder.isTypeSupported(c)){mime=c;break;}
    }
    mediaRec=new MediaRecorder(stream,mime?{mimeType:mime}:{});
    mediaRec.ondataavailable=e=>{if(e.data.size>0)recChunks.push(e.data);};
    mediaRec.onstop=()=>{
      recBlob=new Blob(recChunks,{type:mediaRec.mimeType||'audio/webm'});
      recUrl=URL.createObjectURL(recBlob);
      stream.getTracks().forEach(t=>t.stop());
      stopWaveAnim();
      document.getElementById('voiceRecBtn').classList.remove('rec');
      document.getElementById('voiceRecLabel').textContent='Record Again';
      document.getElementById('voiceRecStatus').textContent='✅ Done! Name and save below.';
      document.getElementById('postRecActions').style.display='block';
      document.getElementById('voiceNameIn').value='My Voice '+new Date().toLocaleTimeString();
    };
    mediaRec.start(100);
    document.getElementById('voiceRecBtn').classList.add('rec');
    document.getElementById('voiceRecLabel').textContent='Stop Recording';
    document.getElementById('voiceRecStatus').textContent='🔴 Recording…';
    animateWave();
    setTimeout(()=>{if(mediaRec?.state==='recording')stopVoiceRec();},60000);
  }catch(e){showToast('Microphone access denied.');}
}
function stopVoiceRec(){if(mediaRec&&mediaRec.state==='recording')mediaRec.stop();}
function previewRec(){if(recUrl)new Audio(recUrl).play().catch(()=>showToast('Cannot play'));}
async function analyzeAndSave(){
  if(!recBlob){showToast('No recording');return;}
  const name=document.getElementById('voiceNameIn').value.trim()||'My Voice';
  const btn=document.getElementById('analyzeBtn');
  btn.disabled=true;btn.innerHTML='<span class="spin" style="border-top-color:#000"></span> Analyzing…';
  let analysis='Warm, clear voice — excellent for focused reading sessions.';
  try{
    const sz=recChunks.reduce((a,c)=>a+c.size,0);
    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-6',max_tokens:180,
        messages:[{role:'user',content:`A user recorded their voice (${Math.round(sz/1000)}KB) for a text-to-speech reading app. In 1-2 sentences, describe their voice characteristics encouragingly and how it suits reading aloud.`}]})
    });
    const d=await res.json();
    analysis=d.content?.[0]?.text||analysis;
  }catch(e){}
  savedVoices.push({id:Date.now(),name,date:new Date().toLocaleDateString(),url:recUrl,analysis});
  saveVoiceStorage();renderVoiceList();
  btn.disabled=false;btn.innerHTML='✨ Analyze &amp; Save Voice';
  document.getElementById('postRecActions').style.display='none';
  document.getElementById('voiceRecStatus').textContent='Saved! Select below to use.';
  showToast('🎙️ Voice saved!');
}
function renderVoiceList(){
  const el=document.getElementById('voiceRecList');
  const lbl=document.getElementById('savedVLabel');
  const banner=document.getElementById('activeVBanner');
  if(!savedVoices.length){el.innerHTML='';lbl.style.display='none';banner.style.display='none';return;}
  lbl.style.display='block';
  banner.style.display=activeVoiceId?'block':'none';
  el.innerHTML=savedVoices.map(v=>`
    <div class="vr-item ${activeVoiceId===v.id?'active-v':''}">
      <div class="vr-body">
        <div class="vr-name">${esc(v.name)}${activeVoiceId===v.id?'<span class="ai-badge">ON</span>':''}</div>
        <div class="vr-date">${v.date}</div>
        ${v.analysis?`<div class="vr-analysis">${esc(v.analysis)}</div>`:''}
      </div>
      <div class="vr-acts">
        <button class="vr-play-btn" onclick="playVRec(${v.id})">▶</button>
        <button class="vr-del-btn" onclick="delVRec(${v.id})">🗑️</button>
      </div>
    </div>
    <button class="fbtn ${activeVoiceId===v.id?'ghost':'fire'}" onclick="selVoice(${v.id})" style="margin-bottom:10px;padding:11px">
      ${activeVoiceId===v.id?'✓ Selected':'Use This Voice'}
    </button>`).join('');
}
function playVRec(id){
  const v=savedVoices.find(r=>r.id===id);
  if(v?.url)new Audio(v.url).play().catch(()=>showToast('Cannot play recording'));
  else showToast('Re-record to preview — recordings reset on page reload');
}
function selVoice(id){
  activeVoiceId=activeVoiceId===id?null:id;
  renderVoiceList();
  showToast(activeVoiceId?'🎙️ Custom voice active':'Switched to AI voice');
}
function delVRec(id){
  savedVoices=savedVoices.filter(v=>v.id!==id);
  if(activeVoiceId===id)activeVoiceId=null;
  saveVoiceStorage();renderVoiceList();showToast('Deleted');
}
function animateWave(){
  const wf=document.getElementById('voiceWave');wf.innerHTML='';
  const cols=['#ff3a00','#ff6600','#ff9900','#ffcc00'];
  for(let i=0;i<26;i++){
    const b=document.createElement('div');
    b.className='wb';b.style.background=cols[i%4];wf.appendChild(b);
  }
  const data=new Uint8Array(analyser.frequencyBinCount);
  function draw(){
    if(!mediaRec||mediaRec.state!=='recording')return;
    analyser.getByteFrequencyData(data);
    wf.querySelectorAll('.wb').forEach((b,i)=>{
      b.style.height=Math.max(4,(data[i]||0)/255*46)+'px';
    });
    waveAnimId=requestAnimationFrame(draw);
  }
  draw();
}
function stopWaveAnim(){
  if(waveAnimId)cancelAnimationFrame(waveAnimId);
  const wf=document.getElementById('voiceWave');
  const h=[5,10,20,14,28,16,24,8,18,30,12,26,6,22,16,30,12,24,18,6,14,24,18,8,16,10];
  const c=['#ff3a00','#ff6600','#ff9900','#ffcc00'];
  wf.innerHTML=h.map((ht,i)=>`<div class="wb" style="height:${ht}px;opacity:.65;background:${c[i%4]}"></div>`).join('');
}

// ── LOCK SCREEN / BACKGROUND AUDIO ──
function initSilentAudio(){
  try{
    const ctx=new AudioContext();
    const buf=ctx.createBuffer(1,ctx.sampleRate,ctx.sampleRate);
    const src=ctx.createBufferSource();src.buffer=buf;src.loop=true;
    const dest=ctx.createMediaStreamDestination();src.connect(dest);src.start();
    silentAudio=new Audio();silentAudio.srcObject=dest.stream;
    silentAudio.loop=true;silentAudio.volume=0.001;
    ctx.close();
  }catch(e){silentAudio=null;}
}
function startSilentAudio(){if(silentAudio)silentAudio.play().catch(()=>{});}
function stopSilentAudio(){if(silentAudio){silentAudio.pause();silentAudio.currentTime=0;}}

function setupMediaSession(){
  if(!('mediaSession' in navigator))return;
  const title=currentDoc?currentDoc.name.replace(/\.[^.]+$/,''):'FireRead';
  navigator.mediaSession.metadata=new MediaMetadata({
    title,artist:'🔥 FireRead',album:'AI Reading',
    artwork:[{
      src:'data:image/svg+xml;charset=utf-8,'+encodeURIComponent(
        '<svg xmlns="http://www.w3.org/2000/svg" width="512" height="512">'+
        '<rect width="512" height="512" rx="96" fill="#090409"/>'+
        '<text x="256" y="360" font-size="300" text-anchor="middle">🔥</text></svg>'),
      sizes:'512x512',type:'image/svg+xml'
    }]
  });
  const h={
    play:()=>{if(!isPlaying)resumeOrStart();},
    pause:()=>{if(isPlaying)pauseSpeech();},
    stop:()=>stopSpeech(),
    previoustrack:()=>skipW(-30),
    nexttrack:()=>skipW(30),
    seekbackward:()=>skipW(-8),
    seekforward:()=>skipW(8),
  };
  Object.entries(h).forEach(([a,fn])=>{try{navigator.mediaSession.setActionHandler(a,fn);}catch(e){}});
}
function updateMediaSession(){
  if(!('mediaSession' in navigator))return;
  navigator.mediaSession.playbackState=isPlaying?'playing':'paused';
  if(words.length>0){
    try{
      const wpm=150*speechRate,total=(words.length/wpm)*60;
      const pos=Math.min((wordIdx/words.length)*total,total-.1);
      navigator.mediaSession.setPositionState({duration:total,playbackRate:speechRate,position:pos});
    }catch(e){}
  }
}
async function requestWakeLock(){
  try{if('wakeLock' in navigator)wakeLock=await navigator.wakeLock.request('screen');}catch(e){}
}
function releaseWakeLock(){try{if(wakeLock){wakeLock.release();wakeLock=null;}}catch(e){}}
document.addEventListener('visibilitychange',async()=>{
  if(document.visibilityState==='visible'&&isPlaying){
    await requestWakeLock();if(synth.paused)synth.resume();updateMediaSession();
  }
});

// ── AI ──
async function genSummary(){
  if(!currentDoc){showToast('Open a document first');return;}
  const btn=document.getElementById('summaryBtn');
  btn.disabled=true;btn.innerHTML='<span class="spin" style="border-top-color:#000"></span> Analyzing…';
  const box=document.getElementById('summaryResult');box.textContent='Generating…';
  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-6',max_tokens:700,
        messages:[{role:'user',content:`Summarize this study text in 4-5 bullet points using • symbols. Be clear and student-focused.\n\n${currentDoc.text.slice(0,5000)}`}]})
    });
    const d=await res.json();
    box.textContent=d.content?.[0]?.text||'Could not generate summary.';
  }catch(e){box.textContent='AI unavailable. Check internet connection.';}
  btn.disabled=false;btn.innerHTML='✨ Summarize Document';
}
async function askAI(){
  if(!currentDoc){showToast('Open a document first');return;}
  const q=document.getElementById('qaIn').value.trim();
  if(!q){showToast('Enter a question');return;}
  const box=document.getElementById('qaResult');box.textContent='🔥 Thinking…';
  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-6',max_tokens:500,
        messages:[{role:'user',content:`Based on this text, answer clearly and concisely.\n\nText:\n${currentDoc.text.slice(0,5000)}\n\nQuestion: ${q}`}]})
    });
    const d=await res.json();
    box.textContent=d.content?.[0]?.text||'No answer found.';
  }catch(e){box.textContent='AI unavailable. Try again.';}
}

// ── STORAGE ──
function saveStorage(){
  try{
    localStorage.setItem('fr_lib',JSON.stringify(library));
    localStorage.setItem('fr_bm',JSON.stringify(bookmarks));
    localStorage.setItem('fr_notes',JSON.stringify(savedNotes));
    if(currentDoc) localStorage.setItem('fr_last_id', currentDoc.id);
  }catch(e){}
}
function saveVoiceStorage(){
  try{
    localStorage.setItem('fr_voices',JSON.stringify(savedVoices.map(v=>({id:v.id,name:v.name,date:v.date,analysis:v.analysis}))));
  }catch(e){}
}
function loadStorage(){
  try{
    const l=localStorage.getItem('fr_lib');if(l){library=JSON.parse(l);renderLibrary();}
    const b=localStorage.getItem('fr_bm');if(b){bookmarks=JSON.parse(b);renderBookmarks();}
    const n=localStorage.getItem('fr_notes');if(n)savedNotes=JSON.parse(n);
    const v=localStorage.getItem('fr_voices');
    if(v){savedVoices=JSON.parse(v).map(m=>({...m,url:null}));renderVoiceList();}
    const lastId=localStorage.getItem('fr_last_id');
    if(lastId){
      const doc=library.find(d=>d.id==lastId);
      if(doc) loadDoc(doc);
    }
  }catch(e){}
}

// ── TOAST ──
function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),3000);
}

// ── KEYBOARD SHORTCUTS ──
document.addEventListener('keydown',e=>{
  if(['TEXTAREA','INPUT','SELECT'].includes(e.target.tagName))return;
  if(e.code==='Space'){e.preventDefault();togglePlay();}
  if(e.code==='ArrowRight')skipW(8);
  if(e.code==='ArrowLeft')skipW(-8);
  if(e.code==='KeyB')addBookmark();
});

// ── INIT ──
window.addEventListener('load',()=>{
  spawnEmbers();
  initQuotes();
  initPbTrack();
  initSilentAudio();
  loadVoices();
  synth.onvoiceschanged=loadVoices;
  // Retry voices after 1s (iOS Safari delay)
  setTimeout(()=>{if(!voices.length)loadVoices();},1000);
  loadStorage();
});
</script>
</body>
</html>
