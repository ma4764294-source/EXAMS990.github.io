<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>AHST Exams — AI Study Tool</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Noto+Sans+Arabic:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
:root{
  --bg:#0c0c0e;
  --bg2:#111114;
  --card:#16161a;
  --card2:#1c1c21;
  --border:#222228;
  --border2:#2a2a32;
  --g:#00c896;
  --g2:#00a87e;
  --g-dim:rgba(0,200,150,.12);
  --g-glow:rgba(0,200,150,.25);
  --text:#f2f2f4;
  --text2:#8888a0;
  --text3:#444455;
  --red:#ff4d6a;
  --red-dim:rgba(255,77,106,.1);
  --yellow:#f5c542;
  --yellow-dim:rgba(245,197,66,.1);
  --blue:#4f8ef7;
  --blue-dim:rgba(79,142,247,.1);
  --r:16px;
  --rs:12px;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html{-webkit-text-size-adjust:100%;scroll-behavior:smooth}
body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;-webkit-font-smoothing:antialiased}
html[lang="ar"] body{font-family:'Noto Sans Arabic','Inter',sans-serif}

/* ── LANG BAR ── */
#langbar{
  position:sticky;top:0;z-index:999;
  display:flex;align-items:center;justify-content:space-between;
  padding:12px 20px;
  background:rgba(12,12,14,.92);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border-bottom:1px solid var(--border);
}
.lb-brand{font-size:13px;font-weight:800;letter-spacing:.06em;color:var(--text);opacity:.7}
.lb-brand span{color:var(--g)}
.lb-switch{display:flex;background:var(--card);border:1px solid var(--border);border-radius:50px;padding:3px;gap:3px}
.lbtn{display:flex;align-items:center;gap:6px;padding:6px 14px;border-radius:50px;border:none;cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:12px;font-weight:600;color:var(--text2);background:transparent;transition:.2s;white-space:nowrap}
.lbtn .lflag{font-size:16px;line-height:1}
.lbtn:hover{color:var(--text)}
.lbtn.active{background:var(--g);color:#000;font-weight:700}

/* ── SCREENS ── */
.screen{display:none;width:100%;min-height:calc(100vh - 50px);flex-direction:column}
.screen.active{display:flex;animation:appear .4s ease both}
@keyframes appear{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}

/* ── WELCOME ── */
#welcome{
  background:
    radial-gradient(ellipse 80% 50% at 50% -10%,rgba(0,200,150,.08),transparent 60%),
    var(--bg);
  padding:0 20px 60px;align-items:center;
}
.w-wrap{width:100%;max-width:460px;margin:0 auto}
.w-top{padding:32px 0 0;display:flex;align-items:center;justify-content:space-between}
.w-logo{font-size:15px;font-weight:800;letter-spacing:.05em;color:var(--text)}
.w-logo span{color:var(--g)}

.w-hero{padding:40px 0 32px;text-align:center}
.w-eyebrow{display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:20px}
.w-eyebrow-line{width:24px;height:1px;background:var(--g);opacity:.6}
.w-eyebrow-txt{font-size:11px;font-weight:600;letter-spacing:.12em;text-transform:uppercase;color:var(--g);opacity:.8}
.w-h1{font-size:clamp(32px,9vw,52px);font-weight:800;line-height:1.04;letter-spacing:-.03em;color:var(--text);margin-bottom:16px}
.w-h1 em{font-style:normal;color:var(--g)}
.w-desc{font-size:clamp(13px,3.8vw,15px);color:var(--text2);line-height:1.75;font-weight:300;margin-bottom:36px}

/* Upload */
.upload-zone{
  width:100%;border:1px solid var(--border);border-radius:var(--r);
  padding:36px 20px;text-align:center;cursor:pointer;transition:.3s;
  position:relative;background:var(--card);
}
.upload-zone:hover,.upload-zone.drag{border-color:var(--g);background:var(--g-dim)}
.upload-zone input{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%}
.upload-icon-wrap{
  width:56px;height:56px;border-radius:14px;
  background:var(--g-dim);border:1px solid rgba(0,200,150,.2);
  display:flex;align-items:center;justify-content:center;
  margin:0 auto 16px;
}
.upload-icon-wrap svg{width:26px;height:26px;stroke:var(--g);stroke-width:1.5;fill:none}
.upload-title{font-size:15px;font-weight:700;color:var(--text);margin-bottom:6px}
.upload-hint{font-size:12px;color:var(--text3)}
.upload-hint strong{color:var(--text2);font-weight:500}

