m<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
<meta name="theme-color" content="#07070f">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="UNIAP">
<title>UNIAP Connect</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800;900&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#07070f;--sur:#11111d;--a:#ff3b6e;--a2:#7c3bff;--a3:#00e5ff;--tx:#f0f0f8;--mu:#5a5a78;--cd:#181828;--gr:#00cc66;--gd:#ffd000;--gw:0 0 28px rgba(255,59,110,.4)}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden}
body{background:var(--bg);color:var(--tx);font-family:'DM Sans',sans-serif}
.sc{position:fixed;inset:0;z-index:10;display:none;flex-direction:column;background:var(--bg)}
.sc.on{display:flex}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes up{from{transform:translateY(100%);opacity:0}to{transform:translateY(0);opacity:1}}
@keyframes pop{0%{transform:scale(0)}70%{transform:scale(1.2)}100%{transform:scale(1)}}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
@keyframes tick{0%{transform:translateX(80%)}100%{transform:translateX(-100%)}}
@keyframes rain{0%{transform:translateY(-10px);opacity:1}100%{transform:translateY(110vh);opacity:0}}
@keyframes lvl{0%{transform:scale(0) rotate(-10deg);opacity:0}60%{transform:scale(1.3);opacity:1}100%{transform:scale(1);opacity:1}}

/* BLOBS */
.bl{position:fixed;border-radius:50%;filter:blur(70px);pointer-events:none;z-index:0;animation:float 8s ease-in-out infinite}
.bl1{width:260px;height:260px;background:rgba(255,59,110,.14);top:-60px;right:-50px}
.bl2{width:220px;height:220px;background:rgba(124,59,255,.11);bottom:-50px;left:-50px;animation-direction:reverse}

