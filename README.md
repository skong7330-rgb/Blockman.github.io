<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Blockcraft</title>
<link rel="manifest" href="manifest.webmanifest">
<meta name="theme-color" content="#3b6ea5">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="Blockcraft">
<link rel="icon" type="image/svg+xml" href="icon.svg">
<style>
:root{--panel:rgba(30,30,30,.85);--edge:#c6c6c6;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--panel:rgba(12,12,12,.88)}}
:root[data-theme="dark"]{--panel:rgba(12,12,12,.88)}
html,body{height:100%;margin:0;background:#7fb8e6;overflow:hidden;font:bold 15px "Courier New",monospace;color:#fff;text-shadow:2px 2px #3f3f3f;user-select:none}
canvas.main{display:block;width:100%;height:100%}
/* Minecraft Java-style dirt background for menus */
#msg{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,.52);cursor:pointer;text-align:center;z-index:50}
#msg.menu-bg{background:rgba(0,0,0,.35)}
#msg.java-main{background:linear-gradient(180deg,rgba(0,0,0,.18),rgba(0,0,0,.48));align-items:center}
#msg.java-main:before{content:"";position:absolute;inset:0;pointer-events:none;background:linear-gradient(90deg,rgba(0,0,0,.16),transparent 35%,transparent 65%,rgba(0,0,0,.16))}
#box{background:var(--panel);border:3px solid #222;outline:2px solid var(--edge);padding:22px 30px;max-width:360px;line-height:1.7;position:relative}
#box.java-main-box{background:transparent;border:0;outline:0;padding:0;width:min(520px,92vw);max-width:none;line-height:1.2}
#box.wide{max-width:580px;background:#c6c6c6;border:3px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;color:#404040;text-shadow:none;text-align:left;padding:10px}
#msg h1{margin:0 0 8px;font-size:32px}
/* Minecraft Java-style beveled buttons */
button{display:block;margin:8px auto;font:inherit;padding:8px 18px;min-width:220px;background:#8b8b8b;color:#fff;border:3px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;cursor:pointer;text-shadow:2px 2px #3f3f3f;transition:background .06s}
button:hover{background:#5b7fbd;border-color:#fff #25385f #25385f #fff}
button:active{border-color:#555 #fff #fff #555;background:#707070}
button.small{min-width:80px;padding:4px 10px;font-size:13px;display:inline-block;margin:2px}
.java-main-buttons{margin:10px auto 0;width:min(460px,88vw)}
.java-main-buttons button{width:100%;margin:7px 0;min-width:0;height:42px;font-size:16px}
.java-row{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.java-logo{position:relative;display:inline-block;margin:0 auto 5px;font-family:Impact,"Arial Black",sans-serif;font-size:clamp(52px,9vw,86px);font-weight:900;letter-spacing:-3px;line-height:.82;color:#eee;text-transform:uppercase;-webkit-text-stroke:2px #111;text-shadow:5px 5px 0 #333,8px 8px 0 #111;transform:scaleX(.92)}
.java-logo .logo-sub{display:block;font-family:"Courier New",monospace;font-size:12px;letter-spacing:2px;line-height:1;margin-top:12px;color:#ddd;text-shadow:2px 2px #111;-webkit-text-stroke:0}
.java-splash{font-size:16px;color:#ffff55;text-shadow:2px 2px #222;font-style:italic;transform:rotate(-12deg);position:absolute;right:-105px;top:35px;white-space:nowrap;animation:menuSplash 1.4s ease-in-out infinite alternate}
@keyframes menuSplash{from{transform:rotate(-12deg) scale(1)}to{transform:rotate(-12deg) scale(1.08)}}
.java-footer{position:fixed;left:12px;bottom:10px;text-align:left;color:#fff;font:12px "Courier New",monospace;text-shadow:2px 2px #222;line-height:1.35}
.java-footer-right{position:fixed;right:12px;bottom:10px;text-align:right;color:#fff;font:12px "Courier New",monospace;text-shadow:2px 2px #222}

/* ===== Java-style world/inventory UI overhaul ===== */
#msg.world-screen{background:#c6c6c6;color:#202020;text-shadow:none;align-items:center}
#msg.world-screen #box{background:#c6c6c6;border:3px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;color:#202020;text-shadow:none;width:min(760px,94vw);max-width:none;max-height:88vh;overflow:auto;padding:8px}
.mc-screen-title{font-size:25px;font-weight:bold;text-align:center;color:#202020;margin:4px 0 10px;text-shadow:1px 1px #fff}
.mc-tabs{display:flex;border-bottom:2px solid #555;margin:0 2px 8px}
.mc-tab{flex:1;margin:0!important;min-width:0!important;padding:7px 8px!important;height:auto!important;background:#8b8b8b!important;color:#fff!important;border:2px solid!important;border-color:#fff #555 #555 #fff!important;outline:0!important;text-shadow:2px 2px #3f3f3f!important}
.mc-tab.active{background:#c6c6c6!important;color:#202020!important;border-color:#555 #fff #fff #555!important;text-shadow:1px 1px #fff!important}
.mc-page{padding:4px 14px 8px;text-align:left}
.mc-field{margin:8px 0}.mc-field label{display:block;margin-bottom:3px;color:#202020;font-size:13px}.mc-field input,.mc-field select{box-sizing:border-box;width:100%;height:32px;background:#fff;border:2px solid;border-color:#555 #fff #fff #555;outline:1px solid #000;color:#202020;font:14px 'Courier New',monospace;padding:4px;text-shadow:none}
.mc-toggle{display:flex;align-items:center;justify-content:space-between;background:#aaa;border:2px solid;border-color:#555 #fff #fff #555;padding:7px 9px;margin:7px 0;color:#202020}
.mc-toggle button{min-width:110px;width:110px;margin:0;padding:5px 8px}
.mc-world-actions{display:flex;gap:8px;justify-content:space-between;border-top:2px solid #777;padding-top:8px;margin-top:8px}.mc-world-actions button{min-width:0;flex:1;margin:0}
.world-list{background:#555;border:3px solid;border-color:#373737 #fff #fff #373737;padding:3px;max-height:430px;overflow:auto}
.world-row{display:grid;grid-template-columns:64px 1fr auto;gap:10px;align-items:center;background:#aaa;border:2px solid;border-color:#fff #555 #555 #fff;padding:6px;margin:3px 0;color:#202020;text-align:left;cursor:pointer}
.world-row.selected{background:#c6c6c6;outline:2px solid #fff;box-shadow:inset 0 0 0 2px #555}
.world-thumb{width:60px;height:46px;position:relative;overflow:hidden;border:2px solid #222;background:linear-gradient(#78b9df 0 48%,#78a84c 49% 61%,#70543b 62% 100%)}
.world-thumb:before{content:"";position:absolute;left:8px;bottom:11px;width:22px;height:23px;background:#6a4b32;box-shadow:17px -7px 0 2px #6a4b32,32px 3px 0 5px #6a4b32}
.world-thumb:after{content:"";position:absolute;left:4px;bottom:29px;width:34px;height:10px;background:#4e9a3e;box-shadow:19px -7px 0 1px #4e9a3e,33px 1px 0 3px #4e9a3e}
.world-info{min-width:0}.world-name{font-size:16px;font-weight:bold;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}.world-meta{font-size:11px;color:#404040;margin-top:4px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.world-select-dot{width:10px;height:10px;background:#555;display:inline-block;margin-right:5px}.world-row.selected .world-select-dot{background:#5b7fbd}
.world-buttons{display:flex;gap:4px}.world-buttons button{min-width:82px!important;width:82px!important;margin:0!important;padding:5px 4px!important;font-size:11px!important}
.mc-small{font-size:11px;color:#404040;text-align:center;margin:6px}.mc-description{font-size:11px;color:#404040;line-height:1.4;margin:7px 0}.mc-section{font-size:14px;font-weight:bold;border-bottom:1px solid #777;padding-bottom:3px;margin-top:10px;color:#202020}
/* Pixel GUI surfaces */
.inv-panel{background:#c6c6c6;border:3px solid;border-color:#555 #fff #fff #555;box-shadow:0 0 0 2px #000;padding:5px}
.is{background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737;box-shadow:none}
.is:hover{background:#9e9ec2}.grid{gap:0}
#bar{background:#8b8b8b;border:3px solid;border-color:#373737 #fff #fff #373737;outline:2px solid #000;padding:2px}
.s{width:52px;height:52px;background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737}.s.on{border-color:#fff;box-shadow:0 0 0 2px #fff,inset 0 0 0 2px #373737}

/* Login form */
.mc-title{font-size:48px;color:#f0f0f0;text-shadow:3px 3px 0 #2a2a2a,6px 6px 0 #1a1a1a;letter-spacing:2px;margin:0 0 4px}
.mc-subtitle{font-size:14px;color:#e0e000;text-shadow:1px 1px #222;margin:0 0 16px}
.splash{font-size:18px;color:#ff0;text-shadow:1px 1px #222;font-style:italic;transform:rotate(-15deg);display:inline-block;margin:4px 0 12px}
/* Login form */
.login-box{background:rgba(0,0,0,.7);border:3px solid #555;outline:2px solid #c6c6c6;padding:24px 30px;max-width:340px}
.login-box h2{margin:0 0 12px;font-size:24px;color:#fff}
.login-box input{width:100%;box-sizing:border-box;margin:6px 0}
.login-box .user-info{color:#7f7;font-size:13px;margin:8px 0}
/* Hotbar slots */
#cross{position:fixed;left:50%;top:50%;width:20px;height:20px;z-index:20;margin:-10px;pointer-events:none;mix-blend-mode:difference}
#cross:before,#cross:after{content:"";position:absolute;background:#fff}
#cross:before{left:9px;top:2px;width:2px;height:16px}#cross:after{top:9px;left:2px;height:2px;width:16px}
#bar{position:fixed;left:50%;bottom:calc(4px + env(safe-area-inset-bottom,0px));transform:translateX(-50%);display:flex;background:rgba(0,0,0,.4);border:2px solid #373737;outline:2px solid #fff;padding:2px;gap:0}
.s{width:50px;height:50px;box-sizing:border-box;position:relative;background:rgba(139,139,139,.3);border:2px solid #373737;image-rendering:pixelated}
.s canvas,.s img{width:100%;height:100%;image-rendering:pixelated;display:block}
.s i{position:absolute;left:3px;top:1px;font-size:11px;font-style:normal;color:#fff;text-shadow:1px 1px #222}
.s b{position:absolute;right:3px;bottom:1px;font-size:12px;color:#fff;text-shadow:1px 1px #222}
.s.on{border-color:#fff;box-shadow:0 0 0 2px #fff,inset 0 0 0 2px #fff;z-index:1}
#off{position:fixed;bottom:calc(4px + env(safe-area-inset-bottom,0px));left:calc(50% - 322px);width:50px;height:50px;border:2px solid #373737;outline:2px solid #fff;background:rgba(0,0,0,.4);box-sizing:border-box;padding:2px;display:none}
#off img{width:100%;height:100%;image-rendering:pixelated;display:block}
#off b{position:absolute;right:3px;bottom:1px;font-size:12px;color:#fff;text-shadow:1px 1px #222}
#status{position:fixed;left:50%;bottom:calc(58px + env(safe-area-inset-bottom,0px));transform:translateX(-50%);display:flex;flex-direction:column;align-items:center;gap:2px;pointer-events:none}
#bars{display:flex;justify-content:space-between;width:454px;align-items:flex-end}
#hearts{display:flex;flex-wrap:wrap;width:227px;justify-content:flex-start}
#hunger{display:flex;flex-wrap:wrap;width:227px;justify-content:flex-end;flex-direction:row-reverse}
#xpbar{position:relative;width:454px;height:9px;background:#000;outline:1px solid #222;display:none}
#xpfill{height:100%;width:0;background:#7fff00}
#xplvl{position:absolute;left:50%;top:-16px;transform:translateX(-50%);font-size:16px;color:#7fff00;text-shadow:1px 1px #222,-1px -1px #222,1px -1px #222,-1px 1px #222;display:none}
#pw{position:fixed;left:50%;top:calc(50% + 24px);width:60px;height:5px;margin-left:-30px;background:rgba(0,0,0,.5);display:none}
#pb{height:100%;width:0;background:#fff}
#dbg{position:fixed;left:10px;top:calc(8px + env(safe-area-inset-top,0px));font-size:13px;pointer-events:none;text-shadow:1px 1px #222}
#uw{position:fixed;inset:0;background:rgba(20,50,150,.45);display:none;pointer-events:none}
.inv-title{font-size:14px;color:#404040;margin:0 0 6px;font-weight:bold;padding-left:2px}
.inv-panel{background:#8b8b8b;border:3px solid;border-color:#373737 #fff #fff #373737;padding:4px;display:inline-block;vertical-align:top}
.is{width:40px;height:40px;box-sizing:border-box;background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737;position:relative;display:inline-block;vertical-align:top;cursor:grab}
.is:hover{background:#a8a8c8}
.is[data-sel]{outline:2px solid #fff;z-index:1}
.is[draggable="true"]{cursor:grab}
.is.drag-over{outline:3px solid #0f0;z-index:2}
.is img{width:100%;height:100%;image-rendering:pixelated;display:block}
.is b{position:absolute;right:2px;bottom:0;font-size:12px;color:#fff;text-shadow:1px 1px #222}
.grid{display:grid;grid-template-columns:repeat(9,40px);gap:0;margin:4px 0}
.inv-row{display:flex;gap:6px;margin-bottom:8px;align-items:flex-start}
.inv-left{display:flex;flex-direction:column;gap:4px;align-items:center}
.armor-slots{display:grid;grid-template-columns:repeat(2,40px);gap:0}
.craft-area{display:flex;align-items:center;gap:4px}
.craft-grid{display:grid;grid-template-columns:repeat(2,40px);gap:0}
input{font:inherit;padding:6px;width:210px;display:block;margin:8px auto;background:#222;color:#fff;border:2px solid #888;text-align:center}
small{display:block;margin-top:10px;font-size:12px;line-height:1.5;color:#555}
#hp{display:none}
#actionbar{position:fixed;left:50%;top:calc(40%);transform:translateX(-50%);background:rgba(0,0,0,.6);color:#fff;padding:6px 16px;font-size:14px;display:none;text-align:center;text-shadow:1px 1px #222;pointer-events:none;border:1px solid #555}
/* Command/chat input */
#cmdinput{position:fixed;bottom:calc(70px + env(safe-area-inset-bottom,0px));left:50%;transform:translateX(-50%);width:400px;padding:6px 10px;background:rgba(0,0,0,.6);border:2px solid #555;color:#fff;font:bold 15px "Courier New",monospace;text-align:left;text-shadow:1px 1px #222;display:none;z-index:100}
/* Multiplayer status */
#mpstatus{position:fixed;right:10px;top:calc(8px + env(safe-area-inset-top,0px));font-size:13px;color:#7f7;text-shadow:1px 1px #222;display:none;pointer-events:none}
/* Settings slider */
.slider-row{margin:8px 0;color:#404040;font-size:13px}
.slider-row label{display:inline-block;width:120px}
input[type=range]{width:200px;vertical-align:middle}

/* ===== Minecraft Java HUD scale/layout ===== */
#bar{position:fixed;left:50%;bottom:4px;transform:translateX(-50%);width:182px;height:22px;box-sizing:border-box;background:#8b8b8b;border:2px solid #000;outline:0;padding:1px;display:flex;gap:0;z-index:20}
.s{width:20px;height:20px;box-sizing:border-box;position:relative;background:#8b8b8b;border:2px solid #373737;image-rendering:pixelated;padding:0}
.s img{width:16px!important;height:16px!important;margin:0;object-fit:contain}
.s i{display:none}.s b{right:1px;bottom:-1px;font-size:10px}.s.on{border-color:#fff;box-shadow:inset 0 0 0 1px #555;z-index:1}
#off{position:fixed;left:calc(50% - 114px);bottom:4px;width:22px;height:22px;box-sizing:border-box;background:#8b8b8b;border:2px solid #000;outline:0;padding:1px;z-index:19}
#off img{width:18px!important;height:18px!important;object-fit:contain}.s img,#off img{image-rendering:pixelated}
#status{position:fixed;left:50%;bottom:28px;transform:translateX(-50%);width:182px;display:flex;flex-direction:column;align-items:center;gap:1px;pointer-events:none;z-index:18}
#bars{display:grid;grid-template-columns:91px 91px;width:182px;align-items:end}
#hearts,#hunger{display:flex;flex-wrap:nowrap;width:91px;height:9px;gap:0;overflow:hidden}
#hearts{justify-content:flex-start}.hearts img,#hearts img{width:9px!important;height:9px!important}.hunger img,#hunger img{width:9px!important;height:9px!important}
#hunger{justify-content:flex-end;flex-direction:row-reverse}
#armor{display:flex;position:absolute;left:0;bottom:9px;width:91px;height:9px;gap:0;justify-content:flex-start}
#armor span{width:9px;height:9px;display:block}
#xpbar{position:absolute;bottom:-7px;left:0;width:182px;height:4px;background:#000;outline:1px solid #222;display:none}
#xpfill{height:100%;width:0;background:#80ff20}.xplvl{font-size:10px!important}
#xplvl{top:-13px;font-size:11px}
#attack-indicator{position:fixed;left:50%;bottom:29px;transform:translateX(-50%);width:16px;height:2px;background:#222;z-index:19;display:none;pointer-events:none}
#attack-fill{height:100%;width:0;background:#fff}
/* Java-style inventory GUI */
#box.wide{max-width:420px;padding:8px;background:#c6c6c6;border:2px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000}
.inv-row{display:flex;gap:14px;margin-bottom:8px;align-items:flex-start}.inv-left{display:flex;flex-direction:row;gap:6px;align-items:flex-start}
.inv-title{font-size:11px;color:#404040;margin:2px 0 3px;font-weight:bold;padding-left:2px}
.inv-panel{background:#c6c6c6;border:0;box-shadow:none;padding:2px}
.is{width:36px;height:36px;background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737;box-sizing:border-box}
.grid{display:grid;grid-template-columns:repeat(9,36px);gap:0;margin:0}.is img{width:32px;height:32px}
.armor-slots{display:grid;grid-template-columns:36px;gap:0}.craft-area{display:flex;align-items:center;gap:8px}.craft-grid{display:grid;grid-template-columns:repeat(2,36px);gap:0}
/* inventory panel gets the familiar 9x3 + hotbar proportions */
#msg.world-screen #box.wide{max-width:420px}

/* ===== Java HUD scale/layout =====
   Compact classic Java HUD: 9-slot hotbar, hearts/hunger above, XP below. */
#bar{position:fixed;left:50%;bottom:4px;transform:translateX(-50%);width:182px;height:22px;padding:0;background:#0b0b0b;border:2px solid #101010;box-sizing:content-box;display:flex;gap:0;z-index:20;image-rendering:pixelated}
#bar .s{width:20px;height:22px;background:#8b8b8b;border:2px solid;border-color:#373737 #c6c6c6 #c6c6c6 #373737;box-sizing:border-box;box-shadow:none;padding:0;overflow:hidden}
#bar .s.on{border-color:#fff;box-shadow:inset 0 0 0 1px #555;z-index:2}
#bar .s img{width:16px!important;height:16px!important;margin:1px 0 0 1px;object-fit:contain;image-rendering:pixelated}
#bar .s i{display:none}
#bar .s b{position:absolute;right:1px;bottom:-1px;font:bold 8px Arial;color:#fff;text-shadow:1px 1px #222}
#off{left:calc(50% + 102px);bottom:4px;width:22px;height:22px;background:#0b0b0b;border:2px solid #101010;outline:0;padding:0;display:none;z-index:19}
#off img{width:18px!important;height:18px!important;margin:1px;object-fit:contain;image-rendering:pixelated}
#off b{position:absolute;right:1px;bottom:-1px;color:#fff;font:bold 8px Arial;text-shadow:1px 1px #222}
#status{position:fixed;left:50%;bottom:28px;transform:translateX(-50%);width:182px;display:flex;flex-direction:column;align-items:center;gap:1px;pointer-events:none;z-index:18}
#bars{width:182px;grid-template-columns:91px 91px;display:grid;align-items:end}
#hearts,#hunger{width:91px;height:9px;display:flex;flex-wrap:nowrap;gap:0;overflow:hidden}
#hearts{justify-content:flex-start}
#hunger{justify-content:flex-end;flex-direction:row-reverse}
#hearts img,#hunger img{width:9px!important;height:9px!important;image-rendering:pixelated;margin:0;padding:0}
#armor{display:flex;position:absolute;left:0;bottom:9px;width:91px;height:9px;gap:0;justify-content:flex-start}
#armor span{width:9px;height:9px;display:block}
#xpbar{position:absolute;left:0;bottom:-7px;width:182px;height:5px;background:#111;outline:1px solid #222;display:none}
#xpfill{height:100%;width:0;background:#65d51f}
#xplvl{top:-13px;font-size:11px}
#attack-indicator{position:fixed;left:50%;bottom:29px;transform:translateX(-50%);width:16px;height:2px;background:#222;z-index:19;display:none;pointer-events:none}
#attack-fill{height:100%;width:0;background:#fff}
/* Inventory: Java-like centered gray container, 9x3 storage and bottom hotbar */
#box.mc-inventory{width:176px;height:166px;max-width:none;min-width:0;padding:0;background:#c6c6c6;border:2px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;color:#404040;text-shadow:none;box-sizing:border-box;font-family:Arial,sans-serif;font-weight:400}
.mc-inv-root{position:relative;width:176px;height:166px;image-rendering:pixelated;background:#c6c6c6;box-sizing:border-box}
.java-slot{position:relative;width:18px;height:18px;box-sizing:border-box;background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737;overflow:hidden}
.java-slot img{position:absolute;left:0;top:0;width:16px;height:16px;object-fit:contain;image-rendering:pixelated}.java-slot b{position:absolute;right:1px;bottom:-1px;color:#fff;font:bold 10px Arial;text-shadow:1px 1px #222}
.mc-inv-preview{position:absolute;left:6px;top:6px;width:82px;height:73px}.mc-avatar-preview{position:absolute;left:26px;top:5px;width:48px;height:66px}.avp-head{position:absolute;left:13px;top:0;width:22px;height:22px;background:#c98f70;box-shadow:inset 0 -3px #9a654f,0 -3px 0 #4a2c1b}.avp-body{position:absolute;left:14px;top:22px;width:20px;height:25px;background:#3f78c5}.avp-arm{position:absolute;top:23px;width:8px;height:25px;background:#3f78c5}.avp-arm.l{left:5px}.avp-arm.r{right:5px}.avp-leg{position:absolute;top:47px;width:9px;height:19px;background:#273d91}.avp-leg.l{left:14px}.avp-leg.r{right:14px}
.mc-armor{position:absolute;left:0;top:0;display:flex;flex-direction:column;gap:1px}.mc-armor .armor{width:18px;height:18px}.mc-crafting-title{position:absolute;left:96px;top:4px;font:11px Arial;color:#404040}.mc-crafting{position:absolute;left:96px;top:16px;width:72px;height:40px}.craft2{display:grid;grid-template-columns:18px 18px;grid-template-rows:18px 18px;gap:0;width:36px}.craft-arrow{position:absolute;left:39px;top:10px;font:20px Arial;color:#777}.craft-output{position:absolute;left:54px;top:4px;width:18px;height:18px;background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737}.craft-output img{width:16px;height:16px;image-rendering:pixelated}.craft-output b{position:absolute;right:1px;bottom:-1px;color:#fff;font:bold 10px Arial;text-shadow:1px 1px #222}.recipe-book{position:absolute;left:73px;top:59px;width:22px!important;height:22px!important;min-width:0!important;margin:0!important;padding:0!important;background:#8b8b8b;border:2px solid;border-color:#fff #555 #555 #fff;outline:0;font:16px Arial;color:#2b7a3e;text-shadow:none}.mc-inv-grid{position:absolute;left:6px;top:82px;display:grid;grid-template-columns:repeat(9,18px);grid-template-rows:repeat(3,18px)}.mc-hotbar-grid{position:absolute;left:6px;top:141px;display:grid;grid-template-columns:repeat(9,18px)}.mc-hotbar-grid .java-slot.sel{outline:1px solid #fff;box-shadow:inset 0 0 0 1px #555}.mc-inv-offhand{position:absolute;left:62px;top:59px}.inv-done{display:none!important}

/* ===== FINAL HUD SCALE + RECIPE BOOK ===== */
#bar{width:264px;height:32px;bottom:8px;padding:0;transform:translateX(-50%);}
#bar .s{width:29.333px;height:32px;border-width:3px}
#bar .s img{width:24px!important;height:24px!important;margin:2px 0 0 2px!important}
#bar .s b{right:2px;bottom:0;font-size:11px}
#status{bottom:45px;width:264px}
#bars{width:264px;grid-template-columns:132px 132px}
#hearts,#hunger{width:132px;height:13px}
#hearts img,#hunger img{width:13px!important;height:13px!important}
#armor{bottom:13px;width:132px;height:13px}
#armor span{width:13px;height:13px;font-size:13px!important}
#xpbar{width:264px;height:7px;bottom:-10px}
#xplvl{top:-17px;font-size:14px}
#attack-indicator{bottom:46px;width:24px;height:3px}
#off{left:calc(50% + 146px);bottom:8px;width:32px;height:32px}
#off img{width:28px!important;height:28px!important;margin:1px}
#off b{font-size:11px}

/* Larger Java-style inventory */
#box.mc-inventory{width:352px;height:332px}
.mc-inv-root{width:352px;height:332px}
.java-slot{width:27px;height:27px;border-width:3px}
.java-slot img{width:24px;height:24px}
.java-slot b{font-size:13px;right:1px;bottom:-1px}
.mc-inv-preview{left:9px;top:9px;width:123px;height:110px}
.mc-avatar-preview{left:39px;top:8px;width:72px;height:99px;transform:scale(1.5);transform-origin:top left}
.mc-armor .armor{width:27px;height:27px}
.mc-crafting-title{left:144px;top:6px;font-size:16px}
.mc-crafting{left:144px;top:24px;width:108px;height:60px}
.craft2{grid-template-columns:27px 27px;grid-template-rows:27px 27px;width:54px}
.craft-arrow{left:58px;top:13px;font-size:28px}
.craft-output{left:81px;top:5px;width:27px;height:27px;border-width:3px}
.craft-output img{width:24px;height:24px}
.craft-output b{font-size:13px}
.recipe-book{left:110px;top:88px;width:33px!important;height:33px!important;font-size:22px}
.mc-inv-grid{left:9px;top:123px;grid-template-columns:repeat(9,27px);grid-template-rows:repeat(3,27px)}
.mc-hotbar-grid{left:9px;top:211px;grid-template-columns:repeat(9,27px)}
.mc-inv-offhand{left:93px;top:88px}

/* Large Java-style recipe book: all recipes fit without a scrolling list. */
.recipe-book-panel{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);width:min(720px,92vw);height:min(560px,86vh);background:#c6c6c6;z-index:20;box-sizing:border-box;padding:14px;border:3px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000}
.recipe-book-head{height:42px;display:flex;align-items:center;justify-content:space-between;color:#404040;font:bold 22px Arial}
.recipe-book-sub{font:14px Arial;color:#555;margin:0 0 8px}
.recipe-book-list{height:calc(100% - 78px);overflow:hidden;display:grid;grid-template-columns:repeat(3,minmax(0,1fr));grid-auto-rows:minmax(66px,1fr);gap:7px;padding:2px}
.recipe-card{position:relative;min-height:66px;background:#8b8b8b;border:3px solid;border-color:#373737 #fff #fff #373737;display:flex;align-items:center;gap:9px;padding:6px;box-sizing:border-box;cursor:pointer;color:#fff;text-align:left}
.recipe-card:hover{background:#9f9f9f}
.recipe-card.locked{opacity:.42}
.recipe-card img{width:42px;height:42px;image-rendering:pixelated;flex:0 0 auto}
.recipe-card .rn{font:bold 14px Arial;line-height:16px;text-shadow:1px 1px #222;overflow:hidden}
.recipe-card .rcount{position:absolute;right:5px;bottom:3px;font:bold 12px Arial;color:#fff;text-shadow:1px 1px #222}
.recipe-close{height:34px;min-width:86px;background:#8b8b8b;border:3px solid;border-color:#fff #373737 #373737 #fff;font:bold 14px Arial;color:#222;cursor:pointer}
.recipe-close:hover{background:#aaa}
/* More faithful Java inventory proportions + usable drag targets */
.mc-inventory{image-rendering:pixelated}
.mc-inv-root{width:320px;height:302px;background:#c6c6c6;border:3px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;box-sizing:border-box;image-rendering:pixelated}
.mc-inv-root .java-slot{width:36px;height:36px;background:#8b8b8b;border:3px solid;border-color:#373737 #fff #fff #373737;box-sizing:border-box;cursor:grab}
.mc-inv-root .java-slot:hover{background:#999}
.mc-inv-root .java-slot.drag-over{outline:2px solid #fff;box-shadow:inset 0 0 0 2px #555}
.mc-inv-root .java-slot.dragging{opacity:.45}
.mc-inv-root .java-slot.empty{cursor:default}
.mc-inv-root .java-slot img{width:32px;height:32px;left:0;top:0}
.mc-inv-root .java-slot b{font-size:13px}
.mc-inv-grid{left:10px;top:166px;grid-template-columns:repeat(9,36px);grid-template-rows:repeat(3,36px)}
.mc-hotbar-grid{left:10px;top:294px;grid-template-columns:repeat(9,36px)}
.mc-hotbar-grid .java-slot.sel{outline:2px solid #fff;box-shadow:inset 0 0 0 2px #555}
.mc-inv-preview{left:10px;top:10px;width:158px;height:145px}
.mc-avatar-preview{left:54px;top:10px;transform:scale(2.15);transform-origin:top left}
.mc-armor{left:2px;top:6px;gap:3px}.mc-armor .armor{width:36px;height:36px}
.mc-crafting-title{left:196px;top:12px;font-size:17px}.mc-crafting{left:196px;top:36px;width:150px;height:84px}.craft2{grid-template-columns:36px 36px;grid-template-rows:36px 36px;width:72px}.craft-arrow{left:78px;top:23px;font-size:28px}.craft-output{left:112px;top:10px;width:36px;height:36px;border-width:3px}.craft-output img{width:30px;height:30px}.craft-output b{font-size:14px}.recipe-book{left:196px;top:122px;width:36px!important;height:36px!important;font-size:24px!important}
@media(max-width:700px){.recipe-book-panel{width:94vw;height:84vh}.recipe-book-list{grid-template-columns:repeat(2,minmax(0,1fr));grid-auto-rows:66px}.recipe-card img{width:36px;height:36px}.recipe-card .rn{font-size:12px;line-height:14px}}

/* ===== Creative inventory ===== */
#box.mc-inventory.creative-inv{width:min(620px,94vw);height:min(470px,88vh);max-width:none;padding:0}
.creative-inv .mc-inv-root{width:100%;height:100%;background:#c6c6c6}
.creative-tabs{position:absolute;left:10px;top:10px;right:10px;height:30px;display:flex;gap:3px}
.creative-tab{width:30px!important;min-width:30px!important;height:30px!important;margin:0!important;padding:0!important;background:#8b8b8b!important;border:3px solid!important;border-color:#fff #555 #555 #fff!important;outline:0!important;font:bold 16px Arial!important;color:#333!important;text-shadow:1px 1px #fff!important}
.creative-tab.active{background:#c6c6c6!important;border-color:#555 #fff #fff #555!important}
.creative-grid{position:absolute;left:10px;top:50px;display:grid;grid-template-columns:repeat(9,60px);grid-auto-rows:60px;gap:0}
.creative-grid .java-slot{width:60px;height:60px;border-width:3px;cursor:pointer}
.creative-grid .java-slot img{width:54px;height:54px}
.creative-grid .java-slot:hover{background:#aaa}
.creative-hotbar-label{position:absolute;left:10px;bottom:78px;font:13px Arial;color:#404040}
.creative-hotbar{position:absolute;left:10px;bottom:18px;display:grid;grid-template-columns:repeat(9,60px)}
.creative-hotbar .java-slot{width:60px;height:60px;border-width:3px}
.creative-hotbar .java-slot img{width:54px;height:54px}
.creative-close{position:absolute;right:10px;top:10px;width:30px!important;min-width:30px!important;height:30px!important;margin:0!important;padding:0!important;background:#8b8b8b!important;font:bold 20px Arial!important;color:#222!important;text-shadow:1px 1px #fff!important}
.creative-hint{position:absolute;left:10px;right:10px;bottom:1px;text-align:center;font:11px Arial;color:#555}
@media(max-width:700px){.creative-grid{grid-template-columns:repeat(9,calc((100vw - 32px)/9));grid-auto-rows:calc((100vw - 32px)/9)}.creative-grid .java-slot,.creative-hotbar .java-slot{width:calc((100vw - 32px)/9);height:calc((100vw - 32px)/9)}.creative-grid .java-slot img,.creative-hotbar .java-slot img{width:calc((100vw - 38px)/9);height:calc((100vw - 38px)/9)}.creative-hotbar{grid-template-columns:repeat(9,calc((100vw - 32px)/9))}}
</style></head><body>
<div id="cross"></div><div id="pw"><div id="pb"></div></div><div id="uw"></div><div id="dbg"></div>
<div id="status"><div id="bars"><div id="hearts"></div><div id="hunger"></div></div><div id="armor"></div><div id="xpbar"><div id="xpfill"></div><span id="xplvl"></span></div></div>
<div id="attack-indicator"><div id="attack-fill"></div></div><div id="bar"></div>
<div id="off"></div><div id="hp" style="display:none"></div>
<input id="cmdinput" type="text" placeholder="Type /help for commands" autocomplete="off">
<div id="actionbar"></div><div id="mpstatus"></div><div id="msg"><div id="box"></div></div>
<script>
(function(){
  window.addEventListener("error",function(e){
    if(!e||!e.message)return;
    var el=document.getElementById("boot-error");
    if(!el){el=document.createElement("div");el.id="boot-error";el.style.cssText="position:fixed;inset:0;z-index:99999;background:#111;color:#fff;padding:28px;font:14px monospace;white-space:pre-wrap;overflow:auto";document.body.appendChild(el)}
    el.textContent="Blockcraft could not start.\n\n"+e.message+(e.filename?"\n\n"+e.filename+":"+e.lineno:"")+"\n\nReload the page after checking the browser console.";
  });
  window.addEventListener("unhandledrejection",function(e){if(e.reason)window.dispatchEvent(new ErrorEvent("error",{message:String(e.reason.message||e.reason)}));});
})();
</script>
<script>/* Robust Three.js loader: try multiple CDNs before the game script executes. */
(function(){
  var urls=[
    "https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js",
    "https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js",
    "https://unpkg.com/three@0.128.0/build/three.min.js"
  ];
  var i=0;
  window.__bcLoadNext=function(){
    if(i>=urls.length){
      var e=document.createElement("div");e.style.cssText="position:fixed;inset:0;z-index:999999;background:#111;color:#fff;padding:28px;font:16px monospace;white-space:pre-wrap";
      e.textContent="Blockcraft could not start.\n\nThree.js could not be loaded from any of the available CDNs. Make sure your browser has internet access, then reload the file.";
      document.body.appendChild(e);return;
    }
    var u=urls[i++].replace(/\"/g,"&quot;");
    document.write('<script src="'+u+'" onerror="window.__bcLoadNext()"><\/script>');
  };
  window.__bcLoadNext();
})();</script>
<script>
/* Storage wrapper with in-memory fallback for sandboxed iframes */
const _ms={};const store={get(k){try{return localStorage.getItem(k)}catch(e){return _ms[k]||null}},set(k,v){try{localStorage.setItem(k,v)}catch(e){_ms[k]=String(v)}},del(k){try{localStorage.removeItem(k)}catch(e){delete _ms[k]}}};
/* =====================================================================
   BLOCKCRAFT v9 JAVA-STYLE — single-file voxel game
   SECTIONS:
   0. Config & Settings     · 1. Blocks, Items & Recipes  · 2. World Gen
   3. Textures               · 4. Renderer & Meshing        · 5. Sky & Mobs
   6. Hands & Hotbar UI      · 7. Inventory (drag-drop)     · 8. Redstone
   9. Commands               · 10. Player & Input           · 11. Menus & Save
   12. Villages              · 13. Main Loop
   ===================================================================== */

/* ====================== 0. CONFIG & SETTINGS ====================== */
const CFG={
  world:{height:96,seaLevel:24,viewChunks:6,terrainBase:-9,terrainHeight:36,noiseScale:44,
         treeChance:.015,caves:true,caveA:.69,caveB:.55,villageChance:.04},
  render:{fogNear:32,fogFar:96,underwaterFog:20},
  time:{dayLength:360,start:.2},
  player:{walk:4.317,sprint:5.612,sneak:1.3,fly:11,swimFactor:.65,jump:8.43,gravity:27.8,reach:4.5,hitbox:.30,
          fov:70,sprintFov:80,sensitivity:.0022,attackDamage:4},
  creative:{mineTime:.12,attackDamage:99},
  survival:{maxHp:20,maxFood:20,hungerSeconds:25,regenSeconds:4,regenMinFood:18,fallSpeed:13,startItems:{}},
  mobs:{pigCap:10,zombieCap:6,
        pig:{hp:8,wander:1.2,drop:{id:30,min:1,max:2}},
        zombie:{hp:12,wander:1.2,chase:2.8,aggro:28,damage:2,cooldown:1}},
  tools:{p:[[41,5],[40,3]],a:[[42,3]],s:[[43,3]]},
  save:{interval:8},
};
CFG.mobs.advanced={
  cow:{hp:10,wander:1.05,behavior:"passive",damage:0,drop:{id:30,min:1,max:3}},
  sheep:{hp:8,wander:1.1,behavior:"passive",damage:0,drop:{id:32,min:1,max:1}},
  chicken:{hp:4,wander:1.25,behavior:"passive",damage:0,drop:{id:30,min:1,max:1}},
  villager:{hp:20,wander:.8,behavior:"passive",damage:0},
  cat:{hp:10,wander:1.5,behavior:"passive",damage:0},
  wolf:{hp:8,wander:1.6,behavior:"neutral",damage:3},
  enderman:{hp:40,wander:1.2,behavior:"neutral",damage:7},
  piglin:{hp:16,wander:1.1,behavior:"neutral",damage:5},
  golem:{hp:100,wander:.8,behavior:"neutral",damage:10},
  skeleton:{hp:20,wander:1.1,behavior:"hostile",chase:2.4,aggro:30,damage:3,cooldown:1.4},
  creeper:{hp:20,wander:1.0,behavior:"hostile",chase:2.2,aggro:22,damage:18,cooldown:2.5},
  phantom:{hp:20,wander:2.6,behavior:"hostile",chase:3.2,aggro:35,damage:6,cooldown:1.2},
  creaking:{hp:20,wander:.7,behavior:"hostile",chase:1.8,aggro:24,damage:5,cooldown:1.2}
};
// Runtime settings (adjustable via Settings menu)
let settings={fov:70,renderDist:6,graphics:"fancy",sensitivity:.0022,showOffhand:true};
const isSurvivalMode=()=>mode==="survival"||mode==="hardcore";
const isCreativeMode=()=>mode==="creative";
let dimension="overworld",biomeName="Plains",hardcore=false,dimStates={};try{Object.assign(settings,JSON.parse(store.get("bcsettings")||"{}"))}catch(e){}
// The offhand HUD/slot is part of the normal inventory UI; it is not optional.
settings.showOffhand=true;
/* Lightweight WebAudio sound system: starts only after a user gesture. */
let audioCtx=null,audioMaster=.075;
function audioReady(){try{if(!audioCtx)audioCtx=new (window.AudioContext||window.webkitAudioContext)();if(audioCtx.state==="suspended")audioCtx.resume();return audioCtx}catch(e){return null}}
function playSfx(type,pitch=1){const c=audioReady();if(!c)return;const t=c.currentTime,o=c.createOscillator(),g=c.createGain();o.type=type==="step"?"triangle":type==="hurt"?"sawtooth":"square";const f={click:520,select:700,jump:330,land:110,attack:145,crit:210,break:75,place:190,drop:120,pickup:660,eat:250,hurt:90,death:55,open:420,close:300,swap:500,level:880,craft:600,swim:180}[type]||220;o.frequency.setValueAtTime(f*pitch,t);if(type==="jump"||type==="level")o.frequency.exponentialRampToValueAtTime(f*1.7,t+.13);else if(type==="attack"||type==="crit")o.frequency.exponentialRampToValueAtTime(f*.55,t+.09);else o.frequency.exponentialRampToValueAtTime(Math.max(35,f*.7),t+.07);g.gain.setValueAtTime(.0001,t);g.gain.exponentialRampToValueAtTime(audioMaster,t+.006);g.gain.exponentialRampToValueAtTime(.0001,t+(type==="break"?.11:.09));o.connect(g).connect(c.destination);o.start(t);o.stop(t+.13)}
let stepTimer=0;

/* ===================== 1. BLOCKS, ITEMS & RECIPES ===================== */
const BLOCKS={
  1:{name:"Grass",tiles:[0,1,2],hard:.6,tool:"s",drop:2},
  2:{name:"Dirt",tiles:[2,2,2],hard:.5,tool:"s"},
  3:{name:"Stone",tiles:[3,3,3],hard:1.3,tool:"p",needsTool:true,drop:8},
  4:{name:"Log",tiles:[5,4,5],hard:1,tool:"a"},
  5:{name:"Leaves",tiles:[6,6,6],hard:.15,drop:0,bonus:{id:32,chance:.15}},
  6:{name:"Planks",tiles:[7,7,7],hard:.9,tool:"a"},
  7:{name:"Sand",tiles:[8,8,8],hard:.4,tool:"s"},
  8:{name:"Cobblestone",tiles:[9,9,9],hard:1.5,tool:"p",needsTool:true},
  9:{name:"Water",tiles:[10,10,10],creative:false},
  10:{name:"Glass",tiles:[11,11,11],hard:.3},
  11:{name:"Furnace",tiles:[12,12,12],hard:1.5,tool:"p",needsTool:true},
  12:{name:"Torch",tiles:[13,13,13],hard:0},
  13:{name:"Redstone Ore",tiles:[14,14,14],hard:3,tool:"p",needsTool:true,drop:47},
  14:{name:"Redstone Lamp",tiles:[15,15,15],hard:.3,lamp:true},
  15:{name:"Lever",tiles:[16,16,16],hard:.3,lever:true},
  16:{name:"Bookshelf",tiles:[17,17,17],hard:1.5,tool:"a"},
  17:{name:"Stone Bricks",tiles:[18,18,18],hard:1.5,tool:"p",needsTool:true},
  18:{name:"Hay Block",tiles:[19,19,19],hard:.5},
  19:{name:"Gravel Path",tiles:[20,20,20],hard:.4,tool:"s"},
};
const ITEMS={30:{name:"Raw Pork",food:2},31:{name:"Stick"},32:{name:"Apple",food:3},33:{name:"Cooked Pork",food:6},
  40:{name:"Wood Pickaxe"},41:{name:"Stone Pickaxe"},42:{name:"Wood Axe"},43:{name:"Wood Shovel"},44:{name:"Shield",offhand:true},
  45:{name:"Bone"},47:{name:"Redstone"},48:{name:"Bookshelf Item"}};

/* ================= EXPANDED JAVA-STYLE CONTENT REGISTRY =================
   IDs 60+ are additional placeable blocks; IDs 300+ are additional items. This keeps block and item registries disjoint.
   The registry is intentionally data-driven so the creative inventory, hotbar,
   item icons and placement system can expose the same content pipeline. */
const EXTRA_BLOCK_NAMES=[
"Oak Planks","Spruce Log","Spruce Planks","Birch Log","Birch Planks","Jungle Log","Jungle Planks","Acacia Log","Acacia Planks","Dark Oak Log","Dark Oak Planks","Mangrove Log","Mangrove Planks","Cherry Log","Cherry Planks",
"Stone Slab","Smooth Stone","Stone Stairs","Cobblestone Slab","Cobblestone Stairs","Oak Slab","Oak Stairs","Oak Fence","Oak Fence Gate","Oak Door","Oak Trapdoor","Spruce Door","Birch Door","Jungle Door","Acacia Door","Dark Oak Door","Cherry Door",
"Coal Ore","Iron Ore","Gold Ore","Diamond Ore","Emerald Ore","Copper Ore","Deepslate","Deepslate Coal Ore","Deepslate Iron Ore","Deepslate Gold Ore","Deepslate Diamond Ore","Deepslate Emerald Ore","Deepslate Copper Ore","Andesite","Diorite","Granite","Tuff","Calcite","Dripstone Block","Pointed Dripstone","Amethyst Block","Buddding Amethyst","Obsidian","Crying Obsidian","Netherrack","Soul Sand","Soul Soil","Basalt","Blackstone","End Stone","Purpur Block","Nether Bricks","Red Nether Bricks","Quartz Block","Glowstone","Magma Block","Prismarine","Prismarine Bricks","Dark Prismarine","Sea Lantern","Ice","Packed Ice","Blue Ice","Snow Block","Clay","Terracotta","White Wool","Black Wool","Red Wool","Blue Wool","Green Wool","Yellow Wool","Orange Wool","Purple Wool","Pink Wool","Brown Wool","Gray Wool","Light Gray Wool","Cyan Wool","Lime Wool","Magenta Wool","Light Blue Wool","Glass Pane","Iron Bars","Ladder","Chain","Lantern","Soul Lantern","Campfire","Soul Campfire","Chest","Barrel","Crafting Table","Smithing Table","Stonecutter","Grindstone","Anvil","Enchanting Table","Beacon","End Portal Frame","Dragon Egg","Sponge","Wet Sponge","TNT","Note Block","Jukebox","Dispenser","Dropper","Piston","Sticky Piston","Observer","Hopper","Daylight Detector","Redstone Block","Target","Tripwire Hook","Scaffolding","Honey Block","Slime Block","Bee Nest","Beehive","Composter","Lodestone","Respawn Anchor","Crying Obsidian","Sculk","Sculk Catalyst","Sculk Sensor","Sculk Shrieker","Froglight","Copper Block","Cut Copper","Exposed Copper","Weathered Copper","Oxidized Copper","Copper Grate","Copper Bulb","Tuff Bricks","Chiseled Tuff","Trial Spawner","Vault","Crafter","Heavy Core","Bamboo Block","Bamboo Planks","Moss Block","Moss Carpet","Azalea Leaves","Flowering Azalea Leaves","Cherry Leaves","Mangrove Roots","Mud","Packed Mud","Mud Bricks","Mushroom Block","Nether Wart Block","Warped Wart Block","Hay Bale","Bookshelf","Lectern","Cartography Table","Fletching Table","Loom","Stone Button","Oak Button","Oak Pressure Plate","Weighted Pressure Plate","Iron Trapdoor","Cauldron","Water Cauldron","Lava Cauldron","Powder Snow","Cobweb","Fire","Soul Fire","Torchflower","Pitcher Plant"
];
EXTRA_BLOCK_NAMES.forEach((name,i)=>{const id=60+i,t=21+i;let top=t,side=t,bottom=t;
  if(/Slab|Stairs|Door|Trapdoor|Fence|Gate|Pane|Bars|Ladder|Chain|Button|Pressure Plate|Carpet|Torch|Flower|Plant|Fire|Cobweb|Cauldron|Lantern/.test(name)){top=t;side=t;bottom=t}
  BLOCKS[id]={name,tiles:[top,side,bottom],hard:.7+(i%5)*.25,tool:(i%3==0?"p":i%3==1?"a":"s")};
});
ITEMS[49]={name:"Wood Sword"};ITEMS[50]={name:"Stone Sword"};ITEMS[51]={name:"Iron Sword"};ITEMS[52]={name:"Diamond Sword"};ITEMS[53]={name:"Netherite Sword"};
const EXTRA_ITEM_NAMES=[
"Coal","Iron Ingot","Gold Ingot","Diamond","Emerald","Copper Ingot","Netherite Ingot","Raw Iron","Raw Gold","Raw Copper","Quartz","Amethyst Shard","Flint","String","Feather","Leather","Gunpowder","Slimeball","Magma Cream","Blaze Rod","Blaze Powder","Ender Pearl","Ender Eye","Ghast Tear","Nether Star","Shulker Shell","Phantom Membrane","Spider Eye","Fermented Spider Eye","Sugar","Wheat","Wheat Seeds","Carrot","Potato","Baked Potato","Beetroot","Beetroot Seeds","Melon Slice","Pumpkin Seeds","Cookie","Bread","Golden Apple","Enchanted Golden Apple","Milk Bucket","Water Bucket","Lava Bucket","Powder Snow Bucket","Bucket","Arrow","Bow","Crossbow","Trident","Mace","Fishing Rod","Flint and Steel","Shears","Lead","Name Tag","Saddle","Map","Compass","Clock","Spyglass","Elytra","Totem of Undying","Firework Rocket","Boat","Minecart","Chest Minecart","Furnace Minecart","Hopper Minecart","Oak Sign","Book","Written Book","Book and Quill","Paper","Map Item"
];
EXTRA_ITEM_NAMES.forEach((name,i)=>{const id=300+i;ITEMS[id]={name,tiles:[21+(i%EXTRA_BLOCK_NAMES.length),21+(i%EXTRA_BLOCK_NAMES.length),21+(i%EXTRA_BLOCK_NAMES.length)]};});

// Dimension-specific and feature blocks. Legacy IDs above remain unchanged so old worlds stay compatible.
const SPECIAL_BLOCKS={
  249:{name:"Lava",tiles:[214,214,214],hard:100,creative:true},
  250:{name:"Sulfur Ore",tiles:[215,215,215],hard:3,tool:"p",needsTool:true,drop:410},
  251:{name:"Sulfur Block",tiles:[216,216,216],hard:1.2,tool:"p"},
  252:{name:"Crimson Stem",tiles:[217,217,217],hard:2,tool:"a"},
  253:{name:"Warped Stem",tiles:[218,218,218],hard:2,tool:"a"},
  254:{name:"Chorus Plant",tiles:[219,219,219],hard:.4},
  255:{name:"Chorus Flower",tiles:[220,220,220],hard:.4},
  256:{name:"Nether Portal",tiles:[221,221,221],hard:0,creative:false},
  257:{name:"End Portal",tiles:[222,222,222],hard:0,creative:false},
  258:{name:"End Gateway",tiles:[223,223,223],hard:0,creative:false},
  259:{name:"Crying Obsidian Portal",tiles:[224,224,224],hard:50,tool:"p",needsTool:true},
  260:{name:"Redstone Repeater",tiles:[225,225,225],hard:.2,repeater:true},
  261:{name:"Redstone Comparator",tiles:[226,226,226],hard:.2,comparator:true}
};
Object.assign(BLOCKS,SPECIAL_BLOCKS);
const SPECIAL_ITEMS={
  400:{name:"Leather Helmet",armor:1},401:{name:"Leather Chestplate",armor:3},402:{name:"Leather Leggings",armor:2},403:{name:"Leather Boots",armor:1},
  404:{name:"Iron Helmet",armor:2},405:{name:"Iron Chestplate",armor:6},406:{name:"Iron Leggings",armor:5},407:{name:"Iron Boots",armor:2},
  408:{name:"Diamond Helmet",armor:3},409:{name:"Diamond Chestplate",armor:8},410:{name:"Sulfur",food:0},
  411:{name:"Diamond Leggings",armor:6},412:{name:"Diamond Boots",armor:3},413:{name:"Netherite Helmet",armor:3},414:{name:"Netherite Chestplate",armor:8},415:{name:"Netherite Leggings",armor:6},416:{name:"Netherite Boots",armor:3},
  417:{name:"Potion of Healing",food:0,heal:6},418:{name:"Potion of Swiftness",food:0,effect:"speed"},419:{name:"Potion of Fire Resistance",food:0,effect:"fire_resistance"},
  420:{name:"Enchanted Book",enchanted:true},421:{name:"Wolf Spawn Egg"},422:{name:"Cow Spawn Egg"},423:{name:"Sheep Spawn Egg"},424:{name:"Chicken Spawn Egg"},425:{name:"Cat Spawn Egg"},426:{name:"Enderman Spawn Egg"},427:{name:"Creeper Spawn Egg"},428:{name:"Skeleton Spawn Egg"},429:{name:"Phantom Spawn Egg"},430:{name:"Piglin Spawn Egg"},431:{name:"Iron Golem Spawn Egg"}
};
Object.assign(ITEMS,SPECIAL_ITEMS);

const JAVA_STRUCTURES=[
"Village","Desert Pyramid","Jungle Pyramid","Swamp Hut","Igloo","Woodland Mansion","Pillager Outpost","Ocean Monument","Ocean Ruin","Shipwreck","Buried Treasure","Nether Fortress","Bastion Remnant","Ruined Portal","Stronghold","End City","Ancient City","Trail Ruins","Trial Chambers","Mineshaft","Dungeon","Geode","Fossil","Witch Hut","Monster Room","End Gateway","Nether Fossil","Amethyst Geode"
];

const RECIPES=[["1 Log -> 4 Planks",{4:1},{6:4}],["2 Planks -> 4 Sticks",{6:2},{31:4}],
  ["Wood Pickaxe",{6:3,31:2},{40:1}],["Stone Pickaxe",{8:3,31:2},{41:1}],["Wood Axe",{6:3,31:2},{42:1}],
  ["Wood Shovel",{6:1,31:2},{43:1}],["Furnace (8 Cobble)",{8:8},{11:1}],["2 Cobble -> 1 Stone",{8:2},{3:1}],
  ["Smelt Sand -> Glass",{7:1,6:1},{10:1},1],["Cook Pork",{30:1,6:1},{33:1},1],
  ["Shield",{6:1,31:1},{44:1}],["4 Torches",{6:1,31:1},{12:4}],
  ["Redstone Lamp",{8:1,47:4},{14:1}],["Lever",{6:1,31:1},{15:1}],
  ["Bookshelf",{6:6,4:3},{16:1}],["4 Stone Bricks",{8:4},{17:4}],["Hay Block",{4:9},{18:1}]];
CFG.creativeHotbar=[1,2,3,8,4,6,12,10,11];

/* ======================= 2. WORLD GENERATION ======================= */
const H=CFG.world.height,SEA=CFG.world.seaLevel;
let R=CFG.world.viewChunks;
const isW=b=>b==9||(b>=20&&b<=26)||b==249,lv=b=>b==9?8:b>=20&&b<=26?b-19:b==249?8:0;
const opaque=b=>b>0&&b!=5&&!isW(b)&&b!=10&&b!=12&&b!=14, solid=b=>b>0&&!isW(b);
const TILES={},HARD={},NM={};
for(const id in BLOCKS){TILES[id]=BLOCKS[id].tiles;HARD[id]=BLOCKS[id].hard;NM[id]=BLOCKS[id].name}
for(const id in ITEMS)NM[id]=ITEMS[id].name;
const PALETTE=[...Object.keys(BLOCKS).map(Number).filter(i=>i!=9&&BLOCKS[i].creative!==false),...Object.keys(ITEMS).map(Number)];
const HOT=[...CFG.creativeHotbar];
const hash=(x,z)=>{let h=Math.imul(x,374761393)^Math.imul(z,668265263);h=Math.imul(h^(h>>>13),1274126177);return((h^(h>>>16))>>>0)/4294967296};
const sm=t=>t*t*(3-2*t);
function vn(x,z){const xi=Math.floor(x),zi=Math.floor(z),fx=sm(x-xi),fz=sm(z-zi),a=hash(xi,zi),b=hash(xi+1,zi),c=hash(xi,zi+1),d=hash(xi+1,zi+1);return a+(b-a)*fx+(c-a)*fz+(a-b-c+d)*fx*fz}
function fbm(x,z){let s=0,a=1,t=0;for(let i=0;i<4;i++){s+=vn(x,z)*a;t+=a;a/=2;x*=2;z*=2}return s/t}
let seed=1,CE={};const chunks=new Map(),meshes=new Map(),dirty=new Set();
const hAt=(x,z)=>Math.floor(SEA+CFG.world.terrainBase+fbm((x+seed*131)/CFG.world.noiseScale+9,(z+seed*71)/CFG.world.noiseScale+4)*CFG.world.terrainHeight);

// Lamp lit state tracking (block position -> powered)
const lampLit=new Set();
function isLampLit(b){return b==14&&lampLit.size>0} // approx: actual per-block check done in meshing

function BI(name){for(const id in BLOCKS)if(BLOCKS[id].name.toLowerCase()===name.toLowerCase())return +id;return 0}
const BIOME_IDS={grass:1,dirt:2,stone:3,log:4,leaves:5,sand:7,water:9,netherrack:BI("Netherrack"),soulSand:BI("Soul Sand"),soulSoil:BI("Soul Soil"),basalt:BI("Basalt"),blackstone:BI("Blackstone"),endStone:BI("End Stone"),purpur:BI("Purpur Block"),lava:249,moss:BI("Moss Block"),drip:BI("Dripstone Block"),pointed:BI("Pointed Dripstone"),sulfur:250,crimson:252,warped:253,chorus:254,chorusFlower:255}
function biomeAt(x,z){
  if(dimension!=="overworld")return dimension==="nether"?"Nether Wastes":"The End";
  const t=fbm((x+seed*19)/150,(z-seed*13)/150),r=fbm((x-seed*31)/90,(z+seed*7)/90);
  if(t<.20)return"Snowy Plains"; if(t>.82&&r<.45)return"Desert"; if(t>.72&&r>.65)return"Savanna";
  if(r<.22)return"Swamp"; if(r>.90&&t>.40&&t<.72)return"Dappled Forest"; if(r>.80)return"Forest"; if(t<.35)return"Taiga"; return"Plains";
}
function overworldHeight(x,z){
  const b=biomeAt(x,z),base=hAt(x,z);
  if(b==="Desert")return base+2+Math.floor(vn(x*.02,z*.02)*3);
  if(b==="Mountains")return base+7;
  if(b==="Snowy Plains")return base-1;
  if(b==="Swamp")return Math.min(base,SEA-1+Math.floor(vn(x*.04,z*.04)*2));
  return base;
}
function endHeight(x,z){
  const r=Math.hypot(x,z),island=20+fbm(x/26,z/26)*24;
  if(r<island)return Math.floor(30+Math.max(0,18-r*.7)+fbm(x/12,z/12)*6);
  const outer=Math.max(0,1-(r-42)/22);
  return outer>0?Math.floor(29+outer*10+fbm(x/10,z/10)*5):-1;
}
function netherHeight(x,z){return Math.floor(42+fbm((x+seed)/34,(z-seed)/34)*24)}
function genStructure(a,cx,cz,X0,Z0,I){
  const put=(x,y,z,v)=>{const lx=x-X0,lz=z-Z0;if(lx>=0&&lx<16&&lz>=0&&lz<16&&y>=0&&y<H)a[I(lx,y,lz)]=v};
  const b=(n)=>BI(n); const h=dimension==="nether"?netherHeight(X0+8,Z0+8):overworldHeight(X0+8,Z0+8);
  if(dimension==="nether"&&hash(cx*97+seed,cz*131+seed)<.035){
    const w=9,d=7,base=Math.min(H-8,h+1),brick=b("Nether Bricks");
    for(let x=-w;x<=w;x++)for(let z=-d;z<=d;z++){put(X0+8+x,base,Z0+8+z,brick);if(Math.abs(x)==w||Math.abs(z)==d)for(let y=1;y<=4;y++)put(X0+8+x,base+y,Z0+8+z,brick)}
    for(let x=-4;x<=4;x++)put(X0+8+x,base+1,Z0+8,0);
  }else if(dimension==="end"&&hash(cx*73+seed,cz*37+seed)<.025){
    const pur=b("Purpur Block"),base=Math.max(32,endHeight(X0+8,Z0+8)+1);for(let x=-4;x<=4;x++)for(let z=-4;z<=4;z++)if(Math.abs(x)+Math.abs(z)<7)for(let y=0;y<4;y++)put(X0+8+x,base+y,Z0+8+z,pur);
  }else if(dimension==="overworld"){
    const hsh=hash(cx*43+seed,cz*71+seed),bname=biomeAt(X0+8,Z0+8);
    if(hsh<.018&&bname==="Desert"){const sand=b("Sandstone"),base=overworldHeight(X0+8,Z0+8)+1;for(let x=-4;x<=4;x++)for(let z=-4;z<=4;z++)for(let y=0;y<4;y++)if(Math.abs(x)+Math.abs(z)<=6-y)put(X0+8+x,base+y,Z0+8+z,sand)}
    else if(hsh<.032){const brick=b("Stone Bricks"),base=6;for(let x=-5;x<=5;x++)for(let z=-5;z<=5;z++)if(Math.abs(x)==5||Math.abs(z)==5)for(let y=0;y<4;y++)put(X0+8+x,base+y,Z0+8+z,brick)}
  }
}
function gen(cx,cz){
  const a=new Uint16Array(256*H),X0=cx*16,Z0=cz*16,I=(x,y,z)=>(y*16+z)*16+x;
  biomeName=biomeAt(X0+8,Z0+8);
  if(dimension==="nether"){
    for(let lx=0;lx<16;lx++)for(let lz=0;lz<16;lz++){const x=X0+lx,z=Z0+lz,h=netherHeight(x,z);for(let y=0;y<=h;y++){let v=y<h-4?BIOME_IDS.netherrack:BIOME_IDS.netherrack;if(y<7&&hash(x*3+seed,z*5+seed)<.18)v=BIOME_IDS.soulSoil||v;a[I(lx,y,lz)]=v}for(let y=28;y<34;y++)if(y>h)a[I(lx,y,lz)]=BIOME_IDS.lava;
      if(hash(x*11+seed,z*17+seed)>.96){const stem=hash(x,z)>.5?BIOME_IDS.crimson:BIOME_IDS.warped;for(let y=h+1;y<h+5&&y<H;y++)a[I(lx,y,lz)]=stem}
    }
    genStructure(a,cx,cz,X0,Z0,I);return a;
  }
  if(dimension==="end"){
    for(let lx=0;lx<16;lx++)for(let lz=0;lz<16;lz++){const x=X0+lx,z=Z0+lz,h=endHeight(x,z);if(h<0)continue;for(let y=0;y<=h;y++)a[I(lx,y,lz)]=BIOME_IDS.endStone;const top=h+1;if(hash(x*7+seed,z*11+seed)>.965&&top<H-3){a[I(lx,top,lz)]=BIOME_IDS.chorus}}
    genStructure(a,cx,cz,X0,Z0,I);return a;
  }
  // Overworld: biome-specific surfaces, caves, trees, and cave biomes.
  for(let lx=0;lx<16;lx++)for(let lz=0;lz<16;lz++){
    const x=X0+lx,z=Z0+lz,b=biomeAt(x,z),h=overworldHeight(x,z),beach=h<=SEA+1;
    let top=1,sub=2;if(b==="Desert"||beach)top=7;else if(b==="Snowy Plains")top=BI("Snow Block");else if(b==="Swamp")top=2;
    for(let y=0;y<=h;y++)a[I(lx,y,lz)]=y==h?top:y>h-4?sub:3;
    for(let y=3;CFG.world.caves&&y<h-3;y++)if(vn(x*.07+y*.19+seed,z*.07-y*.15)>CFG.world.caveA&&vn(x*.11-y*.1,z*.11+y*.2+seed)>CFG.world.caveB){a[I(lx,y,lz)]=0;if(hash(x*3+y+seed,z*7+y)>.94){const cv=hash(x+y,z-y);if(cv<.34)a[I(lx,y,lz)]=BIOME_IDS.moss;else if(cv<.67)a[I(lx,y,lz)]=BIOME_IDS.drip;else a[I(lx,y,lz)]=BIOME_IDS.sulfur}}
    for(let y=h+1;y<=SEA;y++)a[I(lx,y,lz)]=9;
  }
  for(let tx=X0-2;tx<X0+18;tx++)for(let tz=Z0-2;tz<Z0+18;tz++){
    const b=biomeAt(tx,tz),chance=b==="Dappled Forest"?.065:b==="Forest"?.05:b==="Taiga"?.035:b==="Savanna"?.025:.015;if(hash(tx*3+1+seed,tz*7+2)<1-chance)continue;const h=overworldHeight(tx,tz);if(h<=SEA+1||h>=H-8)continue;const th=4+(hash(tx,tz)>.5?1:0),wood=b==="Taiga"?BI("Spruce Log"):b==="Savanna"?BI("Acacia Log"):b==="Forest"?4:4,leaf=b==="Taiga"?BI("Spruce Leaves"):5;const put=(x,y,z,v)=>{const lx=x-X0,lz=z-Z0;if(lx>=0&&lx<16&&lz>=0&&lz<16&&y<H&&!a[I(lx,y,lz)])a[I(lx,y,lz)]=v};for(let t=1;t<=th;t++)put(tx,h+t,tz,wood);for(let dx=-2;dx<=2;dx++)for(let dz=-2;dz<=2;dz++)for(let dy=th-2;dy<=th+1;dy++){if(Math.abs(dx)==2&&Math.abs(dz)==2)continue;if(dy>th-1&&Math.abs(dx)+Math.abs(dz)>2)continue;put(tx+dx,h+dy,tz+dz,leaf)}}
  genStructure(a,cx,cz,X0,Z0,I);tryGenVillage(cx,cz,a,X0,Z0,I);return a;
}
const ck=(cx,cz)=>(cx+32768)*65536+(cz+32768);
function getC(cx,cz){
  const k=ck(cx,cz);let c=chunks.get(k);
  if(!c){c=gen(cx,cz);const e=CE[cx+","+cz];if(e)for(const i in e)c[i]=e[i];chunks.set(k,c)}
  return c;
}
const li=(x,y,z)=>(y*16+(z&15))*16+(x&15);
const inb=(x,y,z)=>y>=0&&y<H;
const get=(x,y,z)=>y<0?1:y>=H?0:getC(x>>4,z>>4)[li(x,y,z)];
const set=(x,y,z,v)=>{if(y>=0&&y<H)getC(x>>4,z>>4)[li(x,y,z)]=v};
const pset=(x,y,z,v)=>{set(x,y,z,v);const k=(x>>4)+","+(z>>4);(CE[k]=CE[k]||{})[li(x,y,z)]=v};
/* ======================= 3. TEXTURES ======================= */
// Original Blockcraft pixel atlas: 32x16 tiles, 16px per tile.
// It follows the visual language of classic voxel textures—hard pixel edges,
// restrained palettes, directional shading, and readable material patterns—
// while generating its own artwork rather than copying the reference atlas.
const ATLAS_COLS=32,ATLAS_ROWS=16,TILE_PX=16;
const T=document.createElement("canvas");T.width=ATLAS_COLS*TILE_PX;T.height=ATLAS_ROWS*TILE_PX;const g=T.getContext("2d");
g.imageSmoothingEnabled=false;
const col=(b,r,a=1)=>`rgba(${b.map(c=>Math.min(255,Math.max(0,c*(.78+r*.44)))|0)},${a})`;
function tile(i,fn){const ox=(i%ATLAS_COLS)*TILE_PX,oy=Math.floor(i/ATLAS_COLS)*TILE_PX;
  for(let y=0;y<TILE_PX;y++)for(let x=0;x<TILE_PX;x++){const c=fn(x,y,hash(x+i*31+7,y*17+i*5+3));if(c){g.fillStyle=c;g.fillRect(ox+x,oy+y,1,1)}}
}
const GR=[94,156,58],DI=[132,91,58];
// Core blocks get hand-shaped pixel patterns so the starting world has the same
// dense, readable texture language as the expanded catalog.
tile(0,(x,y,r)=>{let c=GR.slice();if((x+y*3)%13===0)c=c.map(v=>v+28);if(r<.04)c=c.map(v=>v-22);return col(c,r*.4)});
tile(1,(x,y,r)=>y<4+(hash(x,99)>.55?1:0)?col([82,150,52],r*.5):col(DI,r*.35));
tile(2,(x,y,r)=>{let c=DI.slice();if((x*7+y*11)%17===0)c=c.map(v=>v-28);return col(c,r*.4)});
tile(3,(x,y,r)=>{let c=[126,126,126];if((x*3+y*5)%17===0)c=c.map(v=>v-32);if((x+y)%19===0)c=c.map(v=>v+22);return col(c,r*.35)});
tile(4,(x,y,r)=>{let c=[126,82,47];if(x%4===0)c=c.map(v=>v-26);if((x+y)%9===0)c=c.map(v=>v+18);return col(c,r*.4)});
tile(5,(x,y,r)=>{let c=[171,128,71];const d=Math.hypot(x-7.5,y-7.5);if(d>6.2)c=c.map(v=>v-35);if(d<3.8)c=c.map(v=>v+24);if((x+y)%5===0)c=c.map(v=>v-12);return col(c,r*.3)});
tile(6,(x,y,r)=>{if(hash(x*9+4,y*13+2)<.18)return null;let c=[58,132,49];if((x+y)%7===0)c=c.map(v=>v+24);return col(c,r*.5)});
tile(7,(x,y,r)=>{let c=[171,128,74];if(x%4===0)c=c.map(v=>v-25);if(y%8===0)c=c.map(v=>v+12);return col(c,r*.35)});
tile(8,(x,y,r)=>{let c=[220,202,145];if((x+y*2)%13===0)c=c.map(v=>v-18);return col(c,r*.3)});
tile(9,(x,y,r)=>{let c=[105,105,105];if((x*5+y*3)%7<2)c=c.map(v=>v-30);if((x+y)%11===0)c=c.map(v=>v+24);return col(c,r*.4)});
tile(10,(x,y,r)=>{let c=[54,117,190];if((x+y)%7===0)c=c.map(v=>v+22);return col(c,r*.25,.78)});
tile(11,(x,y,r)=>{if(x===0||y===0||x===15||y===15)return col([216,240,246],r,.9);if((x+y)%7===0)return col([255,255,255],r,.5);return null});
tile(12,(x,y,r)=>{let c=[92,92,92];if(x>3&&x<12&&y>5&&y<12)c=[48,48,48];if((x+y)%9===0)c=c.map(v=>Math.min(255,v+22));return col(c,r*.25)});
tile(13,(x,y,r)=>{const cx=7.5,cy=9,dist=Math.hypot(x-cx,(y-cy)*1.25);if(dist<2.4)return col([255,207,71],r);if(dist<3.4)return col([241,111,22],r);if(x>=6&&x<=9&&y>=8&&y<=14)return col([120,74,35],r);return null});
tile(14,(x,y,r)=>{let c=[78,64,65];if(hash(x*3+1,y*7+2)<.15)c=[188,32,38];return col(c,r*.3)});
tile(15,(x,y,r)=>{let c=[101,77,68];if((x+y)%5===0)c=c.map(v=>v+18);return col(c,r*.35)});
tile(16,(x,y,r)=>{if(x>=6&&x<=9&&y>=3&&y<=11)return col([125,81,36],r);return col([118,118,118],r*.3)});
tile(17,(x,y,r)=>{if(y<12){const cs=[[185,50,45],[48,62,156],[50,132,65],[210,180,56]];return col(cs[(x*3+y)%4],r*.3)}return col([128,86,43],r*.35)});
tile(18,(x,y,r)=>{let c=[116,116,116];if(y%5<2||x%8<2)c=c.map(v=>v-30);return col(c,r*.3)});
tile(19,(x,y,r)=>{let c=hash(x*5,y*7)<.5?[126,114,101]:[101,94,83];if((x+y)%9===0)c=c.map(v=>v+22);return col(c,r*.35)});
tile(20,(x,y,r)=>{let c=[238,166,100];if((x+y)%4===0)c=[255,215,150];return col(c,r*.2)});
// Rich, original material textures for the expanded registry.
// The reference image is used only as a visual target: dense pixel detail,
// strong silhouettes, varied palettes, and recognizable material families.
const TEXPAL={
  wood:[151,105,63],plank:[178,135,78],stone:[126,126,126],deepslate:[65,70,76],
  dirt:[119,83,52],grass:[86,151,55],sand:[218,199,143],snow:[238,242,244],
  ice:[129,190,218],brick:[151,69,53],nether:[105,44,42],blackstone:[49,47,51],
  quartz:[218,213,199],end:[206,198,113],purpur:[165,108,174],ore:[112,112,108],
  copper:[183,104,66],gold:[219,174,54],iron:[188,188,178],diamond:[55,195,207],
  emerald:[45,173,91],amethyst:[151,91,190],wool:[205,205,205],glass:[188,225,235],
  leaf:[64,139,52],moss:[77,145,67],terracotta:[170,92,67],redstone:[122,55,55],
  obsidian:[34,28,48],prismarine:[72,154,145],tuff:[113,112,102],clay:[162,170,170],
  sculk:[20,65,70],mud:[91,76,61]
};
function texFamily(name){
  const n=name.toLowerCase();
  if(/diamond/.test(n))return"diamond"; if(/emerald/.test(n))return"emerald";
  if(/gold/.test(n))return"gold"; if(/iron/.test(n))return"iron"; if(/copper/.test(n))return"copper";
  if(/amethyst/.test(n))return"amethyst"; if(/redstone/.test(n))return"redstone";
  if(/deepslate/.test(n))return"deepslate"; if(/obsidian/.test(n))return"obsidian";
  if(/prismarine|sea lantern/.test(n))return"prismarine"; if(/quartz/.test(n))return"quartz";
  if(/purpur/.test(n))return"purpur"; if(/nether|netherrack|soul|magma/.test(n))return"nether";
  if(/blackstone/.test(n))return"blackstone"; if(/end stone|dragon egg|end portal/.test(n))return"end";
  if(/brick|bricks/.test(n))return"brick"; if(/sand|sandstone/.test(n))return"sand";
  if(/snow|powder snow/.test(n))return"snow"; if(/ice/.test(n))return"ice";
  if(/glass|pane/.test(n))return"glass"; if(/wool/.test(n))return"wool";
  if(/leaf|leaves|azalea|moss|flower/.test(n))return"leaf"; if(/grass/.test(n))return"grass";
  if(/dirt|mud|root/.test(n))return"mud"; if(/terracotta/.test(n))return"terracotta";
  if(/tuff/.test(n))return"tuff"; if(/clay/.test(n))return"clay";
  if(/wood|log|plank|bamboo|cherry|oak|spruce|birch|jungle|acacia|mangrove/.test(n))return /plank/.test(n)?"plank":"wood";
  if(/ore/.test(n))return"ore"; if(/sculk/.test(n))return"sculk";
  return"stone";
}
function richTile(i,name){
  const base=(TEXPAL[texFamily(name)]||TEXPAL.stone).slice(),n=name.toLowerCase();
  tile(i,(x,y,r)=>{
    let c=base.slice(); const edge=(x===0||y===0||x===15||y===15);
    const jitter=(r-.5)*18;c=c.map(v=>v+jitter);
    if(/plank/.test(n)){if(x%4===0)c=c.map(v=>v-28);if(y%5===0)c=c.map(v=>v+10)}
    else if(/log/.test(n)){const d=Math.hypot(x-7.5,y-7.5);if(d<5)c=c.map(v=>v+18);if(x===7||x===8||y===7||y===8)c=c.map(v=>v-18)}
    else if(/brick/.test(n)){if(y%5<2||x%8<2)c=c.map(v=>v-28)}
    else if(/ore/.test(n)){if(hash(x*13+i,y*7+i)<.10)c=(/diamond/.test(n)?TEXPAL.diamond:/emerald/.test(n)?TEXPAL.emerald:/gold/.test(n)?TEXPAL.gold:/iron/.test(n)?TEXPAL.iron:[180,70,55]).slice()}
    else if(/wool/.test(n)){const seam=(x+y)%9===0||Math.abs(x-y)<1;if(seam)c=c.map(v=>v-20)}
    else if(/glass|pane/.test(n)){if(edge)c=[220,242,248];else if((x+y)%7===0)c=c.map(v=>v+24)}
    else if(/leaves|leaf|moss/.test(n)){if(hash(x*9+i,y*11+i)<.18)c=c.map(v=>v-35);if(hash(x*17+i,y*3+i)>.93)c=c.map(v=>v+35)}
    else if(/stone|deepslate|tuff|cobble|granite|diorite|andesite/.test(n)){if((x+y*3)%11===0)c=c.map(v=>v-24)}
    else if(/gold|iron|copper|diamond|emerald|amethyst/.test(n)){if((x+y)%5===0)c=c.map(v=>v+20)}
    if(edge)c=c.map(v=>v-8); return col(c,r*.35);
  });
}
for(let i=0;i<EXTRA_BLOCK_NAMES.length;i++)richTile(21+i,EXTRA_BLOCK_NAMES[i]);
const SPECIAL_TEXTURE_NAMES={214:"Lava",215:"Sulfur Ore",216:"Sulfur Block",217:"Crimson Stem",218:"Warped Stem",219:"Chorus Plant",220:"Chorus Flower",221:"Nether Portal",222:"End Portal",223:"End Gateway",224:"Crying Obsidian Portal",225:"Redstone Repeater",226:"Redstone Comparator"};
for(const [ti,nm] of Object.entries(SPECIAL_TEXTURE_NAMES))richTile(+ti,nm);
const tex=new THREE.CanvasTexture(T);tex.magFilter=tex.minFilter=THREE.NearestFilter;tex.flipY=true;tex.generateMipmaps=false;
const mat=new THREE.MeshBasicMaterial({map:tex,vertexColors:true,alphaTest:.5});
const wmat=new THREE.MeshBasicMaterial({map:tex,vertexColors:true,transparent:true,depthWrite:false,opacity:.78});
/* ======================= 4. RENDERER & MESHING ======================= */
const renderer=new THREE.WebGLRenderer({antialias:true});renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.domElement.className="main";document.body.prepend(renderer.domElement);
const scene=new THREE.Scene();scene.background=new THREE.Color(0x7fb8e6);scene.fog=new THREE.Fog(0x7fb8e6,30,85);

const cam=new THREE.PerspectiveCamera(72,1,.05,500);cam.rotation.order="YXZ";scene.add(cam);
function resize(){renderer.setSize(innerWidth,innerHeight);cam.aspect=innerWidth/innerHeight;cam.updateProjectionMatrix()}
addEventListener("resize",resize);resize();
const F=[{d:[1,0,0],c:[[1,0,0],[1,1,0],[1,1,1],[1,0,1]],l:.8},{d:[-1,0,0],c:[[0,0,1],[0,1,1],[0,1,0],[0,0,0]],l:.8},
{d:[0,1,0],c:[[0,1,1],[1,1,1],[1,1,0],[0,1,0]],l:1},{d:[0,-1,0],c:[[0,0,0],[1,0,0],[1,0,1],[0,0,1]],l:.5},
{d:[0,0,1],c:[[0,0,1],[1,0,1],[1,1,1],[0,1,1]],l:.68},{d:[0,0,-1],c:[[1,0,0],[0,0,0],[0,1,0],[1,1,0]],l:.68}];
const AX=[[1,2],[0,2],[0,1]],AOV=[.5,.68,.84,1];
function face(B,x,y,z,fi,b,useAO){
  const f=F[fi],d=f.d,na=d[0]?0:d[1]?1:2,[a1,a2]=AX[na];
  const bidForTile=isLampLitBlock(x,y,z,b)?14:b;
  const fluidTile=b==249?249:(isW(b)?9:bidForTile);const t=TILES[fluidTile][fi==2?0:fi==3?2:1],tx=(t%ATLAS_COLS)/ATLAS_COLS,ty=1-(Math.floor(t/ATLAS_COLS)+1)/ATLAS_ROWS,n=B.p.length/3,ao=[];
  for(let k=0;k<4;k++){
    const v=f.c[k];let a=3;
    if(useAO){
      const o=[x+d[0],y+d[1],z+d[2]],p1=o.slice(),p2=o.slice(),p3=o.slice(),g1=v[a1]?1:-1,g2=v[a2]?1:-1;
      p1[a1]+=g1;p2[a2]+=g2;p3[a1]+=g1;p3[a2]+=g2;
      const s1=opaque(get(...p1))?1:0,s2=opaque(get(...p2))?1:0,c=opaque(get(...p3))?1:0;
      a=s1&&s2?0:3-s1-s2-c;
    }
    ao.push(a);
    const u=d[1]==0?(d[0]?v[2]:v[0]):v[0],w=d[1]==0?v[1]:v[2],l=f.l*AOV[a];
    B.p.push(x+v[0],y+v[1],z+v[2]);B.c.push(l,l,l);B.u.push(tx+(.01+u*.98)/ATLAS_COLS,ty+(.01+w*.98)/ATLAS_ROWS);
  }
  B.i.push(...(ao[0]+ao[2]<ao[1]+ao[3]?[n+1,n+2,n+3,n+1,n+3,n]:[n,n+1,n+2,n,n+2,n+3]));
}
// Check if a lamp block at position is lit
function isLampLitBlock(x,y,z,b){return b==14&&lampLit.has(x+","+y+","+z)}
const mk=()=>({p:[],c:[],u:[],i:[]});
const torchGroups=new Map();
function removeTorchChunk(k){const g=torchGroups.get(k);if(!g)return;scene.remove(g);g.traverse(o=>{if(o.geometry)o.geometry.dispose();if(o.material){if(Array.isArray(o.material))o.material.forEach(m=>m.dispose&&m.dispose());else if(o.material.dispose)o.material.dispose()}});torchGroups.delete(k)}
const torchFlameCanvas=document.createElement("canvas");torchFlameCanvas.width=torchFlameCanvas.height=16;const tfc=torchFlameCanvas.getContext("2d");tfc.imageSmoothingEnabled=false;
[[7,1,2,3,"#fff3a0"],[5,3,6,9,"#ffd34e"],[7,2,4,10,"#ff9d18"],[4,7,8,6,"#ff5a12"],[8,7,4,6,"#ff6b12"]].forEach(a=>{tfc.fillStyle=a[4];tfc.fillRect(a[0],a[1],a[2],a[3])});
const torchFlameTex=new THREE.CanvasTexture(torchFlameCanvas);torchFlameTex.magFilter=torchFlameTex.minFilter=THREE.NearestFilter;torchFlameTex.generateMipmaps=false;
function makeTorch(x,y,z){const g=new THREE.Group();g.position.set(x+.5,y,z+.5);
 const smat=new THREE.MeshLambertMaterial({color:0x6b3d22});
 const fmat=new THREE.MeshBasicMaterial({map:torchFlameTex,transparent:true,alphaTest:.08,side:THREE.DoubleSide,depthWrite:false});
 const glowMat=new THREE.MeshBasicMaterial({color:0xffa52a,transparent:true,opacity:.16,side:THREE.DoubleSide,depthWrite:false,blending:THREE.AdditiveBlending});
 const stick=new THREE.Mesh(new THREE.BoxGeometry(.105,.58,.105),smat);stick.position.y=.29;g.add(stick);
 const flameA=new THREE.Mesh(new THREE.PlaneGeometry(.25,.34),fmat);flameA.position.y=.68;flameA.rotation.y=Math.PI*.25;g.add(flameA);
 const flameB=flameA.clone();flameB.rotation.y=-Math.PI*.25;g.add(flameB);
 const glow=new THREE.Mesh(new THREE.SphereGeometry(.22,12,8),glowMat);glow.position.y=.68;g.add(glow);
 g.userData.flames=[flameA,flameB];g.userData.phase=(x*13+z*7)%10;return g}
function animateTorches(t){for(const g of torchGroups.values())g.traverse(o=>{if(o.userData&&o.userData.flames){const q=Math.sin(t*8+(o.userData.phase||0))*.035;o.userData.flames[0].scale.y=1+q;o.userData.flames[1].scale.y=1-q;}})}
function rebuildTorchChunk(cx,cz){const k=cx+","+cz;removeTorchChunk(k);const g=new THREE.Group(),a=getC(cx,cz),X0=cx*16,Z0=cz*16,I=(x,y,z)=>(y*16+z)*16+x;for(let y=0;y<H;y++)for(let lz=0;lz<16;lz++)for(let lx=0;lx<16;lx++)if(a[I(lx,y,lz)]==12)g.add(makeTorch(X0+lx,y,Z0+lz));if(g.children.length){scene.add(g);torchGroups.set(k,g)}}
function toMesh(B,m){
  const gm=new THREE.BufferGeometry();
  gm.setAttribute("position",new THREE.Float32BufferAttribute(B.p,3));
  gm.setAttribute("color",new THREE.Float32BufferAttribute(B.c,3));
  gm.setAttribute("uv",new THREE.Float32BufferAttribute(B.u,2));
  gm.setIndex(B.i);gm.computeBoundingSphere();return new THREE.Mesh(gm,m);
}
function buildChunk(cx,cz){
  const A=mk(),Wt=mk(),c0=getC(cx,cz);
  for(let y=0;y<H;y++)for(let lz=0;lz<16;lz++)for(let lx=0;lx<16;lx++){
    const b=c0[(y*16+lz)*16+lx];if(!b)continue;const x=cx*16+lx,z=cz*16+lz;if(b==12)continue;
    for(let fi=0;fi<6;fi++){
      const d=F[fi].d,nb=get(x+d[0],y+d[1],z+d[2]);
      if(isW(b)){if(nb==0)face(Wt,x,y,z,fi,b,false)}
      else if(!opaque(nb)&&!(nb==b&&b==10))face(A,x,y,z,fi,b,true);
    }
  }
  const k=cx+","+cz,old=meshes.get(k);
  if(old)old.forEach(m=>{scene.remove(m);m.geometry.dispose()});
  const ms=[toMesh(A,mat)];if(Wt.i.length){const w=toMesh(Wt,wmat);w.renderOrder=1;ms.push(w)}
  ms.forEach(m=>scene.add(m));meshes.set(k,ms);rebuildTorchChunk(cx,cz);
}
let uc=0;
function updChunks(all,rad){
  const rd=rad||R;const pcx=Math.floor(p.x)>>4,pcz=Math.floor(p.z)>>4;let built=0;uc++;
  for(const k of dirty){if(!all&&built>=3)break;dirty.delete(k);const[a,b]=k.split(",").map(Number);if(meshes.has(k)){buildChunk(a,b);built++}}
  const need=[];
  for(let dx=-rd;dx<=rd;dx++)for(let dz=-rd;dz<=rd;dz++){if(dx*dx+dz*dz>rd*rd+1)continue;const k=(pcx+dx)+","+(pcz+dz);if(!meshes.has(k))need.push([dx*dx+dz*dz,pcx+dx,pcz+dz])}
  need.sort((a,b)=>a[0]-b[0]);
  for(let i=0;i<(all?need.length:built?1:2)&&i<need.length;i++)buildChunk(need[i][1],need[i][2]);
  if(uc%30==0)for(const[k,ms]of meshes){const[a,b]=k.split(",").map(Number);if(Math.max(Math.abs(a-pcx),Math.abs(b-pcz))>R+2){ms.forEach(m=>{scene.remove(m);m.geometry.dispose()});removeTorchChunk(k);meshes.delete(k);chunks.delete(ck(a,b))}}
}
const act=new Set();
function wake(x,y,z){for(const[a,b,c]of[[0,0,0],[1,0,0],[-1,0,0],[0,1,0],[0,-1,0],[0,0,1],[0,0,-1]])act.add((x+a)+","+(y+b)+","+(z+c))}
function dirtyAt(x,z){for(const a of[-1,0,1])for(const b of[-1,0,1])dirty.add(((x+a)>>4)+","+((z+b)>>4))}
function edit(x,y,z){
  const s=new Set();for(const a of[-1,0,1])for(const b of[-1,0,1])s.add(((x+a)>>4)+","+((z+b)>>4));
  s.forEach(k=>{const[a,b]=k.split(",").map(Number);if(meshes.has(k))buildChunk(a,b)});wake(x,y,z);
}
function setW(x,y,z,v){set(x,y,z,v);dirtyAt(x,z);wake(x,y,z)}
const NB=[[1,0],[-1,0],[0,1],[0,-1]];
function waterTick(){
  const list=[...act].slice(0,200);list.forEach(k=>act.delete(k));
  for(const k of list){
    const[x,y,z]=k.split(",").map(Number),b=get(x,y,z);if(!isW(b))continue;let L=lv(b);
    if(L<8){
      const up=lv(get(x,y+1,z))>0;let mx=0;for(const[a,c]of NB)mx=Math.max(mx,lv(get(x+a,y,z+c)));
      if(!up&&mx<=L){setW(x,y,z,0);continue}
      if(up&&L!=7){setW(x,y,z,26);L=7}
    }
    const bl=get(x,y-1,z);
    if(y>0&&bl==0){setW(x,y-1,z,26);continue}
    if(L>1&&(solid(bl)||lv(bl)>0))for(const[a,c]of NB)if(get(x+a,y,z+c)==0)setW(x+a,y,z+c,19+L-1);
  }
}
/* ======================= 5. SKY & MOBS ======================= */
const sunM=new THREE.Mesh(new THREE.PlaneGeometry(36,36),new THREE.MeshBasicMaterial({color:0xfff4b0,fog:false,depthWrite:false}));
const moonM=new THREE.Mesh(new THREE.PlaneGeometry(26,26),new THREE.MeshBasicMaterial({color:0xdde4ee,fog:false,depthWrite:false}));
scene.add(sunM,moonM);
const cc=document.createElement("canvas");cc.width=cc.height=32;const cg=cc.getContext("2d");
for(let y=0;y<32;y++)for(let x=0;x<32;x++)if(vn(x*.28,y*.28)>.56){cg.fillStyle="#fff";cg.fillRect(x,y,1,1)}
const ctex=new THREE.CanvasTexture(cc);ctex.magFilter=ctex.minFilter=THREE.NearestFilter;ctex.generateMipmaps=false;ctex.wrapS=ctex.wrapT=THREE.RepeatWrapping;ctex.repeat.set(3,3);
const cmat=new THREE.MeshBasicMaterial({map:ctex,transparent:true,opacity:.85,fog:false,side:THREE.DoubleSide,depthWrite:false});
const clouds=new THREE.Mesh(new THREE.PlaneGeometry(700,700),cmat);clouds.rotation.x=-Math.PI/2;scene.add(clouds);
const sel=new THREE.LineSegments(new THREE.EdgesGeometry(new THREE.BoxGeometry(1.005,1.005,1.005)),new THREE.LineBasicMaterial({color:0xffffff}));
sel.visible=false;scene.add(sel);
// Block breaking crack overlay. One transparent cube sits just above the selected
// block and its pixel-art crack texture grows through ten stages as mining progresses.
const crackCanvas=document.createElement("canvas");crackCanvas.width=32;crackCanvas.height=32;
const crackCtx=crackCanvas.getContext("2d");crackCtx.imageSmoothingEnabled=false;
const crackTex=new THREE.CanvasTexture(crackCanvas);crackTex.magFilter=crackTex.minFilter=THREE.NearestFilter;crackTex.generateMipmaps=false;
const crackMat=new THREE.MeshBasicMaterial({map:crackTex,transparent:true,opacity:1,depthWrite:false,side:THREE.DoubleSide,polygonOffset:true,polygonOffsetFactor:-1,polygonOffsetUnits:-1});
const crackMesh=new THREE.Mesh(new THREE.BoxGeometry(1.012,1.012,1.012),crackMat);crackMesh.visible=false;crackMesh.renderOrder=998;scene.add(crackMesh);
const crackPaths=[[[15,2],[14,7],[11,10],[13,15],[9,20],[8,29]],[[15,2],[14,7],[11,10],[13,15],[9,20],[8,29],[[23,4],[20,9],[22,13],[18,17]]],[[15,2],[14,7],[11,10],[13,15],[9,20],[8,29],[[23,4],[20,9],[22,13],[18,17]],[[5,5],[9,9],[7,13]]],[[15,2],[14,7],[11,10],[13,15],[9,20],[8,29],[[23,4],[20,9],[22,13],[18,17]],[[5,5],[9,9],[7,13]],[[26,22],[21,21],[18,24],[15,23]]],[[15,2],[14,7],[11,10],[13,15],[9,20],[8,29],[[23,4],[20,9],[22,13],[18,17]],[[5,5],[9,9],[7,13]],[[26,22],[21,21],[18,24],[15,23]],[[28,10],[25,14],[27,18]]]];
function drawCracks(stage){crackCtx.clearRect(0,0,32,32);if(stage<=0){crackTex.needsUpdate=true;return}crackCtx.strokeStyle="rgba(18,18,18,.9)";crackCtx.lineWidth=1.6;crackCtx.lineCap="square";const count=Math.min(crackPaths.length,Math.ceil(stage/2));for(let n=0;n<count;n++){const path=crackPaths[n];if(!path||!path[0])continue;crackCtx.beginPath();crackCtx.moveTo(path[0][0],path[0][1]);for(let j=1;j<path.length;j++)crackCtx.lineTo(path[j][0],path[j][1]);crackCtx.stroke()}if(stage>=7){crackCtx.strokeStyle="rgba(0,0,0,.55)";crackCtx.lineWidth=1;for(let n=0;n<3;n++){const y=5+n*9;crackCtx.beginPath();crackCtx.moveTo(2,y);crackCtx.lineTo(12,y+3);crackCtx.lineTo(18,y+1);crackCtx.lineTo(29,y+5);crackCtx.stroke()}}crackTex.needsUpdate=true}
drawCracks(0);
const mobs=[],MM={};
const mm=c=>MM[c]||(MM[c]=Object.assign(new THREE.MeshBasicMaterial({color:c}),{base:new THREE.Color(c)}));
function bx(g,w,h,d,c,x,y,z){const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),typeof c=="number"?mm(c):c);m.position.set(x,y,z);g.add(m)}
function tmat(base,fn){
  const c=document.createElement("canvas");c.width=c.height=16;const x=c.getContext("2d");
  for(let i=0;i<16;i++)for(let j=0;j<16;j++){const r=hash(i*7+base[0],j*5+base[1]);x.fillStyle=(fn&&fn(i,j,r))||col(base,r);x.fillRect(i,j,1,1)}
  const t=new THREE.CanvasTexture(c);t.magFilter=t.minFilter=THREE.NearestFilter;t.generateMipmaps=false;
  const m=new THREE.MeshBasicMaterial({map:t});m.base=new THREE.Color(1,1,1);MM["t"+m.id]=m;return m;
}
let PIG=null;
function pigMats(){
  if(PIG)return PIG;
  const skin=tmat([240,152,163],(i,j,r)=>r<.1?col([222,130,144],r):0),
   face=tmat([240,152,163],(i,j)=>((i==3||i==12)&&(j==6||j==7))?"#2a1520":((i==2||i==13)&&(j==6||j==7))?"#fff":0),
   sn=tmat([226,122,138],(i,j)=>((i==5||i==10)&&(j==8||j==9))?"#7a3040":0),
   leg=tmat([226,134,148],(i,j,r)=>j>12?col([150,90,100],r):0);
  return PIG={skin,face,sn,leg};
}
function addMob(t,x,y,z){
  const g=new THREE.Group();
  const parts=[];
  const addPart=(name,w,h,d,mat,px,py,pz)=>{const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),typeof mat=="number"?mm(mat):mat);m.position.set(px,py,pz);m.userData.mobPart=name;g.add(m);parts.push(m);return m};
  if(t=="pig"){
    const q=pigMats();
    addPart("body",.6,.55,.9,q.skin,0,.55,0);
    addPart("head",.5,.46,.46,q.face,0,.72,.64);
    addPart("snout",.24,.18,.1,q.sn,0,.64,.92);
    for(const[a,b]of[[-.2,-.3],[.2,-.3],[-.2,.3],[.2,.3]])addPart("leg",.18,.3,.18,q.leg,a,.15,b);
  }else{
    addPart("leftLeg",.2,.7,.2,0x29376f,-.15,.35,0);addPart("rightLeg",.2,.7,.2,0x29376f,.15,.35,0);
    addPart("body",.5,.7,.3,0x3a4a9a,0,.95,0);addPart("head",.5,.5,.5,0x5a9a4a,0,1.65,0);
    addPart("leftArm",.2,.2,.8,0x5a9a4a,-.35,1.3,.35);addPart("rightArm",.2,.2,.8,0x5a9a4a,.35,1.3,.35);
  }
  // Per-body-part red flash overlays. They are slightly inflated so the red layer
  // sits visibly over the hit part without changing the mob's geometry.
  const flashMat=new THREE.MeshBasicMaterial({color:0xff2222,transparent:true,opacity:0,depthWrite:false,side:THREE.DoubleSide});
  const flashes=[];
  for(const part of parts){const f=new THREE.Mesh(part.geometry,flashMat.clone());f.position.copy(part.position);f.rotation.copy(part.rotation);f.scale.set(1.025,1.025,1.025);f.visible=false;f.renderOrder=50;g.add(f);flashes.push(f)}
  g.position.set(x,y,z);scene.add(g);
  mobs.push({t,g,x,y,z,vy:0,hp:CFG.mobs[t].hp,a:null,w:0,cd:0,kx:0,kz:0,W:t=="pig"?.35:.3,Hh:t=="pig"?.9:1.8,parts,flashes,hitFlash:0,walkT:Math.random()*10});
}

const ADV_MOB_COLORS={cow:0x6b4635,sheep:0xe8e4d8,chicken:0xf0f0e8,villager:0x7b4b9a,cat:0x9a6a42,wolf:0xa8a8a8,enderman:0x171322,piglin:0x8e5d5d,golem:0xbcbcbc,skeleton:0xd8d8d0,creeper:0x4c9a55,phantom:0x6c83a8,creaking:0x68725b};
function addAdvancedMob(t,x,y,z){
  const spec=CFG.mobs.advanced[t];if(!spec)return;
  const g=new THREE.Group(),parts=[],flashes=[];const base=ADV_MOB_COLORS[t]||0x888888,mat=mm(base);
  const add=(name,w,h,d,m,px,py,pz)=>{const q=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),m);q.position.set(px,py,pz);q.userData.mobPart=name;g.add(q);parts.push(q);return q};
  if(t==="chicken"){add("body",.42,.35,.52,mat,0,.48,0);add("head",.30,.30,.30,mat,0,.78,.28);add("beak",.12,.08,.16,mm(0xd49b3a),0,.74,.48);for(const sx of[-.11,.11])add("leg",.07,.25,.07,mm(0xd49b3a),sx,.17,0)}
  else if(t==="cow"||t==="sheep"){add("body",.72,.55,1.0,mat,0,.68,0);add("head",.48,.45,.48,mat,0,.78,.63);for(const sx of[-.25,.25])for(const sz of[-.32,.32])add("leg",.16,.38,.16,mat,sx,.25,sz)}
  else if(t==="cat"||t==="wolf"){add("body",.42,.42,.72,mat,0,.50,0);add("head",.38,.38,.38,mat,0,.76,.40);for(const sx of[-.15,.15])for(const sz of[-.25,.25])add("leg",.10,.30,.10,mat,sx,.18,sz)}
  else if(t==="villager"){add("body",.5,.72,.35,mat,0,.85,0);add("head",.48,.48,.48,mm(0xd6a77a),0,1.48,0);add("nose",.12,.18,.16,mm(0x8d5b42),0,1.43,.30)}
  else if(t==="golem"){add("body",.7,.9,.5,mat,0,.9,0);add("head",.55,.55,.55,mat,0,1.62,0);add("arm",.2,.85,.2,mat,-.48,.95,0);add("arm",.2,.85,.2,mat,.48,.95,0)}
  else if(t==="phantom"){add("body",.55,.28,.85,mat,0,.85,0);add("wing",.10,.10,.95,mat,-.58,.85,0);add("wing",.10,.10,.95,mat,.58,.85,0)}
  else if(t==="enderman"){add("body",.30,.85,.24,mat,0,1.1,0);add("head",.38,.38,.38,mat,0,1.82,0);add("arm",.12,.95,.12,mat,-.30,1.1,0);add("arm",.12,.95,.12,mat,.30,1.1,0);add("leg",.13,.85,.13,mat,-.10,.42,0);add("leg",.13,.85,.13,mat,.10,.42,0)}
  else {add("body",.48,.78,.32,mat,0,.9,0);add("head",.48,.48,.48,mat,0,1.52,0);add("arm",.18,.62,.18,mat,-.34,1.0,0);add("arm",.18,.62,.18,mat,.34,1.0,0);add("leg",.18,.68,.18,mat,-.12,.34,0);add("leg",.18,.68,.18,mat,.12,.34,0)}
  g.position.set(x,y,z);scene.add(g);mobs.push({t,g,x,y,z,vy:0,hp:spec.hp,a:null,w:0,cd:0,kx:0,kz:0,W:t==="golem"?.5:.3,Hh:t==="phantom"?.8:t==="enderman"?2.6:1.7,parts,flashes,hitFlash:0,walkT:Math.random()*10,advanced:true});
}
const addLegacyMob=addMob;
addMob=function(t,x,y,z){if(CFG.mobs.advanced[t])return addAdvancedMob(t,x,y,z);return addLegacyMob(t,x,y,z)};
function spawnAdvancedMobs(day){
  const dim=dimension,b=biomeName;let candidates=[];
  if(dim==="overworld"){if(b==="Dappled Forest"||b==="Forest"||b==="Plains"){candidates=["cow","sheep","chicken","villager","cat"];if(day<.35)candidates.push("zombie","skeleton","creeper")}if(b==="Taiga")candidates.push("wolf");if(day<.25)candidates.push("zombie","skeleton","creeper")}
  if(dim==="nether")candidates=["piglin"];
  if(dim==="end")candidates=["enderman"];if(dim==="overworld"&&day<.18){candidates.push("phantom");if(b==="Dappled Forest")candidates.push("creaking");}
  const t=candidates[Math.floor(Math.random()*candidates.length)];if(!t||mobs.length>24)return;const a=Math.random()*6.28,d=18+Math.random()*24,x=Math.floor(p.x+Math.cos(a)*d),z=Math.floor(p.z+Math.sin(a)*d);let y=H-1;while(y>0&&!get(x,y,z))y--;if(solid(get(x,y,z))&&!get(x,y+1,z))addMob(t,x+.5,y+1,z+.5);
}
function mobTick(dt,br,day){
  for(const k in MM)MM[k].color.copy(MM[k].base).multiplyScalar(br);
  zt-=dt;pt-=dt;
  if(isSurvivalMode()&&Math.random()<dt*.18)spawnAdvancedMobs(day);
  if(pt<=0&&mobs.filter(m=>m.t=="pig").length<CFG.mobs.pigCap){pt=2;const a=Math.random()*6.28,d=20+Math.random()*30,x=Math.floor(p.x+Math.cos(a)*d),z=Math.floor(p.z+Math.sin(a)*d);let y=H-1;while(y>0&&!get(x,y,z))y--;if(get(x,y,z)==1)addMob("pig",x+.5,y+1,z+.5)}
  if(isSurvivalMode()&&day<.4&&zt<=0&&mobs.filter(m=>m.t=="zombie").length<CFG.mobs.zombieCap){
    zt=3;const a=Math.random()*6.28,d=14+Math.random()*10,x=Math.floor(p.x+Math.cos(a)*d),z=Math.floor(p.z+Math.sin(a)*d);
    let y=H-1;while(y>0&&!get(x,y,z))y--;if(solid(get(x,y,z))&&!get(x,y+1,z))addMob("zombie",x+.5,y+1,z+.5)
  }
  for(let i=mobs.length-1;i>=0;i--){
    const m=mobs[i],dxp=p.x-m.x,dzp=p.z-m.z,dist=Math.hypot(dxp,dzp)||1;m.hurtCd=Math.max(0,(m.hurtCd||0)-dt);
    let dx=0,dz=0,sp=CFG.mobs[m.t].wander;
    if(m.advanced&&CFG.mobs.advanced[m.t].behavior==="hostile"&&isSurvivalMode()&&dist<CFG.mobs.advanced[m.t].aggro){dx=dxp/dist;dz=dzp/dist;sp=CFG.mobs.advanced[m.t].chase||2.2}
    else if(m.advanced&&CFG.mobs.advanced[m.t].behavior==="neutral"&&m.hurtCd>0&&dist<18){dx=dxp/dist;dz=dzp/dist;sp=CFG.mobs.advanced[m.t].chase||2.0}
    else if(m.t=="zombie"&&isSurvivalMode()&&dist<CFG.mobs.zombie.aggro){dx=dxp/dist;dz=dzp/dist;sp=CFG.mobs.zombie.chase}
    else{m.w-=dt;if(m.w<=0){m.w=1+Math.random()*3;m.a=Math.random()<.4?null:Math.random()*6.28}if(m.a!=null){dx=Math.cos(m.a);dz=Math.sin(m.a)}}
    m.kx*=.9;m.kz*=.9;m.vy-=28*dt;
    // Mobs have a physical collision box and cannot occupy the player's body.
    const nx=m.x+(dx*sp+m.kx)*dt,nz=m.z+(dz*sp+m.kz)*dt;let blk=false,gr=false;
    const playerOverlap=(x,z)=>{const rr=m.W+.30;return Math.hypot(x-p.x,z-p.z)<rr&&m.y<p.y+1.8&&m.y+m.Hh>p.y+.05};
    if(!hit(nx,m.y,m.z,m.W,m.Hh)&&!playerOverlap(nx,m.z))m.x=nx;else blk=true;
    if(!hit(m.x,m.y,nz,m.W,m.Hh)&&!playerOverlap(m.x,nz))m.z=nz;else blk=true;
    // If a mob is pushed into the player, separate it instead of letting the meshes overlap.
    const pdx=m.x-p.x,pdz=m.z-p.z,pdist=Math.hypot(pdx,pdz),minDist=m.W+.30;
    if(pdist<minDist&&pdist>.0001&&m.y<p.y+1.8&&m.y+m.Hh>p.y+.05){const q=(minDist-pdist)/pdist;m.x+=pdx*q;m.z+=pdz*q;m.kx*=.25;m.kz*=.25;}
    const ny=m.y+m.vy*dt;
    if(hit(m.x,ny,m.z,m.W,m.Hh)){gr=m.vy<0;m.vy=0}else m.y=ny;
    if(gr&&blk)m.vy=8;
    if(m.y<-10||dist>90||(m.t=="zombie"&&(!isSurvivalMode()||(day>.6&&Math.random()<dt*.05)))){scene.remove(m.g);mobs.splice(i,1);continue}
    if(m.t=="zombie"){m.cd-=dt;if(dist<.85&&Math.abs(p.y-m.y)<1.6&&m.cd<=0){hurt(CFG.mobs.zombie.damage);m.cd=CFG.mobs.zombie.cooldown}}else if(m.advanced&&CFG.mobs.advanced[m.t].behavior==="hostile"&&dist<1.0&&Math.abs(p.y-m.y)<2.2){m.cd-=dt;if(m.cd<=0){hurt(CFG.mobs.advanced[m.t].damage);m.cd=CFG.mobs.advanced[m.t].cooldown}}
    m.g.position.set(m.x,m.y,m.z);if(dx||dz)m.g.rotation.y=Math.atan2(dx,dz);
    // Simple Minecraft-like mob animation: legs/arms swing while moving, body bobs
    // while walking, and the entire pose settles when stationary.
    const moving=Math.hypot(dx,dz)>0.05; m.walkT+=dt*(moving?(6+sp*1.5):1);
    const wa=moving?Math.sin(m.walkT)*.55:Math.sin(m.walkT*.7)*.025;
    if(m.t=="pig"){
      const legs=m.parts.filter(q=>q.userData.mobPart=="leg");
      legs.forEach((q,n)=>q.rotation.x=(n%2==0?wa:-wa));
      if(m.parts[0])m.parts[0].position.y=.55+(moving?Math.abs(Math.sin(m.walkT))*0.035:0);
      if(m.parts[1])m.parts[1].rotation.y=moving?Math.sin(m.walkT)*.025:0;
    }else{
      const la=m.parts.find(q=>q.userData.mobPart=="leftArm"),ra=m.parts.find(q=>q.userData.mobPart=="rightArm"),ll=m.parts.find(q=>q.userData.mobPart=="leftLeg"),rl=m.parts.find(q=>q.userData.mobPart=="rightLeg");
      if(la)la.rotation.x=wa;if(ra)ra.rotation.x=-wa;if(ll)ll.rotation.x=-wa;if(rl)rl.rotation.x=wa;
      const body=m.parts.find(q=>q.userData.mobPart=="body");if(body)body.position.y=.95+(moving?Math.abs(Math.sin(m.walkT))*0.025:0);
    }
    m.hitFlash=Math.max(0,(m.hitFlash||0)-dt);
    if(m.flashes)for(const f of m.flashes){f.visible=m.hitFlash>0;f.material.opacity=m.hitFlash>0?Math.min(.62,m.hitFlash/.12*.62):0}
  }
}
/* ======================= 6. HANDS & HOTBAR ======================= */
const hmat=new THREE.MeshBasicMaterial({map:tex,vertexColors:true,alphaTest:.5,fog:false,depthTest:false,transparent:true});
const isBlockId=id=>Number.isInteger(+id)&&Object.prototype.hasOwnProperty.call(BLOCKS,+id);
function handGeo(b){const B=mk();const bid=isBlockId(b)?b:1;for(let i=0;i<6;i++)face(B,0,0,0,i,bid,false);const gm=toMesh(B,hmat).geometry;gm.translate(-.5,-.5,-.5);return gm}
/* --- Custom 3D item models for tools, food, shield, etc --- */
const ITC={},IMC={},IGC={};
function itemCanvas(id){
  const c=document.createElement("canvas");c.width=c.height=16;const x=c.getContext("2d");x.imageSmoothingEnabled=false;
  const P=(cx,cy,col,w,h)=>{x.fillStyle=col;x.fillRect(cx,cy,w,h)};
  if(id==31){P(7,2,"#8a6a3a",2,12);P(6,3,"#6a4a2a",1,10)}
  else if(id==40){P(7,2,"#8a6a3a",2,12);P(2,2,"#b88a4a",12,3);P(1,3,"#b88a4a",2,2);P(13,3,"#b88a4a",2,2)}
  else if(id==41){P(7,2,"#8a6a3a",2,12);P(2,2,"#8d8d8d",12,3);P(1,3,"#8d8d8d",2,2);P(13,3,"#8d8d8d",2,2)}
  else if(id==42){P(7,2,"#8a6a3a",2,12);P(8,1,"#b88a4a",7,5);P(6,2,"#b88a4a",4,4);P(8,1,"#d8aa6a",5,2)}
  else if(id==43){P(7,2,"#8a6a3a",2,12);P(7,11,"#b88a4a",6,4);P(8,14,"#b88a4a",4,2)}
  else if(id==30){P(3,4,"#e8909a",10,8);P(4,5,"#f6c0c6",5,3);P(3,4,"#c07080",10,1)}
  else if(id==33){P(3,4,"#9a5a30",10,8);P(4,5,"#c98050",5,3);P(3,4,"#7a4a20",10,1)}
  else if(id==32){P(4,5,"#d02a2a",8,8);P(5,6,"#f04040",4,4);P(8,2,"#3a9a2a",3,4);P(7,3,"#5aba4a",2,2)}
  else if(id>=49&&id<=53){const C={49:"#b88a4a",50:"#8d8d8d",51:"#d8d8d8",52:"#5bd6ff",53:"#3b233f"}[id];P(7,2,C,2,10);P(6,1,C,4,3);P(4,2,C,8,2);P(6,12,"#8a6a3a",4,2);P(7,14,"#5a3a20",2,2)}
  else if(id==44){P(2,1,"#5a5a6a",12,12);P(3,2,"#7a7a8a",10,10);P(6,2,"#9a9aaa",4,9);P(6,5,"#3a3a4a",4,2);P(2,1,"#3a3a4a",12,1)}
  else if(id==45){P(2,2,"#e8e8d8",4,4);P(10,2,"#e8e8d8",4,4);P(2,10,"#e8e8d8",4,4);P(10,10,"#e8e8d8",4,4);P(4,3,"#d8d8c8",8,2);P(4,11,"#d8d8c8",8,2);P(3,4,"#d8d8c8",2,8);P(11,4,"#d8d8c8",2,8)}
  else if(id==47){P(4,4,"#d00",8,8);P(5,5,"#f44",4,4);P(4,4,"#800",8,1);P(4,4,"#800",1,8)}
  else if(id==48){P(2,2,"#8a4a2a",12,12);P(3,3,"#a00",4,6);P(8,3,"#050",4,6);P(3,9,"#00a",4,3)}
  else{const h=hash(id*13, id*29);const r=Math.floor(70+h*150),gg=Math.floor(70+hash(id*17,id*7)*150),bb=Math.floor(70+hash(id*5,id*11)*150);P(3,3,`rgb(${r},${gg},${bb})`,10,10);for(let q=0;q<8;q++){P(4+((q*3+id)%8),4+((q*5+id)%8),`rgb(${Math.min(255,r+40)},${Math.min(255,gg+40)},${Math.min(255,bb+40)})`,2,2)}}
  return c
}
function itemTex(id){if(ITC[id])return ITC[id];const c=itemCanvas(id);const t=new THREE.CanvasTexture(c);t.magFilter=t.minFilter=THREE.NearestFilter;t.generateMipmaps=false;return ITC[id]=t}
function itemMat(id){if(IMC[id])return IMC[id];
  return IMC[id]=new THREE.MeshBasicMaterial({map:itemTex(id),transparent:true,alphaTest:.05,fog:false,depthTest:false,side:THREE.DoubleSide})}
function mergeGeos(geos){
  const mg=new THREE.BufferGeometry();let pc=0,ic=0;
  for(const g of geos){pc+=g.attributes.position.count;ic+=g.index?g.index.count:g.attributes.position.count}
  const pos=new Float32Array(pc*3),uv=new Float32Array(pc*2),idx=new Uint16Array(ic);let pi=0,ii=0,off=0;
  for(const g of geos){pos.set(g.attributes.position.array,pi*3);uv.set(g.attributes.uv.array,pi*2);
    if(g.index){const ai=g.index.array;for(let i=0;i<ai.length;i++)idx[ii++]=ai[i]+off}
    else{for(let i=0;i<g.attributes.position.count;i++)idx[ii++]=i+off}
    off+=g.attributes.position.count;pi+=g.attributes.position.count}
  mg.setAttribute("position",new THREE.BufferAttribute(pos,3));mg.setAttribute("uv",new THREE.BufferAttribute(uv,2));mg.setIndex(new THREE.BufferAttribute(idx,1));return mg
}
function toolPart(size,pos,rot){
  const g=new THREE.BoxGeometry(size[0],size[1],size[2]);
  g.translate(pos[0],pos[1],pos[2]);
  if(rot)g.rotateX(rot[0]||0),g.rotateY(rot[1]||0),g.rotateZ(rot[2]||0);
  return g;
}
function mergeToolParts(parts){
  const geos=parts.map(a=>a[0]);
  const mg=mergeGeos(geos);
  const cols=[];
  let off=0;
  for(const [g,c] of parts){
    const n=g.attributes.position.count;
    for(let i=0;i<n;i++)cols.push(c[0],c[1],c[2]);
    off+=n;
  }
  mg.setAttribute('color',new THREE.Float32BufferAttribute(cols,3));
  return mg;
}
function toolGeometry(id){
  if(IGC[id])return IGC[id];
  const wood=[0.48,0.29,0.13], wood2=[0.66,0.40,0.18];
  const metal=id==40||id==42?[0.72,0.47,0.18]:id==41?[0.55,0.58,0.60]:id==51?[0.78,0.80,0.82]:id==52?[0.18,0.78,0.92]:id==53?[0.20,0.10,0.24]:[0.55,0.58,0.60];
  const p=[];
  const add=(size,pos,col,rot=[0,0,0])=>p.push([toolPart(size,pos,rot),col]);
  if(id==40||id==41){
    add([.075,.72,.075],[0,-.04,0],wood2);
    add([.52,.095,.10],[0,.30,0],metal);
    add([.12,.12,.11],[-.23,.30,0],metal);add([.12,.12,.11],[.23,.30,0],metal);
  }else if(id==42){
    add([.075,.72,.075],[0,-.04,0],wood2);
    add([.16,.32,.10],[.13,.29,0],metal,[0,0,-.25]);
    add([.30,.10,.10],[.04,.39,0],metal,[0,0,-.25]);
  }else if(id==43){
    add([.075,.72,.075],[0,-.04,0],wood2);
    add([.24,.17,.10],[0,.34,0],metal,[0,0,-.12]);
    add([.15,.09,.10],[0,.43,0],metal,[0,0,-.12]);
  }else if(id>=49&&id<=53){
    add([.075,.78,.075],[0,-.06,0],wood2);
    add([.25,.075,.085],[0,.30,0],wood);
    add([.19,.56,.10],[0,.60,0],metal);
    add([.13,.075,.10],[0,.90,0],metal);
  }else return null;
  return IGC[id]=mergeToolParts(p);
}
function handGeoItem(id){
  if(IGC[id])return IGC[id];
  const tg=toolGeometry(id);
  if(tg)return tg;
  const geo=new THREE.PlaneGeometry(.42,.42);
  return IGC[id]=geo;
}
const handToolMat=new THREE.MeshBasicMaterial({vertexColors:true,transparent:true,depthTest:false,fog:false});
const hand=new THREE.Mesh(handGeo(1),hmat);hand.renderOrder=999;hand.scale.setScalar(.3);cam.add(hand);
const bar=document.getElementById("bar");let cur=0,off=0,invOpen=false,mode=null;
const ICO={},GC={},hg=b=>GC[b]||(GC[b]=isBlockId(b)?handGeo(b):handGeoItem(b));
function icon(id){
  if(ICO[id])return ICO[id];
  const c=document.createElement("canvas");c.width=c.height=32;const x=c.getContext("2d");x.imageSmoothingEnabled=false;
  const px=(a,col)=>{x.fillStyle=col;a.forEach(([i,j])=>x.fillRect(i*2,j*2,2,2))};
  const hd=()=>px(Array.from({length:9},(_,k)=>[3+k,12-k]),"#8a6a3a");
  if(isBlockId(id)&&!isW(id)){
    const t=TILES[id],dr=(n,m,sh)=>{x.setTransform(...m);x.drawImage(T,(n%ATLAS_COLS)*16,Math.floor(n/ATLAS_COLS)*16,16,16,0,0,16,16);if(sh){x.fillStyle=`rgba(0,0,0,${sh})`;x.fillRect(0,0,16,16)}x.setTransform(1,0,0,1,0,0)};
    dr(t[0],[.875,.4375,-.875,.4375,16,2],0);dr(t[1],[.875,.4375,0,.875,2,9],.22);dr(t[1],[.875,-.4375,0,.875,16,16],.4);
  }else if(id==31)hd();
  else if(id==40||id==41){hd();px([[3,5],[4,4],[5,3],[6,3],[7,3],[8,3],[9,4],[10,5],[11,6],[12,7]],id==40?"#b88a4a":"#8d8d8d")}
  else if(id==42){hd();px([[8,2],[9,2],[8,3],[9,3],[10,3],[8,4],[9,4],[10,4],[9,5]],"#b88a4a")}
  else if(id==43){hd();px([[10,2],[11,2],[10,3],[11,3],[12,3],[11,4],[12,4]],"#b88a4a")}
  else if(id==30||id==33){x.fillStyle=id==30?"#e8909a":"#9a5a30";x.fillRect(5,10,22,13);x.fillStyle=id==30?"#f6c0c6":"#c98050";x.fillRect(8,12,11,5)}
  else if(id==32){x.fillStyle="#d02a2a";x.beginPath();x.arc(16,18,9,0,7);x.fill();x.fillStyle="#3a9a2a";x.fillRect(16,5,6,5)}
  else if(id==12){x.fillStyle="#6b3d22";x.fillRect(14,13,4,13);x.fillStyle="#ff9b18";x.beginPath();x.moveTo(16,2);x.lineTo(10,13);x.lineTo(22,13);x.closePath();x.fill();x.fillStyle="#ffe36b";x.fillRect(14,7,4,5)}
  else if(id==44){x.fillStyle="#5a5a6a";x.beginPath();x.moveTo(6,4);x.lineTo(26,4);x.lineTo(26,18);x.quadraticCurveTo(16,30,6,18);x.fill();x.fillStyle="#7a7a8a";x.fillRect(14,4,4,22);x.fillStyle="#3a3a4a";x.fillRect(14,8,4,3)}
  else if(id==45){x.fillStyle="#e8e8d8";x.fillRect(6,6,4,4);x.fillRect(22,6,4,4);x.fillRect(6,20,4,4);x.fillRect(22,20,4,4);x.fillStyle="#e8e8d8";x.fillRect(8,7,16,2);x.fillRect(8,21,16,2);x.fillRect(8,8,2,14);x.fillRect(22,8,2,14)}
  else if(id==47){x.fillStyle="#d00";x.fillRect(12,6,8,20);x.fillStyle="#f44";x.fillRect(14,8,4,16)}
  else if(id==48){x.fillStyle="#8a4a2a";x.fillRect(4,6,24,20);x.fillStyle="#a00";x.fillRect(6,8,4,8);x.fillStyle="#050";x.fillRect(12,8,4,8);x.fillStyle="#00a";x.fillRect(18,8,4,8)}
  else if(isBlockId(id)&&isW(id)){x.fillStyle="#3f8fe8";x.fillRect(3,3,26,26);x.fillStyle="rgba(180,225,255,.55)";x.fillRect(5,6,22,4);}
  else { // Generic item fallback: never leave registry items as blank/transparent icons.
    const cc=itemCanvas(id);x.drawImage(cc,0,0,16,16,0,0,32,32);
  }
  return ICO[id]=c.toDataURL();
}
function drawBar(){bar.innerHTML="";for(let i=0;i<9;i++){const b=HOT[i]||0;const d=document.createElement("div");d.className="s"+(i==cur?" on":"");d.innerHTML=b?`<img src="${icon(b)}"><b>${isSurvivalMode()&&inv[b]>1?inv[b]:""}</b>`:"";d.onclick=()=>pick(i);bar.appendChild(d)}}
drawBar();
function pick(i){cur=((i+1)%10+10)%10-1;[...bar.children].forEach((e,k)=>e.classList.toggle("on",k==cur));refreshHands();if(mode)playSfx("select")}
const hand2=new THREE.Mesh(handGeo(1),hmat);hand2.renderOrder=999;hand2.scale.setScalar(.3);cam.add(hand2);
const heldTorch=new THREE.Group();{const sm=new THREE.MeshBasicMaterial({color:0x6b3d22}),fm=new THREE.MeshBasicMaterial({color:0xffa21a,transparent:true,opacity:.95});const st=new THREE.Mesh(new THREE.BoxGeometry(.07,.42,.07),sm);st.position.y=.05;heldTorch.add(st);const fl=new THREE.Mesh(new THREE.ConeGeometry(.09,.20,5),fm);fl.position.y=.36;heldTorch.add(fl);heldTorch.scale.setScalar(.9);heldTorch.visible=false;heldTorch.renderOrder=1000;cam.add(heldTorch)}
const sk=document.createElement("canvas");sk.width=sk.height=16;
{const x=sk.getContext("2d");for(let i=0;i<16;i++)for(let j=0;j<16;j++){let r=214,gc=164,b=128;if(j<3){r=185;gc=132;b=101}else if(j>12){r=224;gc=176;b=139}const n=hash(i,j+9);r+=((n-.5)*12)|0;gc+=((hash(i,j+3)-.5)*10)|0;b+=((hash(i+4,j)-.5)*8)|0;x.fillStyle=`rgb(${Math.max(0,r)},${Math.max(0,gc)},${Math.max(0,b)})`;x.fillRect(i,j,1,1)}}
const skT=new THREE.CanvasTexture(sk);skT.magFilter=skT.minFilter=THREE.NearestFilter;skT.generateMipmaps=false;
const amat=new THREE.MeshBasicMaterial({map:skT,fog:false,depthTest:false,transparent:true});
const mkArm=()=>{const m=new THREE.Mesh(new THREE.BoxGeometry(.17,.17,.62),amat);m.renderOrder=998;cam.add(m);return m};
const arm=mkArm(),arm2=mkArm();
// Tool system: checks HELD item, not just inventory
const heldItem=()=>cur>=0?HOT[cur]:0;
function tier(c){const h=heldItem();const t=(CFG.tools[c]||[]).find(([id])=>id==h);return t?t[1]:1}
function attackDamage(){const h=heldItem();if(h==53)return 8;if(h==52)return 7;if(h==51)return 6;if(h==50)return 5;if(h==49)return 4;if(h==42)return 7;if(h==41)return 5;if(h==40)return 4;if(h==43)return 3;return CFG.player.attackDamage}
function attackSpeed(){const h=heldItem();if(h>=49&&h<=53)return 1.6;if(h==42)return 1.0;if(h==41)return 1.2;if(h==40)return 1.6;if(h==43)return 1.0;return 4.0}
function attackCooldown(){return 1/attackSpeed()}
function attackStrength(){return Math.min(1,1-atkCd/attackCooldown())}
const stocked=id=>id&&(!isSurvivalMode()||inv[id]>0);
const heldBlock=()=>{const m=cur>=0?HOT[cur]:0;if(stocked(m)&&isBlockId(m)&&!isW(m))return m;if(stocked(off)&&isBlockId(off)&&!isW(off))return off;return 0};
function refreshHands(){
  const m=cur>=0?HOT[cur]:0,mo=stocked(m),oo=stocked(off);
  // Java-style first person: the skin arm stays visible; the held item is layered in front of it.
  arm.visible=!thirdPerson; arm2.visible=!thirdPerson&&!!oo;
  hand.visible=!thirdPerson&&!!mo&&m!=12;
  heldTorch.visible=!thirdPerson&&!!mo&&m==12;
  if(heldTorch.visible){heldTorch.position.set(.45,-.40,-.78);heldTorch.rotation.set(-.35,.25,-.35)}
  if(mo){
    hand.geometry=hg(m);
    const isBlk=isBlockId(m); const isTool=(m>=40&&m<=43)||(m>=49&&m<=53);
    hand.material=isBlk?hmat:(isTool?handToolMat:itemMat(m));
    // Java-style first-person silhouette: tools are true 3-D models, held diagonally
    // in the lower-right hand, close to the camera like the reference screenshot.
    hand.scale.setScalar(isBlk?.46:(isTool?.58:.50));
    if(isBlk){hand.userData.bR=[-.18,.34,-.18];hand.userData.bP=[.43,-.38,-.82]}
    else if(isTool){
      // Java-style first-person tool pose: handle/blade rises from the lower-right
      // hand instead of lying sideways across the screen.
      // The tool shaft points away from the camera (into the world), then rolls slightly
      // to reproduce the lower-right Minecraft-style first-person grip.
      if(m>=49&&m<=53) hand.userData.bR=[-1.12,.18,-.28];
      else if(m==40||m==41) hand.userData.bR=[-1.02,.22,-.34];
      else if(m==42) hand.userData.bR=[-1.08,.16,-.42];
      else if(m==43) hand.userData.bR=[-1.10,.20,-.36];
      else hand.userData.bR=[-1.08,.18,-.32];
      hand.userData.bP=[.46,-.42,-.88]
    }
    else if(m==44){hand.userData.bR=[-.12,.30,-.18];hand.userData.bP=[.44,-.39,-.82]}
    else{hand.userData.bR=[-.10,.30,-.12];hand.userData.bP=[.44,-.36,-.79]}
  }
  hand2.visible=!thirdPerson&&!!oo;
  if(oo){
    hand2.geometry=hg(off);const isBlk2=isBlockId(off);hand2.material=isBlk2?hmat:itemMat(off);hand2.scale.setScalar(isBlk2?.28:.55);
    if(isBlk2){hand2.userData.bR=[-.05,-.15,.08];hand2.userData.bP=[-.42,-.36,-.76]}
    else{hand2.userData.bR=[-.05,-.18,.2];hand2.userData.bP=[-.42,-.30,-.78]}
  }
}
function swapHands(){if(cur<0)return;[HOT[cur],off]=[off,HOT[cur]];updHot()}
function toggleInv(){invOpen=!invOpen;if(locked)document.exitPointerLock();else if(invOpen)ov();else lock()}


function recipeCanCraft(r){return Object.keys(r[1]).every(k=>(inv[k]||0)>=r[1][k])&&(!r[3]||nearFurnace())}
let recipeBookOpen=false;
function craftRecipeAt(index){
  const r=RECIPES[index]; if(!r)return;
  if(!recipeCanCraft(r)){actionBar(r[3]&&!nearFurnace()?"You need a furnace nearby":"Missing ingredients");return}
  for(const k in r[1])takeItem(+k,r[1][k]);
  for(const k in r[2])addItem(+k,r[2][k]);
  updHot(); playSfx("craft");
  // Keep the recipe book open after crafting; refresh it so counts/availability update.
  if(recipeBookOpen) showRecipeBook(); else invUI();
}
function showRecipeBook(){
  recipeBookOpen=true;
  msg.style.display="flex";box.className="mc-inventory";
  const cards=RECIPES.map((r,i)=>{
    const out=+Object.keys(r[2])[0],qty=r[2][out]||1,ok=recipeCanCraft(r);
    return `<button class="recipe-card ${ok?"":"locked"}" data-recipe="${i}" title="${r[0]}">
      <img src="${icon(out)}"><span class="rn">${NM[out]||r[0]}</span><span class="rcount">${qty>1?qty:""}</span>
    </button>`;
  }).join("");
  box.innerHTML=`<div class="recipe-book-panel">
    <div class="recipe-book-head"><span>Recipe Book</span><button class="recipe-close" id="recipe-close" aria-label="Close">×</button></div>
    <div class="recipe-book-sub">${RECIPES.length} recipes · Bright recipes can be crafted now; dim recipes need ingredients${RECIPES.some(r=>r[3])?" or a nearby furnace":""}.</div>
    <div class="recipe-book-list">${cards||'<div style="color:#404040">No recipes available.</div>'}</div>
  </div>`;
  box.querySelectorAll("[data-recipe]").forEach(e=>e.onclick=()=>craftRecipeAt(+e.dataset.recipe));
  const close=box.querySelector("#recipe-close");
  if(close)close.onclick=()=>{recipeBookOpen=false;invUI()};
}

/* ======================= 7. INVENTORY (DRAG-DROP) ======================= */
let dragSrc=null; // {type:"inv"|"hot"|"off", idx:number, id:number}

function creativeInvUI(){
  recipeBookOpen=false;
  msg.style.display="flex";box.className="mc-inventory creative-inv";
  const slot=(id,extra="")=>`<div class="java-slot ${extra}" data-creative-id="${id}" title="${NM[id]||id}"><img src="${icon(id)}"></div>`;
  const page=window._creativePage||0;
  const pageSize=45;
  const start=page*pageSize;
  const ids=PALETTE.slice(start,start+pageSize);
  let grid="";
  for(let i=0;i<pageSize;i++){const id=ids[i]||0;grid+=id?slot(id):`<div class="java-slot empty"></div>`}
  let hb="";
  HOT.forEach((id,i)=>{hb+=`<div class="java-slot ${i==cur?"sel":""}" data-ch="${i}" title="Hotbar slot ${i+1}">${id?`<img src="${icon(id)}">`:""}</div>`});
  const pages=Math.max(1,Math.ceil(PALETTE.length/pageSize));
  box.innerHTML=`<div class="mc-inv-root">
    <div class="creative-tabs">
      <button class="creative-tab active" title="All Items">▦</button>
      <button class="creative-tab" id="creative-prev" title="Previous page">◀</button>
      <button class="creative-tab" id="creative-next" title="Next page">▶</button>
      <button class="creative-tab" id="creative-page" title="Page">${page+1}</button>
      <button class="creative-close" id="creative-close" aria-label="Close">×</button>
    </div>
    <div class="creative-grid">${grid}</div>
    <div class="creative-hotbar-label">Hotbar — click an item to put it in the selected slot</div>
    <div class="creative-hotbar">${hb}</div>
    <div class="creative-hint">Left click = select item · Right click = add to first empty hotbar slot · Creative items are unlimited</div>
  </div>`;
  box.querySelectorAll("[data-creative-id]").forEach(el=>{
    const id=+el.dataset.creativeId;
    el.addEventListener("click",()=>{
      if(!id)return;
      HOT[cur>=0?cur:0]=id; if(cur<0)cur=0; updHot(); creativeInvUI(); playSfx("select");
    });
    el.addEventListener("contextmenu",e=>{
      e.preventDefault(); if(!id)return;
      let idx=HOT.findIndex(x=>!x); if(idx<0)idx=cur>=0?cur:0; HOT[idx]=id; cur=idx; updHot(); creativeInvUI(); playSfx("select");
    });
  });
  box.querySelectorAll("[data-ch]").forEach(el=>el.onclick=()=>{pick(+el.dataset.ch);creativeInvUI()});
  const prev=box.querySelector("#creative-prev"),next=box.querySelector("#creative-next"),close=box.querySelector("#creative-close");
  if(prev)prev.onclick=()=>{window._creativePage=Math.max(0,page-1);creativeInvUI()};
  if(next)next.onclick=()=>{window._creativePage=Math.min(pages-1,page+1);creativeInvUI()};
  if(close)close.onclick=()=>{invOpen=false;msg.style.display="none";recipeBookOpen=false;lock()};
}

function invUI(){
  recipeBookOpen=false;
  msg.style.display="flex";box.className="mc-inventory";
  const sv_=isSurvivalMode();
  if(sv_)syncInvOrder();
  if(mode=="creative") return creativeInvUI();
  const slot=(id,n,ex="",attrs="")=>`<div class="java-slot ${ex}" ${attrs}>${id?`<img src="${icon(id)}">`:""}${n?`<b>${n}</b>`:""}</div>`;
  // Java inventory has fixed physical slots. Never compress items together: empty slots remain empty.
  let invGrid="";
  // The hotbar is the bottom row of the same player inventory in Java Edition,
  // but it must NOT be rendered a second time in the 27-slot main grid.
  // Keep physical slot positions intact; slots occupied by hotbar/offhand items
  // are simply empty in the main-inventory view.
  const reservedIds=new Set(HOT.filter(Boolean)); if(off)reservedIds.add(off);
  for(let i=0;i<27;i++){
    let id=sv_?invSlotId(i):(PALETTE[i]||0);
    if(sv_&&id&&reservedIds.has(id))id=0;
    invGrid+=slot(id,id&&sv_?(inv[id]||0):"",id?"":"empty",`data-inv="${i}" data-id="${id}"${id?' draggable="true"':''} title="${id?(NM[id]||id):'Empty slot'}"`);
  }
  let hb="";HOT.forEach((id,i)=>hb+=slot(id,id&&sv_?(inv[id]||0):"",i==cur?"sel":"",`data-h="${i}" data-id="${id}"${id?' draggable="true"':''} title="${id?(NM[id]||id):'Hotbar slot '+(i+1)}"`));
  const offSlot=slot(off,off&&sv_?(inv[off]||0):"",off?"":"empty",`data-o="1" data-id="${off}"${off?' draggable="true"':''} title="Offhand"`);
  // Crafting preview: choose the first craftable recipe, matching the compact Java inventory output slot.
  const craftIndex=RECIPES.findIndex(r=>Object.keys(r[1]).every(k=>(inv[k]||0)>=r[1][k])&&(!r[3]||nearFurnace()));
  const craftRecipe=craftIndex>=0?RECIPES[craftIndex]:null;
  const craftOut=craftRecipe?+Object.keys(craftRecipe[2])[0]:0;
  box.innerHTML=`<div class="mc-inv-root">
    <div class="mc-inv-preview">
      <div class="mc-avatar-preview"><div class="avp-head"></div><div class="avp-body"></div><div class="avp-arm l"></div><div class="avp-arm r"></div><div class="avp-leg l"></div><div class="avp-leg r"></div></div>
      <div class="mc-armor">${ARMOR.map((id,i)=>slot(id,id&&sv_?(inv[id]||1):"","armor",`data-armor="${i}" draggable="${id?"true":"false"}"`)).join("")}</div>
    </div>
    <div class="mc-crafting-title">Crafting</div>
    <div class="mc-crafting"><div class="craft2">${slot(0,"","craft-slot",`data-craft="0"`)}${slot(0,"","craft-slot",`data-craft="1"`)}${slot(0,"","craft-slot",`data-craft="2"`)}${slot(0,"","craft-slot",`data-craft="3"`)}</div><div class="craft-arrow">➜</div><div class="craft-output" ${craftOut?`data-r="${craftIndex}" title="${craftRecipe[0]}"`:''}>${craftOut?`<img src="${icon(craftOut)}">`:''}${craftOut&&craftRecipe[2][craftOut]>1?`<b>${craftRecipe[2][craftOut]}</b>`:''}</div></div>
    <button class="recipe-book" id="recipe-book" title="Recipe Book">▤</button>
    <div class="mc-inv-grid">${invGrid}</div>
    <div class="mc-hotbar-grid">${hb}</div>
    <div class="mc-inv-offhand">${offSlot}</div>
  </div>`;

  let dragSrc=null;
  const readSlot=el=>({type:el.dataset.inv!=null?'inv':el.dataset.h!=null?'hot':el.dataset.o?'off':el.dataset.armor!=null?'armor':null,idx:el.dataset.inv!=null?+el.dataset.inv:el.dataset.h!=null?+el.dataset.h:el.dataset.armor!=null?+el.dataset.armor:0,id:+(el.dataset.id||0)});
  const getSlot=(type,idx)=>type==='inv'?invSlotId(idx):type==='hot'?(HOT[idx]||0):type==='off'?off:type==='armor'?(ARMOR[idx]||0):0;
  const putSlot=(type,idx,id)=>{if(type==='inv')setInvSlot(idx,id);else if(type==='hot')HOT[idx]=id;else if(type==='off')off=id;else if(type==='armor')ARMOR[idx]=id};
  const slots=box.querySelectorAll('[data-inv],[data-h],[data-o],[data-armor]');
  slots.forEach(el=>{
    el.addEventListener('dragstart',e=>{
      const d=readSlot(el); if(!d.id){e.preventDefault();return;}
      dragSrc=d;e.dataTransfer.effectAllowed='move';e.dataTransfer.setData('text/plain',String(d.id));el.classList.add('dragging');
    });
    el.addEventListener('dragend',()=>el.classList.remove('dragging'));
    el.addEventListener('dragover',e=>{e.preventDefault();el.classList.add('drag-over')});
    el.addEventListener('dragleave',()=>el.classList.remove('drag-over'));
    el.addEventListener('drop',e=>{
      e.preventDefault();el.classList.remove('drag-over');if(!dragSrc)return;
      const target=readSlot(el);if(!target.type)return;
      if(dragSrc.type===target.type&&dragSrc.idx===target.idx){dragSrc=null;return;}
      const a=dragSrc.id,b=getSlot(target.type,target.idx);
      if(target.type==='armor'){if(armorSlotFor(a)!==target.idx){dragSrc=null;return;}putSlot('armor',target.idx,a);if(dragSrc.type==='inv'){takeItem(a,1)}else if(dragSrc.type==='hot'){HOT[dragSrc.idx]=b}else if(dragSrc.type==='off'){off=b}renderArmor();updHot();invUI();dragSrc=null;return;}
      // Swap whole stacks between fixed slots, like inventory management in Java.
      putSlot(target.type,target.idx,a);
      putSlot(dragSrc.type,dragSrc.idx,b);
      dragSrc=null;updHot();invUI();
    });
    el.addEventListener('contextmenu',e=>{
      e.preventDefault();
      const d=readSlot(el);if(!d.id)return;
      if(d.type==='inv'&&armorSlotFor(d.id)>=0){const ai=armorSlotFor(d.id),old=ARMOR[ai];ARMOR[ai]=d.id;takeItem(d.id,1);if(old)addItem(old,1)}else if(d.type==='inv'){off=d.id}else if(d.type==='hot'){const old=off;off=d.id;HOT[d.idx]=old}else if(d.type==='off'){const old=HOT[cur]>=0?HOT[cur]:0;off=old;HOT[cur]=d.id}else if(d.type==='armor'){const old=ARMOR[d.idx];ARMOR[d.idx]=0;if(old)addItem(old,1)}
      updHot();invUI();
    });
    el.addEventListener('click',()=>{
      const d=readSlot(el);
      if(d.type==='hot'){pick(d.idx);invUI();}
      else if(d.type==='inv'&&d.id){const ai=armorSlotFor(d.id);if(ai>=0){const old=ARMOR[ai];ARMOR[ai]=d.id;takeItem(d.id,1);if(old)addItem(old,1);updHot();renderArmor();invUI();}else{if(cur<0)cur=0;const old=HOT[cur];HOT[cur]=d.id;setInvSlot(d.idx,old);updHot();invUI();}}
      else if(d.type==='armor'&&d.id){const old=d.id;ARMOR[d.idx]=0;addItem(old,1);renderArmor();invUI();}
    });
  });
  const co=box.querySelector('.craft-output');
  if(co&&craftOut)co.onclick=()=>craftRecipeAt(craftIndex);
  const rb=box.querySelector('#recipe-book');if(rb)rb.onclick=()=>showRecipeBook();
}
/* ======================= 8. REDSTONE ======================= */
const leverStates=new Set(); // "x,y,z" of active levers
function redstoneTick(){
  lampLit.clear();
  const powered=new Set([...leverStates]);
  const q=[...leverStates];let qi=0,steps=0;
  while(qi<q.length&&steps++<128){const k=q[qi++],[x,y,z]=k.split(",").map(Number);for(const[a,b,c]of[[1,0,0],[-1,0,0],[0,1,0],[0,-1,0],[0,0,1],[0,0,-1]]){const nx=x+a,ny=y+b,nz=z+c,b2=get(nx,ny,nz),nk=nx+","+ny+","+nz;if(b2==14)lampLit.add(nk);if(b2==260&&!powered.has(nk)){powered.add(nk);q.push(nk)}if(b2==261&&!powered.has(nk)){powered.add(nk);q.push(nk)}}}
  // Powered pistons push the block immediately beyond them away from the source.
  for(const k of powered){const[x,y,z]=k.split(",").map(Number);for(const[a,b,c]of[[1,0,0],[-1,0,0],[0,1,0],[0,-1,0],[0,0,1],[0,0,-1]]){const px=x+a,py=y+b,pz=z+c,pb=get(px,py,pz);if(pb==BI("Piston")||pb==BI("Sticky Piston")){const tx=px+a,ty=py+b,tz=pz+c;if(get(tx,ty,tz)==0&&get(px,py,pz)!=0){const moved=get(px,py,pz);pset(px,py,pz,0);pset(tx,ty,tz,moved)}}}}
  if(lampLit.size>0||leverStates.size>0){for(const k of[...lampLit,...leverStates,...powered]){const[x,,z]=k.split(",").map(Number);dirtyAt(x,z)}}
}
function toggleLever(x,y,z){
  const k=x+","+y+","+z;
  if(leverStates.has(k)){leverStates.delete(k);actionBar("Lever off")}else{leverStates.add(k);actionBar("Lever on")}
  redstoneTick();
}

function switchDimension(next){
  if(!mode||next===dimension)return;
  dimStates[dimension]={CE,p:[p.x,p.y,p.z,p.yaw,p.pitch],tod};
  dimension=next;biomeName=dimension==="nether"?"Nether Wastes":dimension==="end"?"The End":"Plains";CE=dimStates[dimension]?.CE||{};
  chunks.clear();meshes.forEach(ms=>ms.forEach(o=>{scene.remove(o);o.geometry.dispose()}));meshes.clear();dirty.clear();mobs.splice(0).forEach(o=>scene.remove(o.g));worldDrops.splice(0).forEach(o=>scene.remove(o.g));lampLit.clear();leverStates.clear();
  const ds=dimStates[dimension];if(ds?.p){p.x=ds.p[0];p.y=ds.p[1];p.z=ds.p[2];p.yaw=ds.p[3]||0;p.pitch=ds.p[4]||0;tod=ds.tod??CFG.time.start}else{const sp=findSpawn();Object.assign(p,sp);p.vy=0;tod=CFG.time.start}
  updChunks(true,3);actionBar("Entered "+(dimension==="overworld"?"the Overworld":dimension==="nether"?"the Nether":"The End"));save();
}
/* ======================= 9. COMMANDS ======================= */
const cmdInput=document.getElementById("cmdinput");
function structureListUI(){
  msg.style.display="flex";box.className="wide";
  box.innerHTML=`<h1>Structures</h1><p>Available structure types in this Blockcraft content registry:</p><div style="display:grid;grid-template-columns:1fr 1fr;gap:5px;text-align:left">${JAVA_STRUCTURES.map(n=>`<div style="background:#aaa;padding:5px;border:1px solid #555">${escHtml(n)}</div>`).join("")}</div><button id=slback>Back</button>`;
  box.querySelector("#slback").onclick=()=>ov();
}

function openCommand(){cmdInput.style.display="block";cmdInput.value="/";cmdInput.focus()}
function closeCommand(){cmdInput.style.display="none";cmdInput.blur()}
function runCommand(input){
  const parts=input.trim().split(/\s+/);
  const cmd=parts[0].toLowerCase().replace(/^\//,"");
  const args=parts.slice(1);
  if(cmd=="give"){
    const name=args[0]?args[0].toLowerCase():"";const cnt=args[1]?+args[1]:1;
    const id=findByName(name);if(id==null){actionBar("Unknown item: "+name);return}
    addItem(id,cnt);updHot();actionBar("Gave "+cnt+" "+NM[id]);
  }else if(cmd=="tp"){
    if(args.length>=3){p.x=+args[0];p.y=+args[1];p.z=+args[2];actionBar("Teleported")}
    else actionBar("Usage: /tp <x> <y> <z>");
  }else if(cmd=="time"){
    if(args[0]=="day")tod=0.25;else if(args[0]=="noon")tod=0.5;else if(args[0]=="night")tod=0;else if(args[0]=="set"&&args[1])tod=Math.max(0,Math.min(1,+args[1]/24));
    else{actionBar("Usage: /time <day|night|noon>");return}
    actionBar("Time set");
  }else if(cmd=="gamemode"){
    if(args[0]=="survival"||args[0]=="s"){mode="survival";actionBar("Gamemode: survival")}
    else if(args[0]=="creative"||args[0]=="c"){mode="creative";actionBar("Gamemode: creative")}
    else actionBar("Usage: /gamemode <survival|creative>");
  }else if(cmd=="kill"){hp=0;hurt(99);actionBar("Killed")}
  else if(cmd=="fly"){fly=!fly;actionBar(fly?"Flying":"Not flying")}
  else if(cmd=="seed"){actionBar("Seed: "+seed)}
  else if(cmd=="heal"){hp=CFG.survival.maxHp;food=CFG.survival.maxFood;actionBar("Healed")}
  else if(cmd=="help"){actionBar("/give /tp /time /gamemode /kill /fly /seed /heal /weather")}
  else if(cmd=="weather"){actionBar("Weather not implemented yet")}
  else actionBar("Unknown command. Type /help");
}
function findByName(name){
  for(const id in NM){if(NM[id].toLowerCase().replace(/\s/g,"")==name)return+id}
  return null;
}
cmdInput.addEventListener("keydown",e=>{
  if(e.key=="Enter"){runCommand(cmdInput.value);closeCommand()}
  else if(e.key=="Escape"){closeCommand()}
  e.stopPropagation();
});

/* ====================== 10. PLAYER & INPUT ====================== */
const p={x:.5,y:H,z:.5,vx:0,vy:0,vz:0,yaw:0,pitch:0,ground:false};
const sol=(x,y,z)=>solid(get(Math.floor(x),Math.floor(y),Math.floor(z)));
function hit(px,py,pz,w=.3,h=1.79){for(const x of[px-w,px+w])for(const y of[py+.01,py+h/2,py+h])for(const z of[pz-w,pz+w])if(sol(x,y,z))return true;return false}
const keys={};let locked=false,mL=0,mR=0,fly=false,spr=false,lastW=0,lastSp=0;
let thirdPerson=false,debugScreen=false,dropCooldown=0;
const playerAvatar=new THREE.Group(); scene.add(playerAvatar);
const avatarSkin=new THREE.MeshBasicMaterial({color:0xf0c090,fog:false});
const avatarShirt=new THREE.MeshBasicMaterial({color:0x3f78c5,fog:false});
const avatarPants=new THREE.MeshBasicMaterial({color:0x273d91,fog:false});
const avatarHair=new THREE.MeshBasicMaterial({color:0x3b2417,fog:false});
const avatarParts={};
const avatarHeld=new THREE.Mesh(new THREE.PlaneGeometry(.24,.24),new THREE.MeshBasicMaterial({transparent:true,side:THREE.DoubleSide,fog:false,depthTest:true}));
avatarHeld.visible=false;avatarHeld.position.set(.43,.95,-.25);avatarHeld.rotation.set(0,0,0);playerAvatar.add(avatarHeld);
function avPart(name,w,h,d,mat,x,y,z){const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),mat);m.position.set(x,y,z);m.renderOrder=10;playerAvatar.add(m);avatarParts[name]=m;return m}
function buildPlayerAvatar(){
  if(playerAvatar.children.length)return;
  avPart("torso",.60,.72,.34,avatarShirt,0,1.05,0);
  avPart("head",.60,.60,.60,avatarSkin,0,1.71,0);
  avPart("hair",.62,.16,.62,avatarHair,0,2.00,0);
  avPart("leftArm",.20,.72,.20,avatarShirt,-.41,1.05,0);
  avPart("rightArm",.20,.72,.20,avatarShirt,.41,1.05,0);
  avPart("leftLeg",.20,.82,.20,avatarPants,-.20,.41,0);
  avPart("rightLeg",.20,.82,.20,avatarPants,.20,.41,0);
  playerAvatar.visible=false;
}
addEventListener("keydown",e=>{
  if(document.activeElement==cmdInput)return;
  keys[e.code]=1;if(e.repeat)return;const n=performance.now();
  if(e.code=="KeyW"){if(n-lastW<300)spr=true;lastW=n}
  if(e.code=="Space"){if(n-lastSp<300&&mode=="creative")fly=!fly;lastSp=n}
  if(e.key>="1"&&e.key<="9"){pick(+e.key-1);playSfx("select")}if(e.key=="0"){pick(-1);playSfx("select")}
  if(e.code=="KeyF"&&mode&&locked){swapHands();playSfx("swap")}
  if(e.code=="KeyG")eat();
  if(e.code=="KeyQ"&&mode&&locked){dropHeld();playSfx("drop")}
  if(e.code=="KeyE"&&mode&&document.activeElement.tagName!="INPUT"){toggleInv();playSfx(invOpen?"open":"close")}
  if(e.code=="KeyF5"&&mode&&locked){thirdPerson=!thirdPerson;actionBar(thirdPerson?"Third person":"First person");playSfx("click")}
  if(e.code=="F1"&&mode&&locked){hudHidden=!hudHidden;["bar","status","off","cross","attack-indicator"].forEach(id=>{const el=document.getElementById(id);if(el)el.style.display=hudHidden?"none":""})}
  if(e.code=="F3"&&mode&&locked){debugScreen=!debugScreen;dbg.style.display=debugScreen?"block":"none"}
  if(e.code=="Escape"&&mode&&locked){document.exitPointerLock()}
  if(e.code=="KeyT"||e.code=="Slash"){if(mode&&locked){e.preventDefault();openCommand()}}
});
addEventListener("auxclick",e=>{
  if(e.button!==1||!locked)return;
  const r=ray();if(!r)return;
  const b=get(...r.c);
  if(mode=="creative"||inv[b]>0){
    const i=HOT.indexOf(b);
    if(i>=0)pick(i);
    else{const slot=cur>=0?cur:0;HOT[slot]=b;pick(slot);updHot()}
  }
});
addEventListener("keyup",e=>{keys[e.code]=0;if(e.code=="KeyW")spr=false});

/* ====================== 11. MENUS, ITEMS & SAVING ====================== */
const msg=document.getElementById("msg"),box=document.getElementById("box"),hpE=document.getElementById("hp");
let hp=20,food=20,sv=0,zt=0,pt=0,wt=0,hT=0,regen=0,wname="",scr="main",pm="survival";
let xp=0,xpLvl=0,shielding=false,atkCd=0,sprintAttackState=false,sneakAttackState=false,hudHidden=false;
const inv={},home={x:.5,y:40,z:.5};
const ARMOR=[0,0,0,0];
function armorSlotFor(id){const n=(ITEMS[id]?.name||"").toLowerCase();if(!ITEMS[id]?.armor)return -1;if(n.includes("helmet"))return 0;if(n.includes("chestplate"))return 1;if(n.includes("leggings"))return 2;if(n.includes("boots"))return 3;return -1}
function armorValue(){return ARMOR.reduce((sum,id)=>sum+(ITEMS[id]?.armor||0),0)}
// Java-style persistent inventory slot order (27 main-inventory slots).
let invOrder=[];
function syncInvOrder(){
  invOrder=invOrder.filter(id=>inv[id]>0);
  for(const id of Object.keys(inv).map(Number)){if(inv[id]>0&&!invOrder.includes(id))invOrder.push(id)}
  invOrder=invOrder.slice(0,27);
}
function invSlotId(i){syncInvOrder();return invOrder[i]||0}
function setInvSlot(i,id){syncInvOrder();const old=invOrder[i]||0;if(id){const other=invOrder.indexOf(id);if(other>=0&&other!==i)invOrder[other]=old||0;invOrder[i]=id}else invOrder[i]=0;invOrder=invOrder.filter(Boolean);syncInvOrder()}
const mineTime=b=>mode=="creative"?CFG.creative.mineTime:(BLOCKS[b]?.hard||.5)/tier(BLOCKS[b]?.tool);
function addItem(id,n=1){inv[id]=(inv[id]||0)+n;if(isSurvivalMode())syncInvOrder();if(BLOCKS[id]&&id!=9&&isSurvivalMode()&&!HOT.includes(id)){const i=HOT.indexOf(0);if(i>=0)HOT[i]=id}}
function takeItem(id,n=1){inv[id]=(inv[id]||0)-n;if(inv[id]<=0){delete inv[id];invOrder=invOrder.filter(x=>x!==id);if(isSurvivalMode()){HOT.forEach((h,i)=>{if(h==id)HOT[i]=0});if(off==id)off=0}}}
function drop(b,x=p.x,y=p.y,z=p.z){
  if(!isSurvivalMode())return;const d=BLOCKS[b];if(!d)return;
  if(d.needsTool&&tier(d.tool)<=1)return;
  const id=d.drop===undefined?b:d.drop;if(id)spawnDrop(id,x,y,z);
  if(d.bonus&&Math.random()<d.bonus.chance)spawnDrop(d.bonus.id,x,y,z);
  if(b==3||b==8||b==13)addXp(1);
}

const FOODS=Object.entries(ITEMS).filter(e=>e[1].food).sort((a,b)=>b[1].food-a[1].food);
function eat(){if(!isSurvivalMode()||food>=CFG.survival.maxFood)return;const m=cur>=0?HOT[cur]:0;if(m&&ITEMS[m]?.food&&inv[m]>0){takeItem(m);food=Math.min(CFG.survival.maxFood,food+ITEMS[m].food);updHot();playSfx("eat");return}if(off&&ITEMS[off]?.food&&inv[off]>0){takeItem(off);food=Math.min(CFG.survival.maxFood,food+ITEMS[off].food);updHot();playSfx("eat");return}for(const[id,it]of FOODS)if(inv[id]>0){takeItem(id);food=Math.min(CFG.survival.maxFood,food+it.food);updHot();playSfx("eat");return}}
const worldDrops=[];
function spawnDrop(id,x,y,z,vx=0,vz=0){if(!id)return;const g=new THREE.Group(),isBlk=isBlockId(id)&&!isW(id);let mesh;if(id==12){const sm=new THREE.MeshBasicMaterial({color:0x6b3d22}),fm=new THREE.MeshBasicMaterial({color:0xffa21a});const st=new THREE.Mesh(new THREE.BoxGeometry(.055,.28,.055),sm);st.position.y=-.02;g.add(st);const fl=new THREE.Mesh(new THREE.ConeGeometry(.065,.16,5),fm);fl.position.y=.20;g.add(fl);mesh=g}else if(isBlk){mesh=new THREE.Mesh(handGeo(id),new THREE.MeshBasicMaterial({map:tex,vertexColors:true,alphaTest:.5,side:THREE.DoubleSide}));mesh.scale.setScalar(.28);g.add(mesh)}else{mesh=new THREE.Mesh(new THREE.BoxGeometry(.24,.24,.08),new THREE.MeshBasicMaterial({map:itemTex(id),transparent:true,alphaTest:.05,side:THREE.DoubleSide}));g.add(mesh)}mesh.rotation.set(Math.random()*Math.PI,Math.random()*Math.PI,Math.random()*Math.PI);g.position.set(x,y,z);scene.add(g);worldDrops.push({id,g,vy:.8,vx,vz,age:0,pickup:.35,spin:1.8+Math.random()*1.2})}
function dropGroundY(x,z,startY){const ix=Math.floor(x),iz=Math.floor(z);let top=0;for(let yy=Math.min(H-1,Math.floor(startY));yy>=0;yy--){if(solid(get(ix,yy,iz))){top=yy+1;break}}return top}
function updateDrops(dt){for(let i=worldDrops.length-1;i>=0;i--){const d=worldDrops[i];d.age+=dt;d.pickup=Math.max(0,d.pickup-dt);d.vy-=18*dt;d.vx*=Math.pow(.08,dt);d.vz*=Math.pow(.08,dt);d.g.position.x+=d.vx*dt;d.g.position.z+=d.vz*dt;const support=dropGroundY(d.g.position.x,d.g.position.z,d.g.position.y+1);const ny=d.g.position.y+d.vy*dt;if(ny-.14<=support){d.g.position.y=support+.16;d.vy=Math.abs(d.vy)*.25;if(Math.abs(d.vy)<.12)d.vy=0}else d.g.position.y=ny;d.g.rotation.y+=d.spin*dt;const base=d.g.position.y;d.g.position.y=base+Math.sin(d.age*3.2)*.025;const dx=p.x-d.g.position.x,dy=p.y+.7-d.g.position.y,dz=p.z-d.g.position.z;if(d.pickup<=0&&Math.hypot(dx,dz)<1.35&&Math.abs(dy)<1.8){addItem(d.id,1);updHot();playSfx("pickup");scene.remove(d.g);gDispose(d.g);worldDrops.splice(i,1)}}}
function gDispose(g){g.traverse(o=>{if(o.geometry)o.geometry.dispose();if(o.material){if(Array.isArray(o.material))o.material.forEach(m=>m.dispose&&m.dispose());else if(o.material.dispose)o.material.dispose()}})}
function dropHeld(){
  if(!isSurvivalMode()||dropCooldown>0||cur<0)return;
  const id=HOT[cur]; if(!id||!inv[id])return;
  const dirX=Math.sin(p.yaw),dirZ=Math.cos(p.yaw);
  takeItem(id,1);
  spawnDrop(id,p.x+dirX*.48,p.y+.85,p.z+dirZ*.48,dirX*3.4,dirZ*3.4);
  updHot();actionBar("Dropped "+(NM[id]||"item"));
  dropCooldown=.15;
}
function nearFurnace(){const x=Math.floor(p.x),y=Math.floor(p.y),z=Math.floor(p.z);for(let a=-4;a<=4;a++)for(let b=-3;b<=4;b++)for(let c=-4;c<=4;c++)if(get(x+a,y+b,z+c)==11)return true;return false}
const worlds=()=>{try{return JSON.parse(store.get("bcwl:"+currentUser))||[]}catch(e){return[]}};
function save(){if(!mode)return;try{dimStates[dimension]={CE,p:[p.x,p.y,p.z,p.yaw,p.pitch],tod};store.set("bcw:"+currentUser+":"+wname,JSON.stringify({mode,seed,CE,inv,hot:HOT,off,armor:ARMOR,hp,food,xp,xpLvl,tod,settings,p:[p.x,p.y,p.z,p.yaw,p.pitch],dimension,hardcore,dimensions:dimStates}));const l=worlds();if(!l.includes(wname)){l.push(wname);store.set("bcwl:"+currentUser,JSON.stringify(l))}}catch(e){}}
/* Login system */
let currentUser=null;
const getUsers=()=>{try{return JSON.parse(store.get("bcusers"))||{}}catch(e){return{}}};
const saveUsers=u=>store.set("bcusers",JSON.stringify(u));
const hashPwd=p=>{let h=0;for(let i=0;i<p.length;i++)h=((h*31)+p.charCodeAt(i))|0;return"h"+Math.abs(h)};
const SPLASHES=["Blocky fun!","Now with multiplayer!","Mine it!","Build it!","100% voxel!","Redstone powered!","Survive the night!","Craft everything!"];
function showLogin(){
  msg.style.display="flex";msg.classList.add("menu-bg");box.className="login-box";
  const splash=SPLASHES[Math.floor(Math.random()*SPLASHES.length)];
  box.innerHTML=`<h2>Blockcraft</h2><div class=splash>${splash}</div>
    <input id=li-user placeholder="Username" maxlength=16 autocomplete=off>
    <input id=li-pwd type=password placeholder="Password" maxlength=20 autocomplete=off>
    <button id=li-login>Login</button>
    <button id=li-reg>Register</button>
    <small>Register creates a new account. Your worlds are saved per-account.</small>`;
  const u=box.querySelector("#li-user"),p=box.querySelector("#li-pwd");
  const on=(id,f)=>{const e=box.querySelector("#"+id);if(e)e.onclick=f};
  on("li-login",()=>{const un=u.value.trim();if(!un||!p.value)return;const users=getUsers();if(users[un]&&users[un].pwd===hashPwd(p.value)){currentUser=un;store.set("bcsession",un);msg.classList.remove("menu-bg");scr="main";ov()}else{alert("Invalid username or password")}});
  on("li-reg",()=>{const un=u.value.trim();if(!un||!p.value)return;const users=getUsers();if(users[un]){alert("Username already exists")}else{users[un]={pwd:hashPwd(p.value)};saveUsers(users);currentUser=un;store.set("bcsession",un);msg.classList.remove("menu-bg");scr="main";ov()}});
}
function logoutUser(){mpLeave();currentUser=null;store.del("bcsession");mode=null;msg.classList.add("menu-bg");showLogin()}
const lock=()=>{try{const pl=renderer.domElement.requestPointerLock();if(pl&&pl.catch)pl.catch(()=>{locked=true})}catch(e){locked=true}};
function updHot(){
  [...bar.children].forEach((e,k)=>{
    const id=HOT[k]||0,n=inv[id]||0;
    const count=e.querySelector("b"); if(count)count.textContent=isSurvivalMode()&&id?n:"";
    e.style.opacity=isSurvivalMode()&&id&&!n?.45:1;
    let img=e.querySelector("img");
    if(id){if(!img){img=document.createElement("img");e.prepend(img)}img.src=icon(id)}else if(img)img.remove();
  });
  const offEl=document.getElementById("off");
  if(offEl){
    offEl.style.display=off?"block":"none";
    offEl.innerHTML=off?`<img src="${icon(off)}" style="width:100%;height:100%;image-rendering:pixelated"><b>${isSurvivalMode()?(inv[off]||0):""}</b>`:"";
  }
  refreshHands();
}
function findSpawn(){
  if(dimension==="nether")return{x:.5,y:50,z:.5};
  if(dimension==="end")return{x:.5,y:Math.max(35,endHeight(0,0)+1),z:.5};
  for(let r=0;r<80;r+=2)for(let dx=-r;dx<=r;dx+=2)for(let dz=-r;dz<=r;dz+=2){if(Math.max(Math.abs(dx),Math.abs(dz))!=r)continue;let y=H-1;while(y>0&&!get(dx,y,dz))y--;if(solid(get(dx,y,dz))&&!get(dx,y+1,dz))return{x:dx+.5,y:y+1,z:dz+.5}}
  return{x:.5,y:40,z:.5};
}
function applySettings(){
  CFG.player.fov=settings.fov;CFG.player.sprintFov=settings.fov+8;
  CFG.player.sensitivity=settings.sensitivity;
  R=settings.renderDist;
  if(settings.graphics=="fast"){renderer.setPixelRatio(1);clouds.visible=false;scene.fog.far=CFG.render.fogFar*0.6}
  else{renderer.setPixelRatio(Math.min(devicePixelRatio,2));clouds.visible=true;scene.fog.far=CFG.render.fogFar}
}
function start(m,name,sd,s){
  mode=m;wname=name;seed=sd;dimension=s?.dimension||"overworld";hardcore=m==="hardcore";dimStates=s?.dimensions||{};CE=s?.CE||{};fly=false;hp=CFG.survival.maxHp;food=CFG.survival.maxFood;xp=0;xpLvl=0;
  if(s&&s.settings){Object.assign(settings,s.settings)}settings.showOffhand=true;applySettings();
  chunks.clear();meshes.forEach(ms=>ms.forEach(o=>{scene.remove(o);o.geometry.dispose()}));meshes.clear();dirty.clear();act.clear();
  lampLit.clear();leverStates.clear();
  mobs.splice(0).forEach(o=>scene.remove(o.g));for(const k in inv)delete inv[k];ARMOR.fill(0);
  HOT.splice(0,HOT.length,...(m=="creative"?CFG.creativeHotbar:new Array(9).fill(0)));off=0;
  const sp=findSpawn();Object.assign(home,sp);p.x=sp.x;p.y=sp.y;p.z=sp.z;p.vy=0;
  if(s){ARMOR.splice(0,4,...(s.armor||[0,0,0,0]));if(s.dimensions&&s.dimensions[dimension]){CE=s.dimensions[dimension].CE||CE;if(s.dimensions[dimension].p)Object.assign(p,{x:s.dimensions[dimension].p[0],y:s.dimensions[dimension].p[1],z:s.dimensions[dimension].p[2],yaw:s.dimensions[dimension].p[3]||0,pitch:s.dimensions[dimension].p[4]||0});}Object.assign(inv,s.inv||{});if(s.hot){HOT.splice(0,9,...s.hot);off=s.off||0}else for(const k in s.inv)addItem(+k,0);hp=s.hp??CFG.survival.maxHp;food=s.food??CFG.survival.maxFood;xp=s.xp||0;xpLvl=s.xpLvl||0;tod=s.tod??CFG.time.start;[p.x,p.y,p.z,p.yaw,p.pitch]=[...(s.p||[p.x,p.y,p.z,0,0]),0,0].slice(0,5);home.x=p.x;home.y=p.y;home.z=p.z}
  if(!s&&isSurvivalMode())for(const k in CFG.survival.startItems)addItem(+k,CFG.survival.startItems[k]);
  updChunks(true,3);updHot();renderArmor();renderHearts();renderHunger();renderXpBar();hpE.style.display="none";document.getElementById("status").style.display=isSurvivalMode()?"flex":"none";document.getElementById("bar").style.display="flex";document.getElementById("off").style.display=off?"block":"none";msg.style.display="none";invOpen=false;lock();
}
function settingsUI(){
  msg.style.display="flex";box.className="wide";
  box.innerHTML=`<div class=inv-title>Settings</div>
    <div class=slider-row><label>FOV</label><input type=range id=sfov min=50 max=110 value=${settings.fov}><span id=sfovval>${settings.fov}</span></div>
    <div class=slider-row><label>Render Distance</label><input type=range id=srd min=2 max=8 value=${settings.renderDist}><span id=srdval>${settings.renderDist}</span></div>
    <div class=slider-row><label>Mouse Sens.</label><input type=range id=ssens min=1 max=50 value=${Math.round(settings.sensitivity*10000)}><span id=ssensval>${settings.sensitivity.toFixed(4)}</span></div>
    <div class=slider-row><label>Graphics</label><button id=sgfx style="display:inline;min-width:80px;padding:4px 10px">${settings.graphics}</button></div><div class=slider-row><label>Offhand HUD</label><button id=soff style="display:inline;min-width:80px;padding:4px 10px">ON</button></div>
    <button id=sback>Back</button>
    <small>Settings apply immediately and are saved with your world.</small>`;
  const fovEl=box.querySelector("#sfov"),fovVal=box.querySelector("#sfovval");
  const offEl=box.querySelector("#soff");if(offEl)offEl.onclick=()=>{settings.showOffhand=true;offEl.textContent="ON";updHot();store.set("bcsettings",JSON.stringify(settings))};
  fovEl.oninput=()=>{settings.fov=+fovEl.value;fovVal.textContent=settings.fov;applySettings()};
  const rdEl=box.querySelector("#srd"),rdVal=box.querySelector("#srdval");
  rdEl.oninput=()=>{settings.renderDist=+rdEl.value;rdVal.textContent=settings.renderDist;applySettings();updChunks(true)};
  const sensEl=box.querySelector("#ssens"),sensVal=box.querySelector("#ssensval");
  sensEl.oninput=()=>{settings.sensitivity=+sensEl.value/10000;sensVal.textContent=settings.sensitivity.toFixed(4);applySettings()};
  const gfxEl=box.querySelector("#sgfx");
  gfxEl.onclick=()=>{settings.graphics=settings.graphics=="fancy"?"fast":"fancy";gfxEl.textContent=settings.graphics;applySettings()};
  box.querySelector("#sback").onclick=()=>{save();ov()};
}
function ov(){
  msg.style.display="flex";
  msg.classList.remove("menu-bg","java-main","world-screen");
  box.className="";
  if(!currentUser){showLogin();return}
  if(mode&&invOpen)return invUI();

  if(!mode&&scr=="main"){
    msg.classList.add("java-main");box.className="java-main-box";
    const splash=SPLASHES[Math.floor(Math.random()*SPLASHES.length)];
    box.innerHTML=`<div class="java-logo">BLOCKCRAFT<span class="java-splash">${splash}</span><span class="logo-sub">JAVA-STYLE EDITION</span></div>
      <div class="java-main-buttons"><button id="a-single">Singleplayer</button><button id="a9">Multiplayer</button><div class="java-row"><button id="a8">Options</button><button id="a-logout">Log Out</button></div><button id="a-quit">Quit Game</button></div>
      <div class="java-footer">Blockcraft 1.0 · Java-style Edition</div><div class="java-footer-right">Not affiliated with Mojang or Microsoft</div>`;
    const on=(id,f)=>{const e=box.querySelector("#"+id);if(e)e.onclick=f};
    on("a-single",()=>{scr="worlds";ov()});on("a9",()=>mpUI());on("a8",()=>settingsUI());on("a-logout",()=>logoutUser());on("a-quit",()=>{mode=null;locked=false;try{window.open("","_self");window.close()}catch(e){}setTimeout(()=>{document.body.innerHTML=`<div style="height:100vh;display:flex;align-items:center;justify-content:center;background:#111;color:#fff;font:16px monospace;text-align:center"><div>Blockcraft has quit.<br><small>Your browser may prevent a webpage from closing its own tab. You can close this tab normally.</small></div></div>`},80)});return;
  }

  msg.classList.add("world-screen");
  box.className="";
  if(!mode&&scr=="worlds")return worldSelectUI();
  if(!mode&&scr=="create")return createWorldUI();

  let h="";
  h+=`<h1 class="mc-screen-title">Game Menu</h1><button id="a4">Back to Game</button><button id="a9">Multiplayer</button><button id="adim">Dimensions</button><button id="a8">Options</button><button id="a5">Save &amp; Quit to Title</button>`;
  h+="<small>WASD move · Space jump · Ctrl sprint · Shift sneak · E inventory · T chat/commands<br>LMB mine/attack · RMB use/place · MMB pick block · F offhand · Q drop · F5 perspective · F3 debug</small>";
  box.innerHTML=h;const on=(id,f)=>{const e=box.querySelector("#"+id);if(e)e.onclick=f};on("a4",lock);on("a5",()=>{mpLeave();save();location.reload()});on("a8",()=>settingsUI());on("adim",()=>dimensionUI());on("a9",()=>mpUI());
}

let selectedWorld=0;
function escHtml(v){return String(v).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function worldSelectUI(){
  msg.classList.add("world-screen");box.className="";
  const list=worlds(); if(selectedWorld>=list.length)selectedWorld=Math.max(0,list.length-1);
  let rows=list.map((n,i)=>{
    let s=null;try{s=JSON.parse(store.get("bcw:"+currentUser+":"+n))}catch(e){}
    const modeName=s?.mode=="creative"?"Creative":s?.mode=="hardcore"?"Hardcore":"Survival";const seedName=s?.seed!=null?"Seed: "+s.seed:"";
    return `<div class="world-row ${i==selectedWorld?'selected':''}" data-world="${i}"><div class="world-thumb"></div><div class="world-info"><div class="world-name"><span class="world-select-dot"></span>${escHtml(n)}</div><div class="world-meta">${modeName} Mode · Blockcraft Java-style ${seedName}</div></div><div class="world-buttons"><button data-play="${i}">Play</button><button data-edit="${i}">Edit</button></div></div>`;
  }).join("");
  if(!rows)rows=`<div class="mc-description" style="padding:30px;text-align:center">No worlds yet.<br>Create a new world to begin your adventure.</div>`;
  box.innerHTML=`<div class="mc-screen-title">Select World</div><div class="world-list">${rows}</div><div class="mc-world-actions"><button id="wc-create">Create New World</button><button id="wc-recreate">Re-Create</button><button id="wc-delete">Delete</button><button id="wc-back">Cancel</button></div><div class="mc-small">Select a world, then choose Play to enter it.</div>`;
  box.querySelectorAll(".world-row").forEach(r=>r.onclick=e=>{if(e.target.closest("button"))return;selectedWorld=+r.dataset.world;worldSelectUI()});
  box.querySelectorAll("[data-play]").forEach(b=>b.onclick=e=>{const i=+b.dataset.play;const n=list[i];try{const s=JSON.parse(store.get("bcw:"+currentUser+":"+n));start(s.mode,n,s.seed,s)}catch(err){alert("This world could not be loaded.")}});
  box.querySelectorAll("[data-edit]").forEach(b=>b.onclick=e=>editWorldUI(+b.dataset.edit));
  box.querySelector("#wc-create").onclick=()=>{pm="survival";scr="create";createTab="game";ov()};
  box.querySelector("#wc-recreate").onclick=()=>{if(selectedWorld<0||!list[selectedWorld])return;let s;try{s=JSON.parse(store.get("bcw:"+currentUser+":"+list[selectedWorld]))}catch(e){}if(s){pm=s.mode||"survival";recreateSeed=s.seed;recreateName=list[selectedWorld]+" (Re-Created)";scr="create";createTab="game";ov()}};
  box.querySelector("#wc-delete").onclick=()=>{if(selectedWorld<0||!list[selectedWorld])return;const n=list[selectedWorld];if(confirm(`Delete world "${n}"? This cannot be undone.`)){store.del("bcw:"+currentUser+":"+n);const l=worlds().filter(x=>x!==n);store.set("bcwl:"+currentUser,JSON.stringify(l));selectedWorld=Math.min(selectedWorld,l.length-1);worldSelectUI()}};
  box.querySelector("#wc-back").onclick=()=>{scr="main";ov()};
}

function editWorldUI(i){
  const list=worlds(), oldName=list[i];
  if(!oldName)return;
  let s=null;try{s=JSON.parse(store.get("bcw:"+currentUser+":"+oldName))}catch(e){}
  msg.classList.add("world-screen");box.className="";
  box.innerHTML=`<div class="mc-screen-title">Edit World</div><div class="mc-page"><div class="mc-field"><label>World Name</label><input id="editname" value="${escHtml(oldName)}" maxlength="24"></div><div class="mc-description">${s?.mode=="creative"?"Creative":s?.mode=="hardcore"?"Hardcore":"Survival"} Mode · Seed: ${s?.seed??"unknown"}</div></div><div class="mc-world-actions"><button id="edplay">Play</button><button id="edrename">Save</button><button id="edback">Cancel</button></div>`;
  box.querySelector("#edplay").onclick=()=>{if(s)start(s.mode,oldName,s.seed,s)};
  box.querySelector("#edrename").onclick=()=>{const nn=box.querySelector("#editname").value.trim()||oldName;if(nn!==oldName){try{const data=JSON.parse(store.get("bcw:"+currentUser+":"+oldName));store.set("bcw:"+currentUser+":"+nn,JSON.stringify(data));store.del("bcw:"+currentUser+":"+oldName);const l=worlds().map(x=>x===oldName?nn:x);store.set("bcwl:"+currentUser,JSON.stringify(l))}catch(e){}}selectedWorld=Math.max(0,worlds().indexOf(nn));scr="worlds";ov()};
  box.querySelector("#edback").onclick=()=>{scr="worlds";ov()};
}

let createTab="game",recreateSeed=null,recreateName=null;
function createWorldUI(){
  msg.classList.add("world-screen");box.className="";
  const seedDefault=recreateSeed!=null?recreateSeed:"";
  const nameDefault=recreateName||"New World";
  const tab=(id,label)=>`<button class="mc-tab ${createTab==id?'active':''}" data-tab="${id}">${label}</button>`;
  let page="";
  if(createTab=="game")page=`<div class="mc-page"><div class="mc-field"><label>World Name</label><input id="wn" value="${escHtml(nameDefault)}" maxlength="24"></div><div class="mc-field"><label>Game Mode</label><select id="wg"><option value="survival" ${pm=="survival"?'selected':''}>Survival</option><option value="creative" ${pm=="creative"?'selected':''}>Creative</option><option value="hardcore" ${pm=="hardcore"?'selected':''}>Hardcore</option></select></div><div class="mc-field"><label>Difficulty</label><select id="wd"><option>Easy</option><option selected>Normal</option><option>Hard</option><option>Peaceful</option></select></div><div class="mc-toggle"><span>Allow Cheats</span><button id="cheat">OFF</button></div><p class="mc-description">Survival: gather resources, craft, fight mobs, manage health and hunger.<br>Creative: unlimited blocks and flight.<br>Hardcore: Survival rules with one life and locked difficulty.</p></div>`;
  if(createTab=="world")page=`<div class="mc-page"><div class="mc-field"><label>World Seed</label><input id="sd" value="${escHtml(seedDefault)}" maxlength="24" placeholder="Leave blank for a random seed"></div><div class="mc-field"><label>World Type</label><select id="wt"><option selected>Default</option><option>Flat</option><option>Large Biomes</option><option>Amplified</option></select></div><div class="mc-toggle"><span>Generate Structures</span><button id="structures">ON</button></div><div class="mc-toggle"><span>Bonus Chest</span><button id="bonus">OFF</button></div><p class="mc-description">The seed controls the generated terrain. World types are presented in the Java-style interface; the current Blockcraft generator remains responsible for actual terrain generation.</p></div>`;
  if(createTab=="more")page=`<div class="mc-page"><div class="mc-section">Game Rules</div><div class="mc-toggle"><span>Daylight Cycle</span><button id="daycycle">ON</button></div><div class="mc-toggle"><span>Mob Spawning</span><button id="mobs">ON</button></div><div class="mc-toggle"><span>Keep Inventory</span><button id="keep">OFF</button></div><div class="mc-toggle"><span>Fire Spreads</span><button id="fire">ON</button></div><div class="mc-section">Data Packs</div><p class="mc-description">No additional data packs are installed. This page mirrors the Java Edition world-creation organization while keeping Blockcraft self-contained.</p></div>`;
  box.innerHTML=`<div class="mc-screen-title">Create New World</div><div class="mc-tabs">${tab("game","Game")}${tab("world","World")}${tab("more","More")}</div>${page}<div class="mc-world-actions"><button id="create">Create New World</button><button id="back">Cancel</button></div>`;
  box.querySelectorAll("[data-tab]").forEach(b=>b.onclick=()=>{createTab=b.dataset.tab;ov()});
  const toggle=(id,yes="ON",no="OFF")=>{const b=box.querySelector("#"+id);if(b)b.onclick=()=>b.textContent=b.textContent==yes?no:yes};toggle("cheat");toggle("structures");toggle("bonus");toggle("daycycle");toggle("mobs");toggle("keep");toggle("fire");
  box.querySelector("#back").onclick=()=>{scr="worlds";recreateSeed=null;recreateName=null;ov()};
  box.querySelector("#create").onclick=()=>{const n=box.querySelector("#wn")?.value.trim()||"New World";const t=box.querySelector("#sd")?.value.trim()||"";const sd=t?(/^-?\d+$/.test(t)?+t:[...t].reduce((a,c)=>(a*31+c.charCodeAt(0))|0,7)):Math.floor(Math.random()*1e6);pm=box.querySelector("#wg")?.value||pm;start(pm,n,Math.abs(sd)%100000,null);recreateSeed=null;recreateName=null};
}

document.addEventListener("pointerlockchange",()=>{locked=document.pointerLockElement===renderer.domElement;if(locked){invOpen=false;msg.style.display="none"}else if(mode){mL=mR=0;ov()}});
/* Multiplayer UI */
function dimensionUI(){
  msg.style.display="flex";box.className="";
  box.innerHTML=`<h1>Dimensions</h1><p>Current dimension: <b>${dimension}</b><br>Biome: ${escHtml(biomeName)}</p><button id=dover>Overworld</button><button id=dnether>Nether</button><button id=dend>The End</button><button id=dback>Back</button><small>Dimensions are separate generated worlds. Your inventory carries between them.</small>`;
  box.querySelector("#dover").onclick=()=>{switchDimension("overworld");ov()};box.querySelector("#dnether").onclick=()=>{switchDimension("nether");ov()};box.querySelector("#dend").onclick=()=>{switchDimension("end");ov()};box.querySelector("#dback").onclick=()=>ov();
}

function mpUI(){
  msg.style.display="flex";box.className="";
  const status=MP.connected?"<span style='color:#7f7'>Connected</span>":"<span style='color:#f77'>Not connected</span>";
  box.innerHTML=`<h1>Multiplayer</h1>
    <p>${status}</p>
    ${MP.connected?"":`<input id=mproom placeholder="Room name" value="${MP.room}" maxlength=20><input id=mpname placeholder="Your name" value="${MP.name}" maxlength=16><button id=mpjoin>Join Game</button>`}
    ${MP.connected?`<button id=mpleave>Leave Game</button>`:""}
    <button id=mpback>Back</button>
    <small>Multiplayer uses HTTP polling — blocks and player positions sync every ~200ms.<br>Share the same room name to play together. The server must be running.</small>`;
  const on=(id,f)=>{const e=box.querySelector("#"+id);if(e)e.onclick=f};
  on("mpjoin",async()=>{const room=box.querySelector("#mproom").value.trim()||"default";const name=box.querySelector("#mpname").value.trim()||"Player";const ok=await mpConnect(room,name);if(ok)ov();else mpUI()});
  on("mpleave",()=>{mpLeave();mpUI()});
  on("mpback",()=>ov());
}

/* ====================== 12. VILLAGES ======================= */
function tryGenVillage(cx,cz,a,X0,Z0,I){
  // Chance based on world hash
  const villageHash=hash(cx*13+seed,cz*17+seed);
  if(villageHash>CFG.world.villageChance)return;
  // Check terrain is reasonably flat
  const cxw=X0+8,czw=Z0+8;
  const baseH=hAt(cxw,czw);
  if(baseH<=SEA+1||baseH>=H-8)return;
  // Build a small village: 3-5 houses around a center path
  const numHouses=3+Math.floor(villageHash*3);
  const placed=new Set();
  for(let h=0;h<numHouses;h++){
    const ang=h/numHouses*6.28+hash(cx*31+h,cz*71)*1.5;
    const dist=6+hash(cx*7+h*3,cz*5+h*9)*8;
    const hx=Math.floor(cxw+Math.cos(ang)*dist),hz=Math.floor(czw+Math.sin(ang)*dist);
    const hy=hAt(hx,hz);
    if(hy<=SEA+1||hy>=H-6)continue;
    const key=hx+","+hz+":"+h;
    if(placed.has(key))continue;placed.add(key);
    genHouse(a,hx,hy,hz,X0,Z0,I);
  }
  // Central path
  genPath(a,cxw,czw,baseH,X0,Z0,I);
}
function genHouse(a,hx,hy,hz,X0,Z0,I){
  const w=4+Math.floor(hash(hx*3,hz*5)*3),d=4+Math.floor(hash(hx*7,hz*9)*3);
  const put=(x,y,z,v)=>{const lx=x-X0,lz=z-Z0;if(lx>=0&&lx<16&&lz>=0&&lz<16&&y>=0&&y<H)a[I(lx,y,lz)]=v};
  // Foundation
  for(let dx=0;dx<=w;dx++)for(let dz=0;dz<=d;dz++)put(hx-w+dx,hy,hz-d+dz,17); // stone brick floor
  // Walls
  for(let dx=0;dx<=w;dx++)for(let dz=0;dz<=d;dz++){
    const isEdge=dx==0||dz==0||dx==w||dz==d;
    if(!isEdge)continue;
    for(let dy=1;dy<=3;dy++)put(hx-w+dx,hy+dy,hz-d+dz,6); // plank walls
  }
  // Door gap (front wall)
  const doorX=hx+Math.floor(w/2)-1;
  put(doorX,hy+1,hz,0);put(doorX+1,hy+1,hz,0);put(doorX,hy+2,hz,0);put(doorX+1,hy+2,hz,0);
  // Windows (glass)
  put(hx,hy+1,hz-d,10);put(hx+w,hy+1,hz-d,10);put(hx,hy+1,hz,10);put(hx+w,hy+1,hz,10);
  // Roof
  for(let dx=0;dx<=w;dx++)for(let dz=0;dz<=d;dz++)put(hx-w+dx,hy+4,hz-d+dz,17);
  // Interior: bed-ish (redstone lamp for light) + bookshelf
  put(hx-1,hy+1,hz-1,14); // lamp
  put(hx+1,hy+1,hz-1,16); // bookshelf
  // Clear interior
  for(let dx=1;dx<w;dx++)for(let dz=1;dz<d;dz++)for(let dy=1;dy<=3;dy++){
    const lx=hx-w+dx-X0,lz=hz-d+dz-Z0;
    if(lx>=0&&lx<16&&lz>=0&&lz<16&&a[I(lx,hy+dy,lz)]==6)a[I(lx,hy+dy,lz)]=0;
  }
}
function genPath(a,cxw,czw,baseH,X0,Z0,I){
  const put=(x,y,z,v)=>{const lx=x-X0,lz=z-Z0;if(lx>=0&&lx<16&&lz>=0&&lz<16&&y>=0&&y<H)a[I(lx,y,lz)]=v};
  for(let dx=-6;dx<=6;dx++)for(let dz=-6;dz<=6;dz++){
    if(Math.abs(dx)>4&&Math.abs(dz)>4)continue;
    const x=cxw+dx,z=czw+dz,h=hAt(x,z);
    if(h==baseH)put(x,h,z,19); // gravel path
  }
}

/* ====================== 12b. MULTIPLAYER ====================== */
const MP={
  api:"port/3000", // rewritten by deploy to proxy to server
  pid:null,room:"default",connected:false,players:{},remoteMeshes:{},
  lastSend:0,lastPoll:0,lastEid:0,name:"Player"
};
const mpStatus=document.getElementById("mpstatus");

async function mpConnect(room,name){
  MP.room=room||"default";MP.name=name||"Player";
  try{
    const r=await fetch(MP.api+"/api/join",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({x:p.x,y:p.y,z:p.z,name:MP.name,room:MP.room})});
    const d=await r.json();
    MP.pid=d.pid;MP.connected=true;MP.lastEid=0;
    mpStatus.style.display="block";mpStatus.textContent="MP: connected";
    // Store events from join
    if(d.events)for(const e of d.events)if(e.eid>MP.lastEid)MP.lastEid=e.eid;
    actionBar("Multiplayer connected!");
    return true;
  }catch(e){
    mpStatus.style.display="block";mpStatus.textContent="MP: offline";
    actionBar("Multiplayer unavailable");
    return false;
  }
}
async function mpSendState(){
  if(!MP.connected)return;
  if(Date.now()-MP.lastSend<120)return;
  MP.lastSend=Date.now();
  try{
    await fetch(MP.api+"/api/state",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({pid:MP.pid,x:p.x,y:p.y,z:p.z,yaw:p.yaw,pitch:p.pitch,item:cur>=0?HOT[cur]:0,name:MP.name,room:MP.room})});
  }catch(e){MP.connected=false;mpStatus.textContent="MP: disconnected"}
}
async function mpPoll(){
  if(!MP.connected)return;
  if(Date.now()-MP.lastPoll<180)return;
  MP.lastPoll=Date.now();
  try{
    const r=await fetch(MP.api+"/api/sync?room="+MP.room+"&pid="+MP.pid+"&since="+MP.lastEid);
    const d=await r.json();
    MP.players=d.players||{};
    // Apply remote block events
    if(d.events)for(const e of d.events){
      if(e.eid<=MP.lastEid)continue;
      MP.lastEid=e.eid;
      if(e.type=="block"&&e.x!=null){
        pset(e.x,e.y,e.z,e.v);edit(e.x,e.y,e.z);
      }
    }
  }catch(e){MP.connected=false;mpStatus.textContent="MP: disconnected"}
}
async function mpSendBlock(x,y,z,v){
  if(!MP.connected)return;
  try{
    await fetch(MP.api+"/api/block",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({pid:MP.pid,type:"block",x,y,z,v,room:MP.room})});
  }catch(e){}
}
function mpLeave(){
  if(!MP.connected)return;
  fetch(MP.api+"/api/leave",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({pid:MP.pid,room:MP.room})}).catch(()=>{});
  MP.connected=false;MP.pid=null;
  mpStatus.style.display="none";
  // Clean up remote meshes
  for(const pid in MP.remoteMeshes){scene.remove(MP.remoteMeshes[pid]);delete MP.remoteMeshes[pid]}
  MP.players={};
}
function mpRenderRemotePlayers(){
  if(!MP.connected)return;
  // Remove stale remote meshes
  for(const pid in MP.remoteMeshes){
    if(!MP.players[pid]){scene.remove(MP.remoteMeshes[pid]);delete MP.remoteMeshes[pid]}
  }
  // Update/create remote player meshes
  for(const pid in MP.players){
    const rp=MP.players[pid];
    let m=MP.remoteMeshes[pid];
    if(!m){
      // Create a simple blocky avatar (like a mob)
      m=new THREE.Group();
      const bodyCol=mm(0x4a7ad8);const headCol=mm(0xf0c090);const legCol=mm(0x3a3a3a);
      bx(m,.5,.6,.3,bodyCol,0,.6,0); // body
      bx(m,.45,.45,.45,headCol,0,1.05,0); // head
      bx(m,.5,.5,.3,mm(0x222266),0,1.06,.23); // face
      bx(m,.2,.6,.2,legCol,-.15,0,0); // left leg
      bx(m,.2,.6,.2,legCol,.15,0,0); // right leg
      bx(m,.2,.6,.2,mm(0x4a7ad8),-.35,.65,0); // left arm
      bx(m,.2,.6,.2,mm(0x4a7ad8),.35,.65,0); // right arm
      scene.add(m);
      MP.remoteMeshes[pid]=m;
    }
    m.position.set(rp.x,rp.y,rp.z);
    m.rotation.y=rp.yaw||0;
    // Show name tag (simple debug text)
  }
}
function mpTick(dt){
  if(!MP.connected)return;
  mpSendState();mpPoll();mpRenderRemotePlayers();
}

/* ====================== 13. MAIN LOOP ======================= */
function hurt(n){
  if(!isSurvivalMode())return;
  if(shielding&&off==44){n=Math.max(1,Math.floor(n*.3));actionBar("Blocked!")}
  const reduction=Math.min(.8,armorValue()*.04);n=Math.max(0.5,n*(1-reduction));hp-=n;playSfx("hurt");
  if(hp<=0){
    // Java-style death: drop the entire survival inventory around the death point.
    const deathX=p.x, deathY=p.y, deathZ=p.z;
    if(isSurvivalMode()){
      const stacks=[];
      for(const k of Object.keys(inv)){
        const id=+k, count=Math.max(0,Math.floor(inv[id]||0));
        if(id&&count>0)stacks.push([id,count]);
      }
      // Each inventory item is represented by its own dropped entity. Spread
      // the drops randomly within a 3-block radius of the death location.
      for(const [id,count] of stacks){
        for(let n=0;n<count;n++){
          const a=Math.random()*Math.PI*2;
          const r=Math.sqrt(Math.random())*3;
          const dx=Math.cos(a)*r, dz=Math.sin(a)*r;
          spawnDrop(id,deathX+dx,deathY+.55,deathZ+dz,dx*.9,dz*.9);
        }
      }
      for(const k in inv)delete inv[k];
      HOT.fill(0);off=0;ARMOR.fill(0);
    }
    if(mode==="hardcore"){
      const list=worlds().filter(x=>x!==wname);store.set("bcwl:"+currentUser,JSON.stringify(list));store.del("bcw:"+currentUser+":"+wname);mode=null;locked=false;document.exitPointerLock?.();actionBar("You died. Hardcore world deleted.");playSfx("death");scr="worlds";setTimeout(()=>ov(),350);return;
    }
    hp=CFG.survival.maxHp;food=CFG.survival.maxFood;xp=0;xpLvl=0;
    p.x=home.x;p.y=home.y;p.z=home.z;p.vy=0;
    updHot();actionBar("You died! Your items were scattered nearby.");playSfx("death");
  }
}
function attack(){
  const strength=attackStrength();
  if(strength<.92||shielding)return;
  atkCd=attackCooldown(); swing=1;
  const d=new THREE.Vector3();cam.getWorldDirection(d);
  let best=null,bestT=Infinity;
  for(const m of mobs){
    const vx=m.x-cam.position.x,vy=m.y+m.Hh*.55-cam.position.y,vz=m.z-cam.position.z,t=vx*d.x+vy*d.y+vz*d.z;
    if(t<0||t>CFG.player.reach)continue;
    const side=Math.hypot(vx-d.x*t,vy-d.y*t,vz-d.z*t);
    if(side<=.65&&t<bestT&&(!m.hurtCd||m.hurtCd<=0)){best=m;bestT=t}
  }
  if(!best)return;
  const h=heldItem();playSfx("attack",h>=49&&h<=53?1.15:1);
  // Java-style attacks are strongest at a full meter; early clicks are refused rather than doing weak damage.
  let dmg=attackDamage();
  const critical=!p.ground&&p.vy<0&&!sneakAttackState;
  if(critical)dmg*=1.5;
  best.hp-=dmg;best.hurtCd=.5;best.hitFlash=.16;
  const kb=sprintAttackState?8:4;
  best.kx=d.x*kb;best.kz=d.z*kb;best.vy=sprintAttackState?4.0:2.2;
  if(sprintAttackState)spr=false;
  if(critical){actionBar("Critical!");playSfx("crit")}
  // Sword sweep: a charged grounded sword hit also knocks nearby mobs back for reduced damage.
  if(strength>=.99&&h>=49&&h<=53&&p.ground){
    for(const m of mobs){
      if(m===best||m.hurtCd>0)continue;
      const dd=Math.hypot(m.x-best.x,m.z-best.z);
      if(dd<1.6){m.hp-=dmg*.25;m.hurtCd=.35;m.hitFlash=.16;m.kx=d.x*3;m.kz=d.z*3;m.vy=1.2}
    }
  }
  if(best.hp<=0){
    if(isSurvivalMode()){if(CFG.mobs[best.t].drop){const dr=CFG.mobs[best.t].drop;Array.from({length:dr.min+Math.floor(Math.random()*(dr.max-dr.min+1))},()=>spawnDrop(dr.id,best.x,best.y+.6,best.z))}addXp(best.t=="zombie"?5:3);if(best.t=="zombie"&&Math.random()<.5)spawnDrop(45,best.x,best.y+.6,best.z);updHot()}
    scene.remove(best.g);mobs.splice(mobs.indexOf(best),1);
  }
}


function hpTick(dt){
  if(isSurvivalMode()){
    regen+=dt;hT+=dt*((keys.ControlLeft||spr)?2:1);
    if(hT>CFG.survival.hungerSeconds){hT=0;if(food>0)food--}
    if(regen>CFG.survival.regenSeconds){regen=0;if(food>=CFG.survival.regenMinFood&&hp<CFG.survival.maxHp)hp++;else if(food<=0)hurt(1)}
    renderArmor();renderHearts();renderHunger();renderXpBar();
  }
  wt+=dt;if(wt>.25){wt=0;waterTick();redstoneTick()}
  sv+=dt;if(sv>CFG.save.interval){sv=0;save()}
  atkCd=Math.max(0,atkCd-dt);
  renderXpBar();
}
/* Heart/hunger rendering */
const HEART_CV=document.createElement("canvas");HEART_CV.width=HEART_CV.height=9;
function drawHeart(canvas,full,half){canvas.width=canvas.height=9;const ctx=canvas.getContext("2d");ctx.clearRect(0,0,9,9);ctx.imageSmoothingEnabled=false;
  const P=(x,y,c)=>{ctx.fillStyle=c;ctx.fillRect(x,y,1,1)};
  const bg=full?"#e00":"#600";const fg=full?"#f55":"#933";
  P(1,2,bg);P(2,1,bg);P(3,1,bg);P(4,2,bg);P(5,1,bg);P(6,1,bg);P(7,2,bg);
  P(0,3,bg);P(1,3,fg);P(2,3,fg);P(3,3,fg);P(4,3,fg);P(5,3,fg);P(6,3,fg);P(7,3,bg);P(8,3,bg);
  P(0,4,bg);P(1,4,fg);P(2,4,"#fff");P(3,4,fg);P(4,4,fg);P(5,4,fg);P(6,4,fg);P(7,4,bg);P(8,4,bg);
  P(0,5,bg);P(1,5,fg);P(2,5,fg);P(3,5,fg);P(4,5,fg);P(5,5,fg);P(6,5,bg);P(7,5,bg);P(8,5,bg);
  P(1,6,bg);P(2,6,bg);P(3,6,bg);P(4,6,bg);P(5,6,bg);P(6,6,bg);P(7,6,bg);
  if(half){ctx.fillStyle="#600";ctx.fillRect(5,1,3,6)}
  return canvas.toDataURL();
}
const HEART_FULL=drawHeart(document.createElement("canvas"),true,false);
const HEART_HALF=drawHeart(document.createElement("canvas"),false,true);
const HEART_EMPTY=drawHeart(document.createElement("canvas"),false,false);
function renderArmor(){const el=document.getElementById("armor");if(!el)return;el.innerHTML="";if(!isSurvivalMode())return;const points=armorValue();for(let i=0;i<10;i++){const s=document.createElement("span");s.textContent="◆";s.style.font="9px monospace";s.style.color="#cfd6df";s.style.textShadow="1px 1px #222";s.style.opacity=i<Math.ceil(points/2)?"1":".25";el.appendChild(s)}}
function renderHearts(){const el=document.getElementById("hearts");el.innerHTML="";const full=Math.floor(hp/2),half=hp%2;for(let i=0;i<10;i++){const img=document.createElement("img");img.style.width="9px";img.style.height="9px";img.style.imageRendering="pixelated";if(i<full)img.src=HEART_FULL;else if(i==full&&half)img.src=HEART_HALF;else img.src=HEART_EMPTY;el.appendChild(img)}}
const HUNGER_CV=document.createElement("canvas");HUNGER_CV.width=HUNGER_CV.height=9;
function drawHunger(canvas,full){canvas.width=canvas.height=9;const ctx=canvas.getContext("2d");ctx.clearRect(0,0,9,9);ctx.imageSmoothingEnabled=false;
  const P=(x,y,c)=>{ctx.fillStyle=c;ctx.fillRect(x,y,1,1)};
  const col=full?"#8a5a2a":"#3a2a10";const bone=full?"#d8a050":"#5a4020";
  P(1,3,col);P(2,2,col);P(3,2,col);P(4,3,col);P(5,3,col);P(6,2,col);P(7,2,col);
  P(0,4,col);P(1,4,bone);P(2,4,bone);P(3,4,bone);P(4,4,bone);P(5,4,bone);P(6,4,bone);P(7,4,col);P(8,4,col);
  P(1,5,col);P(2,5,bone);P(3,5,bone);P(4,5,bone);P(5,5,bone);P(6,5,col);
  P(2,6,col);P(3,6,col);P(4,6,col);P(5,6,col);
  return canvas.toDataURL();
}
const HUNGER_FULL=drawHunger(document.createElement("canvas"),true);
const HUNGER_EMPTY=drawHunger(document.createElement("canvas"),false);
function renderHunger(){const el=document.getElementById("hunger");el.innerHTML="";const shanks=Math.ceil(food/2);for(let i=0;i<10;i++){const img=document.createElement("img");img.style.width="9px";img.style.height="9px";img.style.imageRendering="pixelated";img.src=i<shanks?HUNGER_FULL:HUNGER_EMPTY;el.appendChild(img)}}
function xpForLevel(l){return l*l+6*l}
function renderXpBar(){const bar=document.getElementById("xpbar"),fill=document.getElementById("xpfill"),lvl=document.getElementById("xplvl");if(!isSurvivalMode()){bar.style.display="none";lvl.style.display="none";return}bar.style.display="block";const need=xpForLevel(xpLvl+1)-xpForLevel(xpLvl);const pct=Math.min(1,xp/need);fill.style.width=(pct*100)+"%";if(xpLvl>0){lvl.style.display="block";lvl.textContent=xpLvl}else lvl.style.display="none"}
function addXp(n){if(!isSurvivalMode())return;xp+=n;let need=xpForLevel(xpLvl+1)-xpForLevel(xpLvl);while(xp>=need){xp-=need;xpLvl++;need=xpForLevel(xpLvl+1)-xpForLevel(xpLvl);actionBar("Level Up! Now level "+xpLvl);playSfx("level")}}
let abTimer=0;
function actionBar(text){const el=document.getElementById("actionbar");el.textContent=text;el.style.display="block";abTimer=3}
// Restore session if available
try{const _savedUser=store.get("bcsession");if(_savedUser){const _users=getUsers();if(_users[_savedUser])currentUser=_savedUser}}catch(e){}
try{ov()}catch(e){console.error('ov error:',e)}
document.addEventListener("mousemove",e=>{if(!locked)return;p.yaw-=e.movementX*CFG.player.sensitivity;p.pitch=Math.max(-1.55,Math.min(1.55,p.pitch-e.movementY*CFG.player.sensitivity))});
addEventListener("wheel",e=>{if(locked)pick(cur+(e.deltaY>0?1:-1))});
addEventListener("contextmenu",e=>e.preventDefault());
// Service worker for PWA
try{if("serviceWorker" in navigator)navigator.serviceWorker.register("sw.js").catch(()=>{})}catch(e){}
addEventListener("mousedown",e=>{if(!locked)return;if(e.button==0){mL=1;attack()}if(e.button==2){mR=1;if(off==44)shielding=true}});
addEventListener("mouseup",e=>{if(e.button==0)mL=0;if(e.button==2){mR=0;shielding=false}});
function ray(){
  const d=new THREE.Vector3();cam.getWorldDirection(d);let prev=null,lastCell="";
  for(let t=0;t<=CFG.player.reach;t+=.025){
    const x=Math.floor(cam.position.x+d.x*t),y=Math.floor(cam.position.y+d.y*t),z=Math.floor(cam.position.z+d.z*t);
    const key=x+","+y+","+z;
    if(key===lastCell)continue; lastCell=key;
    const b=get(x,y,z);
    if(b>0)return{c:[x,y,z],prev};
    prev=[x,y,z];
  }
  return null;
}
const lerp=(a,b,t)=>a+(b-a)*t,NIGHT=[8,12,34],DAY=[127,184,230],DUSK=[235,130,80];
const dbg=document.getElementById("dbg"),pw=document.getElementById("pw"),pb=document.getElementById("pb"),uw=document.getElementById("uw");
let last=performance.now(),tod=CFG.time.start,mp=0,mkey="",cd=0,swing=0,fov=72,fa=0,fc=0;
function frame(now){
  const dt=Math.min((now-last)/1000,.05);last=now;fc++;fa+=dt;
  if(!mode){updChunks();cam.position.set(8,40,8);cam.rotation.set(-.3,now/9000,0);hand.visible=hand2.visible=arm.visible=arm2.visible=false;sel.visible=false;document.getElementById("status").style.display="none";document.getElementById("bar").style.display="none";document.getElementById("off").style.display="none";renderer.render(scene,cam);requestAnimationFrame(frame);return}
  tod=(tod+dt/CFG.time.dayLength)%1;
  const f=(keys.KeyW?1:0)-(keys.KeyS?1:0),s=(keys.KeyD?1:0)-(keys.KeyA?1:0),sneak=keys.ShiftLeft||keys.ShiftRight;
  const sprint=(keys.ControlLeft||spr)&&f>0&&!sneak&&!shielding; sprintAttackState=sprint; sneakAttackState=sneak;
  const wet=isW(get(Math.floor(p.x),Math.floor(p.y+.5),Math.floor(p.z)));
  const P=CFG.player;
  let sp=fly?P.fly:sneak?P.sneak:sprint?P.sprint:P.walk;
  if(wet&&!fly)sp*=P.swimFactor;
  const sn=Math.sin(p.yaw),cs=Math.cos(p.yaw),mx=-sn*f+cs*s,mz=-cs*f-sn*s,l=Math.hypot(mx,mz)||1;
  p.vx=mx/l*sp;p.vz=mz/l*sp;
  if(fly)p.vy=((keys.Space?1:0)-(sneak?1:0))*9;
  else if(wet){p.vy=Math.max(p.vy-9*dt,-3);if(keys.Space)p.vy=Math.min(p.vy+30*dt,3.6)}
  else{p.vy-=P.gravity*dt;if(keys.Space&&p.ground){p.vy=P.jump;p.ground=false}}
  const nx=p.x+p.vx*dt;if(!hit(nx,p.y,p.z))p.x=nx;
  const nz=p.z+p.vz*dt;if(!hit(p.x,p.y,nz))p.z=nz;
  const ny=p.y+p.vy*dt;
  if(hit(p.x,ny,p.z)){if(p.vy<0){p.ground=true;if(!wet&&!fly&&p.vy<-CFG.survival.fallSpeed)hurt(Math.ceil((-p.vy-CFG.survival.fallSpeed+1)/2))}p.vy=0}else{p.y=ny;p.ground=false}
  if(p.y<-20){p.y=H;p.vy=0}
  const eyeY=p.y+(sneak?1.45:1.62);
  if(thirdPerson){
    const back=new THREE.Vector3(0,0,3.2).applyAxisAngle(new THREE.Vector3(0,1,0),p.yaw);
    const tp=new THREE.Vector3(p.x,p.y+1.35,p.z).add(back);
    cam.position.lerp(tp,.35);cam.lookAt(p.x,p.y+1.2,p.z);
    playerAvatar.visible=true;playerAvatar.position.set(p.x,p.y,p.z);playerAvatar.rotation.y=p.yaw;
    const held3=cur>=0?HOT[cur]:0;
    if(held3&&stocked(held3)){
      if(isBlockId(held3)){avatarHeld.geometry=handGeo(held3);avatarHeld.material=hmat;avatarHeld.scale.setScalar(.22);avatarHeld.position.set(.43,1.00,-.28);avatarHeld.rotation.set(0,0,0)}
      else{avatarHeld.geometry=handGeoItem(held3);avatarHeld.material=itemMat(held3);avatarHeld.scale.setScalar(.9);avatarHeld.position.set(.43,.98,-.28);avatarHeld.rotation.set(0,Math.PI,0)}
      avatarHeld.visible=true;
    }else avatarHeld.visible=false;
    const walkAmp=Math.min(0.55,Math.hypot(p.vx,p.vz)*.10),walkT=now*.012;
    if(avatarParts.leftArm)avatarParts.leftArm.rotation.x=Math.sin(walkT)*walkAmp;
    if(avatarParts.rightArm)avatarParts.rightArm.rotation.x=-Math.sin(walkT)*walkAmp;
    if(avatarParts.leftLeg)avatarParts.leftLeg.rotation.x=-Math.sin(walkT)*walkAmp;
    if(avatarParts.rightLeg)avatarParts.rightLeg.rotation.x=Math.sin(walkT)*walkAmp;
  }else{
    cam.position.set(p.x,eyeY,p.z);cam.rotation.set(p.pitch,p.yaw,0);playerAvatar.visible=false;avatarHeld.visible=false;
  }
  refreshHands();
  fov=lerp(fov,sprint?P.sprintFov:P.fov,Math.min(1,dt*8));cam.fov=fov;cam.updateProjectionMatrix();
  const r=locked?ray():null;sel.visible=!!r;
  if(r)sel.position.set(r.c[0]+.5,r.c[1]+.5,r.c[2]+.5);
  cd-=dt;dropCooldown=Math.max(0,dropCooldown-dt);swing=Math.max(0,swing-dt*4.5); const aind=document.getElementById("attack-indicator"),afill=document.getElementById("attack-fill"); if(aind&&afill){const ap=attackStrength();aind.style.display=hudHidden?"none":"block";afill.style.width=(ap*100)+"%";}
  let prog=0;
  if(mL&&attackStrength()>=.92)attack();
  if(r&&mL){
    const k=r.c.join();if(k!=mkey){mkey=k;mp=0}
    const b0=get(...r.c);mp+=dt;const need=mineTime(b0);prog=mp/need;if(swing<=0)swing=1;
    // Lever interaction: right-click already handles placing; levers toggled by left-click on lever block
    if(mp>=need){
      if(b0==15){toggleLever(...r.c);mp=0;prog=0}
      else{pset(...r.c,0);edit(...r.c);drop(b0,r.c[0],r.c[1],r.c[2]);playSfx("break",1+(b0%4)*.04);mpSendBlock(r.c[0],r.c[1],r.c[2],0);mp=0;prog=0}
    }
  }else{mp=0;mkey=""}
  pw.style.display=prog>0?"block":"none";pb.style.width=prog*100+"%";
  if(r&&prog>0){crackMesh.visible=true;crackMesh.position.set(r.c[0]+.5,r.c[1]+.5,r.c[2]+.5);drawCracks(Math.max(1,Math.min(10,Math.ceil(prog*10))));}
  else {crackMesh.visible=false;drawCracks(0);}
  if(r&&mR&&cd<=0&&r.prev&&inb(...r.prev)&&get(...r.prev)<=0||(r&&mR&&cd<=0&&r.prev&&isW(get(...r.prev)))){
    const pv=r.prev,old=get(...pv),bid=heldBlock();
    if(bid&&(mode=="creative"||inv[bid]>0)){
      pset(...pv,bid);
      if(solid(bid)&&hit(p.x,p.y,p.z))pset(...pv,old);else{edit(...pv);if(isSurvivalMode()){takeItem(bid);updHot()}mpSendBlock(pv[0],pv[1],pv[2],bid);playSfx("place",1+(bid%3)*.05)}
    }
    cd=.22;swing=1;
  }
  refreshHands();const sw=Math.sin(swing*Math.PI);
  const hr=hand.userData.bR||[-.10,.30,-.12],hp=hand.userData.bP||[.44,-.36,-.79];
  hand.position.set(hp[0]-swing*.035,hp[1]-sw*.028,hp[2]-swing*.018);hand.rotation.set(hr[0]-sw*.22,hr[1]+sw*.10,hr[2]-sw*.08);
  const hr2=hand2.userData.bR||[0,-.15,0],hp2=hand2.userData.bP||[-.43,-.38,-.80];
  hand2.position.set(hp2[0],hp2[1],hp2[2]);hand2.rotation.set(hr2[0],hr2[1],hr2[2]);
  // Lower-right Java-style arm silhouette. Keep the wrist close to the item instead of
  // projecting a long arm into the center of the screen.
  arm.position.set(.37-swing*.018,-.52-sw*.025,-.78-swing*.025);arm.rotation.set(.22-sw*.34,.12,-.16);
  arm2.position.set(-.37,-.52,-.78);arm2.rotation.set(.22,-.12,.16);
  const ang=tod*Math.PI*2,sh=Math.sin(ang),day=Math.min(1,Math.max(0,(sh+.15)/.4)),dusk=Math.max(0,1-Math.abs(sh)/.22)*.55;
  const sky=[0,1,2].map(i=>lerp(lerp(NIGHT[i],DAY[i],day),DUSK[i],dusk)/255),br=.92+.08*day;
  const head=isW(get(Math.floor(cam.position.x),Math.floor(cam.position.y),Math.floor(cam.position.z)));
  uw.style.display=head?"block":"none";
  const sc=head?[.1*br,.25*br,.55*br]:sky;
  scene.background.setRGB(...sc);scene.fog.color.setRGB(...sc);scene.fog.near=head?.1:CFG.render.fogNear;scene.fog.far=head?CFG.render.underwaterFog:scene.fog.far;
  mat.color.setScalar(1);wmat.color.setScalar(1);cmat.color.setScalar(.75+.25*day);
  const dir=new THREE.Vector3(Math.cos(ang),Math.sin(ang),.25).normalize();
  sunM.position.copy(cam.position).addScaledVector(dir,300);sunM.lookAt(cam.position);
  moonM.position.copy(cam.position).addScaledVector(dir,-300);moonM.lookAt(cam.position);
  clouds.position.set(cam.position.x,H+14,cam.position.z);ctex.offset.x+=dt*.002;
  updChunks();dimensionPortalTick();mobTick(dt,br,day);updateDrops(dt);animateTorches(performance.now()/1000);hpTick(dt);mpTick(dt);
  if(abTimer>0){abTimer-=dt;if(abTimer<=0)document.getElementById("actionbar").style.display="none"}
  renderer.render(scene,cam);
  if(fa>.4){dbg.textContent=`${dimension} · ${biomeName} · XYZ ${p.x.toFixed(1)} ${p.y.toFixed(1)} ${p.z.toFixed(1)}  ${Math.round(fc/fa)} FPS  ${tod<.5?"Day":"Night"}${fly?"  Flying":""}  ${mode}`;fa=0;fc=0}
  requestAnimationFrame(frame);
}
requestAnimationFrame(frame);
</script>
<script data-pplx-inline-edit>
(function () {
  if (window === window.top) return;

  const allowedParentOrigins = ["https://www.perplexity.ai","https://perplexity.ai","https://testing.perplexity.ai","https://staging.perplexity.ai","https://*.preview.i.perplexity.ai","http://perplexity.localhost","http://localhost:1420","http://127.0.0.1:1420","http://localhost:3000","http://127.0.0.1:3000","http://localhost:5173","http://127.0.0.1:5173"];
  const MAX_FONT_BYTES = 500 * 1024;
  const MAX_TOTAL_FONT_BYTES = 2 * 1024 * 1024;
  const DEFAULT_LINE_HEIGHT = 16;
  let scrollForwarding = false;
  let scrollRaf = 0;
  let trustedTopOrigin = null;

  // Allow entries like "https://*.preview.i.perplexity.ai" — the wildcard
  // matches a single DNS label (no dots), so "https://*.foo" cannot stretch
  // across multiple labels.
  function matchesAllowedOrigin(origin) {
    if (!origin) return false;
    for (const entry of allowedParentOrigins) {
      if (!entry.includes("*")) {
        if (entry === origin) return true;
        continue;
      }
      const pattern = new RegExp(
        "^" +
          entry.replace(/[.+?^${}()|[\]\\]/g, "\\$&").replace(/\*/g, "[^.]+") +
          "$",
      );
      if (pattern.test(origin)) return true;
    }
    return false;
  }

  // Trust decision: when the sender is same-origin-visible (event.origin is a
  // real origin like https://www.perplexity.ai) we trust event.origin directly.
  // When event.origin is "null" (opaque broker srcdoc), we fall back to the
  // broker's stamped `parentOrigin` to identify the top window. The fallback
  // is claim-only — we rely on the browser's native `targetOrigin` enforcement
  // on the response path (see postToTrustedTop) to ensure replies can't be
  // delivered to anyone but the actual top window of that claimed origin.
  function getTrustedParentOrigin(event) {
    const forwardedParentOrigin =
      typeof event.data.parentOrigin === "string" ? event.data.parentOrigin : null;
    const parentOrigin = event.origin === "null" ? forwardedParentOrigin : event.origin;
    return matchesAllowedOrigin(parentOrigin) ? parentOrigin : null;
  }

  // All responses go to window.top with targetOrigin = the allowlisted origin.
  // An attacker that iframes us inside their own null-origin broker can claim
  // any parentOrigin they like, but the browser will drop the reply whenever
  // the real top's origin doesn't match — so the screenshot never leaves.
  function postToTrustedTop(message) {
    if (!trustedTopOrigin) return;
    try {
      window.top.postMessage(message, trustedTopOrigin);
    } catch (_error) {}
  }

  function inlineAll(original, clone) {
    if (original.nodeType !== 1 || clone.nodeType !== 1) return;

    try {
      const computedStyle = getComputedStyle(original);
      // cssText on a computed style is the serialized declaration in modern
      // Chromium/Safari — a single read beats enumerating ~400 longhand
      // properties. Firefox returns "" here, so we fall back on empty.
      const serialized = computedStyle.cssText;
      if (serialized) {
        clone.style.cssText = serialized;
      } else {
        const parts = new Array(computedStyle.length);
        for (let index = 0; index < computedStyle.length; index += 1) {
          const property = computedStyle[index];
          parts[index] = `${property}:${computedStyle.getPropertyValue(property)};`;
        }
        clone.style.cssText = parts.join("");
      }
    } catch (_error) {}

    const originalChildren = original.children;
    const clonedChildren = clone.children;
    for (
      let index = 0;
      index < originalChildren.length && index < clonedChildren.length;
      index += 1
    ) {
      inlineAll(originalChildren[index], clonedChildren[index]);
    }
  }

  function extractFontUrl(srcValue) {
    const matches = [
      ...srcValue.matchAll(
        /url\(["']?([^"')]+)["']?\)(?:\s*format\(["']?([^"')]+)["']?\))?/gi,
      ),
    ];
    if (matches.length === 0) return null;
    const woff2 = matches.find((m) => m[2] && m[2].toLowerCase().includes("woff2"));
    if (woff2) return woff2[1];
    const woff = matches.find((m) => m[2] && m[2].toLowerCase().includes("woff"));
    if (woff) return woff[1];
    return matches[0][1];
  }

  // Cache resolved font URL -> data URI across captures. Fonts on a page
  // essentially never change, and a batch run emits multiple captures back to
  // back — without this we'd refetch + re-base64 every time.
  const fontDataUriCache = new Map();
  const SRC_DECLARATION_RE = /src\s*:\s*[^;}]+/i;

  async function fetchAsDataUri(url) {
    if (fontDataUriCache.has(url)) return fontDataUriCache.get(url);
    let dataUri = null;
    try {
      const response = await fetch(url, { mode: "cors", credentials: "omit" });
      if (response.ok) {
        const blob = await response.blob();
        if (blob.size <= MAX_FONT_BYTES) {
          dataUri = await new Promise((resolve) => {
            const reader = new FileReader();
            reader.onloadend = () =>
              resolve(typeof reader.result === "string" ? reader.result : null);
            reader.onerror = () => resolve(null);
            reader.readAsDataURL(blob);
          });
        }
      }
    } catch (_error) {
      dataUri = null;
    }
    fontDataUriCache.set(url, dataUri);
    return dataUri;
  }

  function collectFontFaceRuleTexts() {
    const rules = [];
    for (const sheet of document.styleSheets) {
      let cssRules;
      try {
        cssRules = sheet.cssRules;
      } catch (_error) {
        continue;
      }
      if (!cssRules) continue;
      for (const rule of cssRules) {
        const cssText = rule.cssText || "";
        if (cssText.startsWith("@font-face")) rules.push(cssText);
      }
    }
    return rules;
  }

  async function buildInlinedFontCss() {
    const ruleTexts = collectFontFaceRuleTexts();
    if (ruleTexts.length === 0) return null;

    const resolved = ruleTexts.map((cssText) => {
      if (!SRC_DECLARATION_RE.test(cssText)) return null;
      const srcMatch = cssText.match(/src\s*:\s*([^;}]+)[;}]/i);
      if (!srcMatch) return null;
      const url = extractFontUrl(srcMatch[1]);
      if (!url) return null;
      try {
        return { cssText, url: new URL(url, document.baseURI).href };
      } catch (_error) {
        return null;
      }
    });

    const dataUris = await Promise.all(
      resolved.map((entry) => (entry ? fetchAsDataUri(entry.url) : Promise.resolve(null))),
    );

    const inlined = [];
    let totalBytes = 0;
    for (let index = 0; index < resolved.length; index += 1) {
      const entry = resolved[index];
      const dataUri = dataUris[index];
      if (!entry || !dataUri) continue;
      const approxBytes = dataUri.length * 0.75;
      if (totalBytes + approxBytes > MAX_TOTAL_FONT_BYTES) break;
      totalBytes += approxBytes;
      inlined.push(entry.cssText.replace(SRC_DECLARATION_RE, `src: url("${dataUri}")`));
    }
    return inlined.length > 0 ? inlined.join("\n") : null;
  }

  function stripExternal(clone) {
    const images = clone.querySelectorAll("img");
    for (let index = 0; index < images.length; index += 1) {
      const src = images[index].getAttribute("src");
      if (src && !src.startsWith("data:")) images[index].removeAttribute("src");
    }

    const elements = clone.querySelectorAll("*");
    for (let index = 0; index < elements.length; index += 1) {
      const style = elements[index].style.cssText;
      if (style && style.includes("url(")) {
        elements[index].style.cssText = style.replace(
          /url\(["']?(?!data:)[^)"']*["']?\)/gi,
          "none",
        );
      }
    }
  }

  function emitScroll() {
    scrollRaf = 0;
    if (!scrollForwarding) return;
    postToTrustedTop({
      type: "INLINE_EDIT_SCROLL",
      scrollX: window.scrollX,
      scrollY: window.scrollY,
    });
  }

  window.addEventListener(
    "scroll",
    function () {
      if (!scrollForwarding || scrollRaf) return;
      scrollRaf = requestAnimationFrame(emitScroll);
    },
    { passive: true, capture: true },
  );

  async function handleCaptureRequest(event) {
    const requestId = event.data.requestId;
    const scrollX = window.scrollX;
    const scrollY = window.scrollY;
    const width = window.innerWidth;
    const height = window.innerHeight;

    function postResult(dataUrl) {
      postToTrustedTop({
        type: "INLINE_EDIT_SCREENSHOT_RESULT",
        requestId,
        dataUrl,
        scrollX,
        scrollY,
      });
    }

    try {
      // Wait for any pending web fonts to resolve so both inline metrics and
      // the @font-face inlining below see the same loaded faces.
      if (document.fonts && document.fonts.ready) {
        try {
          await document.fonts.ready;
        } catch (_error) {}
      }

      const clone = document.documentElement.cloneNode(true);
      inlineAll(document.documentElement, clone);

      const removedNodes = clone.querySelectorAll("script,link[rel=\"stylesheet\"],style");
      for (let index = 0; index < removedNodes.length; index += 1) {
        removedNodes[index].remove();
      }

      stripExternal(clone);

      // Re-embed web fonts as data-URI @font-face rules so the SVG rasterizer
      // can resolve them — external font URLs aren't fetched during
      // foreignObject rendering, which would otherwise force a fallback face
      // and change text metrics.
      const inlinedFontCss = await buildInlinedFontCss();
      if (inlinedFontCss) {
        const styleEl = document.createElement("style");
        styleEl.textContent = inlinedFontCss;
        const head = clone.querySelector("head");
        if (head) head.appendChild(styleEl);
        else clone.insertBefore(styleEl, clone.firstChild);
      }

      const html = new XMLSerializer().serializeToString(clone);
      const svg =
        `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}">` +
        '<foreignObject width="100%" height="100%">' +
        `<div xmlns="http://www.w3.org/1999/xhtml" style="width:${width}px;height:${height}px;overflow:hidden">` +
        `<div style="position:relative;left:-${scrollX}px;top:-${scrollY}px">` +
        html +
        "</div></div></foreignObject></svg>";
      const svgUrl = `data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg)}`;
      const image = new Image();
      image.onload = function () {
        const canvas = document.createElement("canvas");
        canvas.width = width;
        canvas.height = height;
        canvas.getContext("2d").drawImage(image, 0, 0);
        postResult(canvas.toDataURL("image/png"));
      };
      image.onerror = function () {
        postResult(null);
      };
      image.src = svgUrl;
    } catch (_error) {
      postResult(null);
    }
  }

  window.addEventListener("message", function (event) {
    if (!event.data) return;
    // Only accept messages from the direct parent frame. Blocks sibling /
    // unrelated-window postMessage senders that could otherwise reach us.
    if (event.source !== window.parent) return;

    const trustedParentOrigin = getTrustedParentOrigin(event);
    if (!trustedParentOrigin) return;
    trustedTopOrigin = trustedParentOrigin;

    if (event.data.type === "INLINE_EDIT_SCROLL_START") {
      scrollForwarding = true;
      emitScroll();
      return;
    }

    if (event.data.type === "INLINE_EDIT_SCROLL_STOP") {
      scrollForwarding = false;
      if (scrollRaf) cancelAnimationFrame(scrollRaf);
      scrollRaf = 0;
      return;
    }

    if (event.data.type === "INLINE_EDIT_SCROLL_BY") {
      const { deltaX, deltaY, deltaMode } = event.data;
      if (!scrollForwarding || !Number.isFinite(deltaX) || !Number.isFinite(deltaY)) return;
      if (deltaMode !== 0 && deltaMode !== 1 && deltaMode !== 2) return;
      const lineHeight =
        parseFloat(getComputedStyle(document.documentElement).lineHeight) || DEFAULT_LINE_HEIGHT;
      const scaleX = deltaMode === 1 ? lineHeight : deltaMode === 2 ? window.innerWidth : 1;
      const scaleY = deltaMode === 1 ? lineHeight : deltaMode === 2 ? window.innerHeight : 1;
      // The selection overlay receives the wheel event instead of this iframe.
      // Instant scrolling preserves trackpad deltas even on smooth-scroll sites.
      window.scrollBy({ left: deltaX * scaleX, top: deltaY * scaleY, behavior: "instant" });
      emitScroll();
      return;
    }

    if (event.data.type !== "INLINE_EDIT_CAPTURE_REQUEST") return;

    handleCaptureRequest(event);
  });
})();


</script></body></html>