.w-cta{
  width:100%;margin-top:12px;
  background:var(--g);color:#000;
  font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:15px;font-weight:700;
  padding:16px;border-radius:var(--rs);border:none;cursor:pointer;
  display:flex;align-items:center;justify-content:center;gap:10px;
  transition:.2s;letter-spacing:-.01em;
}
.w-cta:hover{background:var(--g2);transform:translateY(-1px)}
.w-cta:active{transform:scale(.98)}
.w-cta svg{width:18px;height:18px;flex-shrink:0;stroke:#000;stroke-width:2;fill:none}

/* Info cards */
.w-info{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:24px}
.wi{background:var(--card);border:1px solid var(--border);border-radius:var(--rs);padding:16px}
.wi-icon{font-size:20px;margin-bottom:10px;display:block}
.wi-title{font-size:12px;font-weight:700;color:var(--text);margin-bottom:4px}
.wi-desc{font-size:11px;color:var(--text3);line-height:1.6}

/* ── PROCESSING ── */
#processing{background:var(--bg);align-items:center;justify-content:center}
.proc-wrap{width:100%;max-width:400px;padding:40px 20px;text-align:center}
.proc-circle{
  width:88px;height:88px;margin:0 auto 32px;position:relative
}
.proc-svg{width:88px;height:88px;transform:rotate(-90deg)}
.proc-track{fill:none;stroke:var(--border);stroke-width:3}
.proc-arc{fill:none;stroke:var(--g);stroke-width:3;stroke-linecap:round;stroke-dasharray:251;stroke-dashoffset:251;transition:stroke-dashoffset .6s ease}
.proc-inner{position:absolute;inset:0;display:flex;align-items:center;justify-content:center}
.proc-emoji{font-size:28px}
.proc-h{font-size:20px;font-weight:700;color:var(--text);margin-bottom:8px;letter-spacing:-.02em}
.proc-sub{font-size:13px;color:var(--text3);margin-bottom:32px;line-height:1.6}
.proc-steps{display:flex;flex-direction:column;gap:8px;text-align:left}
html[dir="rtl"] .proc-steps{text-align:right}
.pstep{display:flex;align-items:center;gap:12px;padding:12px 16px;border-radius:var(--rs);background:var(--card);border:1px solid var(--border);transition:.35s}
.pstep.active{border-color:var(--g);background:var(--g-dim)}
.pstep.done{border-color:rgba(0,200,150,.3);background:rgba(0,200,150,.06)}
.pstep-num{width:28px;height:28px;border-radius:50%;background:var(--bg);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700;color:var(--text3);flex-shrink:0;transition:.35s}
.pstep.active .pstep-num{background:var(--g);border-color:var(--g);color:#000;box-shadow:0 0 16px var(--g-glow)}
.pstep.done .pstep-num{background:var(--g);border-color:var(--g);color:#000}
.pstep-lbl{font-size:12px;font-weight:600;color:var(--text3);transition:.3s}
.pstep.active .pstep-lbl{color:var(--g)}
.pstep.done .pstep-lbl{color:var(--text2)}
.proc-err{display:none;margin-top:20px;background:var(--red-dim);border:1px solid rgba(255,77,106,.25);border-radius:var(--rs);padding:16px;font-size:13px;color:var(--red);line-height:1.6}
.proc-err.show{display:block}
.proc-err-retry{margin-top:10px;background:var(--red);color:#fff;border:none;border-radius:8px;padding:8px 20px;font-family:inherit;font-size:12px;font-weight:700;cursor:pointer}

/* ── HOME ── */
#home{background:var(--bg);padding:0 20px 60px}
.home-wrap{width:100%;max-width:460px;margin:0 auto}
.home-top{padding:28px 0 20px;display:flex;align-items:center;justify-content:space-between}
.home-logo{font-size:14px;font-weight:800;letter-spacing:.05em;color:var(--text)}
.home-logo span{color:var(--g)}
.new-btn{display:flex;align-items:center;gap:6px;background:transparent;border:1px solid var(--border);border-radius:50px;padding:8px 16px;font-size:11px;font-weight:600;color:var(--text2);cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;transition:.2s}
.new-btn:hover{border-color:var(--g);color:var(--g)}

.file-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:18px;display:flex;align-items:center;gap:14px;margin-bottom:24px;position:relative;overflow:hidden}
.file-card::before{content:'';position:absolute;top:0;left:0;bottom:0;width:2px;background:var(--g)}
html[dir="rtl"] .file-card::before{left:auto;right:0}
.file-ico{width:44px;height:44px;background:var(--g-dim);border:1px solid rgba(0,200,150,.2);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0}
.file-info{flex:1;min-width:0}
.file-name{font-size:14px;font-weight:700;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.file-status{font-size:11px;color:var(--g);margin-top:3px;font-weight:600;display:flex;align-items:center;gap:5px}
.file-dot{width:5px;height:5px;border-radius:50%;background:var(--g);display:inline-block;animation:blink 2s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}

.home-section{font-size:11px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.1em;margin-bottom:12px}

.action-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:24px}
.ac{
  background:var(--card);border:1px solid var(--border);border-radius:var(--r);
  padding:20px 16px;cursor:pointer;transition:.2s;position:relative;overflow:hidden;
}
.ac:hover{border-color:var(--g);background:rgba(0,200,150,.04);transform:translateY(-1px)}
.ac:active{transform:scale(.98)}
.ac::after{content:'';position:absolute;top:0;right:0;width:60px;height:60px;border-radius:0 var(--r) 0 100%;background:var(--g);opacity:.04}
html[dir="rtl"] .ac::after{right:auto;left:0;border-radius:var(--r) 0 100% 0}
.ac-icon{font-size:26px;margin-bottom:12px;display:block}
.ac-title{font-size:15px;font-weight:700;color:var(--text);margin-bottom:6px}
.ac-desc{font-size:11px;color:var(--text3);line-height:1.6}
.ac-arrow{position:absolute;bottom:14px;right:14px;color:var(--text3);font-size:16px;transition:.2s}
html[dir="rtl"] .ac-arrow{right:auto;left:14px}
.ac:hover .ac-arrow{color:var(--g);transform:translateX(3px)}
html[dir="rtl"] .ac:hover .ac-arrow{transform:translateX(-3px)}

.stats-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
.stat{background:var(--card);border:1px solid var(--border);border-radius:var(--rs);padding:14px 10px;text-align:center}
.stat-n{font-size:22px;font-weight:800;color:var(--g);letter-spacing:-.02em}
.stat-l{font-size:10px;color:var(--text3);margin-top:3px;font-weight:500;text-transform:uppercase;letter-spacing:.06em}

/* ── QUIZ ── */
#quiz{background:var(--bg)}
.quiz-bar{
  background:rgba(12,12,14,.96);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border-bottom:1px solid var(--border);
  padding:12px 16px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;
  position:sticky;top:50px;z-index:90;
}
.qb-left{flex:1;min-width:0}
.qb-title{font-size:13px;font-weight:700;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.qb-sub{font-size:10px;color:var(--text3);margin-top:2px}
.timer-wrap{display:flex;align-items:center;gap:7px;background:var(--card);border:1px solid var(--border);border-radius:50px;padding:7px 14px;flex-shrink:0;transition:.3s}
.timer-wrap svg{width:12px;height:12px;flex-shrink:0;stroke:currentColor;stroke-width:2;fill:none}
.timer-txt{font-size:16px;font-weight:800;min-width:44px;text-align:center;letter-spacing:.04em}
.timer-wrap.warn{border-color:var(--yellow);color:var(--yellow)}
.timer-wrap.hot{border-color:var(--red);color:var(--red);animation:shk .5s}
@keyframes shk{0%,100%{transform:none}25%{transform:translateX(-4px)}75%{transform:translateX(4px)}}
.score-wrap{display:flex;align-items:center;gap:5px;background:var(--card);border:1px solid var(--border);border-radius:50px;padding:7px 14px;font-size:12px;font-weight:700;flex-shrink:0}
.sok{color:var(--g)}.sbad{color:var(--red)}

.q-prog{width:100%;height:2px;background:var(--border)}
.q-progf{height:100%;background:var(--g);transition:width .5s ease;border-radius:2px;box-shadow:0 0 8px var(--g-glow)}

.quiz-body{padding:16px;flex:1}

.qnav-wrap{overflow-x:auto;margin-bottom:16px;padding-bottom:4px}
.qnav-wrap::-webkit-scrollbar{height:2px}
.qnav-wrap::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}
.qnav-inner{display:flex;gap:6px;width:max-content}
.qdot{width:30px;height:30px;border-radius:50%;border:1px solid var(--border);background:var(--card);display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;cursor:pointer;color:var(--text3);transition:.15s;flex-shrink:0;position:relative}
.qdot:hover{border-color:var(--g);color:var(--g)}
.qdot.cur{background:var(--g);border-color:var(--g);color:#000;box-shadow:0 0 12px var(--g-glow)}
.qdot.ans{background:var(--g-dim);border-color:rgba(0,200,150,.4);color:var(--g)}
.qdot.ok{background:rgba(0,200,150,.2);border-color:var(--g);color:var(--g)}
.qdot.bad{background:var(--red-dim);border-color:var(--red);color:var(--red)}
.qdot.fl::after{content:'';position:absolute;top:-2px;right:-2px;width:7px;height:7px;border-radius:50%;background:var(--yellow);border:1px solid var(--bg)}

/* Question card */
.q-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:20px;margin-bottom:12px;animation:appear .25s ease;position:relative;overflow:hidden}
.q-card::before{content:'';position:absolute;top:0;left:0;right:0;height:1px;background:linear-gradient(90deg,var(--g),transparent 60%)}
.q-meta{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;gap:8px}
.q-type{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.08em;color:var(--text3);background:var(--bg);border:1px solid var(--border);border-radius:50px;padding:4px 10px;flex-shrink:0}
.q-meta-right{display:flex;align-items:center;gap:8px}
.flag-btn{width:30px;height:30px;border-radius:50%;border:1px solid var(--border);background:transparent;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:12px;transition:.2s;flex-shrink:0}
.flag-btn:hover,.flag-btn.on{background:var(--yellow-dim);border-color:var(--yellow)}
.diff{font-size:9px;font-weight:700;padding:3px 8px;border-radius:50px;text-transform:uppercase;letter-spacing:.06em;flex-shrink:0}
.d-e{color:var(--g);border:1px solid rgba(0,200,150,.25)}
.d-m{color:var(--yellow);border:1px solid rgba(245,197,66,.25)}
.d-h{color:var(--red);border:1px solid rgba(255,77,106,.25)}
.q-num{font-size:10px;color:var(--text3);font-weight:600;margin-bottom:8px;letter-spacing:.04em;text-transform:uppercase}
.q-text{font-size:clamp(14px,4vw,16px);font-weight:600;color:var(--text);line-height:1.6;margin-bottom:18px;letter-spacing:-.01em}

/* Options */
.opts{display:flex;flex-direction:column;gap:8px}
.opt{display:flex;align-items:center;gap:12px;background:var(--bg);border:1px solid var(--border);border-radius:var(--rs);padding:13px 15px;cursor:pointer;transition:.2s;user-select:none}
.opt:hover{border-color:rgba(0,200,150,.4);background:var(--g-dim)}
.opt.sel{border-color:rgba(0,200,150,.5);background:var(--g-dim)}
.opt.ok{border-color:var(--g);background:rgba(0,200,150,.1);animation:okpop .3s ease}
.opt.bad{border-color:var(--red);background:var(--red-dim);animation:shk .3s ease}
@keyframes okpop{0%{transform:scale(1)}50%{transform:scale(1.01)}100%{transform:scale(1)}}
.opt-l{width:30px;height:30px;border-radius:50%;background:var(--card);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:800;flex-shrink:0;transition:.2s;color:var(--text3)}
.opt.sel .opt-l{background:var(--g);border-color:var(--g);color:#000}
.opt.ok  .opt-l{background:var(--g);border-color:var(--g);color:#000}
.opt.bad .opt-l{background:var(--red);border-color:var(--red);color:#fff}
.opt-txt{font-size:13px;color:var(--text);flex:1;line-height:1.45;letter-spacing:-.01em}
.opt-ic{margin-left:auto;font-size:15px;flex-shrink:0}
html[dir="rtl"] .opt-ic{margin-left:0;margin-right:auto}

/* TF */
.tf-row{display:flex;gap:10px}
.tf-btn{flex:1;padding:18px 12px;border-radius:var(--rs);border:1px solid var(--border);background:var(--bg);cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:14px;font-weight:800;display:flex;flex-direction:column;align-items:center;gap:8px;transition:.2s;color:var(--text3);letter-spacing:-.01em}
.tf-btn:hover{border-color:rgba(0,200,150,.4);background:var(--g-dim);color:var(--g)}
.tf-btn.sel{border-color:rgba(0,200,150,.5);background:var(--g-dim);color:var(--g)}
.tf-btn.ok{border-color:var(--g);background:rgba(0,200,150,.1);color:var(--g);animation:okpop .3s ease}
.tf-btn.bad{border-color:var(--red);background:var(--red-dim);color:var(--red);animation:shk .3s ease}
.tf-em{font-size:26px}

/* Feedback */
.q-fb{display:none;margin-top:12px;border-radius:var(--rs);padding:14px;font-size:12px;line-height:1.7;border-left:2px solid}
html[dir="rtl"] .q-fb{border-left:none;border-right:2px solid}
.q-fb.show{display:block;animation:appear .25s ease}
.q-fb.ok{background:rgba(0,200,150,.07);border-color:var(--g);color:rgba(0,200,150,.9)}
.q-fb.bad{background:var(--red-dim);border-color:var(--red);color:rgba(255,130,145,.9)}
.q-fb-lbl{font-weight:700;font-size:10px;text-transform:uppercase;letter-spacing:.08em;margin-bottom:5px;opacity:.8}

/* Hint */
.q-hint{display:none;margin-top:10px;background:var(--yellow-dim);border:1px solid rgba(245,197,66,.2);border-radius:var(--rs);padding:12px 14px;font-size:12px;color:rgba(245,197,66,.85);line-height:1.65}
.q-hint.show{display:block;animation:appear .25s ease}
.q-hint-lbl{font-weight:700;font-size:10px;text-transform:uppercase;letter-spacing:.08em;margin-bottom:4px;color:var(--yellow)}

/* Bottom bar */
.q-bar{
  background:rgba(12,12,14,.96);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border-top:1px solid var(--border);padding:12px 16px;
  display:flex;gap:8px;position:sticky;bottom:0;z-index:80;
}
.qb-btn{flex:1;display:flex;align-items:center;justify-content:center;gap:6px;padding:13px;border-radius:var(--rs);border:none;cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:12px;font-weight:700;transition:.2s;letter-spacing:-.01em}
.qb-prev{background:var(--card);color:var(--text2);border:1px solid var(--border)}.qb-prev:hover{border-color:var(--g);color:var(--g)}
.qb-hint{background:var(--yellow-dim);color:var(--yellow);border:1px solid rgba(245,197,66,.2)}.qb-hint:hover{background:rgba(245,197,66,.18)}
.qb-next{background:var(--g);color:#000;font-weight:800}.qb-next:hover{background:var(--g2)}
.qb-sub{background:var(--g);color:#000;font-weight:800}.qb-sub:hover{background:var(--g2)}
.qb-btn:active{transform:scale(.96)}.qb-btn:disabled{opacity:.3;cursor:not-allowed;transform:none}
.hidden{display:none!important}

/* ── RESULTS ── */
#results{background:var(--bg);padding:0 20px 60px}
.res-wrap{width:100%;max-width:460px;margin:0 auto}
.res-back{padding:24px 0 16px}
.back-btn{display:inline-flex;align-items:center;gap:8px;background:var(--card);border:1px solid var(--border);border-radius:50px;padding:9px 18px;font-size:12px;font-weight:600;color:var(--text2);cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;transition:.2s}
.back-btn:hover{border-color:var(--g);color:var(--g)}
.back-btn svg{width:13px;height:13px;flex-shrink:0;stroke:currentColor;stroke-width:2.5;fill:none}
html[dir="rtl"] .back-btn svg{transform:scaleX(-1)}

.score-hero{
  background:var(--card);border:1px solid var(--border);border-radius:var(--r);
  padding:32px 20px;text-align:center;margin-bottom:16px;position:relative;overflow:hidden;
}
.score-hero::before{content:'';position:absolute;top:0;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,var(--g),transparent)}
.ring-wrap{position:relative;width:clamp(120px,38vw,150px);height:clamp(120px,38vw,150px);margin:0 auto 20px}
.ring-svg{width:100%;height:100%;transform:rotate(-90deg)}
.ring-bg{fill:none;stroke:var(--border);stroke-width:7}
.ring-fill{fill:none;stroke-width:7;stroke-linecap:round;stroke:var(--g);transition:stroke-dashoffset 1.8s cubic-bezier(.4,0,.2,1);filter:drop-shadow(0 0 8px var(--g-glow))}
.ring-inner{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center}
.ring-pct{font-size:clamp(28px,9vw,38px);font-weight:800;line-height:1;letter-spacing:-.03em;color:var(--g)}
.ring-lbl{font-size:9px;color:var(--text3);letter-spacing:.1em;text-transform:uppercase;margin-top:3px}
.score-grade{font-size:22px;font-weight:800;color:var(--text);letter-spacing:-.02em;margin-bottom:6px}
.score-msg{font-size:13px;color:var(--text2);line-height:1.5}

.score-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin-bottom:16px}
.sg{background:var(--card);border:1px solid var(--border);border-radius:var(--rs);padding:16px;text-align:center}
.sg-val{font-size:clamp(22px,7vw,28px);font-weight:800;margin-bottom:4px;letter-spacing:-.02em}
.sg-lbl{font-size:10px;color:var(--text3);font-weight:500;text-transform:uppercase;letter-spacing:.06em}
.sv-ok{color:var(--g)}.sv-bad{color:var(--red)}.sv-time{color:var(--blue)}.sv-skip{color:var(--yellow)}

.perf-card{background:var(--card);border:1px solid var(--border);border-radius:var(--rs);padding:18px;margin-bottom:16px}
.perf-hd{font-size:11px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:.08em;margin-bottom:14px;display:flex;align-items:center;gap:8px}
.perf-hd::before{content:'';display:block;width:2px;height:12px;background:var(--g);border-radius:2px;flex-shrink:0}
html[dir="rtl"] .perf-hd::before{order:1}
.pbar{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.pbar:last-child{margin-bottom:0}
.pb-lbl{font-size:11px;color:var(--text3);width:24%;flex-shrink:0;font-weight:500}
html[dir="rtl"] .pb-lbl{text-align:right}
.pb-track{flex:1;height:6px;background:var(--bg);border-radius:3px;overflow:hidden}
.pb-fill{height:100%;border-radius:3px;transition:width 1.3s ease .3s;min-width:3px}
.pb-pct{font-size:10px;color:var(--text3);font-weight:700;width:26px;text-align:right;flex-shrink:0}
html[dir="rtl"] .pb-pct{text-align:left}

.rev-hd{font-size:11px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:.08em;margin-bottom:12px;display:flex;align-items:center;gap:8px}
.rev-hd::before{content:'';display:block;width:2px;height:12px;background:var(--g);border-radius:2px;flex-shrink:0}
html[dir="rtl"] .rev-hd::before{order:1}
.rev-item{background:var(--card);border-radius:var(--rs);padding:14px;margin-bottom:8px;border-left:2px solid var(--border);animation:appear .35s ease both}
html[dir="rtl"] .rev-item{border-left:none;border-right:2px solid var(--border)}
.ri-ok{border-color:var(--g)}.ri-bad{border-color:var(--red)}.ri-skip{border-color:var(--yellow)}
.rv-q{font-size:12px;font-weight:600;color:var(--text);margin-bottom:8px;line-height:1.55}
.rv-a{font-size:11px;margin-top:5px;line-height:1.4;font-weight:500}
.a-ok{color:var(--g)}.a-bad{color:var(--red)}.a-cor{color:var(--g);font-weight:700}

.res-acts{display:flex;gap:8px;margin-top:20px}
.ra{flex:1;padding:15px;border-radius:var(--rs);border:none;cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:13px;font-weight:700;transition:.2s;letter-spacing:-.01em}
.ra-p{background:var(--g);color:#000}.ra-p:hover{background:var(--g2)}
.ra-o{background:var(--card);color:var(--text2);border:1px solid var(--border)}.ra-o:hover{border-color:var(--g);color:var(--g)}
.ra:active{transform:scale(.97)}

/* ── SUMMARY ── */
#summary{background:var(--bg);padding:0 20px 60px}
.sum-wrap{width:100%;max-width:460px;margin:0 auto}
.sum-top{padding:24px 0 20px;display:flex;align-items:center;gap:12px}
.sum-title-area{flex:1;min-width:0}
.sum-title{font-size:18px;font-weight:800;color:var(--text);letter-spacing:-.02em}
.sum-file{font-size:11px;color:var(--text3);margin-top:3px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}

.sum-lang{display:flex;background:var(--card);border:1px solid var(--border);border-radius:50px;padding:3px;gap:3px;margin-bottom:20px;width:fit-content}
.slb{flex:1;padding:7px 20px;border-radius:50px;border:none;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:12px;font-weight:700;cursor:pointer;transition:.2s;display:flex;align-items:center;justify-content:center;gap:6px;color:var(--text3);background:transparent;white-space:nowrap}
.slb.on{background:var(--g);color:#000}
.slb:not(.on):hover{color:var(--text)}

.sum-box{
  background:var(--card);border:1px solid var(--border);border-radius:var(--r);
  padding:24px;margin-bottom:16px;min-height:200px;
}
.sum-loading{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px;gap:12px}
.sum-spin{width:32px;height:32px;border:2px solid var(--border);border-top-color:var(--g);border-radius:50%;animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.sum-spin-lbl{font-size:12px;color:var(--text3)}
.sum-txt{font-size:14px;color:var(--text2);line-height:1.9;white-space:pre-wrap;letter-spacing:-.01em}
.sum-txt-ar{font-family:'Noto Sans Arabic',sans-serif;direction:rtl;text-align:right;font-size:15px;line-height:2.1}

.dl-row{display:flex;gap:8px}
.dl-btn{flex:1;display:flex;align-items:center;justify-content:center;gap:8px;padding:15px;border-radius:var(--rs);border:none;cursor:pointer;font-family:'Inter','Noto Sans Arabic',sans-serif;font-size:12px;font-weight:700;transition:.2s;letter-spacing:-.01em}
.dl-en{background:var(--g);color:#000}.dl-en:hover{background:var(--g2)}
.dl-ar{background:var(--card);color:var(--text2);border:1px solid var(--border)}.dl-ar:hover{border-color:var(--g);color:var(--g)}
.dl-btn:active{transform:scale(.97)}



</style>
</head>
<body>

<!-- LANG BAR -->
<div id="langbar">
  <div class="lb-brand">AH<span>ST</span></div>
  <div class="lb-switch">
    <button class="lbtn active" id="btnEN" onclick="setLang('en')"><span class="lflag">🇺🇸</span><span data-en="EN" data-ar="EN">EN</span></button>
    <button class="lbtn" id="btnAR" onclick="setLang('ar')"><span class="lflag">🇸🇦</span><span data-en="AR" data-ar="AR">AR</span></button>
  </div>
</div>

<!-- WELCOME -->
<div id="welcome" class="screen active">
<div class="w-wrap">
  <div class="w-top">
    <div class="w-logo">AH<span>ST</span></div>
  </div>
  <div class="w-hero">
    <div class="w-eyebrow"><div class="w-eyebrow-line"></div><div class="w-eyebrow-txt" data-en="Study Intelligence" data-ar="ذكاء الدراسة">Study Intelligence</div><div class="w-eyebrow-line"></div></div>
    <h1 class="w-h1"><span data-en="Upload PDF." data-ar="ارفع الـ PDF.">Upload PDF.</span><br><em data-en="Master the content." data-ar="أتقن المحتوى.">Master the content.</em></h1>
    <p class="w-desc" data-en="Upload any study PDF and get 50 exam questions generated from the actual content — plus a full summary in English and Arabic, ready to download."
       data-ar="ارفع أي PDF دراسي واحصل على 50 سؤال من المحتوى الفعلي مع ملخص كامل بالإنجليزية والعربية جاهز للتنزيل.">
      Upload any study PDF and get 50 exam questions generated from the actual content — plus a full summary in English and Arabic, ready to download.
    </p>
    <div class="upload-zone" id="uploadZone">
      <input type="file" id="fileInput" accept=".pdf" onchange="handleFile(event)">
      <div class="upload-icon-wrap">
        <svg viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M9 13h6m-3-3v6m5 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>
      </div>
      <div class="upload-title" data-en="Drop your PDF here" data-ar="اسحب الـ PDF هنا">Drop your PDF here</div>
      <div class="upload-hint"><strong data-en="or tap to browse" data-ar="أو اضغط للاختيار">or tap to browse</strong> &nbsp;·&nbsp; PDF files only</div>
    </div>
    <button class="w-cta" onclick="document.getElementById('fileInput').click()">
      <svg viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"/></svg>
      <span data-en="Select PDF to begin" data-ar="اختر الـ PDF للبدء">Select PDF to begin</span>
    </button>
  </div>
  <div class="w-info">
    <div class="wi"><span class="wi-icon">🎯</span><div class="wi-title" data-en="Real Content Quiz" data-ar="اختبار من المحتوى">Real Content Quiz</div><div class="wi-desc" data-en="50 AI questions generated directly from your PDF — not generic questions" data-ar="50 سؤال من محتوى الـ PDF الفعلي وليس أسئلة عامة">50 AI questions generated directly from your PDF — not generic questions</div></div>
    <div class="wi"><span class="wi-icon">📋</span><div class="wi-title" data-en="Full Summary" data-ar="ملخص كامل">Full Summary</div><div class="wi-desc" data-en="Comprehensive summary of key concepts, downloadable as PDF in EN or AR" data-ar="ملخص شامل للمفاهيم الأساسية قابل للتنزيل بالعربية أو الإنجليزية">Comprehensive summary of key concepts, downloadable as PDF in EN or AR</div></div>
    <div class="wi"><span class="wi-icon">💡</span><div class="wi-title" data-en="Hints & Explanations" data-ar="تلميحات وشروح">Hints & Explanations</div><div class="wi-desc" data-en="Every question has a hint and a full explanation of the correct answer" data-ar="كل سؤال يحتوي على تلميح وشرح كامل للإجابة الصحيحة">Every question has a hint and a full explanation of the correct answer</div></div>
    <div class="wi"><span class="wi-icon">📊</span><div class="wi-title" data-en="Performance Analysis" data-ar="تحليل الأداء">Performance Analysis</div><div class="wi-desc" data-en="Detailed breakdown by question type and difficulty after every quiz" data-ar="تحليل تفصيلي حسب نوع السؤال والصعوبة بعد كل اختبار">Detailed breakdown by question type and difficulty after every quiz</div></div>
  </div>
</div>
</div>

<!-- PROCESSING -->
<div id="processing" class="screen">
<div class="proc-wrap">
  <div class="proc-circle">
    <svg class="proc-svg" viewBox="0 0 88 88">
      <circle class="proc-track" cx="44" cy="44" r="40"/>
      <circle class="proc-arc" id="procArc" cx="44" cy="44" r="40"/>
    </svg>
    <div class="proc-inner"><div class="proc-emoji" id="procEmoji">📄</div></div>
  </div>
  <div class="proc-h" id="procH" data-en="Analyzing your PDF…" data-ar="جاري تحليل الـ PDF…">Analyzing your PDF…</div>
  <div class="proc-sub" id="procSub" data-en="This usually takes 20–40 seconds. All three tasks run in parallel." data-ar="يستغرق هذا عادةً 20-40 ثانية. جميع المهام تعمل في وقت واحد.">This usually takes 20–40 seconds. All three tasks run in parallel.</div>
  <div class="proc-steps">
    <div class="pstep" id="ps1"><div class="pstep-num" id="pn1">1</div><div class="pstep-lbl" data-en="Extracting text from PDF" data-ar="استخراج النص من الـ PDF">Extracting text from PDF</div></div>
    <div class="pstep" id="ps2"><div class="pstep-num" id="pn2">2</div><div class="pstep-lbl" data-en="Generating 50 exam questions" data-ar="توليد 50 سؤال">Generating 50 exam questions</div></div>
    <div class="pstep" id="ps3"><div class="pstep-num" id="pn3">3</div><div class="pstep-lbl" data-en="Creating summaries (EN + AR in parallel)" data-ar="إنشاء الملخصات (عربي وإنجليزي في نفس الوقت)">Creating summaries (EN + AR in parallel)</div></div>
    <div class="pstep" id="ps4"><div class="pstep-num" id="pn4">4</div><div class="pstep-lbl" data-en="Finalizing" data-ar="الانتهاء">Finalizing</div></div>
  </div>
  <div class="proc-err" id="procErr">
    <strong>Error:</strong><br><span id="procErrMsg" style="font-size:11px;opacity:.85;white-space:pre-wrap"></span>
    <br><button class="proc-err-retry" onclick="retryProcess()" data-en="Try Again" data-ar="حاول مجدداً">Try Again</button>
  </div>
</div>
</div>

<!-- HOME -->
<div id="home" class="screen">
<div class="home-wrap">
  <div class="home-top">
    <div class="home-logo">AH<span>ST</span></div>
    <button class="new-btn" onclick="goWelcome()">
      <svg width="12" height="12" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
      <span data-en="New PDF" data-ar="PDF جديد">New PDF</span>
    </button>
  </div>
  <div class="file-card">
    <div class="file-ico">📄</div>
    <div class="file-info">
      <div class="file-name" id="homeFName">document.pdf</div>
      <div class="file-status"><span class="file-dot"></span><span data-en="Ready" data-ar="جاهز">Ready</span></div>
    </div>
  </div>
  <div class="home-section" data-en="CHOOSE AN ACTION" data-ar="اختر إجراءً">CHOOSE AN ACTION</div>
  <div class="action-grid">
    <div class="ac" onclick="startQuiz()">
      <span class="ac-icon">🎯</span>
      <div class="ac-title" data-en="Take Quiz" data-ar="ابدأ الاختبار">Take Quiz</div>
      <div class="ac-desc" data-en="50 questions from your PDF with hints, difficulty levels, and full explanations" data-ar="50 سؤال من الـ PDF مع تلميحات ومستويات صعوبة وشروح كاملة">50 questions from your PDF with hints, difficulty levels, and full explanations</div>
      <div class="ac-arrow">→</div>
    </div>
    <div class="ac" onclick="goSummary()">
      <span class="ac-icon">📋</span>
      <div class="ac-title" data-en="Summary" data-ar="الملخص">Summary</div>
      <div class="ac-desc" data-en="Read the full study summary and download as PDF in English or Arabic" data-ar="اقرأ الملخص الكامل وحمّله كـ PDF بالإنجليزية أو العربية">Read the full study summary and download as PDF in English or Arabic</div>
      <div class="ac-arrow">→</div>
    </div>
  </div>
  <div class="stats-strip">
    <div class="stat"><div class="stat-n">50</div><div class="stat-l" data-en="Questions" data-ar="سؤال">Questions</div></div>
    <div class="stat"><div class="stat-n">60</div><div class="stat-l" data-en="Minutes" data-ar="دقيقة">Minutes</div></div>
    <div class="stat"><div class="stat-n">∞</div><div class="stat-l" data-en="Retakes" data-ar="إعادة">Retakes</div></div>
  </div>
</div>
</div>

<!-- QUIZ -->
<div id="quiz" class="screen">
  <div class="quiz-bar">
    <div class="qb-left">
      <div class="qb-title" id="quizTitle">Quiz</div>
      <div class="qb-sub" id="quizSub">30 MCQ · 20 T/F · 60 min</div>
    </div>
    <div class="timer-wrap" id="timerWrap">
      <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      <span class="timer-txt" id="timerTxt">60:00</span>
    </div>
    <div class="score-wrap"><span class="sok" id="liveOk">0</span><span style="color:var(--border);margin:0 3px">/</span><span class="sbad" id="liveBad">0</span></div>
  </div>
  <div class="q-prog"><div class="q-progf" id="qProgF" style="width:2%"></div></div>
  <div class="quiz-body">
    <div class="qnav-wrap"><div class="qnav-inner" id="qnavInner"></div></div>
    <div id="qArea"></div>
  </div>
  <div class="q-bar">
    <button class="qb-btn qb-prev" id="qPrev" onclick="prevQ()" disabled>
      <svg width="13" height="13" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"/></svg>
      <span data-en="Prev" data-ar="السابق">Prev</span>
    </button>
    <button class="qb-btn qb-hint" id="qHint" onclick="showHint()" data-en="💡 Hint" data-ar="💡 تلميح">💡 Hint</button>
    <button class="qb-btn qb-next" id="qNext" onclick="nextQ()">
      <span data-en="Next" data-ar="التالي">Next</span>
      <svg width="13" height="13" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><polyline points="9 6 15 12 9 18"/></svg>
    </button>
    <button class="qb-btn qb-sub hidden" id="qSub" onclick="submitQuiz()" data-en="✓ Submit" data-ar="✓ إنهاء">✓ Submit</button>
  </div>
</div>

<!-- RESULTS -->
<div id="results" class="screen">
<div class="res-wrap">
  <div class="res-back"><button class="back-btn" onclick="show('home')"><svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"/></svg><span data-en="Home" data-ar="الرئيسية">Home</span></button></div>
  <div class="score-hero">
    <div class="ring-wrap">
      <svg class="ring-svg" viewBox="0 0 120 120">
        <circle class="ring-bg" cx="60" cy="60" r="52"/>
        <circle class="ring-fill" id="ringFill" cx="60" cy="60" r="52" stroke-dasharray="326.7" stroke-dashoffset="326.7"/>
      </svg>
      <div class="ring-inner"><div class="ring-pct" id="ringPct">0%</div><div class="ring-lbl" data-en="SCORE" data-ar="النتيجة">SCORE</div></div>
    </div>
    <div class="score-grade" id="scoreGrade"></div>
    <div class="score-msg" id="scoreMsg"></div>
  </div>
  <div class="score-grid" id="scoreGrid"></div>
  <div class="perf-card" id="perfCard"></div>
  <div class="rev-hd"><span id="revHdTxt">ANSWER REVIEW</span></div>
  <div id="revList"></div>
  <div class="res-acts">
    <button class="ra ra-p" onclick="retakeQuiz()" data-en="↺ Retake Quiz" data-ar="↺ إعادة الاختبار">↺ Retake Quiz</button>
    <button class="ra ra-o" onclick="show('home')" data-en="Home" data-ar="الرئيسية">Home</button>
  </div>
</div>
</div>

<!-- SUMMARY -->
<div id="summary" class="screen">
<div class="sum-wrap">
  <div class="sum-top">
    <button class="back-btn" onclick="show('home')"><svg viewBox="0 0 24 24" width="13" height="13" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="15 18 9 12 15 6"/></svg><span data-en="Back" data-ar="رجوع">Back</span></button>
    <div class="sum-title-area">
      <div class="sum-title" data-en="Study Summary" data-ar="ملخص الدراسة">Study Summary</div>
      <div class="sum-file" id="sumFileLbl"></div>
    </div>
  </div>
  <div class="sum-lang">
    <button class="slb on" id="slbEN" onclick="switchSumLang('en')">🇺🇸 <span data-en="English" data-ar="إنجليزي">English</span></button>
    <button class="slb" id="slbAR" onclick="switchSumLang('ar')">🇸🇦 <span data-en="Arabic" data-ar="عربي">Arabic</span></button>
  </div>
  <div class="sum-box" id="sumBox">
    <div class="sum-loading"><div class="sum-spin"></div><div class="sum-spin-lbl" data-en="Loading summary…" data-ar="جاري التحميل…">Loading summary…</div></div>
  </div>
  <div class="dl-row">
    <button class="dl-btn dl-en" onclick="dlPDF('en')">↓ <span data-en="Download English PDF" data-ar="تنزيل PDF إنجليزي">Download English PDF</span></button>
    <button class="dl-btn dl-ar" onclick="dlPDF('ar')">↓ <span data-en="Download Arabic PDF" data-ar="تنزيل PDF عربي">Download Arabic PDF</span></button>
  </div>
</div>
</div>

<script>
// ── CONFIG ──
const PROXY_URL = 'https://script.google.com/macros/s/AKfycbxXNE6GrQxYIPMrTe-fG83iri7SMNPPL9f4W6PooX7QClTR0dghmNW2amH66qmo6twx/exec';
const MODEL     = 'gpt-4o';

// ── STATE ──
let curLang='en', pdfText='', pdfName='', questions=[], sumEN='', sumAR='', curSumLang='en';
let quizQs=[], quizAns={}, quizFlags={}, curQ=0, qDone=false;
let timerID=null, timeLeft=3600, t0=null;
let lastFile=null;

// ── TRANSLATIONS ──
const TR={
  en:{qsub:'30 MCQ · 20 True/False · 60 min',prev:'Prev',next:'Next',hint:'💡 Hint',submit:'✓ Submit',
      home:'Home',retake:'↺ Retake Quiz',back:'Back',dlEN:'Download English PDF',dlAR:'Download Arabic PDF',
      ok:'✅ Correct',wrong:'❌ Wrong',timeL:'⏱ Time',skip:'⏭ Skipped',
      noAns:'Not answered',yourAns:'Your answer:',corrAns:'Correct:',
      perfHd:'PERFORMANCE BREAKDOWN',mcqL:'MCQ',tfL:'True/False',easy:'Easy',med:'Medium',hard:'Hard',
      corrLbl:'Correct',wrongLbl:'Incorrect — correct answer:',hintNA:'No hint available for this question.',
      revHd:'ANSWER REVIEW',sumFrom:'',
      grA:'A+',grB:'A',grC:'B',grD:'C',grE:'D',grF:'F',
      mA:'Outstanding performance 🎉',mB:'Excellent work 💪',mC:'Good result 📚',mD:'Keep studying 📖',mF:'Review and retry 💡',
      ready:'Ready',score:'SCORE'},
  ar:{qsub:'30 اختيار متعدد · 20 صح/خطأ · 60 دقيقة',prev:'السابق',next:'التالي',hint:'💡 تلميح',submit:'✓ إنهاء',
      home:'الرئيسية',retake:'↺ إعادة الاختبار',back:'رجوع',dlEN:'تنزيل PDF إنجليزي',dlAR:'تنزيل PDF عربي',
      ok:'✅ صحيح',wrong:'❌ خاطئ',timeL:'⏱ الوقت',skip:'⏭ لم يُجب',
      noAns:'لم تجب',yourAns:'إجابتك:',corrAns:'الصحيح:',
      perfHd:'تفصيل الأداء',mcqL:'اختيار متعدد',tfL:'صح / خطأ',easy:'سهل',med:'متوسط',hard:'صعب',
      corrLbl:'صحيح',wrongLbl:'خاطئة — الصحيح:',hintNA:'لا يوجد تلميح لهذا السؤال.',
      revHd:'مراجعة الإجابات',sumFrom:'',
      grA:'A+',grB:'A',grC:'B',grD:'C',grE:'D',grF:'F',
      mA:'أداء رائع 🎉',mB:'عمل ممتاز 💪',mC:'نتيجة جيدة 📚',mD:'واصل الدراسة 📖',mF:'راجع وأعد المحاولة 💡',
      ready:'جاهز',score:'النتيجة'}
};
const t=k=>(TR[curLang]||TR.en)[k]||k;

// ── LANG ──
function setLang(l){
  curLang=l;
  document.documentElement.lang=l;
  document.documentElement.dir=l==='ar'?'rtl':'ltr';
  document.getElementById('btnEN').classList.toggle('active',l==='en');
  document.getElementById('btnAR').classList.toggle('active',l==='ar');
  document.querySelectorAll('[data-en]').forEach(el=>{
    const v=el.getAttribute('data-'+l)||el.getAttribute('data-en');
    if(el.tagName==='INPUT'||el.tagName==='TEXTAREA')el.placeholder=v;
    else el.textContent=v;
  });
  if(document.getElementById('quiz').classList.contains('active')&&quizQs.length){
    document.getElementById('quizSub').textContent=t('qsub');
    renderNav();renderQ(curQ);
  }
  document.getElementById('revHdTxt').textContent=t('revHd');
  document.getElementById('ringFill').closest('.ring-inner')&&
    (document.querySelector('.ring-lbl').textContent=t('score'));
}

// ── ROUTER ──
const SCRS=['welcome','processing','home','quiz','results','summary'];
function show(id){SCRS.forEach(s=>document.getElementById(s).classList.remove('active'));document.getElementById(id).classList.add('active');window.scrollTo(0,0);}

// ── UPLOAD ──
const uz=document.getElementById('uploadZone');
uz.addEventListener('dragover',e=>{e.preventDefault();uz.classList.add('drag');});
uz.addEventListener('dragleave',()=>uz.classList.remove('drag'));
uz.addEventListener('drop',e=>{e.preventDefault();uz.classList.remove('drag');const f=e.dataTransfer.files[0];if(f&&f.type==='application/pdf')processFile(f);});
function handleFile(e){const f=e.target.files[0];if(f)processFile(f);}

function retryProcess(){if(lastFile)processFile(lastFile);}

async function processFile(file){
  lastFile=file;
  pdfName=file.name;
  show('processing');
  document.getElementById('procErr').classList.remove('show');
  setStep(1,'active'); setStep(2,'idle'); setStep(3,'idle'); setStep(4,'idle');
  setProcArc(0);

  const reader=new FileReader();
  reader.onload=async e=>{
    try{
      // Step 1: Extract PDF text
      setProcArc(10);
      pdfText=await extractPDF(e.target.result);
      setStep(1,'done'); setStep(2,'active'); setProcArc(25);

      // Steps 2+3 run in PARALLEL — questions + both summaries at same time
      const [qs,[sEN,sAR]]=await Promise.all([
        genQuestions(pdfText,pdfName).then(r=>{setStep(2,'done');setProcArc(65);return r;}),
        Promise.all([
          genSummary(pdfText,pdfName,'en'),
          genSummary(pdfText,pdfName,'ar')
        ]).then(r=>{setStep(3,'done');setProcArc(90);return r;})
      ]);
      questions=qs; sumEN=sEN; sumAR=sAR;

      // Step 4: Done
      setStep(4,'active'); setProcArc(100);
      await sleep(400); setStep(4,'done');
      await sleep(300);
      document.getElementById('homeFName').textContent=pdfName;
      show('home');

    }catch(err){
      const errEl=document.getElementById('procErr');
      document.getElementById('procErrMsg').innerText=err.message;
      errEl.classList.add('show');
    }
  };
  reader.readAsArrayBuffer(file);
}

function setStep(n,state){
  const el=document.getElementById('ps'+n);
  const num=document.getElementById('pn'+n);
  el.classList.remove('active','done');
  if(state==='active'){el.classList.add('active');}
  else if(state==='done'){el.classList.add('done');num.textContent='✓';}
}

function setProcArc(pct){
  const circ=251;
  document.getElementById('procArc').style.strokeDashoffset=circ-(pct/100)*circ;
}

// ── PDF EXTRACTION ──
async function extractPDF(buf){
  const bytes=new Uint8Array(buf);
  let raw='';
  for(let i=0;i<bytes.length;i++) raw+=String.fromCharCode(bytes[i]);

  let text='';

  // Method 1: BT...ET blocks
  let btMatch;
  const btRe=/BT[\s\S]{0,2000}?ET/g;
  while((btMatch=btRe.exec(raw))!==null){
    const block=btMatch[0];
    // Extract (string) Tj
    let m;
    const r1=/\(([^)\\]|\\[\s\S])*\)\s*Tj/g;
    while((m=r1.exec(block))!==null){
      const inner=m[0].slice(1,m[0].lastIndexOf(')'));
      text+=decodePDF(inner)+' ';
    }
    // Extract [(string)offset(string)...]TJ
    const r2=/\[([^\]]*)\]\s*TJ/g;
    while((m=r2.exec(block))!==null){
      const r3=/\(([^)\\]|\\[\s\S])*\)/g;
      let m2;
      while((m2=r3.exec(m[1]))!==null){
        const inner=m2[0].slice(1,-1);
        text+=decodePDF(inner);
      }
      text+=' ';
    }
  }

  // Method 2: fallback — printable ASCII
  if(text.trim().length<80){
    const parts=[];
    for(let i=0;i<Math.min(bytes.length,400000);i++){
      const c=bytes[i];
      if(c>=32&&c<=126) parts.push(String.fromCharCode(c));
      else if(c===10||c===13) parts.push(' ');
    }
    let fb=parts.join('');
    // Remove PDF operator garbage
    fb=fb.replace(/<<[^>]{0,200}>>/g,' ')
          .replace(/endobj|endstream|startxref|xref|trailer/g,' ')
          .replace(/\/\w+/g,' ')
          .replace(/\s+/g,' ').trim();
    if(fb.length>text.length) text=fb;
  }

  text=text.replace(/\s+/g,' ').trim();
  return text.substring(0,20000)||'[Could not extract text from this PDF. The AI will generate general exam questions for the subject.]';
}

function decodePDF(s){
  return s
    .replace(/\\n/g,' ').replace(/\\r/g,' ').replace(/\\t/g,' ')
    .replace(/\\([0-7]{3})/g,(_,o)=>String.fromCharCode(parseInt(o,8)))
    .replace(/\\(.)/g,'$1');
}

// ── AI CALLS ──
async function callAI(sys,user,maxTok){
  // Google Apps Script requires redirect:follow because it returns a 302 first
  let r;
  try{
    r=await fetch(PROXY_URL,{
      method:'POST',
      redirect:'follow',
      headers:{'Content-Type':'text/plain;charset=utf-8'},
      body:JSON.stringify({system:sys,user:user,max_tokens:maxTok||5500})
    });
  }catch(err){
    // If still failing, show the real fix instruction
    const isContentProtocol = location.href.startsWith('content:');
    if(isContentProtocol){
      throw new Error(
        'You opened this file from phone storage (content://).\n\n'+
        'Fix: Open Chrome → type this in address bar:\n'+
        'file:///sdcard/Download/ahst-exams.html\n\n'+
        'Or upload the file to Google Drive and open it from there.\n\nError: '+err.message
      );
    }
    throw new Error('Network error: '+err.message+
      '\n\nMake sure your Apps Script is deployed with "Anyone" access.');
  }
  if(!r.ok){
    const e=await r.text().catch(()=>'');
    throw new Error('Proxy error '+r.status+': '+e.substring(0,200));
  }
  let d;
  try{ d=await r.json(); }
  catch{ throw new Error('Proxy returned non-JSON. Check your Apps Script deployment.'); }
  if(d.error) throw new Error(d.error);
  if(!d.result) throw new Error('Empty AI response. Please try again.');
  return d.result;
}

async function genQuestions(text,name){
  const raw=await callAI(
    'You are an expert exam question writer for medical and health sciences students. Return ONLY a valid JSON array with no markdown, no explanation, no surrounding text whatsoever.',
    'Generate exactly 50 exam questions based ONLY on the content from this PDF titled "'+name+'".\n\n<pdf_content>\n'+text.substring(0,14000)+'\n</pdf_content>\n\nRequirements:\n- 30 MCQ with 4 options (answer = correct index 0-3)\n- 20 True/False (answer = true or false)\n- Questions must come from the actual PDF content above\n- Each question must have: "hint" (1 sentence clue, no answer reveal), "explanation" (1-2 sentences why correct), "difficulty" ("easy","medium","hard")\n\nReturn ONLY this exact JSON format:\n[{"type":"mcq","text":"Question?","options":["A","B","C","D"],"answer":0,"hint":"...","explanation":"...","difficulty":"medium"},{"type":"tf","text":"Statement.","answer":true,"hint":"...","explanation":"...","difficulty":"easy"}]',
    6000
  );
  let qs;
  try{qs=JSON.parse(raw.trim());}
  catch{
    const m=raw.match(/\[[\s\S]*\]/);
    if(!m)throw new Error('Failed to parse questions from AI response. Please try again.');
    qs=JSON.parse(m[0]);
  }
  if(!Array.isArray(qs)||qs.length<5)throw new Error('AI returned too few questions ('+( qs&&qs.length||0)+'). Please try again.');
  return qs.slice(0,50).map((q,i)=>({
    id:i+1,
    type:q.type==='tf'?'tf':'mcq',
    text:String(q.text||'Question '+(i+1)),
    options:q.type==='tf'?null:(Array.isArray(q.options)?q.options.slice(0,4):['Option A','Option B','Option C','Option D']),
    answer:q.type==='tf'?Boolean(q.answer):Math.min(Math.max(Number(q.answer)||0,0),3),
    hint:String(q.hint||''),
    explanation:String(q.explanation||''),
    difficulty:['easy','medium','hard'].includes(q.difficulty)?q.difficulty:'medium'
  }));
}

async function genSummary(text,name,lang){
  const ar=lang==='ar';
  return callAI(
    ar?'You are an expert academic summarizer. Write the ENTIRE response in formal Arabic only. No English. No markdown.':'You are an expert academic summarizer. Write the ENTIRE response in English only. No markdown.',
    'Write a comprehensive study summary for the PDF titled "'+name+'" in '+(ar?'Arabic':'English')+'.\n\n<pdf_content>\n'+text.substring(0,14000)+'\n</pdf_content>\n\nInclude these sections (write section names in '+(ar?'Arabic':'English')+'):\n1. Overview\n2. Key Concepts & Definitions\n3. Main Topics (cover each major topic in detail)\n4. Important Facts & Data\n5. Clinical/Practical Applications\n6. Key Takeaways (8-10 bullet points)\n\nWrite entirely in '+(ar?'Arabic':'English')+'. Be comprehensive — this is for exam preparation.',
    4000
  );
}

function sleep(ms){return new Promise(r=>setTimeout(r,ms));}

// ── QUIZ ──
function startQuiz(){
  if(!questions.length){alert('No questions loaded. Please upload a PDF first.');return;}
  quizQs=[...questions];quizAns={};quizFlags={};curQ=0;qDone=false;timeLeft=3600;t0=Date.now();
  document.getElementById('quizTitle').textContent=pdfName;
  document.getElementById('quizSub').textContent=t('qsub');
  show('quiz');renderNav();renderQ(0);startTimer();
}

function startTimer(){
  stopTimer();updTimer();
  timerID=setInterval(()=>{timeLeft--;updTimer();if(timeLeft<=0){clearInterval(timerID);submitQuiz();}},1000);
}
function stopTimer(){if(timerID){clearInterval(timerID);timerID=null;}}
function updTimer(){
  const m=Math.floor(timeLeft/60),s=timeLeft%60;
  document.getElementById('timerTxt').textContent=p2(m)+':'+p2(s);
  const w=document.getElementById('timerWrap');
  w.className='timer-wrap'+(timeLeft<=300?' hot':timeLeft<=600?' warn':'');
}
function p2(n){return String(n).padStart(2,'0');}

function renderNav(){
  document.getElementById('qnavInner').innerHTML=quizQs.map((_,i)=>{
    let c='qdot';
    if(i===curQ)c+=' cur';
    else if(quizAns[i]!==undefined)c+=qDone?(isOk(i)?' ok':' bad'):' ans';
    if(quizFlags[i])c+=' fl';
    return '<div class="'+c+'" onclick="jumpQ('+i+')">'+(i+1)+'</div>';
  }).join('');
  const el=document.getElementById('qnavInner').children[curQ];
  if(el)el.scrollIntoView({block:'nearest',inline:'center'});
}

function isOk(i){
  const q=quizQs[i],a=quizAns[i];
  if(a===undefined)return false;
  return q.type==='mcq'?a===q.answer:(a==='true')===q.answer;
}

function updLive(){
  document.getElementById('liveOk').textContent=quizQs.filter((_,i)=>quizAns[i]!==undefined&&isOk(i)).length;
  document.getElementById('liveBad').textContent=quizQs.filter((_,i)=>quizAns[i]!==undefined&&!isOk(i)).length;
}

function renderQ(idx){
  curQ=idx;
  const q=quizQs[idx],ua=quizAns[idx],tot=quizQs.length,last=idx===tot-1;
  document.getElementById('qProgF').style.width=((idx+1)/tot*100)+'%';
  document.getElementById('qPrev').disabled=idx===0;
  const nextBtn=document.getElementById('qNext'),subBtn=document.getElementById('qSub');
  nextBtn.classList.toggle('hidden',last&&!qDone);
  subBtn.classList.toggle('hidden',!(last&&!qDone));
  if(qDone){nextBtn.classList.remove('hidden');subBtn.classList.add('hidden');}
  document.getElementById('qHint').textContent=t('hint');

  const dc={easy:'d-e',medium:'d-m',hard:'d-h'};
  const dl={easy:t('easy'),medium:t('med'),hard:t('hard')};

  let h='<div class="q-card">'
    +'<div class="q-meta">'
    +'<span class="q-type">'+(q.type==='mcq'?'MCQ':'TRUE / FALSE')+'</span>'
    +'<div class="q-meta-right">'
    +'<button class="flag-btn '+(quizFlags[idx]?'on':'')+'" onclick="toggleFlag('+idx+')">🚩</button>'
    +'<span class="diff '+(dc[q.difficulty]||'d-m')+'">'+(dl[q.difficulty]||t('med'))+'</span>'
    +'</div></div>'
    +'<div class="q-num">Q '+(idx+1)+' / '+tot+'</div>'
    +'<div class="q-text">'+q.text+'</div>';

  if(q.type==='mcq'){
    h+='<div class="opts">';
    (q.options||[]).forEach((o,oi)=>{
      let c='opt',ic='';
      if(qDone){
        if(oi===q.answer){c+=' ok';ic='<span class="opt-ic">✅</span>';}
        else if(ua===oi){c+=' bad';ic='<span class="opt-ic">❌</span>';}
      }else if(ua===oi)c+=' sel';
      h+='<div class="'+c+'" '+(qDone?'':'onclick="pick('+oi+')"')+'>'
        +'<div class="opt-l">'+'ABCD'[oi]+'</div>'
        +'<div class="opt-txt">'+o+'</div>'+ic+'</div>';
    });
    h+='</div>';
  }else{
    const tv=ua==='true',fv=ua==='false';
    let tc='tf-btn',fc='tf-btn';
    if(qDone){
      if(q.answer===true) tc+=' ok'; else if(tv) tc+=' bad';
      if(q.answer===false) fc+=' ok'; else if(fv) fc+=' bad';
    }else{if(tv)tc+=' sel';if(fv)fc+=' sel';}
    const tL=curLang==='ar'?'صحيح':'TRUE';
    const fL=curLang==='ar'?'خطأ':'FALSE';
    const dT=qDone?'':'onclick="pickTF(\'true\')"';
    const dF=qDone?'':'onclick="pickTF(\'false\')"';
    h+='<div class="tf-row">'
      +'<button class="'+tc+'" '+dT+'><span class="tf-em">✅</span>'+tL+'</button>'
      +'<button class="'+fc+'" '+dF+'><span class="tf-em">❌</span>'+fL+'</button>'
      +'</div>';
  }

  if(qDone&&ua!==undefined){
    const ok2=isOk(idx);
    const corr=q.type==='mcq'?q.options[q.answer]:(q.answer?'TRUE':'FALSE');
    h+='<div class="q-fb show '+(ok2?'ok':'bad')+'">'
      +'<div class="q-fb-lbl">'+(ok2?t('corrLbl'):t('wrongLbl')+' '+corr)+'</div>'
      +(q.explanation||'')+'</div>';
  }
  h+='<div class="q-hint" id="hintBox"></div>';
  h+='</div>';

  document.getElementById('qArea').innerHTML=h;
  renderNav();updLive();
}

function pick(oi){if(qDone)return;quizAns[curQ]=oi;renderQ(curQ);}
function pickTF(v){if(qDone)return;quizAns[curQ]=v;renderQ(curQ);}
function toggleFlag(i){quizFlags[i]=!quizFlags[i];renderQ(curQ);}
function showHint(){
  const b=document.getElementById('hintBox');if(!b)return;
  const q=quizQs[curQ];
  b.classList.add('show');
  b.innerHTML='<div class="q-hint-lbl">💡 '+(curLang==='ar'?'تلميح':'HINT')+'</div>'+(q.hint||t('hintNA'));
}
function prevQ(){if(curQ>0){curQ--;renderQ(curQ);}}
function nextQ(){if(curQ<quizQs.length-1){curQ++;renderQ(curQ);}}
function jumpQ(i){curQ=i;renderQ(i);}

function submitQuiz(){
  stopTimer();qDone=true;
  const el=t0?Math.floor((Date.now()-t0)/1000):0;
  const ok=quizQs.filter((_,i)=>isOk(i)).length;
  const bad=quizQs.filter((_,i)=>quizAns[i]!==undefined&&!isOk(i)).length;
  const sk=quizQs.filter((_,i)=>quizAns[i]===undefined).length;
  const pct=Math.round(ok/quizQs.length*100);
  renderQ(curQ);
  setTimeout(()=>showResults(ok,bad,sk,pct,el),400);
}
function retakeQuiz(){startQuiz();}

// ── RESULTS ──
function showResults(ok,bad,sk,pct,el){
  const gk=pct>=95?'grA':pct>=85?'grB':pct>=75?'grC':pct>=65?'grD':pct>=50?'grE':'grF';
  const mk=pct>=90?'mA':pct>=75?'mB':pct>=60?'mC':pct>=50?'mD':'mF';
  const mm=Math.floor(el/60),ss=el%60;

  const circ=326.7,off=circ-(pct/100)*circ;
  const rf=document.getElementById('ringFill');
  rf.style.strokeDasharray=circ;
  setTimeout(()=>{rf.style.strokeDashoffset=off;},100);
  document.getElementById('ringPct').textContent=pct+'%';
  document.getElementById('scoreGrade').textContent=t(gk);
  document.getElementById('scoreMsg').textContent=t(mk);
  document.getElementById('revHdTxt').textContent=t('revHd');

  document.getElementById('scoreGrid').innerHTML=
    '<div class="sg"><div class="sg-val sv-ok">'+ok+'</div><div class="sg-lbl">'+t('ok')+'</div></div>'
    +'<div class="sg"><div class="sg-val sv-bad">'+bad+'</div><div class="sg-lbl">'+t('wrong')+'</div></div>'
    +'<div class="sg"><div class="sg-val sv-time">'+p2(mm)+':'+p2(ss)+'</div><div class="sg-lbl">'+t('timeL')+'</div></div>'
    +'<div class="sg"><div class="sg-val sv-skip">'+sk+'</div><div class="sg-lbl">'+t('skip')+'</div></div>';

  function grpPct(arr){if(!arr.length)return 0;return Math.round(arr.filter(q=>isOk(quizQs.indexOf(q))).length/arr.length*100);}
  const mcqQs=quizQs.filter(q=>q.type==='mcq');
  const tfQs=quizQs.filter(q=>q.type==='tf');
  const eQs=quizQs.filter(q=>q.difficulty==='easy');
  const mQs=quizQs.filter(q=>q.difficulty==='medium');
  const hQs=quizQs.filter(q=>q.difficulty==='hard');

  document.getElementById('perfCard').innerHTML=
    '<div class="perf-hd">'+t('perfHd')+'</div>'
    +pb(t('mcqL'),grpPct(mcqQs),'var(--g)')
    +pb(t('tfL'),grpPct(tfQs),'var(--blue)')
    +pb(t('easy'),grpPct(eQs),'var(--g)')
    +pb(t('med'),grpPct(mQs),'var(--yellow)')
    +pb(t('hard'),grpPct(hQs),'var(--red)');

  document.getElementById('revList').innerHTML=quizQs.map((q,i)=>{
    const a=quizAns[i],ok2=isOk(i),s2=a===undefined;
    const cls=s2?'ri-skip':ok2?'ri-ok':'ri-bad';
    const ic=s2?'—':ok2?'✓':'✗';
    const ca=q.type==='mcq'?q.options[q.answer]:(q.answer?'TRUE':'FALSE');
    const ua=a===undefined?t('noAns'):(q.type==='mcq'?q.options[a]:String(a).toUpperCase());
    return '<div class="rev-item '+cls+'" style="animation-delay:'+(i*.01)+'s">'
      +'<div class="rv-q">'+ic+' Q'+(i+1)+'. '+q.text+'</div>'
      +'<div class="rv-a '+(ok2?'a-ok':'a-bad')+'">'+t('yourAns')+' '+ua+'</div>'
      +(!ok2?'<div class="rv-a a-cor">'+t('corrAns')+' '+ca+'</div>':'')
      +'</div>';
  }).join('');
  show('results');
}

function pb(lbl,pct,color){
  return '<div class="pbar"><div class="pb-lbl">'+lbl+'</div><div class="pb-track"><div class="pb-fill" style="width:'+pct+'%;background:'+color+'"></div></div><div class="pb-pct">'+pct+'%</div></div>';
}

// ── SUMMARY ──
function goSummary(){
  document.getElementById('sumFileLbl').textContent=pdfName;
  curSumLang='en';
  document.getElementById('slbEN').classList.add('on');
  document.getElementById('slbAR').classList.remove('on');
  renderSumContent();
  show('summary');
}
function switchSumLang(l){
  curSumLang=l;
  document.getElementById('slbEN').classList.toggle('on',l==='en');
  document.getElementById('slbAR').classList.toggle('on',l==='ar');
  renderSumContent();
}
function renderSumContent(){
  const box=document.getElementById('sumBox');
  const txt=curSumLang==='ar'?sumAR:sumEN;
  if(!txt){box.innerHTML='<div class="sum-loading"><div class="sum-spin"></div><div class="sum-spin-lbl">Loading…</div></div>';return;}
  const ar=curSumLang==='ar';
  box.innerHTML='<div class="sum-txt'+(ar?' sum-txt-ar':'')+'" style="'+(ar?'direction:rtl;text-align:right':'')+'">'+txt+'</div>';
}

// ── PDF DOWNLOAD ──
async function dlPDF(lang){
  const {jsPDF}=window.jspdf;
  const doc=new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
  const txt=lang==='ar'?sumAR:sumEN;
  const ar=lang==='ar';
  if(!txt){alert('Summary not available yet.');return;}

  const W=210,H=297,M=20,LH=6.5,maxW=W-M*2;
  let y=M;

  // Header bar
  doc.setFillColor(12,12,14);doc.rect(0,0,W,38,'F');
  doc.setFillColor(0,200,150);doc.rect(0,0,3,38,'F');
  doc.setFontSize(16);doc.setTextColor(242,242,244);doc.setFont('helvetica','bold');
  doc.text(ar?'ملخص الدراسة':'Study Summary',M+6,16);
  doc.setFontSize(9);doc.setTextColor(136,136,160);doc.setFont('helvetica','normal');
  const fn2=pdfName.length>55?pdfName.substring(0,52)+'...':pdfName;
  doc.text(fn2,M+6,25);
  doc.text(new Date().toLocaleDateString(ar?'ar-SA':'en-US'),W-M,25,{align:'right'});
  doc.setTextColor(0,200,150);doc.setFontSize(8);
  doc.text('AHST Study Intelligence',M+6,32);
  y=50;

  doc.setFontSize(10);doc.setTextColor(30,30,35);doc.setFont('helvetica','normal');

  for(const line of txt.split('\n')){
    const tr=line.trim();
    if(!tr){y+=3;continue;}
    const isHdr=/^\d+\./.test(tr)&&tr.length<60&&tr.endsWith(':');
    const isBullet=tr.startsWith('•')||tr.startsWith('-');
    if(isHdr){
      if(y>H-30){doc.addPage();y=M;}
      y+=4;
      doc.setFillColor(240,252,248);
      doc.roundedRect(M-1,y-5,maxW+2,8,1,1,'F');
      doc.setFillColor(0,200,150);doc.rect(M-1,y-5,2,8,'F');
      doc.setFont('helvetica','bold');doc.setFontSize(10);doc.setTextColor(0,100,75);
      doc.text(tr,ar?W-M:M+4,y,{align:ar?'right':'left'});
      doc.setFont('helvetica','normal');doc.setFontSize(10);doc.setTextColor(30,30,35);
      y+=10;
    }else{
      const wrapped=doc.splitTextToSize(tr,maxW-(isBullet?6:0));
      for(const wl of wrapped){
        if(y>H-22){doc.addPage();y=M;}
        doc.text(wl,ar?W-M:(isBullet?M+4:M),y,{align:ar?'right':'left'});
        y+=LH;
      }
    }
  }

  const total=doc.getNumberOfPages();
  for(let pg=1;pg<=total;pg++){
    doc.setPage(pg);
    doc.setFillColor(18,18,20);doc.rect(0,H-12,W,12,'F');
    doc.setFontSize(7.5);doc.setTextColor(100,100,115);
    doc.text('AHST Study Intelligence',M,H-5);
    doc.text(pg+' / '+total,W-M,H-5,{align:'right'});
  }
  const safe=pdfName.replace(/\.pdf$/i,'').replace(/[^a-zA-Z0-9]/g,'_').substring(0,40);
  doc.save(safe+'_'+lang.toUpperCase()+'_summary.pdf');
}

// ── MISC ──
function goWelcome(){stopTimer();show('welcome');document.getElementById('fileInput').value='';}
setLang('en');

// Detect content:// protocol (file opened from Android file manager)
// and show a warning on the welcome screen
(function(){
  if(location.href.startsWith('content:')||location.href.startsWith('file:')){
    const warn=document.createElement('div');
    warn.style.cssText='background:rgba(245,197,66,.1);border:1px solid rgba(245,197,66,.3);border-radius:12px;padding:14px 16px;margin-bottom:14px;font-size:12px;color:rgba(245,197,66,.9);line-height:1.7';
    warn.innerHTML='<strong style="color:#f5c542;display:block;margin-bottom:6px">⚠️ Open this file correctly</strong>'+
      'You opened this as a local file. For the AI features to work, open it one of these ways:<br><br>'+
      '<strong>Option 1:</strong> Upload <code style="background:rgba(255,255,255,.1);padding:1px 5px;border-radius:4px">ahst-exams.html</code> to Google Drive, then open it in Chrome.<br>'+
      '<strong>Option 2:</strong> In Chrome address bar type: <code style="background:rgba(255,255,255,.1);padding:1px 5px;border-radius:4px">file:///sdcard/Download/ahst-exams.html</code>';
    const hero=document.querySelector('.w-hero');
    if(hero) hero.insertBefore(warn, hero.firstChild);
  }
})();

</script>
</body>
</html>