/* SPLASH */
#sp{background:var(--bg);align-items:center;justify-content:center;z-index:100;text-align:center}
.sp-ico{font-size:4rem;animation:float 2s infinite}
.sp-logo{font-family:'Syne',sans-serif;font-weight:900;font-size:2.6rem;background:linear-gradient(135deg,var(--a),var(--a2));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.sp-sub{color:var(--mu);font-size:.8rem;margin-bottom:5px}
.sp-ver{background:rgba(255,208,0,.12);border:1px solid rgba(255,208,0,.3);color:var(--gd);font-size:.66rem;padding:3px 10px;border-radius:20px;margin-bottom:26px;display:inline-block}
.sp-ld{width:40px;height:40px;border:3px solid rgba(255,255,255,.1);border-top-color:var(--a);border-radius:50%;animation:spin .8s linear infinite;margin:0 auto}

/* AUTH */
#auth{overflow-y:auto;align-items:center;padding:16px}
.ac{position:relative;z-index:1;width:100%;max-width:400px;margin:16px auto;background:rgba(17,17,29,.93);border:1px solid rgba(255,255,255,.07);border-radius:24px;padding:24px 20px 20px;backdrop-filter:blur(20px);box-shadow:0 20px 60px rgba(0,0,0,.5)}
.a-ico{width:52px;height:52px;border-radius:14px;background:linear-gradient(135deg,var(--a),var(--a2));display:flex;align-items:center;justify-content:center;font-size:1.5rem;margin:0 auto 8px;box-shadow:var(--gw)}
.a-logo{font-family:'Syne',sans-serif;font-weight:900;font-size:1.45rem;background:linear-gradient(135deg,var(--a),var(--a2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;text-align:center}
.a-sub{color:var(--mu);font-size:.75rem;text-align:center;margin-bottom:14px}
.a-tabs{display:flex;background:var(--cd);border-radius:10px;padding:3px;margin-bottom:14px}
.at{flex:1;padding:8px;border:none;background:transparent;color:var(--mu);font-family:'DM Sans',sans-serif;font-size:.82rem;font-weight:600;border-radius:7px;cursor:pointer;transition:all .2s}
.at.on{background:linear-gradient(135deg,var(--a),var(--a2));color:#fff;box-shadow:var(--gw)}
.af{display:flex;flex-direction:column;gap:10px}
.fl{font-size:.67rem;font-weight:700;color:var(--mu);letter-spacing:.4px;text-transform:uppercase;margin-bottom:3px}
.iw{position:relative}
.ii{position:absolute;left:11px;top:50%;transform:translateY(-50%);font-size:.83rem;pointer-events:none}
.ip{width:100%;background:var(--cd);border:1.5px solid rgba(255,255,255,.07);border-radius:10px;padding:11px 11px 11px 35px;color:var(--tx);font-family:'DM Sans',sans-serif;font-size:.85rem;outline:none;transition:border-color .2s;-webkit-appearance:none}
.ip:focus{border-color:var(--a)}
.ip::placeholder{color:var(--mu)}
.tpw{position:absolute;right:10px;top:50%;transform:translateY(-50%);background:none;border:none;color:var(--mu);cursor:pointer;font-size:.83rem}
.a-btn{width:100%;background:linear-gradient(135deg,var(--a),var(--a2));border:none;border-radius:10px;padding:12px;color:#fff;font-family:'Syne',sans-serif;font-size:.88rem;font-weight:700;cursor:pointer;box-shadow:var(--gw);transition:transform .15s;margin-top:3px}
.a-btn:active{transform:scale(.97)}
.a-btn:disabled{opacity:.5}
.amsg{border-radius:9px;padding:9px 11px;font-size:.77rem;font-weight:500;display:none;margin-top:2px}
.amsg.err{background:rgba(255,68,68,.12);border:1px solid rgba(255,68,68,.25);color:#ff6b6b;display:block}
.amsg.ok{background:rgba(0,204,102,.1);border:1px solid rgba(0,204,102,.25);color:var(--gr);display:block}
.av-g{display:flex;flex-wrap:wrap;gap:6px;justify-content:center;margin-top:3px}
.av{width:34px;height:34px;border-radius:50%;background:var(--cd);border:2px solid transparent;display:flex;align-items:center;justify-content:center;font-size:1.05rem;cursor:pointer;transition:all .2s}
.av.on{border-color:var(--a)}
.sb-bar{display:flex;gap:3px;margin-top:4px}
.sb{flex:1;height:3px;border-radius:4px;background:rgba(255,255,255,.1);transition:background .3s}
.sb.w{background:#ff4444}.sb.m{background:#ffaa00}.sb.s{background:var(--gr)}
.div{display:flex;align-items:center;gap:8px;margin:3px 0}
.dl{flex:1;height:1px;background:rgba(255,255,255,.07)}
.div span{color:var(--mu);font-size:.72rem}
.soc-row{display:flex;gap:7px}
.soc{flex:1;background:var(--cd);border:1.5px solid rgba(255,255,255,.07);border-radius:9px;padding:9px;display:flex;align-items:center;justify-content:center;gap:5px;color:var(--tx);font-size:.76rem;font-weight:600;cursor:pointer}
.terms{font-size:.68rem;color:var(--mu);text-align:center;margin-top:7px;line-height:1.5}
.terms a{color:var(--a);text-decoration:none}

/* TOP NAV */
.tnav{position:fixed;top:0;left:0;right:0;z-index:50;display:flex;align-items:center;justify-content:space-between;padding:11px 14px;background:linear-gradient(to bottom,rgba(0,0,0,.8),transparent)}
.tn-logo{font-family:'Syne',sans-serif;font-weight:900;font-size:1.05rem;background:linear-gradient(135deg,var(--a),var(--a2));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.tn-tabs{display:flex;gap:13px;font-size:.83rem}
.tt{color:rgba(255,255,255,.5);cursor:pointer;font-weight:500;padding-bottom:2px}
.tt.on{color:#fff;border-bottom:2px solid #fff}
.tn-btn{background:none;border:none;color:#fff;font-size:1.05rem;cursor:pointer;padding:4px}
.coin-chip{background:rgba(255,208,0,.13);border:1px solid rgba(255,208,0,.3);border-radius:20px;padding:4px 9px;display:flex;align-items:center;gap:4px;font-size:.74rem;font-weight:700;color:var(--gd);cursor:pointer}

/* BOTTOM NAV */
.bnav{position:fixed;bottom:0;left:0;right:0;z-index:50;display:flex;align-items:center;justify-content:space-around;padding:7px 0 max(12px,env(safe-area-inset-bottom));background:linear-gradient(to top,rgba(0,0,0,.96),transparent)}
.ni{display:flex;flex-direction:column;align-items:center;gap:2px;cursor:pointer;font-size:.55rem;color:rgba(255,255,255,.4);font-weight:500;transition:all .15s;padding:3px 7px;position:relative}
.ni:active{transform:scale(.88)}
.ni.on{color:#fff}
.nico{font-size:1.2rem;line-height:1}
.nup{background:linear-gradient(135deg,var(--a),var(--a2));border:none;border-radius:10px;width:42px;height:30px;display:flex;align-items:center;justify-content:center;font-size:1.35rem;cursor:pointer;box-shadow:var(--gw);color:#fff;transition:transform .15s}
.nup:active{transform:scale(.9)}
.ndot{position:absolute;top:2px;right:4px;width:6px;height:6px;border-radius:50%;background:var(--a);border:1.5px solid var(--bg)}

/* FEED */
#feed-sc{background:#000}
#feed{position:absolute;inset:0;overflow-y:scroll;scroll-snap-type:y mandatory;-webkit-overflow-scrolling:touch;scrollbar-width:none}
#feed::-webkit-scrollbar{display:none}
.ptr{position:fixed;top:0;left:0;right:0;z-index:49;height:0;overflow:hidden;display:flex;align-items:center;justify-content:center;gap:7px;background:linear-gradient(to bottom,rgba(255,59,110,.15),transparent);transition:height .2s;font-size:.77rem;color:var(--a);font-weight:600}
.ptr.on{height:48px}
.pi{font-size:1.1rem}.pi.sp{animation:spin .8s linear infinite}
.vc{height:100dvh;scroll-snap-align:start;position:relative;overflow:hidden;background:#000}
.vc video{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.tap{position:absolute;inset:0;z-index:5}
.pico{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:3rem;opacity:0;transition:opacity .25s;z-index:10;pointer-events:none}
.pico.on{opacity:1}
.vld{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);z-index:6;font-size:1.6rem;animation:spin .8s linear infinite}
.vov{position:absolute;inset:0;z-index:15;background:linear-gradient(to top,rgba(0,0,0,.9) 0%,rgba(0,0,0,0) 45%,rgba(0,0,0,.25) 100%);display:flex;flex-direction:column;justify-content:flex-end;padding:0 0 74px;pointer-events:none}
.vov *{pointer-events:auto}
.vinf{padding:0 11px;max-width:calc(100% - 58px)}
.vun{font-family:'Syne',sans-serif;font-weight:700;font-size:.88rem;color:#fff;display:flex;align-items:center;gap:5px;margin-bottom:4px;flex-wrap:wrap}
.vub{background:var(--a);color:#fff;font-size:.52rem;padding:2px 5px;border-radius:8px}
.vlb{font-size:.54rem;padding:2px 6px;border-radius:8px;font-weight:700;border:1px solid}
.vde{font-size:.8rem;color:rgba(255,255,255,.88);line-height:1.4;margin-bottom:5px}
.vtg span{font-size:.71rem;color:var(--a3);font-weight:500;margin-right:4px}
.vmu{display:flex;align-items:center;gap:5px;margin-top:7px;font-size:.71rem;color:rgba(255,255,255,.7);overflow:hidden}
.vno{display:inline-block;animation:spin 3s linear infinite;flex-shrink:0}
.vmt{white-space:nowrap;overflow:hidden;max-width:170px}
.vmt span{display:inline-block;animation:tick 10s linear infinite}
.vac{position:absolute;right:8px;bottom:82px;z-index:20;display:flex;flex-direction:column;align-items:center;gap:14px}
.ab{display:flex;flex-direction:column;align-items:center;gap:3px;cursor:pointer;transition:transform .15s}
.ab:active{transform:scale(.82)}
.ab .ic{width:40px;height:40px;border-radius:50%;background:rgba(0,0,0,.3);backdrop-filter:blur(10px);border:1px solid rgba(255,255,255,.13);display:flex;align-items:center;justify-content:center;font-size:1.2rem;transition:all .2s}
.ab.lk .ic{background:rgba(255,59,110,.4);border-color:var(--a);box-shadow:var(--gw)}
.ab .cn{font-size:.66rem;color:rgba(255,255,255,.9);font-weight:600;text-shadow:0 1px 4px rgba(0,0,0,.8)}
.vav{width:40px;height:40px;border-radius:50%;background:linear-gradient(135deg,var(--a),var(--a2));display:flex;align-items:center;justify-content:center;font-size:1rem;border:2.5px solid #fff}
.vfb{width:16px;height:16px;border-radius:50%;background:var(--a);display:flex;align-items:center;justify-content:center;font-size:.58rem;color:#fff;margin-top:-10px;border:2px solid #000}
.vht{position:absolute;font-size:4.5rem;pointer-events:none;opacity:0;top:50%;left:50%;transform:translate(-50%,-50%) scale(0);z-index:30;transition:opacity .25s,transform .25s}
.vht.pop{opacity:1;transform:translate(-50%,-50%) scale(1.3)}
.vht.fade{opacity:0;transform:translate(-50%,-50%) scale(.7)}
.vpb{position:absolute;bottom:65px;left:0;right:0;z-index:20;height:2px;background:rgba(255,255,255,.18)}
.vpf{height:100%;background:linear-gradient(90deg,var(--a),var(--a2));width:0%}
.lm{height:100dvh;scroll-snap-align:start;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:12px;background:var(--bg)}
.lm-btn{background:linear-gradient(135deg,var(--a),var(--a2));border:none;border-radius:15px;padding:12px 26px;color:#fff;font-family:'Syne',sans-serif;font-weight:700;font-size:.9rem;cursor:pointer;box-shadow:var(--gw)}

/* GIFT RAIN */
.gr-item{position:fixed;z-index:300;font-size:1.8rem;animation:rain 2s ease-in forwards;pointer-events:none}

/* MESSAGES */
#msg-sc{overflow-y:auto;padding-top:56px;padding-bottom:76px}
.conv{display:flex;align-items:center;gap:10px;padding:12px 13px;cursor:pointer;border-bottom:1px solid rgba(255,255,255,.04)}
.conv:active{background:rgba(255,255,255,.02)}
.cv-av{width:44px;height:44px;border-radius:50%;background:linear-gradient(135deg,var(--a),var(--a2));display:flex;align-items:center;justify-content:center;font-size:1.2rem;flex-shrink:0;position:relative}
.on-dot{position:absolute;bottom:0;right:0;width:10px;height:10px;border-radius:50%;background:var(--gr);border:2px solid var(--bg)}
.cv-body{flex:1;min-width:0}
.cv-name{font-weight:600;font-size:.86rem;margin-bottom:2px}
.cv-prev{font-size:.76rem;color:var(--mu);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.cv-meta{display:flex;flex-direction:column;align-items:flex-end;gap:4px;flex-shrink:0}
.cv-time{font-size:.68rem;color:var(--mu)}
.unr{width:16px;height:16px;border-radius:50%;background:var(--a);display:flex;align-items:center;justify-content:center;font-size:.6rem;font-weight:700}

/* CHAT */
#chat-sc{overflow:hidden}
.ch-nav{padding:12px 13px;display:flex;align-items:center;gap:10px;background:rgba(7,7,15,.95);backdrop-filter:blur(10px);border-bottom:1px solid rgba(255,255,255,.05)}
.ch-back{background:none;border:none;color:#fff;font-size:1.15rem;cursor:pointer;padding:4px}
.ch-av{width:33px;height:33px;border-radius:50%;background:linear-gradient(135deg,var(--a),var(--a2));display:flex;align-items:center;justify-content:center;font-size:.9rem}
.ch-ni{flex:1}
.ch-name{font-family:'Syne',sans-serif;font-weight:700;font-size:.9rem}
.ch-st{font-size:.67rem;color:var(--gr)}
.ch-msgs{flex:1;overflow-y:auto;padding:13px;display:flex;flex-direction:column;gap:9px}
.bw{display:flex;flex-direction:column}
.bw.me{align-items:flex-end}
.bb{max-width:76%;padding:9px 12px;border-radius:17px;font-size:.81rem;line-height:1.4;word-break:break-word}
.bb.them{background:var(--cd);border-radius:17px 17px 17px 4px}
.bb.me{background:linear-gradient(135deg,var(--a),var(--a2));color:#fff;border-radius:17px 17px 4px 17px}
.bt{font-size:.64rem;color:var(--mu);margin-top:2px}
.bw.me .bt{text-align:right}
.ch-inp-row{padding:8px 12px max(12px,env(safe-area-inset-bottom));display:flex;gap:8px;align-items:center;background:rgba(7,7,15,.95);backdrop-filter:blur(10px);border-top:1px solid rgba(255,255,255,.05)}
.ch-inp{flex:1;background:var(--cd);border:1.5px solid rgba(255,255,255,.07);border-radius:19px;padding:9px 13px;color:var(--tx);font-family:'DM Sans',sans-serif;font-size:.82rem;outline:none}
.ch-inp:focus{border-color:var(--a)}
.ch-send{background:linear-gradient(135deg,var(--a),var(--a2));border:none;border-radius:50%;width:36px;height:36px;display:flex;align-items:center;justify-content:center;font-size:.85rem;cursor:pointer;color:#fff;flex-shrink:0}

/* PROFILE */
#prof-sc{overflow-y:auto;padding-bottom:76px}
.pc-cover{height:140px;background:linear-gradient(135deg,rgba(255,59,110,.28),rgba(124,59,255,.28));position:relative;display:flex;align-items:flex-end;padding:0 13px}
.pc-av{width:72px;height:72px;border-radius:50%;background:linear-gradient(135deg,var(--a),var(--a2));display:flex;align-items:center;justify-content:center;font-size:1.8rem;border:3px solid var(--bg);margin-bottom:-36px;box-shadow:var(--gw)}
.pc-info{padding:44px 13px 13px}
.pc-name{font-family:'Syne',sans-serif;font-weight:800;font-size:1.2rem;margin-bottom:2px}
.pc-handle{color:var(--mu);font-size:.8rem;margin-bottom:11px;display:flex;align-items:center;gap:6px}
.pc-stats{display:flex;gap:0;margin-bottom:13px;background:var(--cd);border-radius:14px;overflow:hidden}
.ps{flex:1;padding:12px;text-align:center;border-right:1px solid rgba(255,255,255,.05)}
.ps:last-child{border-right:none}
.ps .n{font-family:'Syne',sans-serif;font-weight:800;font-size:1rem}
.ps .l{color:var(--mu);font-size:.68rem;margin-top:1px}
.pc-act{display:flex;gap:8px;margin-bottom:16px}
.edit-b{flex:1;background:var(--cd);border:1.5px solid rgba(255,255,255,.1);border-radius:10px;padding:9px;color:var(--tx);font-weight:600;cursor:pointer;font-family:'DM Sans',sans-serif;font-size:.8rem}
.out-b{background:rgba(255,59,110,.1);border:1.5px solid rgba(255,59,110,.25);border-radius:10px;padding:9px 14px;color:var(--a);font-weight:600;cursor:pointer;font-family:'DM Sans',sans-serif;font-size:.8rem}
.pg{display:grid;grid-template-columns:repeat(3,1fr);gap:2px;margin-top:2px}
.gi{aspect-ratio:1;background:var(--cd);display:flex;align-items:center;justify-content:center;font-size:1.8rem;cursor:pointer;position:relative;overflow:hidden}
.gi .gv{position:absolute;bottom:3px;left:3px;font-size:.63rem;font-weight:600;color:#fff;text-shadow:0 1px 4px rgba(0,0,0,.8)}

/* LEVELS */
#lvl-sc{overflow-y:auto;padding-top:56px;padding-bottom:76px}
.lv-hero{margin:12px;background:linear-gradient(135deg,rgba(255,208,0,.1),rgba(255,140,0,.08));border:1px solid rgba(255,208,0,.22);border-radius:20px;padding:20px;text-align:center;position:relative;overflow:hidden}
.cur-n{font-family:'Syne',sans-serif;font-weight:900;font-size:3.2rem;background:linear-gradient(135deg,var(--gd),#ff8c00);-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1}
.cur-nm{font-family:'Syne',sans-serif;font-weight:700;font-size:1.05rem;margin-bottom:9px}
.xp-tr{background:rgba(255,255,255,.1);border-radius:20px;height:10px;margin-bottom:5px;overflow:hidden}
.xp-f{height:100%;background:linear-gradient(90deg,var(--gd),#ff8c00);border-radius:20px;transition:width .8s ease}
.xp-tx{font-size:.73rem;color:var(--mu)}
.cd-disp{display:flex;align-items:center;justify-content:center;gap:6px;margin-top:12px;background:rgba(255,208,0,.1);border:1px solid rgba(255,208,0,.22);border-radius:18px;padding:8px 16px;display:inline-flex}
.cd-n{font-family:'Syne',sans-serif;font-weight:800;font-size:1.25rem;color:var(--gd)}
.lv-list{display:flex;flex-direction:column;gap:9px;padding:0 12px 12px}
.lv-row{background:var(--cd);border-radius:15px;padding:13px;border:1.5px solid transparent;transition:all .3s;position:relative}
.lv-row.cur{border-color:var(--gd);background:rgba(255,208,0,.05)}
.lv-row.lock{opacity:.5}
.lv-row.done{border-color:rgba(0,204,102,.3)}
.lv-top{display:flex;align-items:center;gap:11px;margin-bottom:9px}
.lv-em{font-size:1.9rem;flex-shrink:0}
.lv-inf{flex:1}
.lv-name{font-family:'Syne',sans-serif;font-weight:700;font-size:.92rem}
.lv-req{font-size:.72rem;color:var(--mu);margin-top:2px}
.lv-st{font-size:.68rem;padding:3px 8px;border-radius:9px;font-weight:700;flex-shrink:0}
.lst-d{background:rgba(0,204,102,.14);color:var(--gr)}
.lst-c{background:rgba(255,208,0,.14);color:var(--gd)}
.lst-l{background:rgba(255,255,255,.05);color:var(--mu)}
.gfts-row{display:flex;flex-wrap:wrap;gap:6px}
.gft{background:rgba(255,255,255,.05);border-radius:9px;padding:5px 9px;font-size:.72rem;display:flex;align-items:center;gap:4px;border:1px solid rgba(255,255,255,.07)}
.gft.ul{background:rgba(0,204,102,.09);border-color:rgba(0,204,102,.22);color:var(--gr)}
.gft.lk{filter:grayscale(1);opacity:.45}

/* BATTLES */
#bat-sc{overflow-y:auto;padding-top:56px;padding-bottom:76px}
.bt-hero{margin:12px;background:linear-gradient(135deg,rgba(255,59,110,.13),rgba(124,59,255,.13));border-radius:20px;padding:18px;border:1px solid rgba(255,59,110,.2);text-align:center}
.bt-hero h2{font-family:'Syne',sans-serif;font-weight:900;font-size:1.25rem;margin-bottom:4px}
.bt-h