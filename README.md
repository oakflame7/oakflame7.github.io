# oakflame7.github.io[castaway.html](https://github.com/user-attachments/files/30826182/castaway.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<title>Castaway — The Sea of Hollows</title>
<style>
  :root{
    --deep:#08110f;
    --sea:#0d1a1e;
    --ink:#e9e2cf;
    --ash:#8b9793;
    --brass:#e0a141;
    --rust:#9c3f22;
    --lichen:#7f8f6d;
    --bone:#cfc7ae;
    --panel:rgba(8,14,15,.94);
  }
  *{box-sizing:border-box}
  html,body{margin:0;height:100%;background:#05080a;color:var(--ink);
    font-family:"Hoefler Text","Baskerville","Iowan Old Style",Georgia,serif;
    overflow:hidden;-webkit-font-smoothing:antialiased}
  #wrap{position:relative;width:100vw;height:100vh;height:100dvh;display:flex;align-items:center;justify-content:center}
  #stage{position:relative;width:min(100vw,1008px);width:min(100vw,1008px,calc(100vh*12/7));width:min(100vw,1008px,calc(100dvh*12/7));aspect-ratio:12/7}
  canvas{display:block;width:100%;height:100%;background:var(--sea);
    image-rendering:pixelated;border:1px solid #20302e}
  .mono{font-family:"Courier New",monospace}

  /* ---------------- HUD ---------------- */
  #hud{position:absolute;left:0;right:0;top:0;padding:10px 12px;display:flex;gap:14px;
    align-items:flex-start;justify-content:space-between;font-size:13px;pointer-events:none}
  .bars{display:flex;flex-direction:column;gap:5px;min-width:200px}
  .bar{height:9px;background:#0b1213;border:1px solid #2b3a38;position:relative;overflow:hidden}
  .bar i{position:absolute;inset:0;transform-origin:left;transition:transform .12s linear}
  .bar.hp i{background:linear-gradient(90deg,#7a2b18,#c25334)}
  .bar.vg i{background:linear-gradient(90deg,#2c4a4f,#79b0a6)}
  .bar.ms i{background:linear-gradient(90deg,#4a4256,#a396c2)}
  .barlab{display:flex;justify-content:space-between;font-size:10px;color:var(--ash);
    text-transform:uppercase;letter-spacing:.16em;font-family:"Courier New",monospace}
  #who{font-family:"Courier New",monospace;font-size:10px;letter-spacing:.16em;
    text-transform:uppercase;color:var(--ash);margin-bottom:2px}
  #who b{color:var(--ink);font-weight:400}
  #right{text-align:right;font-family:"Courier New",monospace;font-size:11px;color:var(--ash);
    text-transform:uppercase;letter-spacing:.14em;line-height:1.85}
  #right b{color:var(--ink);font-weight:400}
  .thr0{color:var(--lichen)!important}
  .thr1{color:var(--brass)!important}
  .thr2{color:var(--rust)!important}
  .thr3{color:#e0483a!important;text-shadow:0 0 8px rgba(224,72,58,.45)}

  #objective{position:absolute;left:12px;bottom:12px;max-width:44%;font-size:12.5px;
    line-height:1.55;color:#b9c4c0;text-shadow:0 1px 3px #000;pointer-events:none}
  #objective span{display:block;font-family:"Courier New",monospace;font-size:10px;
    letter-spacing:.18em;text-transform:uppercase;color:var(--brass);margin-bottom:3px}
  #party{position:absolute;right:12px;bottom:12px;text-align:right;font-size:12.5px;
    color:#b9c4c0;text-shadow:0 1px 3px #000;pointer-events:none}
  #party .pm{margin-bottom:8px}
  #party em{display:block;font-family:"Courier New",monospace;font-size:10px;
    letter-spacing:.16em;text-transform:uppercase;color:var(--lichen);font-style:normal}
  #party .hb{width:126px;height:6px;background:#0b1213;border:1px solid #2b3a38;
    margin:4px 0 0 auto;overflow:hidden}
  #party .hb i{display:block;height:100%;background:linear-gradient(90deg,#4b6b4a,#8fb07a);
    transform-origin:left;transition:transform .15s linear}
  #party .dn .hb i{background:linear-gradient(90deg,#6b2b1a,#a24a2c)}
  #party .dn em{color:var(--rust)}
  #prompt{position:absolute;left:50%;bottom:16px;transform:translateX(-50%);
    font-family:"Courier New",monospace;font-size:11px;letter-spacing:.16em;
    text-transform:uppercase;color:var(--brass);opacity:0;transition:opacity .15s;pointer-events:none}
  #toast{position:absolute;right:12px;top:110px;max-width:262px;text-align:right;
    font-size:12.5px;line-height:1.5;opacity:0;transition:opacity .4s;pointer-events:none}
  #toast em{color:var(--brass);font-style:normal;font-family:"Courier New",monospace;
    font-size:10px;letter-spacing:.16em;text-transform:uppercase;display:block}
  #paused{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);text-align:center;
    font-family:"Courier New",monospace;font-size:12px;letter-spacing:.34em;text-transform:uppercase;
    color:var(--brass);display:none;pointer-events:none;text-shadow:0 2px 8px #000}
  #paused.on{display:block}
  #paused i{display:block;font-style:normal;font-size:9px;letter-spacing:.2em;color:var(--ash);margin-top:6px}
  #belt{position:absolute;left:50%;bottom:44px;transform:translateX(-50%);display:flex;gap:5px;pointer-events:none}
  .slot{width:52px;height:40px;border:1px solid #2b3a38;background:rgba(8,14,15,.72);
    font-family:"Courier New",monospace;font-size:9px;color:#a9b4b0;padding:3px 4px;
    line-height:1.25;position:relative;overflow:hidden;text-transform:uppercase;letter-spacing:.04em}
  .slot u{position:absolute;left:3px;top:2px;font-size:8px;color:#5d6a67;text-decoration:none}
  .slot b{position:absolute;right:3px;bottom:2px;font-size:11px;color:var(--ink);font-weight:400}
  .slot s{display:block;margin-top:9px;text-decoration:none;color:#c3cdc9}
  .slot.use{border-color:var(--brass)}

  /* ---------------- panels ---------------- */
  .panel{position:absolute;inset:0;background:var(--panel);display:none;
    padding:22px 28px 44px;overflow:auto}
  .panel.on{display:block}
  .panel h2{margin:0 0 2px;font-size:22px;font-weight:400;letter-spacing:.04em}
  .panel .sub{font-family:"Courier New",monospace;font-size:10px;letter-spacing:.2em;
    text-transform:uppercase;color:var(--ash);margin-bottom:16px}
  .hint{position:absolute;right:26px;bottom:16px;font-family:"Courier New",monospace;
    font-size:10px;letter-spacing:.18em;text-transform:uppercase;color:var(--ash)}
  .grid{display:grid;grid-template-columns:repeat(4,1fr);gap:9px;max-width:640px}
  .cell{border:1px solid #2b3a38;background:rgba(255,255,255,.02);padding:9px 10px;min-height:74px;
    cursor:pointer;position:relative}
  .cell:hover{border-color:var(--brass)}
  .cell b{display:block;font-size:13.5px;font-weight:400}
  .cell span{font-size:11px;color:#9aa5a1;line-height:1.35;display:block;margin-top:2px}
  .cell i{position:absolute;right:8px;top:6px;font-style:normal;font-family:"Courier New",monospace;
    font-size:13px;color:var(--brass)}
  .cell.empty{color:#4e5a57;cursor:default;border-style:dashed}
  .cell.empty:hover{border-color:#2b3a38}
  .rec{border-left:2px solid #33423f;padding:4px 0 4px 12px;margin-bottom:11px;cursor:pointer;max-width:600px}
  .rec:hover{border-left-color:var(--brass)}
  .rec b{display:block;font-size:14.5px;font-weight:400}
  .rec span{font-size:12px;color:#9aa5a1}
  .rec .cost{font-family:"Courier New",monospace;font-size:11px;letter-spacing:.06em;color:var(--ash)}
  .rec.ok .cost{color:var(--lichen)}
  .rec.no{opacity:.55;cursor:default}
  .rec.done{opacity:.5;cursor:default}
  .rec.done b:after{content:" — made";color:var(--brass);font-size:11px;font-family:"Courier New",monospace}
  .rk{display:flex;gap:4px;margin:10px 0 14px}
  .rk i{width:44px;height:5px;background:#1b2422;border:1px solid #2b3a38;font-style:normal}
  .rk i.on{background:linear-gradient(90deg,#6a5c86,#a396c2)}
  .ab{border-left:2px solid var(--rust);padding:2px 0 2px 12px;margin-bottom:13px;max-width:600px}
  .ab b{display:block;font-size:14.5px;font-weight:400}
  .ab span{font-size:12.5px;color:#a4afab;line-height:1.5}
  .ab.locked{border-left-color:#2b3a38;opacity:.5}
  .empt{color:var(--ash);font-size:13px;font-style:italic;max-width:520px;line-height:1.6}
  .logline{font-size:12.5px;color:#a4afab;line-height:1.55;margin-bottom:7px;max-width:640px}
  .two{display:grid;grid-template-columns:1fr 1fr;gap:26px;max-width:760px}
  .two h3{margin:0 0 8px;font-size:13px;font-weight:400;font-family:"Courier New",monospace;
    letter-spacing:.18em;text-transform:uppercase;color:var(--ash)}
  .row{display:flex;justify-content:space-between;align-items:baseline;gap:10px;
    border-bottom:1px solid #1c2624;padding:5px 0;cursor:pointer}
  .row:hover{border-bottom-color:var(--brass)}
  .row b{font-weight:400;font-size:13.5px}
  .row span{font-family:"Courier New",monospace;font-size:11px;color:var(--ash)}
  .row.flat{cursor:default}
  .row.flat:hover{border-bottom-color:#1c2624}
  .ok2{color:var(--lichen)!important}
  .no2{color:#5d6a67!important}
  .artnote{max-width:700px;font-size:12.5px;line-height:1.6;color:#a4afab;margin-bottom:14px}
  .artnote code{font-family:"Courier New",monospace;color:var(--brass);font-size:12px}
  .artbtn{display:inline-block;margin:0 10px 6px 0;color:var(--brass);cursor:pointer;
    font-family:"Courier New",monospace;font-size:10.5px;letter-spacing:.14em;text-transform:uppercase}
  .artbtn:hover{color:var(--ink)}
  .artgrid{display:grid;grid-template-columns:repeat(2,1fr);gap:0 26px;max-width:780px}
  .logline em{font-style:normal;font-family:"Courier New",monospace;font-size:10px;
    letter-spacing:.16em;text-transform:uppercase;color:var(--ash);margin-right:8px}

  /* ---------------- dialogue ---------------- */
  #dbox{position:absolute;left:6%;right:6%;bottom:5%;background:var(--panel);
    border:1px solid #2f3d3b;border-left:3px solid var(--brass);padding:15px 20px 20px;display:none}
  #dbox.on{display:block}
  #dname{font-family:"Courier New",monospace;font-size:10.5px;letter-spacing:.2em;
    text-transform:uppercase;color:var(--brass);margin-bottom:7px}
  #dname b{color:var(--ash);font-weight:400;letter-spacing:.12em}
  #dtext{font-size:15.5px;line-height:1.6;min-height:3.1em}
  #dtext i{color:#9fb6ae;font-style:italic}
  #dmore{position:absolute;right:16px;bottom:7px;font-size:11px;color:var(--ash);
    font-family:"Courier New",monospace;letter-spacing:.16em}
  #dopts{display:none;margin-top:12px}
  #dopts.on{display:block}
  #dopts button{display:block;width:100%;text-align:left;background:none;border:none;
    border-left:2px solid #33423f;color:#c8d2ce;font-family:inherit;font-size:14px;
    padding:5px 0 5px 11px;margin-bottom:5px;cursor:pointer}
  #dopts button:hover{border-left-color:var(--brass);color:var(--ink)}
  #dopts button:disabled{opacity:.42;cursor:default}

  /* ---------------- title / interlude / ending ---------------- */
  .full{position:absolute;inset:0;background:#05080a;display:none;align-items:center;justify-content:center}
  .full.on{display:flex}
  .card{max-width:640px;padding:0 34px}
  .card h1{font-size:38px;margin:0 0 4px;font-weight:400;letter-spacing:.03em}
  .card .kick{font-family:"Courier New",monospace;font-size:10.5px;letter-spacing:.24em;
    text-transform:uppercase;color:var(--brass);margin-bottom:20px}
  .card p{font-size:14.5px;line-height:1.68;color:#b6c1bd;margin:0 0 13px}
  .card p.small{font-size:12.5px;color:#8b9793}
  .keys{font-family:"Courier New",monospace;font-size:11px;letter-spacing:.1em;color:var(--ash);
    line-height:2;text-transform:uppercase;margin:16px 0 20px}
  .keys b{color:var(--ink);font-weight:400}
  .btn{background:none;border:1px solid var(--brass);color:var(--brass);
    font-family:"Courier New",monospace;font-size:11px;letter-spacing:.2em;text-transform:uppercase;
    padding:10px 20px;cursor:pointer}
  .btn:hover{background:rgba(224,161,65,.1)}
  #inter .card p:first-of-type{color:#c8d2ce}
  #idle{font-family:"Courier New",monospace;font-size:10px;letter-spacing:.2em;
    text-transform:uppercase;color:var(--ash);margin-top:18px}
  #chartlist .rec{max-width:560px}

  /* ---------------- touch ---------------- */
  #tlayer,#tstick,#tbtns,#tmenu,#rotatehint,.pclose,.tkeys{display:none}
  body.touch{position:fixed;inset:0;width:100%;height:100%;touch-action:none;
    overscroll-behavior:none;-webkit-user-select:none;user-select:none;
    -webkit-touch-callout:none;-webkit-tap-highlight-color:transparent}
  body.touch textarea,body.touch input{-webkit-user-select:text;user-select:text}
  body.touch #tlayer{display:block;position:absolute;inset:0;touch-action:none}
  body.touch #tstick{display:block;position:absolute;left:-999px;top:-999px;width:116px;height:116px;
    margin:-58px 0 0 -58px;border:1px solid rgba(224,161,65,.35);border-radius:50%;
    background:rgba(8,14,15,.28);pointer-events:none;opacity:0;transition:opacity .1s}
  body.touch #tstick.on{opacity:1}
  #tnub{position:absolute;left:50%;top:50%;width:46px;height:46px;margin:-23px 0 0 -23px;
    border-radius:50%;background:rgba(224,161,65,.45);border:1px solid var(--brass)}
  body.touch #tbtns{display:flex;flex-direction:column;gap:10px;position:absolute;right:10px;bottom:104px;
    align-items:flex-end;pointer-events:none}
  .trow{display:flex;gap:10px;align-items:flex-end;justify-content:flex-end}
  .tbtn{pointer-events:auto;touch-action:none;border-radius:50%;border:1px solid #3a4a47;
    background:rgba(8,14,15,.55);color:var(--bone);font-family:"Courier New",monospace;
    font-size:11px;letter-spacing:.08em;text-transform:uppercase;padding:0}
  .tbtn:active{border-color:var(--brass);color:var(--ink);background:rgba(224,161,65,.12)}
  .ts{width:52px;height:52px}
  .tbig{width:78px;height:78px;font-size:13px;border-color:#5a4630}
  body.touch #tmenu{display:flex;gap:6px;position:absolute;left:50%;top:6px;transform:translateX(-50%);
    pointer-events:none}
  .tmb{pointer-events:auto;touch-action:none;border:1px solid #2b3a38;background:rgba(8,14,15,.6);
    color:var(--ash);font-family:"Courier New",monospace;font-size:10px;letter-spacing:.12em;
    text-transform:uppercase;padding:7px 8px}
  .tmb:active{border-color:var(--brass);color:var(--ink)}
  body.touch #belt{pointer-events:auto;bottom:52px}
  body.touch .slot{width:56px;height:44px}
  body.touch .keys{display:none}
  body.touch .tkeys{display:block}
  body.touch .pclose{display:flex;position:absolute;right:10px;top:10px;width:42px;height:42px;
    align-items:center;justify-content:center;border:1px solid #3a4a47;background:rgba(8,14,15,.6);
    color:var(--bone);font-size:16px;cursor:pointer}
  body.touch .panel{touch-action:pan-y;overscroll-behavior:contain;padding-right:64px}
  body.touch .full{touch-action:pan-y;overscroll-behavior:contain;overflow:auto;
    align-items:flex-start;padding:14px 0}
  body.touch .card{padding:0 22px}
  @media (orientation:portrait){
    body.touch #rotatehint{display:block;position:fixed;left:0;right:0;top:0;z-index:9;
      text-align:center;padding:8px 12px;background:rgba(8,14,15,.9);border-bottom:1px solid #2b3a38;
      font-family:"Courier New",monospace;font-size:10px;letter-spacing:.2em;text-transform:uppercase;
      color:var(--brass);pointer-events:none}
  }
</style>
</head>
<body>
<div id="wrap"><div id="stage">
  <canvas id="cv" width="1008" height="588"></canvas>
  <div id="tlayer"></div>
  <div id="tstick"><div id="tnub"></div></div>

  <div id="hud">
    <div class="bars">
      <div id="who">castaway</div>
      <div class="barlab"><span>body</span><span id="hpn">100</span></div>
      <div class="bar hp"><i id="hpb"></i></div>
      <div class="barlab"><span>vigor</span><span id="vgn">100</span></div>
      <div class="bar vg"><i id="vgb"></i></div>
      <div class="barlab"><span id="rkl">mist</span><span id="msn">0</span></div>
      <div class="bar ms"><i id="msb"></i></div>
    </div>
    <div id="right">
      <div id="islname">—</div>
      <div id="thrl">—</div>
      <div>pack <b id="packn">0/8</b> · slain <b id="killn">0</b></div>
      <div id="craftl">no craft</div>
    </div>
  </div>

  <div id="objective"><span id="objhead">ashore</span><div id="objtxt"></div></div>
  <div id="party"></div>
  <div id="prompt"></div>
  <div id="toast"></div>
  <div id="belt"></div>
  <div id="paused">paused<i>close the panel to go on · p</i></div>
  <div id="tbtns">
    <div class="trow">
      <button class="tbtn ts" id="tq">Q</button>
      <button class="tbtn ts" id="tf">F</button>
      <button class="tbtn ts" id="th">H</button>
    </div>
    <div class="trow">
      <button class="tbtn ts" id="tdash">dash</button>
      <button class="tbtn ts" id="tuse">E</button>
      <button class="tbtn tbig" id="tatk">atk</button>
    </div>
  </div>
  <div id="tmenu">
    <button class="tmb" id="tm_pack">pack</button>
    <button class="tmb" id="tm_craft">craft</button>
    <button class="tmb" id="tm_rg">regard</button>
    <button class="tmb" id="tm_map">chart</button>
    <button class="tmb" id="tm_log">log</button>
    <button class="tmb" id="tm_pause">❚❚</button>
    <button class="tmb" id="tm_fs">⛶</button>
  </div>
  <div id="rotatehint">turn the device sideways · the sea is wide</div>

  <div class="panel" id="pack">
    <h2>Backpack</h2><div class="sub" id="packsub">click a stack to pick it up, then a slot to move it</div>
    <div class="grid" id="packgrid"></div>
    <div class="hint">tab closes</div>
  </div>

  <div class="panel" id="craft">
    <h2>What can be made</h2><div class="sub">from what is in the pack</div>
    <div id="reclist"></div>
    <div class="hint">c closes</div>
  </div>

  <div class="panel" id="buildp">
    <h2 id="buildttl">What can be put up</h2><div class="sub" id="buildsub">click to raise it where you are standing</div>
    <div id="buildlist"></div>
    <div id="homestate" style="margin-top:20px"></div>
    <div class="hint">b closes</div>
  </div>

  <div class="panel" id="cachep">
    <h2 id="cacheh2">The hold</h2><div class="sub" id="cachesub">what you keep at the craft · click to move a stack</div>
    <div class="two">
      <div><h3>backpack</h3><div id="cacheL"></div></div>
      <div><h3 id="cacheRh">the hold</h3><div id="cacheR"></div></div>
    </div>
    <div class="hint">e or esc closes</div>
  </div>

  <div class="panel" id="cityp">
    <h2>Cities</h2><div class="sub">large, populated, and no monsters on them</div>
    <div class="artnote" id="citynote"></div>
    <input type="file" id="cityfile" multiple accept=".js,.json" style="display:none">
    <div id="citylist"></div>
    <div class="hint">y closes</div>
  </div>

  <div class="panel" id="artp">
    <h2>Artwork</h2><div class="sub">what the game is drawing with</div>
    <div class="artnote" id="artnote"></div>
    <input type="file" id="artfile" multiple accept=".js,.png,image/png" style="display:none">
    <div class="artgrid" id="artlist"></div>
    <div class="hint">k closes</div>
  </div>

  <div class="panel" id="regard">
    <h2 id="rgname">No Regard</h2><div class="sub" id="rgsub">nothing has looked at you yet</div>
    <div class="rk" id="rgrank"></div>
    <div id="rgbody"></div>
    <div class="hint">r closes</div>
  </div>

  <div class="panel" id="chart">
    <h2>The chart</h2><div class="sub" id="chartsub">where the water goes from here</div>
    <canvas id="mcv" width="760" height="420" style="width:min(100%,560px);height:auto;border:1px solid #2c3a38;background:#0a1214"></canvas>
    <div id="chartlist" style="margin-top:16px"></div>
    <div class="hint">m closes</div>
  </div>

  <div class="panel" id="logp">
    <h2>What has happened</h2><div class="sub">the running account</div>
    <div id="loglist"></div>
    <div class="hint">j closes</div>
  </div>

  <div id="dbox">
    <div id="dname">—</div><div id="dtext"></div>
    <div id="dopts"></div><div id="dmore">e ›</div>
  </div>

  <div class="full on" id="title"><div class="card">
    <h1>Castaway</h1>
    <div class="kick">the sea of hollows · a small, cold survival</div>
    <p>Your ship is gone. You are on sand that isn't on any chart you have seen, and the fog behind you has already closed over the place where the hull was.</p>
    <p>Gather what the island will give up. Kill what walks at you — swings cost vigor now, so watch your breath. If something old is still awake here, it may look at you long enough to offer a <b>Regard</b> — a passive that never switches off and one thing you can do on purpose. You can carry two, and you can always turn one down; a declined Regard goes to whoever is walking with you, or comes apart into mist.</p>
    <p class="small">Mist comes off the dead and is the only thing a Regard grows on. It takes a great deal of it.</p>
    <p class="small">Build a raft and the current chooses your next island. Build a boat and you choose. Lay a
      <b>hearth</b> on a quiet island and it becomes yours — nothing regrows on it, things will grow in it, and it is
      always on the chart.</p>
    <div class="keys">
      <b>WASD</b> move · <b>mouse</b> aim · <b>click</b> strike · <b>space</b> dash<br>
      <b>E</b> gather / talk / the hold / put to sea · <b>1–8</b> use pack slot<br>
      <b>Q</b> first Regard · <b>F</b> second Regard · <b>H</b> get somebody off the ground<br>
      <b>Tab</b> pack · <b>C</b> craft · <b>R</b> regard · <b>M</b> chart · <b>J</b> account<br>
      <b>P</b> pause · <b>K</b> artwork · <b>Y</b> cities (drop your own in)
    </div>
    <div class="keys tkeys"><b>left thumb</b> walks · <b>atk</b> strikes — hold it down and it keeps swinging<br>
      <b>e</b> gathers, talks, opens the hold, puts to sea · <b>dash</b> costs vigor · <b>q</b>/<b>f</b> regards · <b>h</b> lifts a friend<br>
      tap a belt slot to use it · the strip along the top opens everything else</div>
    <button class="btn" id="start">Wake up on the sand</button>
    <button class="btn2" id="contbtn" style="display:none;margin-left:8px">Pick the run back up</button>
    <div id="netui" style="margin-top:14px;border-top:1px solid #2a3436;padding-top:10px">
      <div class="kick">wash ashore together — no server, the two browsers talk straight across</div>
      <div id="netbtns">
        <button class="btn2" id="nethost">Host a crossing</button>
        <button class="btn2" id="netjoin">Join a crossing</button>
      </div>
      <div id="netflow" style="display:none">
        <div class="small" id="netstep"></div>
        <textarea id="netout" readonly rows="3" style="width:100%;background:#0d1416;color:#9fb0ac;border:1px solid #2a3436;font-size:10px" placeholder=""></textarea>
        <textarea id="netin" rows="3" style="width:100%;background:#0d1416;color:#cfc7ae;border:1px solid #2a3436;font-size:10px" placeholder="paste your brother's code here"></textarea>
        <button class="btn2" id="netgo">Use the pasted code</button>
        <span class="small" id="netstat"></span>
      </div>
    </div>
  </div></div>

  <div class="full" id="inter"><div class="card">
    <div class="kick" id="ikick">—</div>
    <h1 id="ittl" style="font-size:29px">—</h1>
    <div id="itxt"></div>
    <div id="idle">press e</div>
  </div></div>

  <div class="full" id="ending"><div class="card">
    <div class="kick">the account closes</div>
    <h1 id="ettl" style="font-size:30px">—</h1>
    <div id="etxt"></div>
    <button class="btn" id="again" style="margin-top:18px">Wash ashore again</button>
  </div></div>

</div></div>

<!-- An external art.js may sit either beside this file or in an art/ folder.
     Both are checked and merged. A 404 for either is harmless. -->
<script>window.__ARTF={};window.__OPTF={};
  window.__artSnap=function(){
    if(window.ART_OVERRIDE) Object.assign(window.__ARTF,window.ART_OVERRIDE);
    if(window.ART_OPTS) Object.assign(window.__OPTF,window.ART_OPTS);
    window.ART_OVERRIDE=null; window.ART_OPTS=null;
  };</script>
<script src="art/art.js" onerror="void 0"></script>
<script>window.__artSnap();</script>
<script src="art.js" onerror="void 0"></script>
<script>window.__artSnap();</script>
<!-- The animation player. The game carries its own copy, so this is optional. -->
<script src="art/art-anim.js" onerror="void 0"></script>
<script src="art-anim.js" onerror="void 0"></script>
<!-- Cities are data too. Either place is checked, and both are merged. -->
<script src="city/city.js" onerror="void 0"></script>
<script src="city.js" onerror="void 0"></script>
<script>
"use strict";
/* ============================================================ SETUP */
const cv=document.getElementById('cv'), ctx=cv.getContext('2d');
const VW=cv.width, VH=cv.height;
const TILE=28;
let GW=76, GH=56;
const ISL_W=76, ISL_H=56;              // what a natural island always is
const MAX_W=220, MAX_H=160;            // what a city district may grow to
const $=id=>document.getElementById(id);
const clamp=(v,a,b)=>v<a?a:v>b?b:v;
const dist=(a,b,c,d)=>Math.hypot(a-c,b-d);
const pick=a=>a[Math.floor(rnd()*a.length)];
const tileHash=(x,y)=>((x*73856093)^(y*19349663))>>>0;

let seed=(Date.now()>>>3)||20260728;
const rnd=()=>{seed=(seed*1664525+1013904223)>>>0;return seed/4294967296;};

const WATER=0,SHOAL=1,SAND=2,GRASS=3,DIRT=4,PATH=5,TREE=6,PINE=7,SCRUB=8,ROCK=9,
      CLIFF=10,SCREE=11,SNOW=12,ICE=13,WALL=14,FLOOR=15,DOOR=16,PLANK=17,RUIN=18,
      MIRE=19,VEIN=20,GRAVE=21,MOSS=22;
const SOLID={0:1,6:1,7:1,9:1,10:1,14:1,18:1,20:1};
const SLOW={1:1,19:1,11:1};
let grid=new Uint8Array(GW*GH);
let reach=new Uint8Array(GW*GH);
function resizeWorld(w,h){
  w=Math.max(20,Math.min(MAX_W,Math.round(w)));
  h=Math.max(20,Math.min(MAX_H,Math.round(h)));
  if(w===GW&&h===GH){ grid.fill(WATER); reach.fill(0); return; }
  GW=w; GH=h;
  grid=new Uint8Array(GW*GH);
  reach=new Uint8Array(GW*GH);
}

const T=(x,y)=>(x<0||y<0||x>=GW||y>=GH)?WATER:grid[y*GW+x];
const setT=(x,y,v)=>{if(x>=0&&y>=0&&x<GW&&y<GH)grid[y*GW+x]=v;};
const solidAt=(x,y)=>!!SOLID[T(Math.floor(x/TILE),Math.floor(y/TILE))];
const slowAt=(x,y)=>!!SLOW[T(Math.floor(x/TILE),Math.floor(y/TILE))];
function free(x,y,r){
  return !solidAt(x-r,y-r)&&!solidAt(x+r,y-r)&&!solidAt(x-r,y+r)&&!solidAt(x+r,y+r);
}
const tc=(tx,ty)=>({x:tx*TILE+TILE/2,y:ty*TILE+TILE/2});
const isReach=(tx,ty)=>tx>=0&&ty>=0&&tx<GW&&ty<GH&&reach[ty*GW+tx]===1;

/* ============================================================ ISLAND GEN */
const THEMES={
  forest:{n:"forest",scale:1.00,ground:GRASS,tint:"rgba(24,44,34,.16)"},
  outcrop:{n:"outcrop",scale:0.70,ground:DIRT,tint:"rgba(34,44,50,.18)"},
  mountain:{n:"mountain",scale:0.96,ground:SCREE,tint:"rgba(40,42,46,.16)"},
  snow:{n:"snow",scale:0.98,ground:SNOW,tint:"rgba(140,168,180,.13)"},
  town:{n:"town",scale:0.94,ground:GRASS,tint:"rgba(52,44,30,.14)"}
};
const ISL_A=["Wiyaka","Tarth","Fris","Kettle","Ganno","Sable","Corbie","Nemin",
             "Hollow","Ashken","Vesper","Quian","Odawa","Skeen","Brill","Tolus","Grange","Merrow"];
const ISL_B=["Rock","Head","Isle","Shoal","Reach","Spit","Bank","Holm","Cairn","Skerry","Bar","Point"];
const BNAMES=["Trade Post","Bakehouse","Meeting Hall","Boat Sheds","Salt House","Lamp Keeper's",
              "The Long Room","Netloft","Cooperage","Winter Store"];

let np=[],np2=[];
function newNoise(){
  np=[];np2=[];
  for(let i=0;i<5;i++){
    np.push({a:rnd()*6.28,b:rnd()*6.28,fx:.06+rnd()*.42,fy:.06+rnd()*.42,am:1/(i+1)});
    np2.push({a:rnd()*6.28,b:rnd()*6.28,fx:.10+rnd()*.60,fy:.10+rnd()*.60,am:1/(i+1)});
  }
}
function nz(set,x,y){
  let v=0,s=0;
  for(const p of set){ v+=p.am*Math.sin(x*p.fx+p.a)*Math.sin(y*p.fy+p.b); s+=p.am; }
  return v/s;
}
function carve(x1,y1,x2,y2,t){
  const sx=x2>x1?1:-1, sy=y2>y1?1:-1;
  for(let x=x1;x!==x2+sx;x+=sx){ const c=T(x,y1); if(c!==WATER&&c!==SHOAL&&c!==WALL&&c!==FLOOR) setT(x,y1,t); }
  for(let y=y1;y!==y2+sy;y+=sy){ const c=T(x2,y); if(c!==WATER&&c!==SHOAL&&c!==WALL&&c!==FLOOR) setT(x2,y,t); }
}
function blob(cx,cy,r,t,onlyLand){
  for(let y=Math.floor(cy-r);y<=cy+r;y++)for(let x=Math.floor(cx-r);x<=cx+r;x++){
    if(dist(x,y,cx,cy)>r*(.7+rnd()*.5)) continue;
    const c=T(x,y);
    if(onlyLand&&(c===WATER||c===SHOAL||c===SAND)) continue;
    setT(x,y,t);
  }
}

function heightField(theme){
  const th=THEMES[theme];
  const rx=GW*.42*th.scale, ry=GH*.42*th.scale;
  const cx=GW/2+(rnd()*6-3), cy=GH/2+(rnd()*4-2);
  const H=new Float32Array(GW*GH);
  for(let y=0;y<GH;y++)for(let x=0;x<GW;x++){
    const nx=(x-cx)/rx, ny=(y-cy)/ry;
    let d=Math.sqrt(nx*nx+ny*ny);
    let h=1-d+.44*nz(np,x,y);
    // hard fade at the frame so the sea always closes the map
    const edge=Math.min(x,y,GW-1-x,GH-1-y);
    if(edge<4) h-=(4-edge)*.35;
    H[y*GW+x]=h;
  }
  return H;
}

function layTerrain(theme){
  const th=THEMES[theme], H=heightField(theme);
  for(let y=0;y<GH;y++)for(let x=0;x<GW;x++){
    const h=H[y*GW+x];
    let t;
    if(h<.20) t=WATER;
    else if(h<.32) t=SHOAL;
    else if(h<.42) t=SAND;
    else t=th.ground;
    grid[y*GW+x]=t;
  }
  // second pass: theme features on interior land
  for(let y=0;y<GH;y++)for(let x=0;x<GW;x++){
    const h=H[y*GW+x]; if(h<.42) continue;
    const f=nz(np2,x,y), g=(tileHash(x,y)%100)/100, inl=clamp((h-.42)/.6,0,1);
    if(theme==="forest"){
      if(f>.02&&g<.62) setT(x,y,TREE);
      else if(f>-.2&&g<.14) setT(x,y,SCRUB);
      else if(f<-.52&&inl>.25) setT(x,y,g<.5?MIRE:SHOAL);
      else if(g<.05) setT(x,y,MOSS);
      else if(g<.07) setT(x,y,ROCK);
    } else if(theme==="outcrop"){
      if(f>.28&&g<.7) setT(x,y,ROCK);
      else if(f>.12&&g<.5) setT(x,y,SCREE);
      else if(f<-.42) setT(x,y,g<.4?SHOAL:MIRE);
      else if(g<.09) setT(x,y,PINE);
      else if(g<.14) setT(x,y,SCRUB);
    } else if(theme==="mountain"){
      if(h>.86) setT(x,y,CLIFF);
      else if(f>.34&&h>.58) setT(x,y,CLIFF);
      else if(f>.18) setT(x,y,ROCK);
      else if(h<.58&&g<.24) setT(x,y,PINE);
      else if(g<.30) setT(x,y,SCREE);
      else if(g<.34) setT(x,y,DIRT);
    } else if(theme==="snow"){
      if(f>.16&&g<.56) setT(x,y,PINE);
      else if(f<-.5&&inl>.2) setT(x,y,ICE);
      else if(g<.08) setT(x,y,ROCK);
      else if(g<.13) setT(x,y,SCRUB);
    } else { // town
      if(f>.42&&g<.5) setT(x,y,TREE);
      else if(g<.06) setT(x,y,SCRUB);
      else if(g<.08) setT(x,y,ROCK);
    }
  }
  // ore next to stone on mountain / outcrop
  if(theme==="mountain"||theme==="outcrop"){
    for(let y=1;y<GH-1;y++)for(let x=1;x<GW-1;x++){
      if(T(x,y)!==CLIFF&&T(x,y)!==ROCK) continue;
      if(rnd()<(theme==="mountain"?.045:.02)) setT(x,y,VEIN);
    }
  }
  // shoal ring becomes ice on snow islands
  if(theme==="snow"){
    for(let y=0;y<GH;y++)for(let x=0;x<GW;x++)
      if(T(x,y)===SHOAL&&rnd()<.55) setT(x,y,ICE);
  }
  return H;
}

function landCount(){ let n=0; for(let i=0;i<grid.length;i++) if(grid[i]!==WATER&&grid[i]!==SHOAL) n++; return n; }

function findShore(){
  const cand=[];
  for(let y=3;y<GH-3;y++)for(let x=3;x<GW-3;x++){
    if(T(x,y)!==SAND) continue;
    if(T(x-1,y)===WATER||T(x+1,y)===WATER||T(x,y-1)===WATER||T(x,y+1)===WATER||
       T(x-1,y)===SHOAL||T(x+1,y)===SHOAL||T(x,y-1)===SHOAL||T(x,y+1)===SHOAL) cand.push([x,y]);
  }
  return cand.length?pick(cand):[Math.floor(GW/2),Math.floor(GH/2)];
}

function floodReach(sx,sy){
  reach.fill(0);
  const q=[sx,sy]; reach[sy*GW+sx]=1; let n=1;
  while(q.length){
    const y=q.pop(), x=q.pop();
    const nb=[[x+1,y],[x-1,y],[x,y+1],[x,y-1]];
    for(const [a,b] of nb){
      if(a<0||b<0||a>=GW||b>=GH) continue;
      if(reach[b*GW+a]) continue;
      const t=T(a,b);
      if(SOLID[t]||t===SHOAL) continue;
      reach[b*GW+a]=1; n++; q.push(a,b);
    }
  }
  return n;
}

function layTown(){
  const cx=Math.floor(GW/2), cy=Math.floor(GH/2);
  const buildings=[];
  // two streets through the middle of whatever land there is
  carve(6,cy,GW-7,cy,PATH); carve(6,cy+1,GW-7,cy+1,PATH);
  carve(cx,8,cx,GH-9,PATH);
  const names=BNAMES.slice(); const out=[];
  for(let tries=0;tries<200&&out.length<7;tries++){
    const w=6+Math.floor(rnd()*5), h=4+Math.floor(rnd()*3);
    const x=8+Math.floor(rnd()*(GW-18)), y=7+Math.floor(rnd()*(GH-16));
    let ok=true;
    for(let b=y-2;b<y+h+2&&ok;b++)for(let a=x-2;a<x+w+2&&ok;a++){
      const t=T(a,b);
      if(t===WATER||t===SHOAL||t===SAND||t===WALL||t===FLOOR||t===CLIFF) ok=false;
    }
    if(!ok) continue;
    for(const o of out) if(Math.abs(o.x-x)<o.w+4&&Math.abs(o.y-y)<o.h+4) ok=false;
    if(!ok) continue;
    for(let b=y;b<y+h;b++)for(let a=x;a<x+w;a++)
      setT(a,b,(b===y||b===y+h-1||a===x||a===x+w-1)?WALL:FLOOR);
    const dx=x+1+Math.floor(rnd()*(w-2)), dy=(y+h-1);
    setT(dx,dy,DOOR);
    carve(dx,dy+1,dx,(dy<cy?cy:cy+1),PATH);
    out.push({n:names.length?names.splice(Math.floor(rnd()*names.length),1)[0]:"Shed",x,y,w,h,dx,dy});
  }
  // docks: planks out over the shallows on one side
  const sh=findShore();
  for(let i=0;i<7;i++){
    const x=sh[0]-i, y=sh[1];
    const t=T(x,y); if(t===WATER||t===SHOAL||t===ICE) setT(x,y,PLANK);
  }
  for(let y=sh[1]-1;y<=sh[1]+1;y++){ const t=T(sh[0]-2,y); if(t===WATER||t===SHOAL) setT(sh[0]-2,y,PLANK); }
  return out;
}

function layRuin(theme){
  // a broken tower / stone shell, and a few graves near it
  for(let tries=0;tries<80;tries++){
    const x=10+Math.floor(rnd()*(GW-24)), y=9+Math.floor(rnd()*(GH-20));
    let ok=true;
    for(let b=y;b<y+5&&ok;b++)for(let a=x;a<x+6&&ok;a++){
      const t=T(a,b); if(t===WATER||t===SHOAL||t===SAND||t===WALL) ok=false;
    }
    if(!ok) continue;
    for(let b=y;b<y+5;b++)for(let a=x;a<x+6;a++){
      if(b===y||b===y+4||a===x||a===x+5) setT(a,b,RUIN); else setT(a,b,MOSS);
    }
    setT(x+2,y+4,MOSS); setT(x+3,y+4,MOSS);      // the wall has fallen in here
    setT(x+5,y+1,MOSS);
    for(let i=0;i<4;i++){
      const gx=x-2+Math.floor(rnd()*10), gy=y-2+Math.floor(rnd()*9);
      const t=T(gx,gy);
      if(t!==WATER&&t!==SHOAL&&!SOLID[t]) setT(gx,gy,GRAVE);
    }
    return {x:x+3,y:y+2};
  }
  return null;
}

/* ---- line of sight over the tile grid: cover actually protects you ---- */
function losClear(x1,y1,x2,y2){
  const d=Math.hypot(x2-x1,y2-y1);
  if(d<1) return true;
  const steps=Math.ceil(d/10);
  for(let i=1;i<steps;i++){
    const t=i/steps, x=x1+(x2-x1)*t, y=y1+(y2-y1)*t;
    const tt=T(Math.floor(x/TILE),Math.floor(y/TILE));
    if(tt===TREE||tt===PINE||tt===ROCK||tt===CLIFF||tt===WALL||tt===RUIN||tt===VEIN) return false;
  }
  return true;
}
/* ============================================================ DATA: ITEMS */
const ITEMS={
  wood :{n:"wood",     d:"Limbs, drift, split plank.",stack:30},
  fiber:{n:"fiber",    d:"Reed and nettle, twisted into cord.",stack:24},
  stone:{n:"stone",    d:"Hand-sized. Sharp enough.",stack:24},
  ore  :{n:"ore",      d:"Rust-red and heavy.",stack:16},
  hide :{n:"hide",     d:"Off something that walked.",stack:16},
  cloth:{n:"cloth",    d:"Sailcloth, mostly ruined.",stack:16},
  pitch:{n:"pitch",    d:"Black and slow. Seals a seam.",stack:12},
  herb :{n:"bitterroot",d:"Chewed, it closes a wound.",stack:8,use:"heal",v:24},
  berry:{n:"berries",  d:"Sour. Better than nothing.",stack:12,use:"heal",v:9},
  bone :{n:"bone",     d:"Not always an animal's.",stack:18},
  glass:{n:"mistglass",d:"Grey, slow, leaning toward whatever you think of.",stack:10,use:"mist",v:10},
  bandage:{n:"bandage",d:"Cloth, cord, bitterroot.",stack:6,use:"heal",v:42,revive:true},
  tonic:{n:"tonic",    d:"Bitterroot steeped in mistglass water.",stack:4,use:"tonic",v:26},
  meat :{n:"raw meat",  d:"Off something that was only an animal.",stack:12,use:"heal",v:18},
  roast:{n:"roast",     d:"Cooked over a furnace. Worth the wait.",stack:10,use:"tonic",v:46},
  ingot:{n:"iron",      d:"Ore cooked down to something that will hold an edge.",stack:14},
  frag :{n:"chart scrap",d:"A corner of a coastline that is not on any chart. Four of them make a course.",stack:8},
  sd_berry:{n:"berry seed",d:"Split out of the fruit.",stack:24},
  sd_herb :{n:"bitterroot seed",d:"Split off the crown.",stack:24},
  sd_fiber:{n:"nettle seed",d:"Shaken out of dry cord.",stack:24},
  sd_wood :{n:"sapling",d:"A limb that has not given up.",stack:16},
  silver:{n:"silver",   d:"Cut coin and shaved bar. What ninety-nine trades out of a hundred are settled in.",stack:400},
  sever:{n:"severance fetish",d:"Glass and bone wound with cord. It can cut a Regard out of you and set it in somebody else — for a price it names itself.",stack:1,use:"sever"},
  port  :{n:"pilot's chart",d:"Somebody's working copy of a coast, with the soundings written in a second hand. Read it and you will know where the place is.",stack:4,use:"port"},
  vial  :{n:"mist vial",d:"A finger of grey that leans toward whatever you are thinking of. Nobody prices these out loud.",stack:12}
};
/* one thing pulled apart into the makings of more of it */
const SPLITS=[
  {id:"sp_berry",n:"Split berries for seed",c:{berry:1},give:{sd_berry:2},d:"Two seeds out of one handful."},
  {id:"sp_herb", n:"Split bitterroot for seed",c:{herb:1},give:{sd_herb:2},d:"The crown goes back in the ground."},
  {id:"sp_fiber",n:"Shake nettle for seed",c:{fiber:2},give:{sd_fiber:2},d:"Dry cord is mostly seed."},
  {id:"sp_wood", n:"Cut a sapling",c:{wood:3},give:{sd_wood:1},d:"One limb that will grow if planted."}
];
const CROPS={
  sd_berry:{n:"berry bush",give:"berry",amt:3,time:110,c:"#7b3c48"},
  sd_herb :{n:"bitterroot",give:"herb", amt:2,time:130,c:"#93a06a"},
  sd_fiber:{n:"nettle",    give:"fiber",amt:4,time:100,c:"#5d7a4d"},
  sd_wood :{n:"sapling",   give:"wood", amt:5,time:190,c:"#5b4632"}
};
const RECIPES=[
  {id:"bandage",n:"Bandages",give:{bandage:2},c:{fiber:2,herb:1},d:"Closes a wound. Gets somebody off the ground."},
  {id:"tonic",  n:"Tonic",   give:{tonic:1},  c:{herb:1,glass:1},d:"Body and vigor both, all at once."},
  {id:"hatchet",n:"Hand hatchet",tool:"hatchet",c:{wood:1,stone:2},d:"Wood comes twice as fast."},
  {id:"pick",   n:"Rock pick",tool:"pick",c:{wood:1,stone:3},d:"Opens ore veins."},
  {id:"vest",   n:"Hide vest",tool:"vest",c:{hide:3,fiber:2},d:"Twenty-five more body."},
  {id:"charm",  n:"Bone charm",tool:"charm",c:{bone:3,glass:1},d:"Mist comes to you thicker."},
  {id:"hidecap",n:"Hide cap",tool:"hidecap",c:{hide:2,fiber:1},d:"For the head. Fourteen more body."},
  {id:"ironhelm",n:"Iron helm",tool:"ironhelm",c:{ingot:2,hide:1},d:"For the head. Body and real armour."},
  {id:"sever", n:"Severance fetish",give:{sever:1},c:{glass:5,bone:6,cloth:2},d:"Cuts a Regard out of you and sets it in a follower. The price is a life, and not yours."},
  {id:"raft",   n:"Raft",craft:"raft",c:{wood:8,fiber:5,cloth:2,pitch:1},d:"The current picks the island. Not you."},
  {id:"boat",   n:"Small boat",craft:"boat",c:{wood:16,fiber:8,cloth:5,pitch:4,ore:3},d:"You pick the island. Build it beside the hold and it will draw from there."}
];

/* ============================================================ DATA: NODES */
/* what you can put up, and what it costs. All of it wants a permanent island
   under it except the hearth, which is what makes an island permanent. */
const BUILDS={
  hearth :{n:"Hearth",c:{wood:10,stone:8},anywhere:true,r:16,
           d:"Lay a fire that does not go out. Makes this island yours — nothing will grow back on it that you have already killed."},
  plot   :{n:"Garden bed",c:{wood:3,fiber:2},r:13,
           d:"Turned ground. Put seed in it and something comes up."},
  furnace:{n:"Furnace",c:{stone:12,ore:2},r:15,
           d:"Cooks ore down to iron, and meat into something worth eating."},
  house  :{n:"House",c:{wood:12,stone:6,cloth:3},r:19,
           d:"Somewhere for somebody to stay. One more person can be left here for each one."},
  pen    :{n:"Pen",c:{wood:8,fiber:4},r:17,
           d:"Fenced ground. Animals kept in it will breed if they are fed."}
};
const NODES={
  log   :{n:"fallen limb", give:"wood", amt:2,c:"#5b4632"},
  drift :{n:"driftwood",   give:"wood", amt:1,c:"#7d6b52"},
  reeds :{n:"reeds",       give:"fiber",amt:2,c:"#8a8f5c"},
  nettle:{n:"nettle",      give:"fiber",amt:1,c:"#5d7a4d"},
  stones:{n:"loose stone", give:"stone",amt:2,c:"#7a8084"},
  chips :{n:"ore chips",   give:"ore",  amt:1,c:"#8a5b3a"},
  vein  :{n:"ore vein",    give:"ore",  amt:3,c:"#a5643c",needs:"pick"},
  root  :{n:"bitterroot",  give:"herb", amt:1,c:"#93a06a"},
  bush  :{n:"berry bush",  give:"berry",amt:2,c:"#7b3c48"},
  seep  :{n:"pitch seep",  give:"pitch",amt:1,c:"#241d19"},
  wreck :{n:"wreck debris",give:"cloth",amt:1,c:"#9a8f76"},
  bones :{n:"bone pile",   give:"bone", amt:1,c:"#cfc7ae"},
  glass :{n:"mistglass",   give:"glass",amt:1,c:"#b2aec6"}
};
// which nodes each theme scatters, and roughly how many
const SCATTER={
  forest :{log:22,nettle:16,reeds:9,bush:12,root:10,stones:7,seep:3,bones:3,glass:1},
  outcrop:{drift:16,stones:20,chips:8,vein:3,reeds:8,root:4,bones:6,seep:4,glass:2},
  mountain:{stones:18,chips:12,vein:8,log:8,nettle:7,root:5,bones:5,seep:2,glass:2},
  snow   :{log:14,drift:10,stones:11,nettle:6,root:6,bones:7,chips:4,seep:2,glass:2},
  town   :{log:10,nettle:10,stones:9,drift:8,wreck:6,root:6,bush:6,seep:3,glass:1}
};

/* ============================================================ DATA: FOES */
const FOES={
  hollow :{n:"hollow",      hp:36,sp:46,r:11,dmg:9, c:"#40525a",mist:6, drop:{bone:.30},
           rng:{d:320,dmg:6, sp:200,cd:3.4,hold:150,c:"#8b9793",s:"stone"}},
  wretch :{n:"wretch",      hp:22,sp:82,r:9, dmg:7, c:"#7b6a55",mist:5, drop:{hide:.25}},
  husk   :{n:"husk",        hp:80,sp:48,r:14,dmg:16,c:"#6b4a3a",mist:12,drop:{hide:.55,bone:.30},
           rng:{d:340,dmg:12,sp:230,cd:4.2,hold:0,  c:"#8a7a63",s:"stone"}},
  drifter:{n:"mist-drifter",hp:26,sp:68,r:10,dmg:6, c:"#9aa7ad",mist:9, drain:true,drop:{glass:.35},
           rng:{d:330,dmg:6, sp:260,cd:2.3,hold:210,c:"#c8d2d6",s:"mist",drain:true}},
  floe   :{n:"floe-thing",  hp:58,sp:44,r:13,dmg:14,c:"#8fb0be",mist:11,chill:true,drop:{glass:.20,bone:.30},
           rng:{d:360,dmg:10,sp:300,cd:3.1,hold:190,c:"#cfe6ee",s:"shard",chill:true}},
  crag   :{n:"crag-walker", hp:98,sp:36,r:15,dmg:19,c:"#6d7377",mist:15,armor:3,drop:{stone:.70,ore:.35},
           rng:{d:380,dmg:17,sp:170,cd:5.2,hold:0,  c:"#7a8084",s:"boulder"}},
  kelp   :{n:"kelp-hound",  hp:42,sp:76,r:11,dmg:11,c:"#3f5b4a",mist:8, drop:{fiber:.50}},
  /* these do not turn up until you have been at this a while */
  gaunt  :{n:"gaunt",       hp:38,sp:96,r:10,dmg:10,c:"#5f5147",mist:12,min:2,drop:{hide:.4,bone:.3},
           rng:{d:300,dmg:9, sp:330,cd:2.0,hold:170,c:"#d6cbb4",s:"shard"}},
  brine  :{n:"brine-thing", hp:150,sp:40,r:17,dmg:22,c:"#2f4a4c",mist:22,min:3,armor:5,drop:{hide:.8,glass:.3,pitch:.3},
           rng:{d:300,dmg:14,sp:190,cd:4.0,hold:0,  c:"#7fa8a2",s:"boulder"}},
  counter:{n:"the thing that counts",hp:60,sp:58,r:12,dmg:8,c:"#8e8471",mist:26,min:4,drain:true,
           drop:{glass:.6,bone:.5},
           rng:{d:360,dmg:10,sp:280,cd:2.6,hold:240,c:"#e0d8bd",s:"mist",drain:true}},
  lampless:{n:"the lampless",hp:190,sp:66,r:16,dmg:26,c:"#3a2f3c",mist:34,min:6,armor:4,blink:true,
           drop:{glass:.7,hide:.7,ore:.4}},
  rimeling:{n:"rimeling",   hp:74,sp:88,r:11,dmg:15,c:"#a9c6d2",mist:18,min:5,chill:true,drop:{glass:.4,bone:.4},
           rng:{d:340,dmg:12,sp:340,cd:2.4,hold:150,c:"#e2f1f6",s:"shard",chill:true}}
};
/* animals: the only things out here that are not wrong */
const ANIMALS={
  squirrel:{n:"squirrel",hp:12, sp:138,r:7, dmg:0, c:"#7a6552",flee:true, meat:1,mist:1},
  deer    :{n:"deer",    hp:46, sp:118,r:12,dmg:0, c:"#8a6f4e",flee:true, meat:2,mist:3,
            feed:"berry",breeds:true},
  wolf    :{n:"wolf",    hp:78, sp:106,r:11,dmg:15,c:"#6b6f74",pack:true, meat:2,mist:6,
            feed:"meat",breeds:true,tame:3},
  bear    :{n:"bear",    hp:240,sp:74, r:18,dmg:32,c:"#4a3a2c",territory:230,meat:5,mist:14}
};
const ANIMAL_POOL={
  forest  :["deer","deer","squirrel","wolf","bear"],
  outcrop :["squirrel","deer","wolf"],
  mountain:["bear","wolf","deer","squirrel"],
  snow    :["wolf","wolf","deer","bear"],
  town    :["squirrel","deer","wolf"]
};
const POOL_ALL={
  forest :["hollow","wretch","husk","kelp","gaunt","counter","lampless","brine"],
  outcrop:["wretch","kelp","hollow","crag","brine","gaunt","counter","lampless"],
  mountain:["crag","husk","hollow","drifter","gaunt","brine","lampless","counter"],
  snow   :["floe","hollow","drifter","husk","rimeling","gaunt","lampless","counter"],
  town   :["hollow","wretch","drifter","husk","gaunt","counter","rimeling","lampless"]
};
/* only what has had time to find you */
function pool(theme){
  const all=POOL_ALL[theme||ISL.theme];
  const ok=all.filter(k=>!FOES[k].min||G.isl>=FOES[k].min);
  return ok.length?ok:["hollow","wretch"];
}
function newArrivals(theme){
  return POOL_ALL[theme].filter(k=>FOES[k].min===G.isl);
}
const MINI={
  forest :{n:"the Bark-Gnawer",c:"#4c3b28"},
  outcrop:{n:"the Tidepool Warden",c:"#37564f"},
  mountain:{n:"the Ore-Eater",c:"#5d5a52"},
  snow   :{n:"the Rimewalker",c:"#7fa2b4"},
  town   :{n:"the Hollow Constable",c:"#54473a"}
};
const BOSS={
  forest :{n:"the Antlered Quiet",c:"#2f3a26"},
  outcrop:{n:"the Thing That Wears the Shoal",c:"#22403f"},
  mountain:{n:"the Scree Mother",c:"#4a4741"},
  snow   :{n:"the White Wail",c:"#6d94ab"},
  town   :{n:"the Drum in the Hall",c:"#5c2b1e"}
};

/* ============================================================ DATA: REGARDS */
/* only at a furnace on your own island. Iron is the gate. */
const RECIPES_HOME=[
  {id:"plate",  n:"Forged plate",tool:"plate",c:{ingot:6,hide:4},forge:true,
   d:"+70 body and +10 armour, and none of the ballast plate's weight."},
  {id:"longiron",n:"Long iron",tool:"longiron",c:{ingot:5,wood:3},forge:true,
   d:"+34 damage and a good deal more reach. Two hands, all day."},
  {id:"crucible",n:"Mist crucible",tool:"crucible",c:{ingot:4,glass:6},forge:true,
   d:"Mist comes 40% thicker and you can hold 120 more of it. Nothing given up for it."},
  {id:"shoes",  n:"Iron-shod boots",tool:"shoes",c:{ingot:3,hide:3,cloth:2},forge:true,
   d:"A tenth faster and +3 armour. Iron on the sole, not on you."}
];
const ELITE={
  forest :"the Root That Remembers",
  outcrop:"the Whole Shoal Standing Up",
  mountain:"what the Mountain Was Built Over",
  snow   :"the Winter That Was Sent For",
  town   :"the Congregation"
};
const REGARDS=[
  {id:"laurentide",n:"Laurentide",tier:"Spirit",of:"a queen who froze her own harbour shut",
   side:"Your hands never warm. Standing water goes still when you look at it.",
   pas:{n:"Cold Standing",d:"Anything that touches you goes stiff for a moment afterwards, and a stiff thing hits you softer."},
   act:{n:"Rime",cost:22,r:140,d:"A wedge of ground in front of you freezes. What is standing in it locks up where it stands."},
   ups:{2:"The lock holds longer and the wedge is wider.",
        3:"Frozen things take half again from everything.",
        4:"The cold comes off you as well as out of you — what breaks the lock is slowed.",
        5:"Rime takes hold on the first thing it touches and spreads to the next."}},

  {id:"trident",n:"Wreathed Trident",tier:"Spirit",of:"a church that took feeling out of people for their own good",
   side:"Tempered. Low. You keep wanting to hand pieces of this to other people.",
   pas:{n:"The Levelling",d:"What you have slowed is somewhere you can speak from. Use any ability and it sounds again out of every slowed thing."},
   act:{n:"The Emptying",cost:20,r:120,d:"Hurts a little and slows for a long while. What it touches is emptied, and stays emptied for as long as it is slow."},
   ups:{2:"Reaches further and slows longer.",
        3:"An emptied thing takes your strikes as though it were not there at all.",
        4:"The echo out of a slowed thing comes at nearly full strength.",
        5:"Some of what you empty does not come back. Be careful who is near you."}},

  {id:"togo",n:"Togo",tier:"Spirit",of:"the winter merchants, who crossed every white mile there is and never once lost a dog",
   side:"Things with four feet arrange themselves around you without being asked.",
   pas:{n:"The Team",d:"Four wolves will walk with you instead of two."},
   act:{n:"Lead Dog's Lantern",cost:18,r:130,d:"A flash of white light out of you — and out of every wolf you keep — that stops whatever it lands on."},
   ups:{2:"The light carries further and holds longer.",
        3:"When you use your other Regard, every wolf of yours uses it with you.",
        4:"The Lantern costs less to raise.",
        5:"You and the team are one animal: their bites carry your weapon, and every wolf makes your own arm a quarter stronger."}},

  {id:"mishaabooz",n:"Mishaabooz",tier:"Spirit",of:"a hare that was drawn on a map before anybody drew a coast",
   side:"You keep finding you have already gone where you meant to go.",
   pas:{n:"Already Gone",d:"You move quicker than you used to, and a dash costs you almost nothing."},
   act:{n:"Redraw",cost:16,r:150,d:"You are elsewhere, a short way off, and the ground between does not get a say. What you leave behind takes the noise."},
   ups:{2:"Further, and you are hard to hit on arrival.",
        3:"The place you leave comes apart harder.",
        4:"Arriving cuts as well — briefly, close in.",
        5:"Two Redraws before the first has finished being one."}},

  {id:"drum",n:"Wiyaka's Drum",tier:"Spirit",of:"a protector spirit that only ever said go back",
   side:"You hear a beat under things. It is usually a warning and you are usually late.",
   pas:{n:"The Long Beat",d:"You and whoever walks with you mend slowly, all the time, as long as you keep walking."},
   act:{n:"Thump",cost:18,r:110,d:"One beat out from your feet. Things go backwards, and what goes backwards stays slow."},
   ups:{2:"Wider, and it hits harder.",
        3:"What it knocks back cannot throw anything for a moment.",
        4:"The beat mends you a little as it goes out.",
        5:"Two beats. The second lands on whatever the first put on the ground."}},

  {id:"kettle",n:"Ember of the Kettle",tier:"Human",of:"a stove that stayed lit through a winter nobody else survived",
   side:"You are always slightly too warm, and you cannot stand a cold room.",
   pas:{n:"Never Out",d:"What you strike catches. Not much, and not for long, but it catches."},
   act:{n:"Banked Coal",cost:18,r:90,d:"You carry a heat for a while. What comes near you burns for as long as it stays near you."},
   ups:{2:"Hotter, and it reaches a little further.",
        3:"Burning things come apart faster under a strike.",
        4:"When something burning dies it goes off, and what is beside it catches.",
        5:"The Ember does not need fuel any more, and neither, apparently, do you."}},

  {id:"lamp",n:"The Fris Lamp",tier:"Human",of:"a light kept burning over a harbour that had already drowned",
   side:"You see a little too well in the dark and you cannot look away from things.",
   pas:{n:"Kept Burning",d:"Mist comes off the dead thicker for you, and the chart fills itself in as you walk."},
   act:{n:"Cast the Lamp",cost:20,r:120,d:"Throw the light a short way. What stands in it is seen, and being seen is a wound of its own."},
   ups:{2:"The light lasts longer and covers more ground.",
        3:"Anything in the light takes half again from everything.",
        4:"The light holds them — they will not leave it while it burns.",
        5:"What dies in the light gives up twice its mist."}},

  {id:"yurei",n:"Yūrei",tier:"Spirit",of:"somebody who drowned in sight of shore and did not accept it",
   side:"Your shadow is a beat late, and sometimes it is not doing what you are doing.",
   pas:{n:"Second Silhouette",d:"When something strikes you, your shadow stands up into the shape of it and takes part of the blow — the bigger the thing, the more the shadow can hold. What it holds, it keeps."},
   act:{n:"Send the Shadow",cost:18,r:170,d:"The shadow goes out ahead of you and hits like everything it has been holding. If it kills what it touches, some of that comes back to you as blood."},
   ups:{2:"The shadow reaches further and hits harder.",
        3:"After being sent, the shadow keeps half of what it was holding.",
        4:"The shadow can hold more, and it takes a bigger share of each blow.",
        5:"The shadow strikes twice before coming home."}},

  {id:"nanook",n:"Nanook",tier:"Spirit",of:"the bear that decides who deserves to hunt at all",
   side:"Your arms ache at the shoulder, the way arms ache when they have been longer than they are.",
   pas:{n:"The Long Arm",d:"Your weapon reaches further and lands heavier than it has any right to. When you are near the end of your blood, it lands heavier still."},
   act:{n:"White Hunger",cost:20,r:0,d:"Drinks nearly all of your vigor at once. For a while your strikes come faster, reach further and hit half again as hard — but the hunger will not let you dash while it holds you."},
   ups:{2:"The arm is longer and the weight of it greater.",
        3:"The desperate strength wakes earlier — well before you are at the end.",
        4:"The Hunger leaves a little more vigor in you than it used to.",
        5:"While the Hunger holds you, you move faster instead of not at all."}},

  {id:"sawbones",n:"Sawbones",tier:"Human",of:"a ship's surgeon who kept sawing long after the ship had no crew",
   side:"Everything around you moves like it is underwater. You have stopped noticing. They have not.",
   pas:{n:"Bedside Manner",d:"Everything near you — friend and foe alike — moves slower, as though the air around you had thickened into ether."},
   act:{n:"Rough Mending",cost:20,r:95,d:"Stitches you and whoever stands close back together, fast and badly. Then the same hands turn outward, and what is standing too near takes a wound the size of the mending."},
   ups:{2:"The ether thickens and spreads wider.",
        3:"What moves slowly around you takes a third again from your strikes.",
        4:"The mending mends more.",
        5:"The turning of the hands staggers what it wounds."}},

  {id:"toltec",n:"Toltec",tier:"Spirit",of:"an altar that understood exactly what everything costs",
   side:"You keep an account in your head now, in blood, and it is always roughly balanced.",
   pas:{n:"Blood Debt",d:"Being wounded fills your lungs — damage taken comes back as vigor, and puts speed in your legs for a moment. And vigor spent trickles back to you as blood, a little, never enough to be free."},
   act:{n:"The Offering",cost:10,r:0,d:"Opens your own vein for it. A bolt goes out the way you are facing and hits with everything the blood was worth, passing through what it does not kill."},
   ups:{2:"The bolt is worth more than the blood put in.",
        3:"The wound-speed lasts longer and carries you faster.",
        4:"The altar asks for less blood each time.",
        5:"If the Offering kills, the blood comes back."}},

  {id:"ravenmocker",n:"Raven Mocker",tier:"Human",of:"the thing that eats the years of the dying to add to its own",
   side:"After a kill you feel briefly heavier, in a way you have decided not to think about.",
   pas:{n:"Eaten Years",d:"Now and then, what you put down does not entirely go to waste — a small chance that a kill settles into you as one more point of body, for good."},
   act:{n:"String the Puppet",cost:20,r:180,d:"Everything ahead of you goes puppetlike — strung up, dragging against wires for three long seconds. Then the strings pull tight: held fast and wounded, for one second more."},
   ups:{2:"The years come easier — the chance is better.",
        3:"The strings reach further and drag harder.",
        4:"The final pull holds them longer.",
        5:"A puppet that dies on its strings gives up twice the mist."}},

  {id:"missionary",n:"Missionary",tier:"Human",of:"a man who walked into spears certain that nothing thrown could touch him",
   side:"You have stopped flinching at things in the air. You have started flinching at hands.",
   pas:{n:"Article of Faith",d:"Nothing thrown, spat or shot can wound you — it comes apart on the air in front of you. But a blow from a hand or a body lands half again as hard, because faith like this was never about hands."},
   act:{n:"The Sermon",cost:16,r:110,d:"Everything near you is pushed back, unharmed. And your next blow — a strike of your own or another Regard's working — lands twice."},
   ups:{2:"The Sermon carries further and pushes harder.",
        3:"The doubled blow is not softened by armour or hide.",
        4:"The push itself staggers what it moves.",
        5:"The Sermon doubles your next two blows, not one."}}
];
/* ranks are expensive: a regard is a career, not a purchase.
   even carrying one alone and feeding it everything, rank 5 should not
   arrive before the tenth coast, and more likely the fifteenth */
const RANKCOST=[0,0,1000,2600,5000,8400];
const MAXREGARDS=2;
/* mist you are carrying and have nothing to spend on */
function mistCap(){ return 400+(G.tools.charm?120:0)+(G.tools.mistblade?80:0)+(G.tools.crucible?120:0); }
function mistMul(){
  return (G.tools.charm?1.25:1)*(G.tools.mistblade?1.15:1)*(G.tools.cord?1.6:1)*
         (G.tools.crucible?1.4:1)*(hasPas("lamp")?1.25:1);
}
function hasPas(id){ for(const g of G.regards) if(g.R.id===id) return g; return null; }
function rankOfPas(id){ const g=hasPas(id); return g?g.rank:0; }

/* recipes you cannot know until something big has died in front of you */
const PLANS={
  ironspear:{n:"Hollow-iron spear",tier:2,d:"Ore drawn out over a fire that isn't quite a fire."},
  harness  :{n:"Bone-plate harness",tier:2,d:"Their ribs laid over yours. It works better than it reads."},
  mistblade:{n:"Mist-tempered blade",tier:3,d:"Quenched in mistglass water. It keeps taking after it stops cutting."},
  coat     :{n:"Warden's coat",tier:3,d:"Cut off what the thing was wearing when it stopped."},
  ironhull :{n:"Ironbound hull",tier:3,d:"Plate the keel. The sea gets less of a say."},
  ballast  :{n:"Ballast plate",tier:4,d:"Scrap iron laid over you like a deck. It works. It also weighs what it weighs."},
  gutter   :{n:"Gutting knife",tier:4,d:"All edge and no guard. Whoever made it had stopped intending to survive."},
  cord     :{n:"Mist-drinker's cord",tier:4,d:"Glass strung on sinew. It pulls the mist in and leaves you thin."},
  fogfoot  :{n:"Fogfoot wraps",tier:4,d:"Sailcloth and hide, bound so light you barely touch the ground — or anything else."},
  lantern  :{n:"Hollow lantern",tier:4,d:"It shows you the whole island. The island gets the same courtesy."}
};
const RECIPES2=[
  {id:"ironspear",n:"Hollow-iron spear",plan:"ironspear",tool:"ironspear",c:{ore:4,wood:3,hide:2},
   d:"Longer than the fishing spear and considerably less polite."},
  {id:"harness", n:"Bone-plate harness",plan:"harness",tool:"harness",c:{hide:5,bone:6,fiber:3},
   d:"Forty more body, and it turns part of every hit."},
  {id:"mistblade",n:"Mist-tempered blade",plan:"mistblade",tool:"mistblade",c:{ore:6,glass:3,hide:2},
   d:"Heavy damage, and mist comes off the dead thicker."},
  {id:"coat",    n:"Warden's coat",plan:"coat",tool:"coat",c:{hide:6,cloth:4,glass:2},
   d:"Fifty-five more body and real armour."},
  {id:"ironhull",n:"Ironbound hull",plan:"ironhull",tool:"ironhull",c:{ore:8,pitch:4,wood:6},
   d:"Four coasts on the chart instead of three, and a wreck takes less off you. Wants a boat under it."},
  /* --- tier four: every one of these costs you something --- */
  {id:"ballast", n:"Ballast plate",plan:"ballast",tool:"ballast",c:{ore:10,stone:12,hide:4},
   d:"+90 body and +8 armour. You move a quarter slower and hold 15 less vigor."},
  {id:"gutter",  n:"Gutting knife",plan:"gutter",tool:"gutter",c:{ore:5,bone:8,fiber:4},
   d:"+28 damage. −35 body. It is not a defensive object."},
  {id:"cord",    n:"Mist-drinker's cord",plan:"cord",tool:"cord",c:{glass:6,fiber:6,bone:5},
   d:"Mist comes 60% thicker. Everything that lands on you lands 18% harder."},
  {id:"fogfoot", n:"Fogfoot wraps",plan:"fogfoot",tool:"fogfoot",c:{cloth:6,hide:4,fiber:8},
   d:"A quarter faster and dashes cost half. Armour counts for nothing while they are on."},
  {id:"lantern", n:"Hollow lantern",plan:"lantern",tool:"lantern",c:{ore:4,glass:5,pitch:3},
   d:"The chart shows the whole island. The island sees you just as clearly — things come for you twice as often."}
];

/* ============================================================ DATA: PEOPLE */
const FIRST=["Alder","Coyle","Marek","Sable","Wren","Tobin","Ivo","Halden","Nessa","Orin","Pell",
             "Quill","Rook","Sena","Torv","Bram","Cass","Dela","Ester","Fenn","Gale","Hesk","Juno",
             "Lark","Mabry","Nye","Ondra","Perrin","Rilla","Selm","Tass","Wyn"];
const LAST =["Vosbury","Kell","Thorpe","Bindon","Hollis","Farsue","Cundy","Marrow","Skeen","Aubry",
             "Tolus","Nettle","Grange","Pike","Ord","Merrow","Quist","Ashken","Brill","Ganno"];
const TRADES=[
  {n:"navigator",b:"chart",d:"You read a coast the way other people read a face."},
  {n:"cooper",b:"wood",d:"Wood goes further in your hands than it has any right to."},
  {n:"deckhand",b:"body",d:"Thicker than you look, and used to being hit by weather."},
  {n:"sailmaker",b:"cloth",d:"You can make cloth out of almost nothing."},
  {n:"surgeon's mate",b:"medic",d:"You are the reason people get up off the ground."},
  {n:"powder monkey",b:"arm",d:"Small, fast, and you swing like you mean it."},
  {n:"cook's mate",b:"food",d:"You know what is food and what only looks like it."},
  {n:"chaplain",b:"faith",d:"People talk themselves into following you and blame it on God."},
  {n:"fisher",b:"shoal",d:"The shallows have never been in your way."},
  {n:"cartographer's boy",b:"chart",d:"You draw what you see whether you meant to or not."}
];
const CAST_ROLE=["deckhand","cook","cooper's girl","lamp keeper","surveyor","net mender","carpenter",
                 "purser","boy from the galley","winter merchant","gunner's mate","preacher's widow"];
const CAST_LINE=[
  "I have been eating limpets for nine days and talking to a rock.",
  "I heard the ship go. I did not see it and I would rather not have.",
  "You are the first thing on this island that walks the right way round.",
  "Don't touch me. Sorry. Don't touch me yet.",
  "I have a knife and no plan. You look like a plan.",
  "There's something up in the trees that counts. Out loud. In order."
];

/* ============================================================ STATE */
const G={
  mode:"title", isl:0, wrecks:0, kills:0, mist:0, spent:0,
  hp:100,mhp:100, vg:60,mvg:60,
  regards:[], wp:null, coal:0, lampT:0, shadow:0, nanookT:0, zeal:0,
  pack:[], cache:[], home:[], tools:{}, plans:{}, craft:null, rescued:0, bossKills:0, spiritsMet:0,
  mistTotal:0, halt:false, spill:0, broken:0, elites:0, tamed:0, bred:0,
  clock:0, cityState:{}, citiesSeen:{}, ports:{},
  shake:0,hurtT:0,hitstop:0, tint:"rgba(0,0,0,0)",
  log:[], name:"", trade:null, deaths:[],
  bp:{}, wpn:null, armor:{head:null,chest:null,legs:null}
};
const player={x:0,y:0,r:11,aim:0,cd:0,swing:0,roll:0,inv:0,rollDir:0,dmg:15,reach:46,sp:142,
               armor:0,dash:0,dashcd:0,spdMul:1,takeMul:1,dashCost:1,armorOff:0,tolT:0};
let ISL={theme:"forest",name:"—",threat:0,buildings:[],spirit:null,story:null};
let nodes=[],foes=[],party=[],neutrals=[],townies=[],parts=[],zones=[],shots=[],corpses=[],props=[];
let animals=[],builds=[],gates=[];
let HOME=null;                 // the permanent island, kept whole between crossings
let nearest=null, cam={x:0,y:0}, prompt="", spawnT=6, dawnPulse=0, hudT=0;

const THREATN=["Mortal","Haunted","Forsaken","Broken"];
const FRAGS_FOR_ELITE=4;
function logit(h,t){ G.log.unshift({h,t}); if(G.log.length>70) G.log.pop(); }

/* ---- pack helpers ---- */
const PACK_SLOTS=8;
function packSlots(){ return PACK_SLOTS; }
function packInit(){ G.pack=new Array(PACK_SLOTS).fill(null); }
/* the pack is a fixed-length array with holes in it. Anything that trims it —
   a drop from the last slot, a wreck — used to leave it short, and the next
   give() then called packInit() and threw away everything you were carrying.
   Nothing trims it any more, and this repairs it without losing a stack. */
function packNorm(){
  if(!Array.isArray(G.pack)) G.pack=[];
  while(G.pack.length<PACK_SLOTS) G.pack.push(null);
  if(G.pack.length>PACK_SLOTS){
    const spill=G.pack.slice(PACK_SLOTS).filter(s=>s);
    G.pack.length=PACK_SLOTS;
    for(const s of spill){
      const i=G.pack.indexOf(null);
      if(i<0) break;
      G.pack[i]=s;
    }
  }
  return G.pack;
}
function packCount(){ let n=0; for(const s of G.pack) if(s) n++; return n; }
function packSwap(a,b){
  if(a===b||a<0||b<0||a>=PACK_SLOTS||b>=PACK_SLOTS) return;
  const t=G.pack[a]; G.pack[a]=G.pack[b]; G.pack[b]=t;
}
function have(id){ let n=0; for(const s of G.pack) if(s&&s.id===id) n+=s.n; return n; }
function give(id,n){
  packNorm();
  const st=(ITEMS[id]&&ITEMS[id].stack)||10;
  let left=n;
  for(const s of G.pack){ if(s&&s.id===id&&s.n<st){ const add=Math.min(st-s.n,left); s.n+=add; left-=add; if(!left) break; } }
  while(left>0){
    let i=-1;
    for(let k=0;k<PACK_SLOTS;k++) if(!G.pack[k]){ i=k; break; }
    if(i<0) return n-left;                       // pack full; the rest stays on the ground
    const add=Math.min(st,left); G.pack[i]={id,n:add}; left-=add;
  }
  return n;
}
function giveTell(id,n){
  const got=give(id,n);
  if(got<n) toast("pack is full",`${n-got} ${ITEMS[id].n} left on the ground.`);
  return got;
}
/* some things are not loot — a chart scrap is a course, and a course you have
   earned should never fall out of a full pack. Pack first, then whatever store
   is within reach, then the hold regardless, and only then owed. */
function giveKeep(id,n){
  let left=n-give(id,n);
  if(left>0){
    const ns=nearStores();
    if(ns.home) left-=cacheGive(id,left,"home");
  }
  if(left>0) left-=cacheGive(id,left,"hold");
  if(left>0){ G.owed=G.owed||{}; G.owed[id]=(G.owed[id]||0)+left; }
  return n-left;
}
/* anything owed comes to you as soon as there is room for it */
function payOwed(){
  if(!G.owed) return;
  for(const id in G.owed){
    if(G.owed[id]<=0){ delete G.owed[id]; continue; }
    const got=give(id,G.owed[id]);
    G.owed[id]-=got;
    if(G.owed[id]<=0) delete G.owed[id];
  }
}
function take(id,n){
  let left=n;
  for(let i=G.pack.length-1;i>=0&&left>0;i--){
    const s=G.pack[i]; if(!s||s.id!==id) continue;
    const t=Math.min(s.n,left); s.n-=t; left-=t;
    if(s.n<=0) G.pack[i]=null;
  }
  return left===0;
}
/* ---- the hold: a bigger store that lives in whatever you beached ---- */
function houseCount(){
  const arr=(ISL&&ISL.home)?builds:((HOME&&HOME.builds)||[]);
  let n=0; for(const b of arr) if(b.k==="house") n++;
  return n;
}
function cacheSlots(){ return 14; }
/* the store at home is its own thing, and a big one — walls do what a hull cannot */
function homeSlots(){ return 40+6*houseCount(); }
const cacheStack=id=>((ITEMS[id]&&ITEMS[id].stack)||10)*3;
/* which store are you standing at? "hold" is the craft, "home" is the hearth and houses */
function storeFor(camp){
  const c=camp===undefined?nearCamp():camp;
  if(c&&(c.k==="hearth"||c.k==="house")) return "home";
  return "hold";
}
function storeArr(w){ return w==="home"?G.home:G.cache; }
function storeCap(w){ return w==="home"?homeSlots():cacheSlots(); }
function cacheHave(id,w){
  const arr=storeArr(w||storeFor());
  let n=0; for(const s of arr) if(s&&s.id===id) n+=s.n; return n;
}
function cacheGive(id,n,w){
  const which=w||storeFor(), arr=storeArr(which);
  const st=cacheStack(id); let left=n;
  for(const s of arr){ if(s&&s.id===id&&s.n<st){ const a=Math.min(st-s.n,left); s.n+=a; left-=a; if(!left) break; } }
  while(left>0){
    let i=arr.findIndex(s=>!s);
    if(i<0){ if(arr.length>=storeCap(which)) return n-left; i=arr.length; }
    const a=Math.min(st,left); arr[i]={id,n:a}; left-=a;
  }
  return n;
}
function cacheTake(id,n,w){
  const arr=storeArr(w||storeFor());
  let left=n;
  for(let i=arr.length-1;i>=0&&left>0;i--){
    const s=arr[i]; if(!s||s.id!==id) continue;
    const t=Math.min(s.n,left); s.n-=t; left-=t; if(s.n<=0) arr[i]=null;
  }
  while(arr.length&&!arr[arr.length-1]) arr.pop();
  return left===0;
}
function nearCamp(){
  for(const p of props) if(p.k==="hull"||p.k==="raft"||p.k==="boat")
    if(dist(p.x,p.y,player.x,player.y)<130) return p;
  if(ISL&&ISL.home) for(const b of builds)
    if((b.k==="hearth"||b.k==="house")&&dist(b.x,b.y,player.x,player.y)<220) return b;
  return null;
}
/* which stores can you reach from where you stand? both is a real answer:
   a boat beached beside the hearth should not hide the hearth's store */
function nearStores(){
  const out={hold:false,home:false};
  for(const p of props) if(p.k==="hull"||p.k==="raft"||p.k==="boat")
    if(dist(p.x,p.y,player.x,player.y)<130){ out.hold=true; break; }
  /* a city rebuilds its props from the district, so the craft you arrived in
     is not on the ground anywhere. It is still moored, and you can still
     reach what is in it. */
  if(!out.hold&&ISL&&ISL.city&&G.craft) out.hold=true;
  if(ISL&&ISL.home) for(const b of builds)
    if((b.k==="hearth"||b.k==="house")&&dist(b.x,b.y,player.x,player.y)<220){ out.home=true; break; }
  return out;
}
const avail=id=>{
  const ns=nearStores();
  return have(id)+(ns.hold?cacheHave(id,"hold"):0)+(ns.home?cacheHave(id,"home"):0);
};
function canPay(c){ for(const k in c) if(avail(k)<discount(k,c[k])) return false; return true; }
function discount(k,v){
  if(k==="cloth"&&G.trade&&G.trade.b==="cloth") return Math.max(1,Math.ceil(v*.5));
  if(k==="wood"&&G.trade&&G.trade.b==="wood") return Math.max(1,Math.ceil(v*.75));
  return v;
}
function pay(c){
  const ns=nearStores();
  for(const k in c){
    let need=discount(k,c[k]);
    const fromPack=Math.min(need,have(k));
    if(fromPack>0){ take(k,fromPack); need-=fromPack; }
    if(need>0&&ns.hold){ const t=Math.min(need,cacheHave(k,"hold")); if(t>0){ cacheTake(k,t,"hold"); need-=t; } }
    if(need>0&&ns.home){ const t=Math.min(need,cacheHave(k,"home")); if(t>0){ cacheTake(k,t,"home"); need-=t; } }
    /* nothing comes out of a store you are not standing beside */
  }
}
function allRecipes(){
  return RECIPES.concat(RECIPES2.filter(r=>G.plans[r.plan]&&!OLD_WPN[r.id]),SPLITS,
    RECIPES_HOME.filter(r=>!OLD_WPN[r.tool]&&!OLD_WPN[r.id]));
}
function atForge(){
  if(!ISL.home) return null;
  for(const b of builds) if(b.k==="furnace"&&dist(b.x,b.y,player.x,player.y)<150) return b;
  return null;
}
function homeCount(k){ let n=0; for(const b of builds) if(b.k===k) n++; return n; }

/* ============================================================ ARTWORK
   Drop PNGs into an "art" folder next to this file using the names below and
   they replace the drawn-in-code versions. Nothing needs recompiling: reload
   the page, or press K and use "look again". A missing file just falls back.
   Optional: art/art.js may set window.ART_OVERRIDE={key:"file.png",...} to
   rename files, and window.ART_OPTS={key:{w:32,h:48,oy:4}} to resize/nudge. */
const ART_DIR="art/";
const ART_SPEC={
  /* --- ground, one tile each, square, drawn at 28x28 (56x56 also fine) --- */
  tile_water:["water.png",28,28],   tile_shoal:["shoal.png",28,28],   tile_sand:["sand.png",28,28],
  tile_grass:["grass.png",28,28],   tile_moss:["moss.png",28,28],     tile_dirt:["dirt.png",28,28],
  tile_path:["path.png",28,28],     tile_scree:["scree.png",28,28],   tile_snow:["snow.png",28,28],
  tile_ice:["ice.png",28,28],       tile_mire:["mire.png",28,28],     tile_floor:["floor.png",28,28],
  tile_plank:["plank.png",28,28],
  /* --- things that stand on a tile: draw taller than 28 if you like, they
         are anchored to the bottom of the tile --- */
  tile_tree:["tree.png",28,42],     tile_pine:["pine.png",28,42],     tile_scrub:["scrub.png",28,28],
  tile_rock:["rock.png",28,34],     tile_cliff:["cliff.png",28,38],   tile_vein:["vein.png",28,34],
  tile_wall:["wall.png",28,38],     tile_door:["door.png",28,34],     tile_ruin:["ruin.png",28,38],
  tile_grave:["grave.png",28,32],
  /* --- people: anchored feet-centre --- */
  player:["player.png",30,44], ally:["ally.png",30,44], castaway:["castaway.png",30,44],
  townie:["townie.png",30,44], down:["down.png",36,24],
  /* --- foes: anchored feet-centre --- */
  foe_hollow:["foe_hollow.png",32,48], foe_wretch:["foe_wretch.png",28,36],
  foe_husk:["foe_husk.png",40,56],     foe_drifter:["foe_drifter.png",34,40],
  foe_floe:["foe_floe.png",36,44],     foe_crag:["foe_crag.png",42,48],
  foe_kelp:["foe_kelp.png",40,32],     foe_mini:["foe_mini.png",64,80],
  foe_boss:["foe_boss.png",90,110],
  foe_gaunt:["foe_gaunt.png",30,50],   foe_brine:["foe_brine.png",44,52],
  foe_counter:["foe_counter.png",32,54],foe_lampless:["foe_lampless.png",42,58],
  foe_rimeling:["foe_rimeling.png",32,42],
  /* --- spirit, props, shots --- */
  spirit:["spirit.png",64,72], prop_hull:["hull.png",72,48],
  prop_raft:["raft.png",56,40], prop_boat:["boat.png",64,56],
  shot_stone:["shot_stone.png",14,14], shot_mist:["shot_mist.png",16,16],
  shot_shard:["shot_shard.png",16,16], shot_boulder:["shot_boulder.png",22,22],
  /* --- nodes you gather, centred --- */
  node_log:["node_log.png",26,26],     node_drift:["node_drift.png",26,26],
  node_reeds:["node_reeds.png",26,30], node_nettle:["node_nettle.png",26,30],
  node_stones:["node_stones.png",26,26],node_chips:["node_chips.png",26,26],
  node_vein:["node_vein.png",26,26],   node_root:["node_root.png",26,26],
  node_bush:["node_bush.png",28,28],   node_seep:["node_seep.png",26,22],
  node_wreck:["node_wreck.png",26,26], node_bones:["node_bones.png",26,22],
  node_glass:["node_glass.png",24,28],
  /* --- animals: anchored feet-centre --- */
  an_deer:["deer.png",36,42],   an_wolf:["wolf.png",38,32],
  an_bear:["bear.png",52,48],   an_squirrel:["squirrel.png",20,20],
  /* --- what you build on your own island --- */
  build_hearth:["hearth.png",40,34],  build_plot:["plot.png",34,26],
  build_furnace:["furnace.png",40,46], build_house:["house.png",64,64],
  build_pen:["pen.png",52,40],
  /* --- city fixtures --- */
  city_gate:["gate.png",44,54],
  /* --- crops at full growth; earlier stages are drawn smaller --- */
  crop_sd_berry:["crop_berry.png",26,26], crop_sd_herb:["crop_herb.png",26,28],
  crop_sd_fiber:["crop_nettle.png",26,30],crop_sd_wood:["crop_sapling.png",26,40]
};
const IMG={}, ARTSTATE={}, ARTSRC={};
let ART_IMPORTED={}, OPT_IMPORTED={}, ANIM_IMPORTED={};
const fileArt =()=>(typeof window!=="undefined"&&window.__ARTF)||{};
const fileOpts=()=>(typeof window!=="undefined"&&window.__OPTF)||{};
const fileAnim=()=>(typeof window!=="undefined"&&window.__ANIMF)||{};
/* clips, lowest source to highest, and then tell the player what it has */
function composeAnim(){
  if(typeof window==="undefined") return;
  const out={};
  for(const src of [ART_ANIM_BAKED,fileAnim(),ANIM_IMPORTED])
    for(const k in (src||{})) out[k]=Object.assign({},out[k]||{},src[k]);
  window.ART_ANIM=out;
  if(window.ART&&window.ART.reload) window.ART.reload();
}
function hasClips(k){ return !!(typeof window!=="undefined"&&window.ART&&window.ART.has&&window.ART.has(k)); }
function clipNames(k){ return (typeof window!=="undefined"&&window.ART&&window.ART.clips)?window.ART.clips(k):[]; }
/* lowest to highest: what the game draws itself, baked in, an art.js on disk, anything you imported */
function artSrc(k){
  if(ART_IMPORTED[k]) return "imported";
  if(fileArt()[k])    return "art.js";
  if(ART_BAKED[k])    return "baked in";
  return "none";
}
function artFile(k){
  return ART_IMPORTED[k]||fileArt()[k]||ART_BAKED[k]||ART_SPEC[k][0];
}
function artOpt(k){
  const sp=ART_SPEC[k];
  return Object.assign({w:sp[1],h:sp[2],ox:0,oy:0},fileOpts()[k]||{},OPT_IMPORTED[k]||{});
}
function loadArt(){
  window.__ANIMLOCK=1;                       // composeAnim writes ART_ANIM; artSnap must not swallow it again
  composeAnim();
  window.__ANIMLOCK=0;
  for(const k in ART_SPEC){
    const f=artFile(k), src=artSrc(k);
    ARTSTATE[k]="looking"; ARTSRC[k]=src;
    const im=new Image();
    im.onload=()=>{ IMG[k]=im; ARTSTATE[k]=src==="none"?"a file named "+ART_SPEC[k][0]:src; ARTSRC[k]=src==="none"?"art folder":src; };
    im.onerror=()=>{ delete IMG[k]; ARTSTATE[k]="drawn in code"; ARTSRC[k]="code"; };
    im.src=/^(https?:|data:|\/|\.)/.test(f)?f:ART_DIR+f;
  }
}
/* fold whatever an art.js just set into the file layer */
function artSnap(){
  if(typeof window==="undefined") return;
  window.__ARTF=window.__ARTF||{}; window.__OPTF=window.__OPTF||{}; window.__ANIMF=window.__ANIMF||{};
  if(window.ART_OVERRIDE) Object.assign(window.__ARTF,window.ART_OVERRIDE);
  if(window.ART_OPTS) Object.assign(window.__OPTF,window.ART_OPTS);
  if(window.ART_ANIM&&!window.__ANIMLOCK)
    for(const k in window.ART_ANIM)                    // an art.js may carry clips as well
      window.__ANIMF[k]=Object.assign({},window.__ANIMF[k]||{},window.ART_ANIM[k]);
  window.ART_OVERRIDE=null; window.ART_OPTS=null; window.ART_ANIM=null;
}
/* re-read an art.js from disk without reloading the page */
function reloadArtFile(then){
  if(typeof document==="undefined"||!document.createElement){ loadArt(); if(then) then(); return; }
  const paths=["art/art.js","art.js"];
  let left=paths.length;
  const done=()=>{ if(--left<=0){ artSnap(); loadArt(); if(then) then(); } };
  for(const p of paths){
    const sc=document.createElement("script");
    sc.onload=done; sc.onerror=done;
    sc.src=p+"?t="+Date.now();
    if(document.body&&document.body.appendChild) document.body.appendChild(sc); else done();
  }
}
/* remember imported art between sessions where the browser allows it */
const ART_STORE="castaway_art_v1";
function saveArt(){
  try{ localStorage.setItem(ART_STORE,JSON.stringify({a:ART_IMPORTED,o:OPT_IMPORTED,n:ANIM_IMPORTED})); return true; }
  catch(e){ return false; }
}
function restoreArt(){
  try{
    const raw=localStorage.getItem(ART_STORE);
    if(!raw) return 0;
    const o=JSON.parse(raw);
    ART_IMPORTED=o.a||{}; OPT_IMPORTED=o.o||{}; ANIM_IMPORTED=o.n||{};
    return Object.keys(ART_IMPORTED).length;
  }catch(e){ return 0; }
}
function forgetArt(){
  ART_IMPORTED={}; OPT_IMPORTED={}; ANIM_IMPORTED={};
  try{ localStorage.removeItem(ART_STORE); }catch(e){}
  loadArt();
}
/* an art.js file, read as text, without letting it touch the real page */
function parseArtJS(text){
  const box={};
  try{ (new Function("window","self","globalThis","document",text))(box,box,box,{}); }
  catch(e){ return null; }
  const a=box.ART_OVERRIDE||{}, o=box.ART_OPTS||{}, n=box.ART_ANIM||{};
  if(!Object.keys(a).length&&!Object.keys(o).length&&!Object.keys(n).length) return null;
  return {a,o,n};
}
/* filename → sprite key, accepting either the manifest name or the key itself */
function keyForFile(name){
  const base=String(name).split(/[\\/]/).pop();
  const bare=base.replace(/\.[^.]+$/,"").toLowerCase();
  for(const k in ART_SPEC){
    if(ART_SPEC[k][0].toLowerCase()===base.toLowerCase()) return k;
    if(k.toLowerCase()===bare) return k;
    if(ART_SPEC[k][0].replace(/\.[^.]+$/,"").toLowerCase()===bare) return k;
  }
  return null;
}
function importArt(entries){
  // entries: [{key, data}] or an art.js payload already parsed
  let n=0, clips=0, skipped=[];
  for(const e of entries){
    if(!e) continue;
    if(e.bulk){
      for(const k in e.bulk.a){ if(ART_SPEC[k]){ ART_IMPORTED[k]=e.bulk.a[k]; n++; } else skipped.push(k); }
      for(const k in e.bulk.o) if(ART_SPEC[k]) OPT_IMPORTED[k]=e.bulk.o[k];
      for(const k in (e.bulk.n||{})){
        if(!ART_SPEC[k]){ skipped.push(k+" clips"); continue; }
        ANIM_IMPORTED[k]=Object.assign({},ANIM_IMPORTED[k]||{},e.bulk.n[k]);
        clips++;
      }
      continue;
    }
    if(e.key&&ART_SPEC[e.key]){ ART_IMPORTED[e.key]=e.data; n++; }
    else if(e.name) skipped.push(e.name);
  }
  const kept=saveArt();
  loadArt();
  return {n,clips,skipped,kept};
}
/* write everything currently in use back out as one art.js */
function exportArtJS(){
  const out={};
  for(const k in ART_SPEC){ const f=artFile(k); if(f&&f.indexOf("data:")===0) out[k]=f; }
  const opts={};
  for(const k in ART_SPEC){
    const o=artOpt(k), sp=ART_SPEC[k];
    if(o.w!==sp[1]||o.h!==sp[2]||o.ox||o.oy) opts[k]={w:o.w,h:o.h,ox:o.ox,oy:o.oy};
  }
  const anim={};
  for(const src of [ART_ANIM_BAKED,fileAnim(),ANIM_IMPORTED])
    for(const k in (src||{})) anim[k]=Object.assign({},anim[k]||{},src[k]);
  let t="/* art/art.js — every sprite the game is currently using.\n"+
        "   Drop this beside castaway.html (or in an art folder next to it). */\n"+
        "window.ART_OVERRIDE = {\n";
  for(const k in out) t+=`  ${k}: "${out[k]}",\n`;
  t+="};\n";
  if(Object.keys(opts).length){
    t+="window.ART_OPTS = {\n";
    for(const k in opts) t+=`  ${k}: ${JSON.stringify(opts[k])},\n`;
    t+="};\n";
  }
  if(Object.keys(anim).length){
    t+="\n/* clips — ignored unless art-anim.js is loaded, or the game's own copy of it */\n";
    t+="window.ART_ANIM = "+JSON.stringify(anim)+";\n";
  }
  return t;
}
function frameFor(k,ent){
  if(typeof window!=="undefined"&&window.ART&&window.ART.of){
    const f=window.ART.of(k,ent);
    if(f) return f;
  }
  return IMG[k]||null;
}
/* draw a sprite anchored bottom-centre; returns false if you have not drawn one.
   Hand it the thing being drawn as the last argument and it animates itself. */
function spr(k,x,y,scale,alpha,rot,ent){
  const im=frameFor(k,ent); if(!im) return false;
  const o=artOpt(k), w=o.w*(scale||1), h=o.h*(scale||1);
  ctx.save();
  if(alpha!==undefined) ctx.globalAlpha=alpha;
  ctx.translate(x+o.ox,y+o.oy);
  if(rot) ctx.rotate(rot);
  ctx.drawImage(im,-w/2,-h,w,h);
  ctx.restore();
  return true;
}
/* draw a sprite centred on a point */
function sprC(k,x,y,scale,alpha,rot,ent){
  const im=frameFor(k,ent); if(!im) return false;
  const o=artOpt(k), w=o.w*(scale||1), h=o.h*(scale||1);
  ctx.save();
  if(alpha!==undefined) ctx.globalAlpha=alpha;
  ctx.translate(x+o.ox,y+o.oy);
  if(rot) ctx.rotate(rot);
  ctx.drawImage(im,-w/2,-h/2,w,h);
  ctx.restore();
  return true;
}
const TILEART={};
TILEART[WATER]="tile_water"; TILEART[SHOAL]="tile_shoal"; TILEART[SAND]="tile_sand";
TILEART[GRASS]="tile_grass"; TILEART[MOSS]="tile_moss";   TILEART[DIRT]="tile_dirt";
TILEART[PATH]="tile_path";   TILEART[SCREE]="tile_scree"; TILEART[SNOW]="tile_snow";
TILEART[ICE]="tile_ice";     TILEART[MIRE]="tile_mire";   TILEART[FLOOR]="tile_floor";
TILEART[PLANK]="tile_plank"; TILEART[TREE]="tile_tree";   TILEART[PINE]="tile_pine";
TILEART[SCRUB]="tile_scrub"; TILEART[ROCK]="tile_rock";   TILEART[CLIFF]="tile_cliff";
TILEART[VEIN]="tile_vein";   TILEART[WALL]="tile_wall";   TILEART[DOOR]="tile_door";
TILEART[RUIN]="tile_ruin";   TILEART[GRAVE]="tile_grave";
const FLATART={};
FLATART[WATER]=1;FLATART[SHOAL]=1;FLATART[SAND]=1;FLATART[GRASS]=1;FLATART[MOSS]=1;
FLATART[DIRT]=1;FLATART[PATH]=1;FLATART[SCREE]=1;FLATART[SNOW]=1;FLATART[ICE]=1;
FLATART[MIRE]=1;FLATART[FLOOR]=1;FLATART[PLANK]=1;FLATART[SCRUB]=1;

/* ============================================================ ANIMATION
   This is art-anim.js, carried inside the game so clips play with no extra
   files. If a real art-anim.js has already loaded, that one is left alone.
   Everything degrades to the still sprite: no clips, no difference. */
(function(){
  if(typeof window==="undefined") return;
  if(window.ART&&window.ART.of) return;            // an external art-anim.js got here first
  var CLIPS={}, ART;
  function build(){
    CLIPS={};
    var defs=window.ART_ANIM||{};
    for(var k in defs){
      var set=CLIPS[k]={};
      for(var c in defs[k]){
        var d=defs[k][c];
        if(!d||!d.frames||!d.frames.length) continue;
        var e={fps:d.fps||6,loop:d.loop!==false,wind:!!d.wind,scatter:!!d.scatter,
               hit:(d.hit==null?-1:d.hit),img:[]};
        for(var i=0;i<d.frames.length;i++){ var im=new Image(); im.src=d.frames[i]; e.img.push(im); }
        set[c]=e;
      }
    }
  }
  function now(){ var p=window.performance; return (p&&p.now)?p.now():Date.now(); }
  function rate(e){
    if(!e.wind) return e.fps;
    var w=ART.wind; if(w<0) w=0; if(w>1) w=1;
    return e.fps*(0.34+1.66*w);
  }
  function pick(e,t,phase){
    var n=e.img.length; if(!n) return null;
    var i=Math.floor(t/(1000/Math.max(0.1,rate(e)))+(phase||0)*n);
    if(e.loop) i=((i%n)+n)%n;
    else i=i<0?0:(i>n-1?n-1:i);
    var im=e.img[i];
    return (im&&im.complete&&im.naturalWidth)?im:null;
  }
  function dur(e){ return e.img.length*1000/Math.max(0.1,rate(e)); }

  ART=window.ART={
    wind:0,
    reload:build,
    has:function(k){ return !!CLIPS[k]; },
    clips:function(k){ return CLIPS[k]?Object.keys(CLIPS[k]):[]; },
    hitAt:function(k){ var e=CLIPS[k]&&CLIPS[k].attack; return e?e.hit:-1; },
    runs:function(k,c){ var e=CLIPS[k]&&CLIPS[k][c||'attack']; return e?dur(e):0; },
    hit:function(ent,c){
      if(!ent) return 0;
      var a=ent._an||(ent._an={});
      a.one=c||'attack'; a.t0=now();
      return 0;
    },
    of:function(k,ent){
      var set=CLIPS[k]; if(!set) return null;
      var t=now();
      if(ent){
        var a=ent._an||(ent._an={});
        if(a.ph==null) a.ph=Math.random();
        if(a.one){
          var one=set[a.one];
          if(one){ if(t-a.t0<dur(one)) return pick(one,t-a.t0,0); a.one=null; }
          else a.one=null;
        }
        if(a.lx!==ent.x||a.ly!==ent.y){
          if(a.lx!=null) a.moved=t;
          a.lx=ent.x; a.ly=ent.y;
        }
        if(set.walk&&a.moved&&t-a.moved<180) return pick(set.walk,t,set.walk.scatter?a.ph:0);
        if(set.idle) return pick(set.idle,t,set.idle.scatter?a.ph:0);
        return null;
      }
      return set.idle?pick(set.idle,t,0):null;
    },
    tile:function(k,x,y){
      var set=CLIPS[k]; if(!set||!set.idle) return null;
      var e=set.idle;
      var ph=e.scatter?(((x*73+y*151)%97)/97):0;
      return pick(e,now(),ph);
    }
  };
  build();
})();

/* ============================================================ BAKED ARTWORK
   Sprites drawn in Sprite Shop and carried inside this file, so the game looks
   right the moment it opens with no art folder and no local server.
   ART_BAKED holds the still of each one; ART_ANIM_BAKED holds the clips that
   art-anim.js plays. An external art.js, or anything imported from the K panel,
   sits on top of both. */
const ART_BAKED={
  tile_water:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAABSElEQVR4AdyVPW4CMRBGLUtRypwgXdIk1ByLC0DPbRAn4BRAAx0noEQUoFd8o8FrWPNjFyA9GY9n5u2wEo6D4fTUkhgaf24KJ+NRgGef6e9/YC1uCsmazRfBFxC7B9WyQkdIUCBTc8W071s/Pr8CbLa7sF4tLf1CSFMOBclCMau88gUJ+GP26mNCyTgUvqjkO3V9eSZkgpKCvoacHw/7kEIcTJiT+SKS+1B+Lk9nERGkSSSksVfsbULfrJYMR0dYU9YR1paZEBEQqE3nJ60ubDWZBmk/ocyt1vebML3Aq02ICLhTuYmA11ZNiAiQCKQPCykGNcutXHkecrLC3O1BspCIZoqla+6MmF1PSERazJ73gQjYU8x6D/S/mJB/HUEjL+F9IBGcl4IIyI8Ifn++A/D0HiQklUpomoMeIiLQJl1LRF6Q1uf2ZwAAAP//0AVFVgAAAAZJREFUAwAUg+tkfT+BGQAAAABJRU5ErkJggg==",
  tile_shoal:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAAB30lEQVR4AaSUvU0DQRCFV1cBATEFYGdIECFCOiAn41wBIEKEqcDQBxEhIiIgwxRAB1Rg6zv5rcbrud3bs6Xn+Xszb70/bqan81Ufrq7fdmpt264s6PV45D00oeJzfvJXwfapVYLpiM/vozTlxseTaQAURwtasd/lTxzIUICAQB2QzwqKBBFMzhaYDun2plxioWvYfGUFN5ysuZndZutpsUrw8OA/XFzO4wzEXhfPMR7iVAky8PHpBTMaW4I65L5piKVn18fVrNRuCXLIDICEFdg6QbmSZZaHLUGGiIQPEFp+zYIQNh+eRen86IUui78jSNICIRvLZ2vBw30bBNUQAMRwsMSgEyRpAUExvsCvki/78X4XBPWwSEE8xQ0kJWXTHE8BUPdEyQ9F9wtF9oZJiBvax+MfiF4A34IcZ42lPwrSxB5jKQJ8hABkAAdQJ04B18LW6Wm0GlYhiEQsX1Y5u+3KiYPlaVmQA41WY4v4FD1wI7kAXs3m9LxkmQniloosgmJrEbMxPtvEIPwcNDcKKpFrYjd4AuIMFRMfGwUJxoLFDu0dLcivQ6RGDH5RMD0fngqN3FLvTKnlUBTMNY+p7SXIJUp3oLSIvQRLw716taD3r+IN7ssVBWtvYZ+Q8msAAAD//+t5IY4AAAAGSURBVAMAv6s2zUqXqFsAAAAASUVORK5CYII=",
  tile_sand:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAACB0lEQVR4AYzTwW0bQRBEUYFJOAdfnIizcCTOwHk4CMWgi3JQFBLeAH/RHC4pCvjq7qrqGeySvLz+//MZ//7+/oRZxVk/NZnJI0/u8vb+8eKv+uvnj5dH/fTtmaGHXfMOj3bxTyjhu15uIo+0zqOBPuvxhAVVFJp9yzS9Cv2zrCe01AUW9TSYq3rk16vBq1fNc/+SqDIE9OqEBx7y9OBBDz30svU3r1SAGWZYgj5k6vcqCxnoZW5eaaYAhGjqGTzIQsYM/dTMxxMaIFDYDJq6k17NN+NsXk+YuV/UgnrmTU2PsywdvKsn7OJqIXXXLNNQryLNHmixvqW7mFntALNeHmbU59FgrtavC4ktqaAV0u888tq3s+euPsMCheZivZovv8PDrjcfn+EMORSF1OmbJ4+8zqnevNIO6pCC3+n8PTvnzjsutBCZ5tmbd84OlUnf99eFzN040xy0s+/x2z3z1oUMIWHoaXqYVdzrec+wLnSIC1RLejXMeVPTp6ug7aSr60IHClX1Z1jY9XZUyFTLmuvX77DhrDqAbinMMKuYOfP0zKAdv0MLwawXaq7mqbR75KuQO57QwSBm7jOPNqHttJ9e3ryecAammT41S8jTQ4YGs0rTB+3qS8MgQhhpavD1KvTP4Lx1oXCLRKTRm2loVkGLZtVuev1xIYM4sYR7Hh3tlKVhzvVfAAAA//+PrRu4AAAABklEQVQDAFLIHqfUKWIrAAAAAElFTkSuQmCC",
  tile_grass:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAABPElEQVR4AcyQ0Q3CMBBDK2ZATMA3P6zD6EyA2AF4SK7cyyW0Iq1aycqdz2cnPVxvl9eWOAwbf/sIPJ2Pq717Hy9c7Xkf4+KFa/7OT95QBEL2Qnb5boHRPPZ6RLdAGfr5uD+9HbhEl0CMJs6NZhLIYrxVY7c6ynzgWJgEQoAeofhE4JsGRmGr180xk87rOP87UCHZqTCfzQpkEfgidcbBO+Jri8DMhCWAUW2uWTbXjLMIhGyhFcye5tQRzBYF+u1ZzgzFoZWGWnwRKBECF9L7jB5kHLyAh2uKQAk5XUjvwMj7Wh09xsC5Bm4czTTDy2fej4EuVl07MWCmk7oGNB5eBNYWnZeBTp8R4H2sFwfK0MPERfOMXxQoA50xwHvXeP0NdIKl2GccGkFzeuoWvoFRwO9i2ZFp0P3i0QDp3gAAAP//5FVXQwAAAAZJREFUAwACEKhFYCWDawAAAABJRU5ErkJggg==",
  tile_tree:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAqCAYAAACgLjskAAACBElEQVR4AeyVO04rQRBFxyzAEqRAMgixAETsjH2wMtbiGLEEJoEYYiSQ0RnptsrVf48TSzzpvq6uulWne2aAs6Hw72Yz7rwenu536O7xNqzEhTF7pSQQCEMvxvPBi+7P6Wv4+f4lDAKKQiITBCAQJFDGX00DlVLmAKTIbVglbmKlfOsK2Hv3gCoKon1pXV+uS+XBQ5PA4oRE0b/PhCWkZiDvjkzuZvZR48EraV+C2lvOQDXnVg21dXsI5VugRSBDkQa2rEARXt4vIpaKQJlYU+DUzfGWVAQyEJUG5Gq6pa9ngdwI2QYLt7H1KLa9wBG1LJBiTnZYymPrHOxtO60Q3iwQI8LEgFRMrSaB5IuADEcy2Fi52srh0Mvz68p7I6A3sKeZFdmYfa9mILdANDMQEStHXBNeVPPNQGuiSSIvOPExFAFbhl69D9EfZg6G1O8/FuUjIE1IhmOvEdACSmBqyPoV525HfQbSiEhIfq8868c1/x+mGVg6kcbaD0k5Vg6GiFFt1gxsMeJBgFm9ACGf9/sApFBq4BYSXsTXSg9i36I9YEsDwwGhFr/3REAGehN78ojYajOOO7uvxRGQBgZ7kZe20xT9UlattiaBtaYl9dMD9j7e07th7/s86Ia9Pwr2UAcB7YDeeBGw94PhcCUg9aPrHxg90iVfKMO6HulSWDeQhqX6AwAA///5f3FjAAAABklEQVQDAPZNEmxR3WEmAAAAAElFTkSuQmCC",
  tile_pine:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAqCAYAAACgLjskAAACO0lEQVR4AeyW22pUQRBFe44Gn3wQgwYEEcVfEPz/B8FfEEQEIUJESPISQm7rwG72qame0z2EQCCBnaqu2+pqJpepPPDXE/DeH/zxPOnR13c3+6y/14aCyY6Ah4GCXJ5czByd50PHt2EgMwXDH9UwULCDwxeVNbLlEHBkcL1NcIaA9LIZevXrmmNV72W6ga8/H9YfAz1rpQ043UC2Wpvbs2UX0Aetbee12QW7gGp02P8PU3n/82pOeZzALugqMGvW8/7++Iz5hTNQNAfuvmV9d+GyE+hNPkybMUACik+d5B80cqgJbMFo0mb4LqD/fpxsXJ7HbwJJIm6LdWUbKu8XVcxtClRTBmOL1oYaTL+kmOwWkHcHhFQkC0z+vnYB5FbZIEAoy43GFsBsKz4Aa0Ppc3l9XGIBZHiUN+NrML4Ut1eNrEMXQA1w68WKRwDxLMblyQHGolUgRZI3KuYWqIscUISPhoA0SMCRzpnNXmcn0Bs0nA3kZ5C1WBN4H7Djb3828QJNoAq1DZsp1mv90upJgbFQMMHV3GPjrBTIIIYjfOQ+Z8nj7iuPdWgTSKEUB7Hxp9PnBb05Pyiex0fqlRV0C0gia6CRnycU/zwR4wOCpQ4xQ+KMmL0A6i8FySgfFnN+pk5SXGBsBQKjgOfCllKqYUA9BOfldf13NWRKoS9qBrKqEnoanbE+6cvboy1CFvMe9ydgQDzY8kcGt2ZM+8DOpq1fIKX3MvOTtm4zGu+BdgO//z3eoLhdPK9d8hYAAP//M7sy/gAAAAZJREFUAwAblGlm246/XwAAAABJRU5ErkJggg==",
  tile_scrub:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAABVElEQVR4AeyTzUoDMRSFo+AraGtBUFz5Bi7d+uIu3bmUbrTqCyiFMvrJnOFyczOZhtLVFA4n9y9fkran6cifGXjwB5+fdH7SvV+g+UezerjqrKaS9wK2APxBqsDF/aqT/LCNOcz1421nc9F6FAhoeXeZkB8+v7nwqbT93qYadBTod9z97BIi//X6iWWqQYvAxd9TZrv1CaCoDwfTrYHyxFZqCoEWtnl5T0gD1gVQzt+aOlIdD4EUxsR3imoH4QDIvkYGtLfzUCDKCWY3Uw2I1vjH09sJjjIgyZIEkatPUNyKOjEuhUBugtQ0xf3GzES5EOhvwHBN/oDEyM+FQJqARgPUItFv88TIfn/Ui0CKDOCRlpuzKD3kAKEh0S8yIE2or2dGTTCcWFIzsdbeM6BvsPHYRvRRR6xLKgIZ9NImz+v1//9KrvwULwJrwy0w9mwGMtyiXwAAAP//+acLQwAAAAZJREFUAwDXtME5QlLShQAAAABJRU5ErkJggg==",
  tile_rock:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAiCAYAAABMfblJAAABTElEQVR4AeyS0Y3DIBBEsduIrg/X4T6iq+WUptxHErfB8VYaacOBbZxTvogyApbZGe2YMXz41w3/PfAeaY+0OYH+aJoj22voke4l1HzfI22ObK/h7Ui/r9eYY8v0tKFMSuJbd6cMEcTo/nwGwL4E8fzdKUMEZPR1uXCsIjetGkIUvNo8z7FkRu3+eNjEtk/T+z7ti4aIikCzP6vuVzghxhCGIez9xlxMZ0SABKgDnYmSe6BavjIxNZJiBTahL1A8ghej2nRp4hdeEjbDvJjqx/+YOTaTW7yu5vXHdV3d1YltmoJvhxEwcWoVqXFZluEdU0wARqDkgz6fjTdgjwbTEvForWZEP2bo/9xu9oT/RAoBQD6CmhkaADN0mI7VImUjiKBz64oJoK+kZa9U7pAEmgTVtlbPxQh4vu7NkIM3rZHh1SDxvHeapog2dfALAAD//ytadAwAAAAGSURBVAMAnN79nwdBPG0AAAAASUVORK5CYII=",
  tile_cliff:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAmCAYAAADX7PtfAAABh0lEQVR4AbyW4a7CIAyF59L0XfdD732VG3/sXW9MNGfapWwtdMA0aYBy6McBNI7Dlz9VwKfxie67ChgtbukOAX+u18WbFPq93aQ7YGIdZDpFICASqKMhGB8NEzhN0xMBkFVQQ3Xf0m5zJnArwvj/8UCTjcix7oBwJlUBkZCctNoZ+giZy7U7oCdmIm8qyZdchoFJVWOAk4i4DAGZYu6MfexSIeBulZOAS2dqTSdA/WBWRbDD9D6F0rEmwGDtJlkRyPTeeRNFLV6BLcep6g24R+8XCroViEGvANSrdQrQgyGfBTIduz+msj4LxI5qw7vH04DePS7AXi80choLMCKs0VhGRitZU9xbs71L1yFT+cVZEKb8OhdoFavJ4fFol6cDt5t0gdjZVtwyFpcusKW4XsuU3unpQA2Hyyyw97ECngVC0DuKwN4ui0A47AVFnRAwCkVBBPQ6kEMgFwZCjEW5gEYHEy3/cXRunOf5wpR+Vz6C5gabkyJMNIC1OPy73y8y0bsFBCGMFwAAAP//z1nE5QAAAAZJREFUAwBQvr5Jx+nN1AAAAABJRU5ErkJggg==",
  tile_vein:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAiCAYAAABMfblJAAABKUlEQVR4AezTQQrCMBCFYRsQXHkON17BpffxBOIJvI9Lr+DGc7gSXFT+0ikxNtM0TgpChWes7czXSdUtJn7NoPmGz1v6v1t62m1q7n6SZwi2Xi3xFsVBsEZq34qDrdMtRcFwOtRioI/J8ysK0rwvRSb0pwtRczCGHS63CtwcpKkWM/C839bhdPxYiEzHjfwMApHH80W/wWSDIASIhBKT8Z0/HcdZoAbRNIZxbjQoGMVawsnk2mQQiPRtnzRjZboYxvlBEIQAEYr6AkQ0jDoVHAPRbAjjmi8QhP8T0SaiWAJE5FhbP0CwFIStk6RCchMdmDKRIBQDET6PSQMymVZkAUl/F5sM5Hi9V4RJJFKYuzoa+wEgALlNtTpHYz/axRbnmmdo0Si1x+TgGwAA//9GiOzYAAAABklEQVQDAB9pmUVfK1hEAAAAAElFTkSuQmCC",
  tile_wall:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAmCAYAAADX7PtfAAAAfUlEQVR4AeySMQrAMAwDS//QuVPn/qZ7//+IFA83q4IQQlDACGSUxIf3bfDJg92BB+kCSJ/raNR7n43CQ/FL8dDyKDwUv7S8LM0CS9N9BHFhlkYA8ttB6jMTiSAVgPx2kPrMRCJIBSC/HaQ+M5EIUgHIb0+F1P/+j8TwCT8AAAD//2/dLhwAAAAGSURBVAMA0ZMtZcotfAMAAAAASUVORK5CYII=",
  tile_door:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAiCAYAAABMfblJAAABUElEQVR4AeyUsQrCMBBAg4Ogs7jo5Op3+BF+oR/h5iq4uTrpIs4KLtoXenDtXVtpm0UqPHO55O41oToKpc92vfzAZjWPYzn25lU58qAVRvh4vQPMJuO4jzgG+Vd5Tpqc7GdehxGyWYppVDdnTfYQ/4IRtpFJTSshTwwUSyPm+8s9wO58DcA6c2AO5JowJ6QAERBXwUNUrdXljVCLaApeA73PW6/KGaFsFBGNQfJdR1eoZSLIfk8h+21GiMkzasg1YYSejCa8FLwgQCw5YoFcE0ZIQZ9XSD+NEXoyObUubBsbYblRnzJ61wpFxqn1y0FMMaOGXBOuEBFQjIyxL4xQi/qW8dBGiARYTIERppDonv8hvE3nAfTJJE5ywsPxFPt70iRCbCIl1iQTIlk87wwFkgjl36dgyidJhHlvdxiE7rV0SQ5X2uX23NrhSt1r6ZL8AgAA//8/ujz3AAAABklEQVQDAO+6q1DWL3UQAAAAAElFTkSuQmCC",
  tile_ruin:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAmCAYAAADX7PtfAAABcUlEQVR4AeyVsXLCMAyGabYOjO3QsWufo0/Nc7AyMsDIwAj3+fhzsoljcGQfA9z9kS3J+izHOYZV598bOB74/+/3BY2OysFrHqlHZzqQYocWttkdPrSw1haBtYVz6x4GenTHJmaB9jhJ9lAWaGFe3bHhLJBgC00C1d3+dA5MzcNk4WMSSE3BGCMv6B3QFt4eT4u/OzZrFQEFs915XhjAERCHpO7+vtYXbUAbUk6NDUAKIRX4WX+uNBfYCxqAAuWsoLn4M/4AZPcoXaguH/WneVPzAFQHU1C7qBS3ublxABLkvWHToupScXKQ/Iyf0cBCZBcBRfIpnkIVlyWvpLFDFlHQCt+c0uJzuYpFQDk9LSdlNdgJY0+YrcXFRAMPiYQWUOpTG0VHyvvD6am0ZgQElCbgWyoul2rcARVoZbsAuRf869BEF6BeE9AuQDoDiroBgaIIyFnjbKkICAgoYtxCI9B+KzdQExOAwNKumJdUs6MrAAAA//+8UCdWAAAABklEQVQDAPMsyvHoSZDtAAAAAElFTkSuQmCC",
  tile_grave:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAgCAYAAAABtRhCAAAA7klEQVR4AeyTwQrEIAxEyx49+TWe/f8P8uClyxyyhFTsjF0WFiwMamrynNS+jh8/G/j1hu+W7pbKHZAuTSnlhGqtp0kl0kCA1OKj/TRwlIwYnGJkRQG9u5QSW3u4jwJapoe11ix8KC4l4IcQJh4eXl2Wj4AexLp8BIzHBxSKcb+WgHAE+QKj+QwqAf2liSDmIMi5BdovMYOhEKtbYCzUez9YNzEXaxmIJEAxQipcBuacwbmIbbkMNJJ3aTFmXAKaS4Oy7nCgJSASDYq5omWgAvF7KaDSMl98NJ8C7acfJa7GpsDVorO8/wLipqrf9w0AAP//bwE5aAAAAAZJREFUAwAJpT42mqY1iwAAAABJRU5ErkJggg==",
  player:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABcElEQVR4AdyUPW7CQBCFTS4QKV0kpCSH4AJpU+UAuQNH4Q5R6lRpU0HlnoIGkKgokKgpjGelATPaGd4sBrFGHu/PzL7Pb7D8UDh+w4/3KhYOiUMpDCbg4ZSYUI5CbJtLCCxFX58ei2aYBCUJgfksw4bfv2GLR9oPG46bC7zYbIP06OvzZAyL+iY7U2+plwusqiQkXODJdBZF/IzLQstFD9SbMJiFCSKj1gnX6O+/FybADQYDWq4SCOxxgtIhMCrmqbtv8OCtX3ncILX37Rhx4K3J13E5X8EfjWZXLnac+uJdDG668MzzAK93z0UsPE65FnLMMD4kx3N5WU9rCEyFbUc3wC/9QUWBdKcbjtkp4rpbjsn5Odd5Ol6uyh4FOYyF5TpPx+wyxfWNHPMjHsdksHRJa4qjtD1LBsdkrZdJ1rcKluLWujWw5lZrf2tgy10slzfY22bqwNUca/8tQSkgsCWiuSVxKyCwJqBBrQdlrT0AAAD//y4UfVEAAAAGSURBVAMAJH5+rzCI/FwAAAAASUVORK5CYII=",
  shot_stone:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAAnElEQVR4AZyOuw2AMAxEQzpKChbOMIg1KKjpEDtQUFKCXqRDBsI30snx+Z4T7xInhLBcSfEdqLCGtuZl4RAZ/A2UgSkRlORRyUaQC4YVgO2Pd/8V0sL4ot2mgfVSdz/0fcp/9OKLf+AIsh74zTfncSLuNpCua1rKpQQR2IEYwASOwudX0gkE1tBWfKmq6ywJKnBXPTS6CzEjI9GvAAAA//84J2R2AAAABklEQVQDABVDZ2W0HZftAAAAAElFTkSuQmCC",
  shot_mist:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAxklEQVR4AZzSAQqCQBQEUPMGQtfrBkF0g6ADRNAxA49gPGFkXZWsYJj/58+fXdO2qX7X13MAMgZ1DTosAmrjt35XgJMgYakf58thFpBBjPjd92gTswAuCyAM00rQy34RkOHa8ul+G2p9MyBB2BKoj12HGs+vaF0pIPyKNkl7FnN66R0foQxZM5UL6tI/BhD/xSKg/LPcJnBAZupgCiivleEengK8iSw4VWBAp2HgBfUYkIbAaFEdpDeLhu2N34EmiDl9eEv/AAAA//+oYX89AAAABklEQVQDACQ3aV6A+gwPAAAAAElFTkSuQmCC",
  shot_shard:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAApUlEQVR4AbyOgQmAMAwEa4fRqRxFEBzFqXQZ5Qovr0YRChaOpkkuaU6V5/8B/TRugs+//kCNfq/LkgAZwgESaBBIoPc8jA3xbQAyBYEEerddl0Dv0wCXkUCN3BK1ndxpAAkkIHYke474GMD2SKTJ8e3kjwFPMpuB5ogygO1R8Spet+NkZN+OJGgQkUyt/EACN0kHETzncab4hjdHcflBVPiaqx6wAwAA//8nQ77OAAAABklEQVQDAPKPYyFy3cHFAAAAAElFTkSuQmCC",
  shot_boulder:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAWCAYAAADEtGw7AAABVUlEQVR4AZTU0W0CMRBFUaCPiEaitEAn/ESpIsoPndACSiOIQghnpbeadbzEiXTl8fPMlWHZ7Dadv+PxeK+0LTn7/Pq61zN7Z7Jf4sPhsGjWpBmprbhdrxt5sJdjFhNiv9/LuxDUg5dHL2RWqPXtyCAYwXBIv33qrPONBc9u67wnkPeYxc+khOgJetnpdNrO4l6DbER4ezzEYAaTeO22o1KilkksHJHoG2USXx8fZXSg9vn4dV/r3fl83r6+vdVsqF6TenAE040V/2FNWr/O6QX5vlw2aOU9QS8jxMf7+zaOxY3b71qzRrKQTK6GGl5lKxZiATnUkakRiRWyoFedfw8Lcf09R64ZrUjWUmdmcZVmQCOeSd0U+jJnncU27aEMHuwaazPT79hw29D7BPoqelCz1IsbJ1xrznlWlwnJsnbFDg1YR/D2VsxMYqFNqFJnf5G5uv4AAAD//zwFV3IAAAAGSURBVAMAJqbetUp+FcwAAAAASUVORK5CYII=",
  node_reeds:"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAeCAYAAAAy2w7YAAABIklEQVR4AeSUSwrCMBCGW+/iyoVHcK83EMF7qEv1Bh5AEG9gN648ggtXHqbyRQamJU3HBCtFYcwkmfkfQ+gg+/Jvv16WUDgi2XCQGmARgjMeDbPLeVs6ItnIZcq62h1zH54jagPWCnUufb4zuZPVRKQV6hwQxsIZeSi8RBaFIVDuDsWVJWOdzTe5I2JzfzzdBX8oRCl5TABc73NEHGpwXyE1BHcII7dEcbrl08Xk/epo8DVbXNEHORihcI5gDRVZwUIYjkgKAJQclXov57FrhSgWxNL3OyJeiEXhpzUVR22PwgqO2DpWhcgKFFPXDyLfiOpupaYfjurq2YsDch39daRd6LxbR01z1YpS824dpaq19P+pI77EPBjLiJpqkkdnFfACAAD//6g5NcsAAAAGSURBVAMAGfBzQSt+W5gAAAAASUVORK5CYII=",
};
const ART_ANIM_BAKED={
  player:{
    idle:{fps:3,loop:true,frames:[
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABtElEQVR4AdSXPU7DQBCFHS6ARIcUCThELkBLxQG4Q46SOyBqKloqqNJT0ABSKgokagrjt2KiZT2/9m6RKM/enZl9387aRXLUBT7rq8ueU8BiX+oGA7hfVQyQg4qwOnWBS9Pzk+Mul0oQki4wrSXY+vY+heiOeAoELiHw+9d3st7cXP+7p8lwKU9mCInfEFh0mZAIgZ9fXlnE3dO2k3LsgiHoBpMxIKUGn/TdPDwu0sBxcYMdXqESE7y6WPbUreWMWsiqQ14Fe01gFJUIngP1rBXB0Q7KegvOgq1FJYTm27ddvbc6YkYb8NzZjj0L59Y0A1snZYIjzztSa4KnHKnVLTxZMBZCKGglFizBPn9OO055vXfDKphMCJYD8jHlqT7PSWMVjEURM9R7ZYK9RtG6quCz5aqHPJuoCvYAqaYJ2NN1EzB1pd2bga2um4G1bpGbBf7YbRcQjDhpXc8CczBvrAp4StdVwHaX44rJ4LJLzKExgo9MBnN22stU1lcFl+bavBpY6lY6/mpgrTsud9jg6DHjBJp1LD1bQCEXWDORuoW5Jhc4Nxg20UFDbPQzB/E/mf8afwEAAP//TR2cUAAAAAZJREFUAwB5fKzuuL4p1gAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABtElEQVR4AdSXPU7DQBCFHS6ARIcUCThELkBLxQG4Q46SOyBqKloqqNJT0ABSKgokagrjt2KiZT2/9m6RKM/enZl9387aRXLUBT7rq8ueU8BiX+oGA7hfVQyQg4qwOnWBS9Pzk+Mul0oQki4wrSXY+vY+heiOeAoELiHw+9d3st7cXP+7p8lwKU9mCInfEFh0mZAIgZ9fXlnE3dO2k3LsgiHoBpMxIKUGn/TdPDwu0sBxcYMdXqESE7y6WPbUreWMWsiqQ14Fe01gFJUIngP1rBXB0Q7KegvOgq1FJYTm27ddvbc6YkYb8NzZjj0L59Y0A1snZYIjzztSa4KnHKnVLTxZMBZCKGglFizBPn9OO055vXfDKphMCJYD8jHlqT7PSWMVjEURM9R7ZYK9RtG6quCz5aqHPJuoCvYAqaYJ2NN1EzB1pd2bga2um4G1bpGbBf7YbRcQjDhpXc8CczBvrAp4StdVwHaX44rJ4LJLzKExgo9MBnN22stU1lcFl+bavBpY6lY6/mpgrTsud9jg6DHjBJp1LD1bQCEXWDORuoW5Jhc4Nxg20UFDbPQzB/E/mf8afwEAAP//TR2cUAAAAAZJREFUAwB5fKzuuL4p1gAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABk0lEQVR4AeyWPU4DMRCFFy6ARIcUCThELkBLxQG4Q46SOyBqKloqqLanoAGkVBRI1BSLn6VZeUdje2bWSIm0UWZtz8/7xpMoynFneG2urwbJDBJjqhoM4FjFNojBmLt4VIG56MXpSZdakZAJqsBUS7DN3UN00Qp/dBgeJvDH90+U3t7eTNZ4CA8+meDKvk3grIojYAK/vL6JiPvnvsvFxILgVINJGBBuQSe+t49PR3GjeKjBCq1ufbkaYJpcFdhyE0A18CoYIjAIWqxWUwTXinkj/ftu/mdshVITWrh44xSqFSKwdhXB2uI5ec3B6bRKjTUFa6FoqAq2iEFQayK4xReqpiGC0XWtEDk509RmwRDlAl+/Z51kyCXjNeTnaxGMZBICEGfJEINRrpTDfVUwCiCKtaWpwC2BpNUUfL5aDyRcW5uCa7A0voDTafzr/jBH/bnr1X91+PjcN54DRRNuMIph3gZcYDsMLU7NBZ5K+E4L2Dc3R9UyasfQfCX7PeoWPxh8LtobDwHOa8czxWgdA4XNHwAAAP//zBoyfgAAAAZJREFUAwALXZGiVYUoEwAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABk0lEQVR4AeyWPU4DMRCFFy6ARIcUCThELkBLxQG4Q46SOyBqKloqqLanoAGkVBRI1BSLn6VZeUdje2bWSIm0UWZtz8/7xpMoynFneG2urwbJDBJjqhoM4FjFNojBmLt4VIG56MXpSZdakZAJqsBUS7DN3UN00Qp/dBgeJvDH90+U3t7eTNZ4CA8+meDKvk3grIojYAK/vL6JiPvnvsvFxILgVINJGBBuQSe+t49PR3GjeKjBCq1ufbkaYJpcFdhyE0A18CoYIjAIWqxWUwTXinkj/ftu/mdshVITWrh44xSqFSKwdhXB2uI5ec3B6bRKjTUFa6FoqAq2iEFQayK4xReqpiGC0XWtEDk509RmwRDlAl+/Z51kyCXjNeTnaxGMZBICEGfJEINRrpTDfVUwCiCKtaWpwC2BpNUUfL5aDyRcW5uCa7A0voDTafzr/jBH/bnr1X91+PjcN54DRRNuMIph3gZcYDsMLU7NBZ5K+E4L2Dc3R9UyasfQfCX7PeoWPxh8LtobDwHOa8czxWgdA4XNHwAAAP//zBoyfgAAAAZJREFUAwALXZGiVYUoEwAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABWElEQVR4AeyVMcoCQQyFx/8CP9gJgnoIL2Br5QG8g0fxDmLtJbTa3sJGBSsLwdpi9cHuojEzvuyOzaDw2E0myTcJUf+c4TMbj3JNhhJVqAlcZYmX8jLCHTRN4H7732kKEjyHJjBqzBYrPF6Ey7w4CMMMnk8n3rIYufdQHJjBIr+2aQIv15kKOlyuqj/kNIGz/akFuNRmu3NQCCTPTODhoJvLAnVtGsxA0TUTh8vSYASzYuBfATMXpMBMBwzsOYYCPyfEek8DjO85O5E0OrYs4Vc6ZkYeHcxAsQcm8PnWcZpQCGKhiKXAJQwJmj6dazkUWEts6ksD3OsO6f9rquPjKWs1Ha3Mp8AyKYb9A8eYIlWj0aixdBBFEkGNwKKWyaTBdTvz3YYG+wpw/veo6GD21ys6+L033fMD63MpvDE3O61RM5udVsfFSgQf1o7zx4K5UkXlylfY1OMOAAD//4jI0a8AAAAGSURBVAMAHxh3n6zjv4sAAAAASUVORK5CYII=",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABWElEQVR4AeyVMcoCQQyFx/8CP9gJgnoIL2Br5QG8g0fxDmLtJbTa3sJGBSsLwdpi9cHuojEzvuyOzaDw2E0myTcJUf+c4TMbj3JNhhJVqAlcZYmX8jLCHTRN4H7732kKEjyHJjBqzBYrPF6Ey7w4CMMMnk8n3rIYufdQHJjBIr+2aQIv15kKOlyuqj/kNIGz/akFuNRmu3NQCCTPTODhoJvLAnVtGsxA0TUTh8vSYASzYuBfATMXpMBMBwzsOYYCPyfEek8DjO85O5E0OrYs4Vc6ZkYeHcxAsQcm8PnWcZpQCGKhiKXAJQwJmj6dazkUWEts6ksD3OsO6f9rquPjKWs1Ha3Mp8AyKYb9A8eYIlWj0aixdBBFEkGNwKKWyaTBdTvz3YYG+wpw/veo6GD21ys6+L033fMD63MpvDE3O61RM5udVsfFSgQf1o7zx4K5UkXlylfY1OMOAAD//4jI0a8AAAAGSURBVAMAHxh3n6zjv4sAAAAASUVORK5CYII=",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABk0lEQVR4AeyWPU4DMRCFFy6ARIcUCThELkBLxQG4Q46SOyBqKloqqLanoAGkVBRI1BSLn6VZeUdje2bWSIm0UWZtz8/7xpMoynFneG2urwbJDBJjqhoM4FjFNojBmLt4VIG56MXpSZdakZAJqsBUS7DN3UN00Qp/dBgeJvDH90+U3t7eTNZ4CA8+meDKvk3grIojYAK/vL6JiPvnvsvFxILgVINJGBBuQSe+t49PR3GjeKjBCq1ufbkaYJpcFdhyE0A18CoYIjAIWqxWUwTXinkj/ftu/mdshVITWrh44xSqFSKwdhXB2uI5ec3B6bRKjTUFa6FoqAq2iEFQayK4xReqpiGC0XWtEDk509RmwRDlAl+/Z51kyCXjNeTnaxGMZBICEGfJEINRrpTDfVUwCiCKtaWpwC2BpNUUfL5aDyRcW5uCa7A0voDTafzr/jBH/bnr1X91+PjcN54DRRNuMIph3gZcYDsMLU7NBZ5K+E4L2Dc3R9UyasfQfCX7PeoWPxh8LtobDwHOa8czxWgdA4XNHwAAAP//zBoyfgAAAAZJREFUAwALXZGiVYUoEwAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABk0lEQVR4AeyWPU4DMRCFFy6ARIcUCThELkBLxQG4Q46SOyBqKloqqLanoAGkVBRI1BSLn6VZeUdje2bWSIm0UWZtz8/7xpMoynFneG2urwbJDBJjqhoM4FjFNojBmLt4VIG56MXpSZdakZAJqsBUS7DN3UN00Qp/dBgeJvDH90+U3t7eTNZ4CA8+meDKvk3grIojYAK/vL6JiPvnvsvFxILgVINJGBBuQSe+t49PR3GjeKjBCq1ufbkaYJpcFdhyE0A18CoYIjAIWqxWUwTXinkj/ftu/mdshVITWrh44xSqFSKwdhXB2uI5ec3B6bRKjTUFa6FoqAq2iEFQayK4xReqpiGC0XWtEDk509RmwRDlAl+/Z51kyCXjNeTnaxGMZBICEGfJEINRrpTDfVUwCiCKtaWpwC2BpNUUfL5aDyRcW5uCa7A0voDTafzr/jBH/bnr1X91+PjcN54DRRNuMIph3gZcYDsMLU7NBZ5K+E4L2Dc3R9UyasfQfCX7PeoWPxh8LtobDwHOa8czxWgdA4XNHwAAAP//zBoyfgAAAAZJREFUAwALXZGiVYUoEwAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABdUlEQVR4AdyXMW4CMRBFl1wgUrpISEkOwQXSpsoBcgeOkjtEqXOJpNo+BQ0gUVEgUVMsfMQs0jAzHpsdpN1IX16Px//522m4qzL+pm+vjaQMi7bVDQaw3cU+rDXW2k5dYG78/HBfcbWOzg8XmLwIRnOM068fDNnKAi822wvA58f7Rc1TyAJ7DL09WeC//5no+/1bV5OXcSMuKkU3mKCAcCneZtkNNl0KFl1gSlvgr24xwXg3SN19xYIKjgLSWVUwNUSNSXA9X40i4ElwBBSeSXDUWyfBOF2EVHDU21IIFUwNUaMJRmooAm6COXC9e6wk8T7P3AUmmGaYWpf2ucDSxmtrwwA/jScN5LmNYSSmpJ7Uw0qM5KnU/Uy8XNUjCAklWan7mZhSlqS+UWI64nksBvOUmENna/urGCzZWv9MvL9TMDe35p2BtbTa9XcGttJJa/0G514zbiAssfa2gEIusGWipYW5JReYGxwOcixxKOonJX9h7gEAAP//uAsL5wAAAAZJREFUAwAvcodfllAq0QAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABdUlEQVR4AdyXMW4CMRBFl1wgUrpISEkOwQXSpsoBcgeOkjtEqXOJpNo+BQ0gUVEgUVMsfMQs0jAzHpsdpN1IX16Px//522m4qzL+pm+vjaQMi7bVDQaw3cU+rDXW2k5dYG78/HBfcbWOzg8XmLwIRnOM068fDNnKAi822wvA58f7Rc1TyAJ7DL09WeC//5no+/1bV5OXcSMuKkU3mKCAcCneZtkNNl0KFl1gSlvgr24xwXg3SN19xYIKjgLSWVUwNUSNSXA9X40i4ElwBBSeSXDUWyfBOF2EVHDU21IIFUwNUaMJRmooAm6COXC9e6wk8T7P3AUmmGaYWpf2ucDSxmtrwwA/jScN5LmNYSSmpJ7Uw0qM5KnU/Uy8XNUjCAklWan7mZhSlqS+UWI64nksBvOUmENna/urGCzZWv9MvL9TMDe35p2BtbTa9XcGttJJa/0G514zbiAssfa2gEIusGWipYW5JReYGxwOcixxKOonJX9h7gEAAP//uAsL5wAAAAZJREFUAwAvcodfllAq0QAAAABJRU5ErkJggg==",
    ]},
    walk:{fps:8,loop:true,frames:[
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABoUlEQVR4AeyWsU4DMQyGU14AiQ2pEvAQtzGxMjEj3qGP0ndAzEysTDDdzsACSJ0YkJg7tHEkSznXTv5cemqvanVOLnbyf7Z7au/E7egzDvDs9malWZ+mwRUT0AJQjMyKa34ILEUvz05dbJpwzgeBWYRhs8fn4OKZ/MFRMBSBv//+g/T84a4zh4UfZGe8y7yKwKZKj0AR+P3jU0U8vbXOiqkHvBMGszBBpHmdcM1fXifhBhhgMKBVtAUCl1SC0iEwKib3NVfTlfTxeqtgCWq/FuZ3DoGlIGddM2fBpVB0fxZcU1Xq7CBgpOokGBFIVZWKJcGpg1osfopzSW8VrCVj+UxwLmNLEPWbYE3gd3nuNIv3xu2O/fIeAjNMHua1jN9fNxwy5ywYrUAScvAkOHdYwnjt38UmZLzWZhPcB3oxbcx/Iwk3wbQxlzXt6WsmeEgoJWuCKTikHcFDdrejXdXqn0VrvlN1KG5zVQXelMM94wPXtJn6Mr6KKWvN0N/rw6lY64Lm2++K/ROsJR18MibXYZMywBWTIBlp8Ez3ZHKNPGBrAAAA//9a2WeTAAAABklEQVQDAMRAig4ZA8I5AAAAAElFTkSuQmCC",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABoklEQVR4AeyWsU4DMQyGU14AiQ2pEvAQtzGxMjEj3qGP0ndAzEysTDDdzsACSJ0YkJgZjjitpTSN499p1KrSVeckZzv/l7i63B25Pf0OAzy7vhpyVlM0eMcElAAUI5PiOT8ETkXPT45dbDlhzQeBWYRhs/vH4OKe/MFhaEzgz5/fID2/u1nrw41v0sp4l3iZwKJKRcAEfn17zyIeXnonxbITvBMGszBBUvM64Zo/PU/CAGhgMKBlSoHAlp2gdAiMilnymoK7i+lAhiwAAqNiBLy97Jx/zgcal0wFW6AlUBpTwekE7Z4fNS2vOVgDcrwIri1z/7FQD5IimFdn6REo6TUFo9AiuLbMJIqYacfff6cuZwgozYHADEsn870W57y4F8F0ArHFE1qNRbD/npqwtYLFOiI4TkLHZ9NOPaNZqymYRZF+BCNVapIzltpUxq9Fr77+loKb7VjqzZoInm3KTJJqqS3HIAmipoJRIcqzVKEazJXwMGKy7fQlER4pvwDqyXgRxb56x7HqChq71LEK9qJOMlW9kKCCC3MpBP+nlBzbPwAAAP//Z8zTkwAAAAZJREFUAwCQ2JRRgfnAPgAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABgUlEQVR4AeyWPU7EMBCFDRdAokNaCThEOipaKmrEHfYoewdETUVLBVV6ChpA2ooCiZoi+FlYcpxx5s1itD/KKhPbM573ZUZOtPtuTb/tAM8vzjvJVmkaXTGAJQBisFJc8lPgXPTk8MClJglrPgocRSJsfnMXXHGEPzgMNxP47fMrSC+uL3tjWPhb3hnvKl4mcFFlhYAJ/PT8IiJuH1tXiokJ3kmDozAguXmdcC3uH/bChLjRYELLtIUCWyph6RSYFbPsGwX716aDWQTZvaNgHCIINaezDqNm7D7ojIKxIcIxr2kq2Apjq64OZh90Avc61b4u6U9gL5FYqK3+L7gKTh/+4/vISZbuYecUOMJKolpcyqPAUqLkuzprHIx5l6uC/X+wcBiZc1EVfDxrugiXOpL6qoJTYW0+gbUOVYtPrTa18n3ZhvdWTxrumFo97EnB85c2Q3J3Wo3vNSrSbHcq1iqN8c2u2J/g+KCDMY/l60HCr4OuGIIw5MURc1i+Zg7YDwAAAP///X29xwAAAAZJREFUAwDpVoQOzXNHlgAAAABJRU5ErkJggg==",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABm0lEQVR4AeyWsU7DMBCGHV4AiQ2pEvAQ2ZhYmZgR79BH6TsgZiZWJpiyM7AAUicGJGaGcL+lq9LU5/znRKoqpfLZ7vnu/+yrleYo7OlzGODl9VWbspKi0ScG0AJgDWatp/wUuC96fnIcupYSHvJRYBVR2PL+Mbp0hD86HJ0L/PnzG6VXdzdbY/wiXb8y4jKbC2yqFCy4wK9v70nEw0sTrLVkgjhpsAoD0jfRiW319FzFCdHRYELLFUKBPSdh6RSYFUOc3PgWhnnOJgfj988BdY0C1xeLVhOYkYEPgj1QT+wgmDlhScwMLqnaJqf5WFdqG6cxyZbac1kMfdOdBZtZEyy4wN9/pyFlJftwgS2AbsZaT/lpMMRTAqU+E9y9WLippQArzwQj4fayxkDb2aKmn+lZME0sCDTBWl4dC7SzKSYYWfLeTL9DId5jWbBHyBs7g10V+1o35B3YlZ1LvVsTwzOmzJCcvNTsY5MGywmx0WjdeXQUdDRYtCsBRsNcbFTzgEeB+skHC6b/fyc/sfzuQQ3imMuIDcFkmm7/AAAA//87/v67AAAABklEQVQDAEnrjMGOnGb5AAAAAElFTkSuQmCC",
    ]},
    attack:{fps:18,loop:false,frames:[
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAABlElEQVR4AeyWPU4DMRCFvVwAiQ4pEnCIXICWigNwhxwld0DUVLRU0LA9BQU/IhUFEjVF8LMYKWs8+I13JLQR0Y69nrHfN+P1RrsT/ug3DfDi5HhdspZNoysGUAMgBtPiJT8FzkUP93bDppWEaz4KLCICW5xfJpf08CeHoTGBn98/kvTy7HTQp0Fs8p2JLvUygVWVhoAJfHv/UERc3PRBixUXRCcNFmFAcos66VpeXXfphmhoMKFlmkKB2UrmR7M1S6fArBjmsXB3MOCMUWC2CgEy8ymwCNb6/mm1Jae6VmlL3HWrLQlsB1hOM3PIplHx2+d+KJk821hpuHt8leGvPVWxwDQlxAFFvOu4V5kCQ9Db3MAHs7nkRpXsBv6mUlDM9QZDk7J/MLVN2qR4wFw/fWgxLaGSn3rGL6u+tDZEP32KcwEKnC+yj3+uYMCoCjZYPaZaCDFgzHO3aYHHbjO2r6li7X21JNQERsYwLQHEakaBxwC0BCiwtjj3WxJ0BSMRFk6B46GBJv4iBz0GmzHcw+Cv2RcAAAD//6kQsz8AAAAGSURBVAMAoi2IDnV3UBcAAAAASUVORK5CYII=",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAACB0lEQVR4AeyWsU7DQAyGr7wAEhtSJWBhYO8EEysTrAhegT5KeQUQK0ysTLDQnYEBhOjEKzAE/6c6nBLbd04asRDlz13ss7/Y16hZC3909AZPjw4rSbl6OoMZpgHg13ywdwJrSbc31kMqADS5wABCzWQMg316fYchYJzsjKt4I1xcYCG+ZZqdHUcbxvn7YhRvhMvKwcygilUo1hSDmy1+enlFfNTN4zyOuGCeg2JdMRiLZ/cPIwhzqAkHFHZrb+GHisEMROUM3N/bRQ5ROXgxWMxORn4ImrZOwKGWgwxuMFdOscGCwm/JDbaSeXyDgPH+QtaDuMHanlkQKcYNtgBNn1X1IGBUCDUfJL0fBMwAhp8eTNhUj4OCQZGgsLvAXAECNWFfoee3zwBp61xgLYlmvzw/0VyhE/jrezOk4uywYV7RgfHi6jbgnwrCfapO4DRBOv9YxL/H+qtjREfqT+cuMCqC0gSYwwZtjX9/vQYTIb5WU0XmVwUyAp6DYp2rYkpatxHBfeQCO0DoDKSGDAIu6UwxuCSZWp7gKAYLsb1Mg4FzHeoFLnm9tLYUgaWnX0Kr5ajlV+1FYCW6fqfz8HaGLFipljOZ7yovksYsWAoiG4AQTbudXcEtmrfdKwO3noQM0jaROZ4mWAr0VhYpwsUEC+tXZvoHm63M7W/OnyZXWy39sNLAvvMfAAAA//90SXHLAAAABklEQVQDANrMtECRS3ebAAAAAElFTkSuQmCC",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAACIklEQVR4AeyYvU7DQAzHr7wAEhtSJWBhYO/ExsoEOzxDH6U8A2JmYmWChe4MDCBEJ16BIfhvxZFzuS8nPSaiOufznf2zfWkqdceNvJbnZ01OEqGbUWAAE0F56XBvl8fQrWkaZwIDCAkFExuAq4dHni6O5g0r6kZQtpnAyj+oPr++sX11dcHj+mMzY8W7zejaGligwlje3g+gUi32FIP9FmuQ1u+e1k5D0W4IdYFbTMVyQsVgZElnN4NAh2gg5iIAiUi7kdDN9aWTBIrBAkTlAjw9ORZWdEQCsgi46MVgcfBHScK3x+ZyDGawVI7AVih8RMxgcRwzynnD90/AL+9fTkNHgfXDggAxERCgoT1VKs5BkUgVMAJrkZeGtlUDy+uRoJrX6dXAHcE5fkU67zKBSx4sOV+PM5iawAPviEHaHFlm8yjw98++08KR6AYbDd2HzjfYZmwYBYZjSD43a5j55w9KSkxgVATxA8IGOZgvuiWqttNDiglMFUVbJ8EBz0Gx1wSmoEVtROCcmMC5YGodnYEoU1+tAi7pTDG4JFi/pvSsGJwOY1+tBs51aBKYvl72UluPInAoe4LyU0tjG8o2FIEjIbvvNME5ici+oDkLTlXbRjRD4ZcFY1NEAIRElrV5qE8B96JZ2701cC+LdhI6pnYp/Y9AyNFamYD8sWrFPkzP/8G6GwM9d765dR0w2urQg6Udp+pR8NTAOf8geJvVxmL9AgAA//+nP7BkAAAABklEQVQDABM8yR45iN6PAAAAAElFTkSuQmCC",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAACRklEQVR4AeyVO04cQRCGe3wBS84sYflxh3XkyA5NAimCKwAnAa4AIoWElAwSNicg4CGICJCICYb6WlurVm+/ZzZjtf9UdXVXfV09OzufzJI+vXz80hKyX4n3zeCd/3/7mKSw6eSDheRafNQMJtnVjy+fjWp/c81uinnhd1iVjK3bBKZTmz27ANw5OrUjtXYwu2jXsyGmawKTqQKKL8UxRrq1NnSRbukemUHgvbPzef2DrfW5f//yOvdlQz2DGRTXahB4d/WfAXJ8MTWq7cMTc3l9Y6VQS/IuzWCKe7Xs8Pevb2Z699Rd3T7aMRe/W2LVYLpwoa5PQV8hKGuqwJOfKz0dkYhiUDbHfEpFYIAoVSg0R7exvCJwqKgbc++n+ikouVlwbMckq/T4FapjnQ/ZLDiUFIr50NyGRwHL4xPaSzI2CLzxZ8IzOweUHLEubgYDpYg8Ohjj/mXaQOaSBcsx2j91vw5/kbyJ5NfrTxWNs2CqAEf4IfFGYiPMsU5kb4HYDhH3VQT2k57fvhpE/PvKxL4g8DWGn1MTWIsCVR/78DQN3hbmfFWB6Qj5RYgh2QjvXsSS5CaqwCUdCRxoVlVgKard5AonuyW5CkzCWFoKuORkisElxWpOoxhcU7Rk7dLAuRMaBC55vGLdF4FDu1eo2hggFi8Cx5I13gLPglPdKrjFZsEtRRdzFiOjgWuPezTwYk/GhG6TrkuCQ4m1nSnIt0mwv3jM8Qc4eZq5+5ubd4tHjzr0w3ITh/pR8NDCufwgeMxuY7XeAQAA///exooeAAAABklEQVQDAEkx6h4rSdV7AAAAAElFTkSuQmCC",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAACB0lEQVR4AeyVv04DMQzGc7wAEhtSKwrv0E5MMMLCjHiH8iT0HRAzC4wwwUJ3Bgb+iE4MSMwMx32RfLJyThznrhtIbhzH/n6x72g3XI+/+dFBnbKEdF0MBjAhrB4Vg7nyZGvThcbPJb8IzLtd3N63uvPL69ZXnKoIDNGLmzvf5eLsBFtv3PeBxEcx+Pz4MCr7/v0TPaODYjAEAIBdPSwdGeKPzy8ONt0d1dhLVgyGsCSIC1D86fWT3M5qBuPF4lDuh+qzvXEYavcmMEbHQeSnOkNNS2NOFhjFMNRJkLAzKQe13LLAvCCE4CwESTnI46aCqVNeNISvgjUIdUurlk/nvcEkZF1N4LCr0/2pq6rKyvT5JjB/aQCFQl1Hv5xwHDUVvHxbiS3hGwq/RlUlHrtwOuENVDAKAIfB1wx5jflH0KwVTKrJAoeFX7/bDob4zmiKxeExUMwHlI8iMGkSlPYfq6U8d0pgqwmMjmCs3ruT8cxPoLkI3jQY4slLmMA5HTVwQFUzgRtR6kYTTnaLYhMYBUPZWsA5k8kG54hZppENtojm5K4NrE2oFzjn3yvWfRZYuj1BaY0BYvEscKyY4iVwFZzqlsAlqwouEe3WdCODga3jHgzc7ck56TFRXhIsFVo7I1C4JsFh8pD7f3Bymtrz1c65eHTU0ovFC/v6UXBfYa1eBA/ZbUzrDwAA//9jy/fDAAAABklEQVQDAEttvR4RHAG3AAAAAElFTkSuQmCC",
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAsCAYAAABygggEAAACrklEQVR4AeyVPW4UQRCFe7gAEhmSLQx3WEdEEEJCjDgD5iT4DCBSSCCECBJvTkCAbdmRA0uOHaz7a02N3vT/eteZR/umqrte1avqGXseuA2ug1cvVjVUSq9uLYxgpXAz1BRe+atVZe/RQxejldMUHvwVF9FpD3/8msIHn79NfsMZZsJ+uFUjYQp//P4zTHn47s20p/60KY7Wnwn74QbhVd0Pr18W4yeXV47GYoLWnwnHRO0wjrFGAHz5vXSG95++uj9//7n9Z7tu8XSneIJVYe0QIQXFdW0+guYf/T8zN7FVYWPr5LxYKqq+8dVqrvpdwjY5iSpkfm0yPQGrQ2NdwjwrQJGcCPsUM8ABts7ZLuE4MS4ar+HTDMDPoSnMpJpIMYAY0Bg+e8TxFTwmXTeFlaw+xQFC7GMBfg76fInPhOkKELgNaAT05M6E6QqUEuOJ3j5fOM9P6PAMSXDcmAmPezNjBbAEsAbW/oQwAbbP1Ab2PCf8Amm8ZYVhjXEmGqxIbPka6cQWJxdBLHueE36sDVlhWEbALo/PB4BfA2LwPGjYeVvMS4R12pLIxfVjh4jnuic7i0BjsuB03hLhnryTs6NA29vdD1ZvvpniF8l4cBJhjpmAkcyyZ5Oyp6L4xLDERhS/7WgkwmNSYiDXJrUExH2TtgzWr5NTyAojEjLkRjJFgWx3ubl6WeFctVxyjsdeD7dbmIK98G96crRxblWY47WEnmLGtXfB1jlbFS4dWatwz3tQFNZp4457CrdOqChcmlabOD1fFv9WlZcboiisibnuTdSs8mM/N0SXcFwoXveIxzlN4dq0cbF11k3hdYqVuWlka8LrHvfWhNOZHN/q4n+wqvBdPV+arApDuCvcC1dPtvXmtuJavHjUuRdLEzf1i8KbFm7lZ4W3OW2p1g0AAAD//8Lv5joAAAAGSURBVAMALGxXLa1u3koAAAAASUVORK5CYII=",
    ]},
  },
};
/* ============================================================ POPULATE */
let spots=[];
function collectSpots(){
  spots=[];
  for(let y=2;y<GH-2;y++)for(let x=2;x<GW-2;x++){
    if(!isReach(x,y)) continue;
    const t=T(x,y);
    if(t===WATER||t===SHOAL||SOLID[t]||t===FLOOR||t===DOOR) continue;
    spots.push([x,y]);
  }
}
function adjTo(x,y,list){
  for(const [a,b] of [[x+1,y],[x-1,y],[x,y+1],[x,y-1],[x+1,y+1],[x-1,y-1],[x+1,y-1],[x-1,y+1]])
    if(list.indexOf(T(a,b))>=0) return true;
  return false;
}
function placeNode(kind,sx,sy){
  const need={reeds:[WATER,SHOAL,MIRE,ICE],seep:[MIRE,ROCK,PINE,SHOAL],
              vein:[CLIFF,ROCK,VEIN],chips:[CLIFF,ROCK,SCREE,VEIN]};
  for(let i=0;i<70;i++){
    const s=spots[Math.floor(rnd()*spots.length)]; if(!s) return;
    const [x,y]=s, t=T(x,y);
    if(kind==="drift"||kind==="wreck"){ if(t!==SAND) continue; }
    else if(t===SAND&&rnd()<.7) continue;
    if(need[kind]&&!adjTo(x,y,need[kind])) continue;
    if(dist(x,y,sx,sy)<3) continue;
    let clash=false;
    for(const n of nodes) if(n.tx===x&&n.ty===y){ clash=true; break; }
    if(clash) continue;
    const p=tc(x,y);
    nodes.push({tx:x,ty:y,x:p.x+(rnd()*10-5),y:p.y+(rnd()*10-5),k:kind,uses:1,ph:rnd()*6.3});
    return true;
  }
  return false;
}
/* ---- how much an island will actually give up ----
   The first island has a raft in it and very little else. Each one after holds
   a little more, and the dangerous ones hold most. */
const BUDGET={
  wood :{b:7,s:2.4}, fiber:{b:6,s:1.9}, stone:{b:8,s:2.3}, ore  :{b:1,s:1.2},
  cloth:{b:2,s:0.6}, pitch:{b:2,s:0.5}, herb :{b:3,s:0.8}, berry:{b:4,s:0.9},
  bone :{b:3,s:0.7}, glass:{b:2,s:0.45}
};
const BUDGET_THEME={
  forest  :{wood:1.5,fiber:1.3,stone:0.8,ore:0.2,berry:1.4,herb:1.3,pitch:0.8,cloth:0.5,bone:0.7,glass:0.7},
  outcrop :{wood:0.8,fiber:0.9,stone:1.4,ore:0.9,berry:0.5,herb:0.6,pitch:1.3,cloth:1.4,bone:1.2,glass:1.1},
  mountain:{wood:0.6,fiber:0.7,stone:1.6,ore:2.2,berry:0.4,herb:0.7,pitch:0.7,cloth:0.4,bone:1.0,glass:1.2},
  snow    :{wood:1.1,fiber:0.8,stone:1.0,ore:0.7,berry:0.5,herb:0.8,pitch:0.6,cloth:0.8,bone:1.3,glass:1.2},
  town    :{wood:0.9,fiber:1.0,stone:0.9,ore:0.4,berry:1.0,herb:1.0,pitch:1.1,cloth:1.8,bone:0.6,glass:0.6}
};
/* stranded is not a puzzle: any island you land on without a craft holds a raft */
const RAFT_FLOOR={wood:9,fiber:6,cloth:2,pitch:1};
function budget(res){
  const B=BUDGET[res]; if(!B) return 0;
  const th=(BUDGET_THEME[ISL.theme]||{})[res];
  let n=(B.b+B.s*G.isl)*(th===undefined?1:th);
  if(ISL.threat===1) n*=1.15;
  else if(ISL.threat>=2) n*=1.35;
  return Math.max(0,Math.round(n*(.88+rnd()*.24)));
}
function scatterByBudget(sx,sy){
  const sc=SCATTER[ISL.theme]||{};
  const byRes={};
  for(const k in sc){ const g=NODES[k].give; (byRes[g]=byRes[g]||[]).push(k); }
  const already={};
  for(const n of nodes){ const d=NODES[n.k]; already[d.give]=(already[d.give]||0)+d.amt; }
  for(const res in byRes){
    let units=budget(res)-(already[res]||0);
    if(!G.craft&&RAFT_FLOOR[res]) units=Math.max(units,RAFT_FLOOR[res]-(already[res]||0));
    const kinds=byRes[res];
    let guard=0;
    while(units>0&&guard++<240){
      const k=kinds[Math.floor(rnd()*kinds.length)];
      if(placeNode(k,sx,sy)) units-=NODES[k].amt;
    }
  }
}
function farSpot(sx,sy,minTiles){
  for(let i=0;i<300;i++){
    const s=spots[Math.floor(rnd()*spots.length)]; if(!s) break;
    if(dist(s[0],s[1],sx,sy)>=minTiles&&free(s[0]*TILE+14,s[1]*TILE+14,12)) return s;
  }
  return spots.length?spots[Math.floor(rnd()*spots.length)]:[sx+6,sy];
}
let HOME_DRIFT=0;
const THREATMUL=[
  {hp:0.85,dmg:0.85,sp:0.95,mist:1.0},    // Mortal — an island you can actually clear
  {hp:1.35,dmg:1.20,sp:1.05,mist:1.3},    // Haunted — it will cost you
  {hp:1.85,dmg:1.55,sp:1.28,mist:1.8},    // Forsaken — a death trap with a reward at the bottom
  {hp:3.60,dmg:2.30,sp:1.48,mist:3.4}     // Broken — you do not come here on your own
];
function mkFoe(kind,x,y){
  const b=FOES[kind], m=THREATMUL[ISL.threat]||THREATMUL[0];
  const scale=1+G.isl*.09;
  const hp=b.hp*m.hp*scale;
  foes.push({x,y,k:kind,n:b.n,hp,mhp:hp,
    sp:b.sp*m.sp,r:b.r,dmg:b.dmg*m.dmg*(1+G.isl*.04),c:b.c,
    mist:Math.round(b.mist*m.mist*(1+G.isl*.10)),armor:(b.armor||0)+Math.floor(G.isl*.25),
    drain:b.drain,chill:b.chill,blink:b.blink,drop:b.drop,
    hit:0,stun:0,froze:0,burn:0,lit:0,t:rnd()*6,cd:0});
  return foes[foes.length-1];
}
function mkBig(tier,x,y){
  const src=tier==="boss"
    ? (ISL.threat>=3?{n:ELITE[ISL.theme],c:"#7a1f14"}:BOSS[ISL.theme])
    : MINI[ISL.theme];
  const m=THREATMUL[ISL.threat]||THREATMUL[0];
  const s=tier==="boss"
    ? {hp:(430+70*G.isl)*m.hp*(ISL.threat>=3?1.5:1),r:ISL.threat>=3?28:23,
       dmg:22*m.dmg,sp:52*m.sp,mist:Math.round(190*m.mist*(1+G.isl*.1))}
    : {hp:(180+48*G.isl)*m.hp,r:18,dmg:17*m.dmg,sp:56*m.sp,mist:Math.round(70*m.mist*(1+G.isl*.1))};
  const f={x,y,k:tier,n:src.n,c:src.c,hp:s.hp,mhp:s.hp,r:s.r,dmg:s.dmg,sp:s.sp,mist:s.mist,
    armor:tier==="boss"?3:2,hit:0,stun:0,froze:0,burn:0,t:0,cd:0,pulse:3.2,sum:7,asleep:true,big:true,
    drop:tier==="boss"?{hide:1,pitch:1,cloth:1,glass:.8,ore:.6}:{hide:.9,bone:.8,glass:.4}};
  foes.push(f); return f;
}

function mkAnimal(kind,x,y,extra){
  const b=ANIMALS[kind];
  const a=Object.assign({x,y,gx:x,gy:y,k:kind,n:b.n,r:b.r,sp:b.sp,c:b.c,
    hp:b.hp,mhp:b.hp,dmg:b.dmg||0,mist:b.mist||2,meat:b.meat||1,
    flee:b.flee,pack:b.pack,territory:b.territory,tame:0,pet:false,fed:0,
    face:rnd()*6.28,ph:rnd()*6.3,cd:0,wt:0,hurt:0,down:0,bleed:0,young:0},extra||{});
  animals.push(a); return a;
}
function spawnAnimals(sx,sy){
  const pl=ANIMAL_POOL[ISL.theme]||["deer"];
  let n;
  if(ISL.home) n=rnd()<.12?1:0;                       // your own island is quiet
  else if(ISL.threat>=3) n=0;                         // nothing living stays in a Broken place
  else n=rnd()<.42?0:1+Math.floor(rnd()*(ISL.threat===0?3:2));
  for(let i=0;i<n;i++){
    const s=farSpot(sx,sy,7), p=tc(s[0],s[1]);
    const k=pick(pl);
    const a=mkAnimal(k,p.x,p.y);
    if(k==="wolf"&&rnd()<.6){                          // wolves come in twos
      const q=farSpot(sx,sy,7), r=tc(q[0],q[1]); mkAnimal("wolf",r.x+20,r.y+10);
    }
  }
}
function populate(sx,sy){
  collectSpots();
  // the wreck you came in on
  for(let i=0;i<5;i++){
    for(let k=0;k<40;k++){
      const a=rnd()*6.28,d=1.5+rnd()*4;
      const x=Math.round(sx+Math.cos(a)*d), y=Math.round(sy+Math.sin(a)*d);
      if(T(x,y)!==SAND) continue;
      const p=tc(x,y);
      nodes.push({tx:x,ty:y,x:p.x,y:p.y,k:i<3?"wreck":"drift",uses:1,ph:rnd()*6.3}); break;
    }
  }
  props.push({k:"hull",x:sx*TILE+14,y:sy*TILE+14,a:rnd()*6.28});
  // resources, to a budget rather than by the handful
  scatterByBudget(sx,sy);
  // foes
  const pl=pool(ISL.theme);
  const RULES=[
    {n:3+Math.floor(G.isl*.4),  minis:[.30,0,0],   bosses:[.06,0]},
    {n:6+Math.floor(G.isl*.9),  minis:[1,.40,0],   bosses:[.65,.10]},
    {n:9+Math.floor(G.isl*1.4), minis:[1,1,.50],   bosses:[1,.45]},
    {n:10+Math.floor(G.isl*1.1),minis:[1,1,1,.65], bosses:[1,1,.60]}
  ][ISL.threat];
  for(let i=0;i<RULES.n;i++){ const s=farSpot(sx,sy,11); const p=tc(s[0],s[1]); mkFoe(pick(pl),p.x,p.y); }
  ISL.bigs=[];
  let wantMini=ISL.theme==="town"?Math.max(1,RULES.minis.filter(p=>rnd()<p).length)
                                 :RULES.minis.filter(p=>rnd()<p).length;
  if(G.isl===2) wantMini=Math.max(1,wantMini);       // the third island always keeps something
  for(let i=0;i<wantMini;i++){
    const s=farSpot(sx,sy,14+i*2), p=tc(s[0],s[1]);
    const f=mkBig("mini",p.x,p.y); ISL.bigs.push(f); if(!ISL.mini) ISL.mini=f;
  }
  const wantBoss=RULES.bosses.filter(p=>rnd()<p).length;
  for(let i=0;i<wantBoss;i++){
    const s=farSpot(sx,sy,18+i*2), p=tc(s[0],s[1]);
    const f=mkBig("boss",p.x,p.y); ISL.bigs.push(f); if(!ISL.boss) ISL.boss=f;
  }
  ISL.cleared=false;
  // spirit
  let sp=.12+(ISL.threat===2?.12:0)+(ISL.ruin?.12:0);
  if(G.isl>3&&!G.regards.length) sp+=.28;     // the sea is not that cruel
  if(G.isl===2&&G.regards.length<MAXREGARDS) sp=1;   // the third island has something old awake on it
  if(G.regards.length>=MAXREGARDS) sp*=.6;    // and not that generous either
  if(rnd()<sp){
    let x,y;
    if(ISL.ruin){ x=ISL.ruin.x; y=ISL.ruin.y; }
    else { const s=farSpot(sx,sy,13); x=s[0]; y=s[1]; }
    const p=tc(x,y);
    ISL.spirit=mkSpirit(p.x,p.y);
  }
  // castaways
  let cn=ISL.theme==="town"?1+(rnd()<.5?1:0):(rnd()<.28?0:rnd()<.78?1:2);
  for(let i=0;i<cn;i++){ const s=farSpot(sx,sy,8), p=tc(s[0],s[1]); neutrals.push(mkCastaway(p.x,p.y)); }
  if(ISL.theme==="town") mkTown(sx,sy);
  spawnAnimals(sx,sy);
}

function mkCastaway(x,y){
  const nm=pick(FIRST)+" "+pick(LAST);
  const mhp=58+Math.floor(rnd()*36)+G.isl*4;
  const c={x,y,gx:x,gy:y,r:11,n:nm,role:pick(CAST_ROLE),line:pick(CAST_LINE),
    hp:mhp,mhp,dmg:9+Math.floor(rnd()*7)+Math.floor(G.isl*.6),sp:132,face:rnd()*6.3,
    c:pick(["#54626b","#6a5a49","#4e5f52","#6b5566","#5d6350"]),cd:0,ph:rnd()*6.3,
    down:0,bleed:0,joined:false,asked:0,want:null};
  const r=rnd();
  if(G.trade&&G.trade.b==="faith"&&r<.45) c.want={k:"free"};
  else if(r<.22) c.want={k:"free"};
  else if(r<.62){ const it=pick(["herb","berry","cloth","wood","bandage","hide"]);
    c.want={k:"item",id:it,n:it==="wood"?4:2}; }
  else if(r<.85) c.want={k:"kills",n:G.kills+3+Math.floor(rnd()*4)};
  else c.want={k:"mini"};
  return c;
}
function mkSpirit(x,y){
  const kinds=[
    {n:"a shape that is mostly antler",ask:"glass",amt:2,line:"It does not look at you so much as it stops looking away."},
    {n:"a drum with nobody behind it",ask:"bone",amt:3,line:"There is a beat and then a gap where the next beat should be."},
    {n:"a cold that stands up",ask:"herb",amt:4,line:"The air in front of you has edges."},
    {n:"something wearing a lamp keeper",ask:"pitch",amt:2,line:"It holds a light that throws no shadow, including its own."}
  ];
  const k=pick(kinds);
  const task=rnd()<.45?{k:"kill",need:"mini"}:{k:"bring",id:k.ask,n:k.amt};
  return {x,y,r:13,n:k.n,line:k.line,task,stage:0,ph:rnd()*6.3,done:false};
}

/* ---- town people & the small business of a town ---- */
const STORIES=[
  {id:"ledger",give:"The ledger pages",
   open:"Somebody has been taking pages out of my shipping book. Not tearing. Cutting. And whatever is doing it walks like a man and does not knock.",
   task:"Kill whatever is out there walking like a man.",
   need:"mini",
   mid:"It is still out there. You would know it if you saw it — nothing on this island stands that straight.",
   close:"That is the whole of the good news, then. Take the sailcloth and the pitch. I have no use for a boat I am afraid to sail.",
   rw:{cloth:4,pitch:2},mist:45},
  {id:"breakwater",give:"The breakwater",
   open:"The breakwater took the winter badly and the next big water takes the netloft with it. I need timber and something to seal it. I cannot go into the trees any more. I have my reasons and they are recent.",
   task:"Bring six wood and one pitch to the breakwater.",
   need:{wood:6,pitch:1},
   mid:"Six timber and a pot of pitch. I am not being particular, I am being exact.",
   close:"Then the netloft sees another year. Here — ore, cloth, and my cousin's boy, who will not stay behind now that he has seen you.",
   rw:{ore:2,cloth:3},mist:35,recruit:true},
  {id:"bell",give:"The quiet bell",
   open:"My sister rang the bell every evening of her life and last evening she did not. She went out past the last house and the fog took the sound of her before it took the shape.",
   task:"Find her out past the houses, and bring her back alive.",
   need:"rescue",
   mid:"Past the last house. Keep the water on your left and do not answer anything that says my name.",
   close:"She is thinner than she went out and she will not say what counted at her. Take the glass. Take the root. Ring the bell yourself tonight if you like.",
   rw:{glass:2,herb:3},mist:55}
];
function mkTown(sx,sy){
  const st=JSON.parse(JSON.stringify(pick(STORIES)));
  st.stage=0; ISL.story=st;
  const nb=ISL.buildings.length;
  const cnt=4+Math.floor(rnd()*4);
  for(let i=0;i<cnt;i++){
    let x,y;
    if(nb&&i<nb){ const b=ISL.buildings[i]; const p=tc(b.dx,b.dy+1); x=p.x; y=p.y+8; }
    else { const s=farSpot(sx,sy,5), p=tc(s[0],s[1]); x=p.x; y=p.y; }
    if(!free(x,y,11)){ const s=farSpot(sx,sy,4), p=tc(s[0],s[1]); x=p.x; y=p.y; }
    townies.push({x,y,gx:x,gy:y,r:11,n:pick(FIRST)+" "+pick(LAST),role:pick(CAST_ROLE),
      face:rnd()*6.3,ph:rnd()*6.3,c:pick(["#5b6a6f","#6d5a48","#4f6157","#6a5566"]),
      line:pick(["Nobody sails in this month. So you didn't sail in, did you.",
                 "There's a lamp still lit in the old keeper's house and no keeper in it.",
                 "Don't go west past the fence at dusk. That's all. That's the whole of the advice.",
                 "You'll want the trade post. If Vosbury's in a mood, come back after.",
                 "Winter merchants are late. Three weeks late. We've stopped setting the place."]),
      giver:false,trader:false});
  }
  townies[0].giver=true; townies[0].line=st.open;
  if(townies[1]){ townies[1].trader=true; townies[1].traded={}; }
  if(st.need==="rescue"){
    const s=farSpot(sx,sy,17), p=tc(s[0],s[1]);
    const c=mkCastaway(p.x,p.y);
    c.n=townies[0].n.split(" ")[0]==="?"?c.n:pick(FIRST)+" "+townies[0].n.split(" ")[1];
    c.want={k:"free"}; c.lost=true; c.line="You are not the fog. Say something the fog wouldn't say.";
    neutrals.push(c); st.lost=c;
  }
  if(st.need==="mini"&&!ISL.mini){
    const s=farSpot(sx,sy,15), p=tc(s[0],s[1]); ISL.mini=mkBig("mini",p.x,p.y);
  }
}

/* ============================================================ ISLAND BUILD */
/* ============================================================ THE PERMANENT ISLAND */
function saveHome(){
  if(!ISL.home||!HOME) return;
  HOME.grid=grid.slice(); HOME.gw=GW; HOME.gh=GH;
  HOME.ISL=JSON.parse(JSON.stringify({name:ISL.name,theme:ISL.theme,threat:ISL.threat,
    buildings:ISL.buildings,start:ISL.start,ruin:ISL.ruin,cleared:true,home:true}));
  HOME.nodes=nodes.map(n=>Object.assign({},n));
  HOME.props=props.map(p=>Object.assign({},p));
  HOME.builds=builds.map(b=>Object.assign({},b));
  HOME.animals=animals.filter(a=>!a.pet).map(a=>Object.assign({},a));
  HOME.residents=(HOME.residents||[]);
  HOME.dug=Object.assign({},G.dug);
  HOME.left=G.isl;
}
function loadHome(){
  resizeWorld(HOME.gw||ISL_W,HOME.gh||ISL_H);
  gates=[];
  ISL=Object.assign({spirit:null,story:null,mini:null,boss:null,bigs:[],seen:0,dests:null},HOME.ISL);
  ISL.home=true; ISL.cleared=true;
  grid.set(HOME.grid);
  nodes=HOME.nodes.map(n=>Object.assign({},n));
  props=HOME.props.map(p=>Object.assign({},p));
  builds=HOME.builds.map(b=>Object.assign({},b));
  const pets=animals.filter(a=>a.pet);
  animals=HOME.animals.map(a=>Object.assign({},a)).concat(pets);
  foes=[]; neutrals=[]; townies=[]; parts=[]; zones=[]; shots=[]; corpses=[];
  G.dug=Object.assign({},HOME.dug||{});
  const away=Math.max(0,(G.isl-(HOME.left||0)));
  const pass=90+away*140;                              // time moves while you are gone
  homeTick(pass,true);
  const st=ISL.start||[Math.floor(GW/2),Math.floor(GH/2)];
  floodReach(st[0],st[1]); collectSpots();
  const p=tc(st[0],st[1]);
  player.x=p.x; player.y=p.y;
  if(!free(player.x,player.y,player.r)){
    for(let rr=1;rr<12;rr++){ let done=false;
      for(let dy=-rr;dy<=rr&&!done;dy++)for(let dx=-rr;dx<=rr&&!done;dx++){
        const q=tc(st[0]+dx,st[1]+dy);
        if(isReach(st[0]+dx,st[1]+dy)&&free(q.x,q.y,player.r)){ player.x=q.x; player.y=q.y; done=true; }
      }
      if(done) break; }
  }
  player.inv=0; player.roll=0; player.cd=0;
  for(const m of party){ m.x=player.x+(rnd()*40-20); m.y=player.y+(rnd()*40-20); m.down=0; m.bleed=0; }
  for(const a of animals) if(a.pet){ a.x=player.x+(rnd()*50-25); a.y=player.y+(rnd()*50-25); a.gx=a.x; a.gy=a.y; }
  // whoever you left here comes out to meet you
  const res=HOME.residents||[];
  res.forEach((m,i)=>{
    m.x=player.x+40+Math.cos(i*1.7)*50; m.y=player.y+30+Math.sin(i*1.7)*50;
    m.gx=m.x; m.gy=m.y; m.down=0; m.bleed=0; m.resident=true;
    townies.push(m);
  });
  G.tint=THEMES[ISL.theme].tint;
  spawnT=1e9; G.mode="play";
  strandGuard(ISL.start);
  logit("home",`${ISL.name}. ${away?away+" crossing"+(away>1?"s":"")+" away.":"Where you left it."}`);
  syncHUD(); drawChart();
}
function claimHome(){
  HOME={grid:grid.slice(),gw:GW,gh:GH,ISL:null,nodes:[],props:[],builds:[],animals:[],residents:[],dug:{},left:G.isl};
  ISL.home=true; ISL.cleared=true;
  foes=[];                                             // and it stays that way
  spawnT=1e9;
  saveHome();
  logit("claimed",`${ISL.name} is yours. Nothing grows back on it that you have already put down.`);
}
/* growth, smelting and breeding, whether or not you are watching */
function homeTick(dt,bulk,here){
  HOME_DRIFT=(HOME_DRIFT||0)+dt;
  if(HOME_DRIFT>420){
    HOME_DRIFT=0;
    let drift=0;
    for(const n of nodes) if(n.k==="drift"||n.k==="wreck") drift++;
    if(drift<8&&ISL.start&&spots.length){
      if(placeNode(rnd()<.7?"drift":"wreck",ISL.start[0],ISL.start[1])&&!bulk)
        toast("the tide","Something on the sand that was not there yesterday.");
    }
  }
  for(const b of builds){
    if(b.k==="plot"&&b.seed&&!here){        // nothing grows while you stand over it
      const c=CROPS[b.seed];
      const hands=(HOME&&HOME.residents?HOME.residents.length:0);
      b.grow=(b.grow||0)+dt*(1+hands*.35);
      if(b.grow>=c.time) b.ripe=true;
    }
    if(b.k==="furnace"&&b.q&&b.q.length){
      b.heat=(b.heat||0)+dt*(here?3:1);
      while(b.q.length&&b.heat>=b.q[0].t){
        b.heat-=b.q[0].t;
        const j=b.q.shift();
        b.out=b.out||{};
        b.out[j.give]=(b.out[j.give]||0)+j.n;
        if(!bulk) toast("the furnace","Something is ready in it.");
      }
    }
    if(b.k==="pen"&&b.breeding&&!here){     // they wait until you have somewhere else to be
      b.bt=(b.bt||0)-dt;
      if(b.bt<=0){
        const kind=b.breeding; b.breeding=null;
        const a=mkAnimal(kind,b.x+(rnd()*40-20),b.y+(rnd()*40-20),{young:1,hp:ANIMALS[kind].hp*.6});
        G.bred++;
        if(!bulk) toast("born",ANIMALS[kind].n+", in the pen.");
        else logit("born",ANIMALS[kind].n+" while you were away.");
      }
    }
  }
}
function rollTheme(first){
  if(first) return pick(["forest","outcrop","snow","mountain"]);
  return rnd()<.20?"town":pick(["forest","outcrop","snow","mountain","forest"]);
}
function rollThreat(){
  const cap=G.isl<2?0:G.isl<4?1:2;
  let t=0;
  if(cap>=1&&rnd()<.55) t=1;
  if(cap>=2&&t===1&&rnd()<.42) t=2;
  return t;
}
function newIsland(theme,threat){
  if(theme==="__home"){ loadHome(); return; }
  ISL={theme,threat,name:(threat>=3?pick(ISL_A)+" "+pick(["Deep","Break","Hollow","Wound"]):pick(ISL_A)+" "+pick(ISL_B)),
       buildings:[],spirit:null,story:null,mini:null,boss:null,ruin:null,seen:0};
  const pets=animals.filter(a=>a.pet);
  nodes=[];foes=[];neutrals=[];townies=[];parts=[];zones=[];shots=[];corpses=[];props=[];
  animals=pets;builds=[];
  resizeWorld(ISL_W,ISL_H);
  gates=[];
  let start=null,tries=0,ok=0;
  do{
    newNoise(); layTerrain(theme);
    if(theme==="town") ISL.buildings=layTown();
    else if(rnd()<.45) ISL.ruin=layRuin(theme);
    start=findShore(); ok=floodReach(start[0],start[1]); tries++;
  } while(ok<240&&tries<9);
  const p=tc(start[0],start[1]);
  player.x=p.x; player.y=p.y;
  if(!free(player.x,player.y,player.r)){
    for(let rr=1;rr<10;rr++){ let done=false;
      for(let dy=-rr;dy<=rr&&!done;dy++)for(let dx=-rr;dx<=rr&&!done;dx++){
        const q=tc(start[0]+dx,start[1]+dy);
        if(isReach(start[0]+dx,start[1]+dy)&&free(q.x,q.y,player.r)){ player.x=q.x; player.y=q.y; done=true; }
      }
      if(done) break; }
  }
  player.inv=0; player.roll=0; player.cd=0;
  ISL.start=[start[0],start[1]];
  populate(start[0],start[1]);
  // the party comes ashore with you
  for(const m of party){ m.x=p.x+(rnd()*40-20); m.y=p.y+(rnd()*40-20); m.down=0; m.bleed=0; }
  for(const a of animals) if(a.pet){ a.x=p.x+(rnd()*50-25); a.y=p.y+(rnd()*50-25); a.gx=a.x; a.gy=a.y; }
  G.tint=THEMES[theme].tint;
  spawnT=8; G.mode="play";
  logit("ashore",`${ISL.name} — ${THEMES[theme].n}, ${THREATN[threat]}. ${
    ["Nothing here that has not been here a long time.","Something keeps this one.",
     "Nothing on this island intends to let you leave.",
     "This is not an island. It is the place an island was, and what is standing on it has been standing a long time."][threat]}`);
  const nw=newArrivals(theme);
  if(nw.length) logit("new to you","There is something walking here you have not seen before — "+
    nw.map(k=>FOES[k].n).join(", ")+".");
  if(G.isl===7) logit("the chart","East of here the fog stops being weather. Two, maybe three more coasts.");
  syncHUD(); drawChart();
}

/* ============================================================ SAILING */
function shoreReady(){
  if(!G.craft) return false;
  const tx=Math.floor(player.x/TILE), ty=Math.floor(player.y/TILE);
  const t=T(tx,ty);
  if(t!==SAND&&t!==PLANK&&t!==ICE) return false;
  return adjTo(tx,ty,[WATER,SHOAL]);
}
function setSail(theme,threat){
  const boat=G.craft==="boat";
  if(ISL.home) saveHome();
  if(theme&&theme.indexOf("__city:")===0){
    const cid=theme.slice(7), c=cityById(cid);
    interlude("The crossing",c?c.name:"a port",
      (c&&c.arrive)||"A coast with lamps on it, which is not something this sea has shown you before.",
      ()=>{ enterCity(cid); });
    return;
  }
  if(theme==="__home"){
    interlude("The crossing","Back to your own sand",
      "You know this water. The boat finds the same gap in the reef it found last time, and the fire you laid is "+
      "still going, because you laid it properly.",
      ()=>{ newIsland("__home",0); });
    return;
  }
  G.isl++;                                     // only a new coast counts as a new coast
  if(threat>=3){
    G.elites++;
    interlude("The crossing","Where the scraps point",
      "The four scraps of chart agree on a place the sea does not. Six hours out the water stops moving with the wind "+
      "and starts moving with something underneath it, and the fog comes up as a wall rather than as weather.<br><br>"+
      "<i>Whatever is on this one has never had to leave.</i>",
      ()=>{ newIsland(theme,3); });
    return;
  }
  interlude("The crossing",boat?"You pick the island":"The current picks the island",
    boat
      ? "The boat takes the chop badly but takes it. Fog for most of a day, then a coast comes up out of it exactly where you put it on the chart, which is the only satisfaction this sea has ever given you."
      : "The raft is four hours of holding on and one hour of paddling. Somewhere in the middle of it the water goes quiet in a way that water should not, and something goes under you long enough to lift the boards.",
    ()=>{ newIsland(theme,threat); });
}
function wreckAgain(){
  G.wrecks++;
  if(HOME){ return wakeAtHome(); }
  const lost=[];
  for(let i=0;i<G.pack.length;i++){
    if(G.pack[i]&&rnd()<(G.tools.ironhull?.32:.62)){ lost.push(ITEMS[G.pack[i].id].n); G.pack[i]=null; }
  }
  packNorm();
  for(const m of party) G.deaths.push(m.n+", drowned");
  const had=party.length; party=[];
  if(!G.tools.ironhull){ G.cache=[]; }
  else for(let i=0;i<G.cache.length;i++) if(G.cache[i]&&rnd()<.25) G.cache[i]=null;
  G.craft=null; G.hp=G.mhp*.5; G.vg=G.mvg;
  G.isl++;
  interlude("Under",had?"You come up alone":"You come up",
    "The water takes the light out from over you and then gives it back, which is not the same thing as saving you."
    +(had?" Nobody else comes up. You wait in the shallows longer than is reasonable, and then you stop waiting."
         :" You lose most of what you were carrying on the way in.")
    +"<br><br>Sand again. A different sand.",
    ()=>{ newIsland(rollTheme(false),Math.max(0,rollThreat()-1)); });
}

/* a raft's worth of material is always somewhere on your own island */
function strandGuard(where){
  if(G.craft) return 0;
  const rc=RECIPES.find(r=>r.id==="raft").c;
  const short={};
  let any=0;
  for(const k in rc){
    let held=have(k)+cacheHave(k,"hold")+cacheHave(k,"home");
    for(const n of nodes){ const d=NODES[n.k]; if(d.give===k) held+=d.amt*n.uses; }
    const need=discount(k,rc[k]);
    if(held<need){ short[k]=need-held; any+=short[k]; }
  }
  if(!any) return 0;
  const kindFor={wood:"drift",fiber:"reeds",cloth:"wreck",pitch:"seep"};
  collectSpots();
  const sx=(where&&where[0])||(ISL.start?ISL.start[0]:Math.floor(GW/2));
  const sy=(where&&where[1])||(ISL.start?ISL.start[1]:Math.floor(GH/2));
  for(const k in short){
    const kind=kindFor[k], amt=NODES[kind]?NODES[kind].amt:1;
    let units=short[k], guard=0;
    while(units>0&&guard++<80){
      if(placeNode(kind,sx,sy)) units-=amt;
      else { giveTell(k,units); units=0; }        // nowhere to put it: it is simply at your feet
    }
  }
  logit("washed up","The tide leaves enough boards and cord on the sand for a raft. It does that when you have none.");
  toast("washed up","Enough on the sand for a raft. The sea gives some of it back.");
  return any;
}

/* you drown, and the water puts you back where your fire is */
function wakeAtHome(){
  const had=party.length;
  for(const m of party) G.deaths.push(m.n+", drowned");
  party=[];
  for(const a of animals) if(a.pet) G.deaths.push(ANIMALS[a.k].n+", drowned");
  animals=animals.filter(a=>!a.pet);
  let lost=0;
  for(let i=0;i<G.pack.length;i++) if(G.pack[i]&&rnd()<.45){ G.pack[i]=null; lost++; }
  for(let i=0;i<G.cache.length;i++) if(G.cache[i]&&rnd()<.25) G.cache[i]=null;
  G.craft=null; G.hp=G.mhp*.5; G.vg=G.mvg;
  interlude("Under","You come up on your own sand",
    "The water takes the light out from over you and then gives it back, which is not the same thing as saving you."+
    (had?" Nobody else comes up.":"")+
    "<br><br>What you come up onto is a beach you know. The fire is where you laid it, because you laid it properly, "+
    "and whatever you had put up is still standing."+
    (lost?` You lose ${lost} thing${lost>1?"s":""} out of your pack on the way in, and the craft is gone entirely.`
        :" The craft is gone entirely."),
    ()=>{ loadHome(); });
  logit("under",`Drowned off ${ISL.name}. Woke on ${HOME.ISL?HOME.ISL.name:"your own island"}.`);
}

/* ============================================================ INTERLUDE */
let interThen=null, interQ=[], interOn=false;
function interlude(kick,ttl,txt,then){
  interQ.push({kick,ttl,txt,then});
  if(!interOn) showInter();
}
function showInter(){
  const it=interQ.shift();
  if(!it){ interOn=false; if(G.mode==="pause") G.mode="play"; return; }
  interOn=true; G.mode="pause"; interThen=it.then;
  $("ikick").textContent=it.kick; $("ittl").textContent=it.ttl;
  $("itxt").innerHTML=`<p>${it.txt}</p>`;
  $("inter").classList.add("on");
}
function closeInter(){
  if(!interOn) return;
  $("inter").classList.remove("on");
  const f=interThen; interThen=null; interOn=false;
  G.mode="play";
  if(f) f();
  if(interQ.length&&!interOn) showInter();
}
/* ============================================================ HUD */
let toastT=0;
function toast(head,body){ $("toast").innerHTML=`<em>${head}</em>${body}`; $("toast").style.opacity=1; toastT=4.2; }
function short(id){ const n=ITEMS[id].n; return n.length>9?n.slice(0,9):n; }
function totalRank(){ let r=0; for(const g of G.regards) r+=g.rank; return r; }

function syncBars(){
  $("hpb").style.transform=`scaleX(${clamp(G.hp/G.mhp,0,1)})`;
  $("vgb").style.transform=`scaleX(${clamp(G.vg/G.mvg,0,1)})`;
  $("hpn").textContent=Math.ceil(G.hp);
  $("vgn").textContent=Math.ceil(G.vg);
}
function syncHUD(){
  $("hpb").style.transform=`scaleX(${clamp(G.hp/G.mhp,0,1)})`;
  $("vgb").style.transform=`scaleX(${clamp(G.vg/G.mvg,0,1)})`;
  $("hpn").textContent=Math.ceil(G.hp);
  $("vgn").textContent=Math.ceil(G.vg);
  // the mist bar tracks whichever regard is currently being fed
  let feeding=null;
  for(const x of G.regards) if(x.rank<5&&(!feeding||x.rank<feeding.rank||(x.rank===feeding.rank&&x.xp<feeding.xp))) feeding=x;
  if(feeding){
    const p=regardProgress(feeding);
    $("msb").style.transform=`scaleX(${p.f})`;
    const short=feeding.R.n.replace(/^(The|Ember of the|Wreathed)\s+/,"").toLowerCase();
    $("rkl").textContent=`${short} · rank ${feeding.rank}→${feeding.rank+1}`;
    $("msn").textContent=Math.floor(feeding.xp)+" / "+p.need;
  } else {
    const cap=mistCap();
    $("msb").style.transform=`scaleX(${clamp(G.mist/cap,0,1)})`;
    $("rkl").textContent=G.regards.length?"mist · nothing left to grow":"mist · held";
    $("msn").textContent=Math.floor(G.mist)+" / "+cap;
  }
  $("who").innerHTML=G.name?`<b>${G.name}</b> · ${G.trade.n}`:"castaway";
  $("islname").innerHTML=ISL.city
    ? `<b>${ISL.name}</b> · ${ISL.sub||""}`
    : `<b>${ISL.name}</b> · ${THEMES[ISL.theme].n}`;
  $("thrl").innerHTML=`<span class="thr${ISL.city?0:ISL.threat}">${
      ISL.city?"a port · nothing here hunts":ISL.home?"yours":THREATN[ISL.threat]+" threat area"}</span> · isle ${G.isl+1}`+
    (fragCount()?` · <b>scraps ${fragCount()}/${FRAGS_FOR_ELITE}</b>`:"");
  $("packn").textContent=packCount()+"/"+packSlots()+(nearCamp()?" ·hold":"");
  $("killn").textContent=G.kills;
  $("craftl").textContent=G.craft?(G.craft==="boat"?"a boat on the sand":"a raft on the sand"):"no craft";
  // belt
  let b="";
  for(let i=0;i<packSlots();i++){
    const s=G.pack[i];
    b+=`<div class="slot${s&&itUsable(s)?" use":""}"><u>${i+1}</u>`+
       (s?`<s>${short(s.id)}</s><b>${s.n}</b>`:``)+`</div>`;
  }
  $("belt").innerHTML=b;
  // party
  let p="";
  for(const m of party){
    p+=`<div class="pm${m.down?" dn":""}"><em>${m.n.split(" ")[0]} · ${m.down?"down":m.role}${
      m.g?" · "+m.g.R.n.replace(/^(The|Ember of the|Wreathed)\s+/,"")+" r"+m.g.rank:""}</em>`+
       `<div class="hb"><i style="transform:scaleX(${clamp(m.hp/m.mhp,0,1)})"></i></div></div>`;
  }
  for(const a of animals){
    if(!a.pet) continue;
    p+=`<div class="pm"><em>${ANIMALS[a.k].n}${a.young?" · young":""}</em>`+
       `<div class="hb"><i style="transform:scaleX(${clamp(a.hp/a.mhp,0,1)})"></i></div></div>`;
  }
  $("party").innerHTML=p;
  // objective
  $("objhead").textContent=ISL.city?(ISL.sub||"the city"):ISL.home?"your island":ISL.theme==="town"?"the town":"the island";
  $("objtxt").innerHTML=objectiveText();
}
function objectiveText(){
  const L=[];
  const lab0=s=>`<span style="color:#8b9793;font-family:'Courier New',monospace;font-size:10px;letter-spacing:.14em;text-transform:uppercase">${s}</span>`;
  const co=cityObjective();
  if(co){
    L.push(lab0(co.title)+"<br>"+co.text+(co.hint?`<br><span style="color:#8b9793">${co.hint}</span>`:""));
    if(G.craft) L.push(`A craft is on the water. <b>M</b> for the chart.`);
    return L.join("<br>");
  }
  const lab=s=>`<span style="color:#8b9793;font-family:'Courier New',monospace;font-size:10px;letter-spacing:.14em;text-transform:uppercase">${s}</span>`;
  if(!G.craft){
    const rc=RECIPES.find(r=>r.id==="raft");
    const missing=[];
    for(const k in rc.c){ const need=discount(k,rc.c[k]), h=avail(k); if(h<need) missing.push(`${ITEMS[k].n} ${h}/${need}`); }
    L.push(missing.length?`Raft still wants: ${missing.join(", ")}.`:`You have raft enough. <b>C</b> to build it.`);
  } else L.push(`Stand on sand at the water and press <b>E</b> to put out.`);
  if(!HOME&&ISL.threat<2&&G.isl>0){
    const B=BUILDS.hearth, m=[];
    for(const k in B.c) if(avail(k)<B.c[k]) m.push(`${ITEMS[k].n} ${avail(k)}/${B.c[k]}`);
    L.push(m.length?`${lab("a hearth would make this island yours")}<br>Wants ${m.join(", ")}.`
                   :`${lab("a hearth would make this island yours")}<br>You have enough. <b>C</b> to lay it.`);
  }
  if(ISL.home){
    const ripe=builds.filter(b=>b.k==="plot"&&b.ripe).length;
    let out=0; for(const b of builds) if(b.k==="furnace") for(const k in (b.out||{})) out+=b.out[k];
    const bits=[];
    if(ripe) bits.push(`${ripe} bed${ripe>1?"s":""} ready`);
    if(out) bits.push(`something out of the furnace`);
    if(bits.length) L.push(lab("on your island")+"<br>"+bits.join(", ")+".");
  }
  if(ISL.story&&!ISL.story.doneAll){
    L.push(lab("town")+"<br>"+(ISL.story.stage===0?ISL.story.give+" — somebody wants a word.":ISL.story.task));
  }
  if(ISL.spirit&&!ISL.spirit.done&&ISL.spirit.stage>0){
    L.push(lab("the spirit wants")+"<br>"+(ISL.spirit.task.k==="kill"
      ? `${ISL.mini?ISL.mini.n:"the thing"} off this island.`
      : `${ISL.spirit.task.n} ${ITEMS[ISL.spirit.task.id].n} — you have ${have(ISL.spirit.task.id)}.`));
  }
  if(ISL.boss&&foes.indexOf(ISL.boss)>=0&&!ISL.boss.asleep) L.push(lab("awake")+"<br>"+ISL.boss.n+" has your scent.");
  return L.join("<br>");
}

/* ============================================================ PANELS */
const PANELS=["pack","craft","regard","chart","logp","cachep","artp","cityp"];
function isPaused(){ return NET.on ? G.halt : (G.halt||!!anyPanel()); }
function anyPanel(){ for(const p of PANELS) if($(p).classList.contains("on")) return p; return null; }
function togglePanel(id){
  if(G.mode!=="play"&&G.mode!=="dialog") return;
  const was=$(id).classList.contains("on");
  for(const p of PANELS) $(p).classList.remove("on");
  if(was) return;
  $(id).classList.add("on");
  if(id==="pack") drawPack();
  if(id==="craft") drawCraft();
  if(id==="regard") drawRegard();
  if(id==="chart") drawChart();
  if(id==="logp") drawLog();
  if(id==="cachep") drawCache();
  if(id==="artp") drawArt();
  if(id==="cityp") drawCityPanel();
}
function closePanels(){ for(const p of PANELS) $(p).classList.remove("on"); }

let packSel=-1;
function drawPack(){
  let h="";
  for(let i=0;i<packSlots();i++){
    const s=G.pack[i], sel=packSel===i;
    const style=sel?' style="border-color:var(--brass);background:rgba(224,161,65,.07)"':"";
    if(!s) h+=`<div class="cell${packSel>=0?"":" empty"}" data-i="${i}" data-e="1"${style}>
      <b>—</b><span>${packSel>=0?"move it here":"empty"}</span><u style="position:absolute;left:9px;bottom:6px;
      font-style:normal;font-family:'Courier New',monospace;font-size:9px;color:#4e5a57">${i+1}</u></div>`;
    else h+=`<div class="cell" data-i="${i}"${style}><i>${s.id==="wpn"||s.id==="arm"||s.id==="brk"?"":s.n}</i>
      <b>${itName(s)}</b><span>${itDesc(s)}</span>
      <u style="position:absolute;left:9px;bottom:6px;font-style:normal;font-family:'Courier New',monospace;
        font-size:9px;color:#4e5a57">${i+1}</u>
      <em style="position:absolute;right:8px;bottom:5px;font-style:normal;font-family:'Courier New',monospace;
        font-size:9px;letter-spacing:.1em">
        ${itUsable(s)?`<span data-use="${i}" style="color:var(--lichen);cursor:pointer">${itUsable(s)}</span> · `:""}
        <span data-drop="${i}" style="color:var(--rust);cursor:pointer">drop</span></em></div>`;
  }
  $("packgrid").innerHTML=h;
  $("packgrid").querySelectorAll(".cell[data-i]").forEach(el=>{
    el.onclick=e=>{
      const i=+el.dataset.i, t=e.target;
      if(t&&t.dataset&&t.dataset.use!==undefined){ useSlot(+t.dataset.use); packSel=-1; drawPack(); syncHUD(); return; }
      if(t&&t.dataset&&t.dataset.drop!==undefined){ dropSlot(+t.dataset.drop); packSel=-1; drawPack(); syncHUD(); return; }
      if(e.shiftKey){ useSlot(i); packSel=-1; }
      else if(packSel<0){ if(G.pack[i]) packSel=i; }
      else if(packSel===i) packSel=-1;
      else { packSwap(packSel,i); packSel=-1; }
      drawPack(); syncHUD();
    };
  });
  $("packsub").innerHTML=packSel>=0
    ? `holding slot ${packSel+1} — click another slot to put it there`
    : `click a stack to pick it up, then a slot to move it · <b>use</b> / <b>drop</b> in the corner · 1–8 uses a slot in play`;
}
function drawCraft(){
  let h="";
  const camp=nearCamp();
  h+=`<div class="sub" style="margin:-6px 0 14px">${camp
      ? "the hold is within reach — these totals include it"
      : "pack only — stand by the wreck or your craft to draw on the hold"}</div>`;
  const rows=r=>{
    const done=(r.tool&&G.tools[r.tool])||(r.craft&&G.craft===r.craft)||(r.craft==="raft"&&G.craft==="boat");
    const blocked=r.tool==="ironhull"&&G.craft!=="boat";
    const ok=canPay(r.c)&&!done&&!blocked;
    const cost=Object.keys(r.c).map(k=>{
      const need=discount(k,r.c[k]), a=avail(k);
      return `${ITEMS[k].n} ${a}/${need}`;
    }).join(" · ");
    return `<div class="rec ${done?"done":ok?"ok":"no"}" ${ok?`data-r="${r.id}"`:""}>
      <b>${r.n}</b><span>${r.d}${blocked?" <i>— no boat yet</i>":""}</span><div class="cost">${cost}</div></div>`;
  };
  for(const r of RECIPES) h+=rows(r);
  const p2=RECIPES2.filter(r=>G.plans[r.plan]&&!OLD_WPN[r.id]);
  if(p2.length){
    h+=`<div class="sub" style="margin:20px 0 10px">taken off things that were guarding something</div>`;
    for(const r of p2) h+=rows(r);
  }
  h+=`<div class="sub" style="margin:20px 0 10px">pulled apart for planting</div>`;
  for(const r of SPLITS){
    const ok=canPay(r.c);
    const cost=Object.keys(r.c).map(x=>`${ITEMS[x].n} ${avail(x)}/${r.c[x]}`).join(" · ");
    const gives=Object.keys(r.give).map(x=>`${r.give[x]} ${ITEMS[x].n}`).join(", ");
    h+=`<div class="rec ${ok?"ok":"no"}" ${ok?`data-r="${r.id}"`:""}>
      <b>${r.n}</b><span>${r.d} Gives ${gives}.</span><div class="cost">${cost}</div></div>`;
  }
  // things you put up rather than carry
  const bl=Object.keys(BUILDS).filter(k=>BUILDS[k].anywhere||ISL.home);
  if(bl.length){
    h+=`<div class="sub" style="margin:20px 0 10px">${ISL.home
        ? "standing on your own island — put up what you like"
        : "a hearth makes an island yours; everything else needs one under it"}</div>`;
    for(const k of bl){
      const B=BUILDS[k], why=whyNotBuild(k), ok=!why&&canPay(B.c);
      const cost=Object.keys(B.c).map(x=>`${ITEMS[x].n} ${avail(x)}/${B.c[x]}`).join(" · ");
      h+=`<div class="rec ${ok?"ok":"no"}" ${ok?`data-b="${k}"`:""}>
        <b>${B.n}${homeCount(k)?` — ${homeCount(k)} up`:""}</b>
        <span>${B.d}${why?` <i>— ${why}</i>`:""}</span><div class="cost">${cost}</div></div>`;
    }
  }
  // and what only a furnace can make
  const forge=atForge();
  h+=`<div class="sub" style="margin:20px 0 10px">${forge
      ? "at the furnace — iron work"
      : "iron work, and it wants a furnace on your own island to stand at"}</div>`;
  for(const r of RECIPES_HOME.filter(x=>!OLD_WPN[x.tool]&&!OLD_WPN[x.id])){
    const done=G.tools[r.tool]&&!ARM[r.tool], ok=forge&&canPay(r.c)&&!(G.tools[r.tool]&&!ARM[r.tool]);
    const cost=Object.keys(r.c).map(x=>`${ITEMS[x].n} ${avail(x)}/${r.c[x]}`).join(" · ");
    h+=`<div class="rec ${done?"done":ok?"ok":"no"}" ${ok?`data-r="${r.id}"`:""}>
      <b>${r.n}</b><span>${r.d}</span><div class="cost">${cost}</div></div>`;
  }
  /* --- the hand you fight with --- */
  h+=`<div class="sub" style="margin:20px 0 10px">weapons — each one made is made once, and your hand learns</div>`;
  h+=`<div class="rec done"><b>In hand: ${G.wpn?WPN[G.wpn.w].n:"nothing"}</b>
    <span>${G.wpn?itDesc(G.wpn):"Bare knuckles against the Sea of Hollows."}</span></div>`;
  for(const wid of Object.keys(WPN)){
    if(G.bp[wid]===undefined||wid==="oar") continue;
    const W=WPN[wid], m=G.bp[wid], ok=canPay(W.c);
    const cost=Object.keys(W.c).map(k=>`${ITEMS[k].n} ${avail(k)}/${W.c[k]}`).join(" · ");
    const f=1/(1+.3*m);
    h+=`<div class="rec ${ok?"ok":"no"}" ${ok?`data-w="${wid}"`:""}>
      <b>${W.n}${m?` — made ${m}`:""}</b>
      <span>${W.d} <i>${WPN_CLSN[W.cls]}, ~${W.dmg[0]+Math.min(m*.45,3.6)|0}±${Math.max(1,Math.round(W.dmg[1]*f))} damage.</i></span>
      <div class="cost">${cost}</div></div>`;
  }
  let brkAny=false;
  for(let bi=0;bi<packSlots();bi++){ const bs=G.pack[bi];
    if(bs&&bs.id==="brk"){ brkAny=true;
      h+=`<div class="rec ok" data-res="${bi}"><b>Research: broken ${WPN[bs.w].n.toLowerCase()}</b>
        <span>${G.bp[bs.w]===undefined?"Take it apart and the blueprint is yours.":"You know the shape — taking it apart steadies the hand."}</span></div>`; } }
  if(!brkAny) h+=`<div class="empt">Broken weapons off guardians, mini-bosses and graves can be researched here.</div>`;
  const unknown=RECIPES2.filter(r=>!G.plans[r.plan]&&!OLD_WPN[r.id]).length;
  if(unknown) h+=`<div class="empt" style="margin-top:16px">${unknown} better thing${unknown>1?"s":""} you do not know how to make.
    Mini-bosses and island guardians know.</div>`;
  $("reclist").innerHTML=h;
  $("reclist").querySelectorAll(".rec[data-r]").forEach(el=>{
    el.onclick=()=>{ doCraft(el.dataset.r); drawCraft(); syncHUD(); };
  });
  $("reclist").querySelectorAll(".rec[data-b]").forEach(el=>{
    el.onclick=()=>{ doBuild(el.dataset.b); closePanels(); };
  });
  $("reclist").querySelectorAll(".rec[data-w]").forEach(el=>{
    el.onclick=()=>{ craftWeapon(el.dataset.w); drawCraft(); syncHUD(); };
  });
  $("reclist").querySelectorAll(".rec[data-res]").forEach(el=>{
    el.onclick=()=>{ researchBroken(+el.dataset.res); drawCraft(); syncHUD(); };
  });
}
function drawRegard(){
  if(!G.regards.length){
    $("rgname").textContent="No Regard";
    $("rgsub").textContent="nothing has looked at you long enough";
    $("rgrank").innerHTML="";
    $("rgbody").innerHTML=`<div class="empt">Mist collects on you anyway — ${Math.floor(G.mist)} of a possible ${mistCap()} —
      and a person can only hold so much. Over the cap it runs off you and is gone.
      Find something willing to leave a Regard behind and it will start spending the stuff.
      Spirits keep to islands nobody wants; whatever guards an island will do just as well, if you can put it down.</div>`;
    return;
  }
  const first=G.regards[0];
  $("rgname").textContent=G.regards.map(g=>g.R.n).join("  ·  ");
  $("rgsub").textContent=`${G.regards.length} of ${MAXREGARDS} carried · ${Math.floor(G.mistTotal)} mist taken off the dead all told`;
  let rk=""; for(let i=1;i<=5;i++) rk+=`<i class="${i<=first.rank?"on":""}"></i>`;
  $("rgrank").innerHTML=rk;
  let h="";
  G.regards.forEach((g,i)=>{
    const R=g.R, p=regardProgress(g);
    let bars=""; for(let k=1;k<=5;k++) bars+=`<i class="${k<=g.rank?"on":""}"></i>`;
    h+=`<div class="ab" style="border-left-color:${i?"#7f8f6d":"#9c3f22"}">
      <b>${R.n} — ${i?"F":"Q"} · ${R.tier} tier · rank ${g.rank}</b>
      <span>of ${R.of}<br>
      <div class="rk" style="margin:8px 0 10px">${bars}</div>
      <b style="display:inline">${R.pas.n}</b> (always) — ${R.pas.d}<br>
      <b style="display:inline">${R.act.n}</b> (${i?"F":"Q"}, ${R.id==="nanook"?"most of your vigor":Math.round(R.act.cost*(g.rank>=4?.82:1))+" vigor"}) — ${R.act.d}<br>
      ${(()=>{ const u=[]; for(let k=2;k<=g.rank;k++) if(R.ups[k]) u.push(R.ups[k]);
               return u.length?"<br>Grown: "+u.join(" ")+"<br>":""; })()}
      ${g.rank<5?`<br>Rank ${g.rank+1} at ${Math.floor(g.xp)} / ${p.need} mist.`:"<br>It does not go higher than this."}
      <br><i>${R.side}</i></span></div>`;
  });
  if(G.regards.length<MAXREGARDS)
    h+=`<div class="ab locked"><b>A second Regard — F</b><span>There is room for one more. A third would have to go to
      somebody walking with you.</span></div>`;
  const carried=party.filter(m=>m.g);
  if(carried.length) h+=`<div class="ab" style="border-left-color:#a396c2"><b>Carried by others</b><span>${
    carried.map(m=>`${m.n} — ${m.g.R.n} (rank ${m.g.rank}). ${m.g.R.pas.d}`).join("<br>")}</span></div>`;
  h+=`<div class="ab"><b>Unspent mist</b><span>Holding ${Math.floor(G.mist)} of ${mistCap()}.
    ${G.regards.length?"It only piles up when everything you carry is finished growing.":"Find something to spend it on."}</span></div>`;
  $("rgbody").innerHTML=h;
}
function drawLog(){
  $("loglist").innerHTML=G.log.length
    ? G.log.map(l=>`<div class="logline"><em>${l.h}</em>${l.t}</div>`).join("")
    : `<div class="empt">Nothing has happened to you yet, which will not last.</div>`;
}

/* ---- chart ---- */
const MC=$("mcv"), mctx=MC.getContext('2d');
function chartKnows(){ return (G.trade&&G.trade.b==="chart")||!!G.tools.lantern||!!hasPas("lamp"); }
function drawChart(){
  const W=MC.width,H=MC.height,s=Math.min(W/GW,H/GH);
  const ox=(W-GW*s)/2, oy=(H-GH*s)/2;
  mctx.fillStyle="#0a1214"; mctx.fillRect(0,0,W,H);
  for(let y=0;y<GH;y++)for(let x=0;x<GW;x++){
    const t=T(x,y); let c=null;
    if(t===WATER) c="#0c181c";
    else if(t===SHOAL||t===ICE) c="#16323a";
    else if(t===SAND) c="#8b8064";
    else if(t===CLIFF||t===ROCK||t===VEIN||t===RUIN) c="#5b6165";
    else if(t===WALL||t===FLOOR||t===DOOR||t===PLANK) c="#7a5c3c";
    else if(t===TREE||t===PINE) c="#2f4a3a";
    else if(t===SNOW) c="#9fb0ae";
    else if(t===PATH) c="#8a8570";
    else c="#43554a";
    mctx.fillStyle=c; mctx.fillRect(ox+x*s,oy+y*s,Math.ceil(s),Math.ceil(s));
  }
  const mark=(x,y,c,txt)=>{
    const sx=ox+(x/TILE)*s, sy=oy+(y/TILE)*s;
    mctx.fillStyle=c; mctx.beginPath(); mctx.arc(sx,sy,3.4,0,6.3); mctx.fill();
    if(txt){ mctx.font="9px 'Courier New',monospace"; mctx.fillStyle=c;
      mctx.textAlign="center"; mctx.fillText(txt,sx,sy-6); mctx.textAlign="left"; }
  };
  if(ISL.start) mark(ISL.start[0]*TILE,ISL.start[1]*TILE,"#8b9793","wreck");
  for(const n of neutrals) if(chartKnows()||n.seen) mark(n.x,n.y,"#7f8f6d","?");
  if(ISL.spirit&&(chartKnows()||ISL.spirit.seen)&&!ISL.spirit.done) mark(ISL.spirit.x,ISL.spirit.y,"#a396c2","spirit");
  if(ISL.bigs) for(const f of ISL.bigs){
    if(foes.indexOf(f)<0) continue;
    if(!(chartKnows()||f.seen)) continue;
    mark(f.x,f.y,f.k==="boss"?"#9c3f22":"#e0a141",f.k==="boss"?"!!":"!");
  }
  mark(player.x,player.y,"#e9e2cf","you");
  // destinations
  let h="";
  if(!G.craft){
    h=`<div class="empt">No craft on the sand. Build a raft and the current decides; build a boat and you do.</div>`;
  } else if(G.craft==="raft"){
    h=`<div class="rec ok" data-d="r"><b>Put out and let it take you</b>
       <span>The raft goes where the water goes. You will find out what kind of island it was when you are standing on it.</span>
       <div class="cost">unknown coast · unknown threat</div></div>`;
  } else {
    if(!ISL.dests){
      ISL.dests=[];
      const N=G.tools.ironhull?4:3;
      for(let i=0;i<N;i++){
        const th=rollTheme(false); let tr=rollThreat();
        if(i===2) tr=Math.min(2,tr+1);
        if(i===3) tr=0;
        ISL.dests.push({theme:th,threat:tr,name:pick(ISL_A)+" "+pick(ISL_B)});
      }
    }
    h=ISL.dests.map((d,i)=>`<div class="rec ok" data-d="${i}"><b>${d.name}</b>
      <span>${THEMES[d.theme].n} · ${d.theme==="town"?"there are people on it":"nobody keeps it"}</span>
      <div class="cost"><span class="thr${d.threat}">${THREATN[d.threat]} threat area</span></div></div>`).join("");
  }
  if(G.craft){
    for(const r of cityChartRows()){
      h+=`<div class="rec ok" data-d="city:${r.id}" style="border-left-color:#b08a3c"><b>${r.name}</b>
        <span>${r.sub}${r.sub?" · ":""}${r.note}</span>
        <div class="cost"><span class="ok2">a port · nothing on it hunts</span>${r.been?" · you have been":""}</div></div>`;
    }
    // your own island: always a course you know
    if(HOME&&!ISL.home){
      const res=(HOME.residents||[]).length, bl=(HOME.builds||[]).length;
      h=`<div class="rec ok" data-d="home" style="border-left-color:var(--lichen)"><b>${HOME.ISL?HOME.ISL.name:"your island"}</b>
         <span>Yours. ${bl} thing${bl===1?"":"s"} standing${res?`, ${res} person${res===1?"":"s"} on it`:""}. Nothing regrows there.</span>
         <div class="cost"><span class="ok2">${G.craft==="raft"?"even a raft knows the way back":"a course you know by heart"}</span></div></div>`+h;
    }
    // and what four scraps of chart agree on
    if(fragCount()>=FRAGS_FOR_ELITE&&G.isl>=5&&G.craft==="boat"){
      h+=`<div class="rec ok" data-d="elite" style="border-left-color:var(--rust)"><b>Where the scraps point</b>
         <span>Four corners of a coastline nobody drew. Whatever is on it has never had to leave, and there is
         more than one of it.</span>
         <div class="cost"><span class="thr3">Broken threat area</span> · costs the four scraps</div></div>`;
    } else if(fragCount()>0&&!G.brokenRouteShown){
      h+=`<div class="empt" style="margin-top:14px">${fragCount()} of ${FRAGS_FOR_ELITE} chart scraps.
         They come off the things that keep Forsaken islands. Four, a boat, and five coasts behind you.</div>`;
    }
    if(G.broken){
      h+=`<div class="rec ok" data-d="route" style="border-left-color:var(--brass)"><b>The sea route</b>
         <span>With a Broken place emptied behind you the fog has an edge, and the edge has a way through it.
         You would not be coming back.</span>
         <div class="cost">the end of it</div></div>`;
    }
    if(G.isl>=15&&G.craft==="boat"){
      h+=`<div class="rec ok" data-d="far" style="border-left-color:var(--brass)"><b>Past the last drawn coast</b>
         <span>East of everything charted the fog stops being weather and starts being a wall. You could point the
         boat at it. You would not be coming back.</span>
         <div class="cost">the end of it</div></div>`;
    }
  }
  $("chartlist").innerHTML=h;
  $("chartsub").textContent=G.craft?"where the water goes from here":"the coast as you have walked it";
  $("chartlist").querySelectorAll(".rec[data-d]").forEach(el=>{
    el.onclick=()=>{
      if(!shoreReady()){ toast("not from here","You need to be on the sand at the water's edge."); return; }
      closePanels();
      const w=el.dataset.d;
      if(w.indexOf("city:")===0) setSail("__city:"+w.slice(5),0);
      else if(w==="r") setSail(rollTheme(false),rollThreat());
      else if(w==="home") setSail("__home",0);
      else if(w==="route"||w==="far"){
        /* the end of a run should never be one misclick away */
        say(w==="route"?"The sea route":"Past the last drawn coast","the end of it",
          [w==="route"
            ? "The gap in the fog is the width of a boat and it is not going to widen. Whatever you have not finished stays unfinished, and whoever is on your island keeps it without you."
            : "The wall of fog does not open for anybody. Going into it is a decision, not a course. Whatever you have not finished stays unfinished."],
          [{t:"Put out. It is time.",fn:()=>showEnding(w==="route"?"route":"far")},
           {t:"Not yet. There is more to do here."}]);
      }
      else if(w==="elite"){
        if(fragCount()<FRAGS_FOR_ELITE){ toast("not enough of it","Four scraps."); return; }
        spendFrags(); setSail(pick(["forest","outcrop","mountain","snow"]),3);
      }
      else { const d=ISL.dests[+w]; setSail(d.theme,d.threat); }
    };
  });
}

/* ============================================================ DIALOGUE */
const D={lines:[],i:0,opts:null,onEnd:null,owed:false};
function say(name,sub,lines,opts,onEnd){
  if(NET.host&&NET.dlgFor){ netSendDlg(name,sub,lines,opts,onEnd); return; }
  G.mode="dialog"; closePanels();
  D.lines=lines.slice(); D.i=0; D.opts=opts||null; D.onEnd=onEnd||null;
  /* something that asks you a question does not go away until you answer it.
     Escaping an offer used to discard it without taking, passing or declining. */
  D.owed=!!(opts&&opts.length);
  $("dname").innerHTML=`${name}${sub?` <b>${sub}</b>`:""}`;
  $("dbox").classList.add("on");
  showLine();
}
function showLine(){
  $("dtext").innerHTML=D.lines[D.i]||"";
  const last=D.i>=D.lines.length-1;
  $("dopts").classList.remove("on"); $("dopts").innerHTML="";
  $("dmore").textContent=last?(D.opts?"— pick one —":ADVW()):ADVW();
  if(last&&D.opts){
    $("dopts").classList.add("on");
    D.opts.forEach((o,i)=>{
      const b=document.createElement("button");
      b.innerHTML=o.t; b.disabled=!!o.off;
      b.onclick=()=>{ if(o.off) return; D.owed=false; closeDialog(true); o.fn&&o.fn(); };
      $("dopts").appendChild(b);
    });
  }
}
function advanceDialog(){
  if(D.i<D.lines.length-1){ D.i++; showLine(); return; }
  if(D.opts) return;
  closeDialog(true); const f=D.onEnd; D.onEnd=null; if(f) f();
}
function closeDialog(force){
  if(D.owed&&!force){
    toast("it is waiting on you","Pick one. Nothing here goes away because you looked away from it.");
    return false;
  }
  $("dbox").classList.remove("on"); $("dopts").classList.remove("on");
  D.opts=null; D.owed=false; G.mode="play"; syncHUD();
  return true;
}

/* ============================================================ THE HOLD */
let cacheView=null;                            // which store the panel is showing when both are near
function drawCache(){
  const ns=nearStores(), both=ns.hold&&ns.home;
  /* home store first when you are at your base; the boat does not get to hide it */
  let which = both ? (cacheView||"home") : (ns.home?"home":(ns.hold?"hold":null));
  if(which&&!ns[which==="home"?"home":"hold"]) which=ns.home?"home":(ns.hold?"hold":null);
  const atHome=which==="home", any=!!which;
  const store=which?storeArr(which):[];
  $("cacheh2").textContent=atHome?"The store":"The hold";
  $("cacheRh").textContent=atHome?"the store":"the hold";
  $("cachesub").innerHTML=!any
    ? "you are not standing by anything that holds things"
    : "click a stack to move it across · "+(atHome
        ? "what you keep on your own island — it does not go under with the boat"
        : (ISL&&ISL.city
            ? "the craft's hold, moored at the quay — it sails with you, and sinks with you"
            : "the craft's hold — it sails with you, and sinks with you"))
      +(both?` · <b id="cacheswap" style="cursor:pointer;color:#b07a2c">⇄ show ${atHome?"the hold":"the store"}</b>`:"");
  const side=(arr,wch)=>{
    let h="";
    for(let i=0;i<arr.length;i++){
      const s=arr[i]; if(!s) continue;
      h+=`<div class="row" data-w="${wch}" data-i="${i}"><b>${itName(s)}</b><span>${s.id==="wpn"||s.id==="arm"||s.id==="brk"?"":s.n} ›</span></div>`;
    }
    const used=(wch==="p"?packCount():store.filter(s=>s).length);
    const cap=(wch==="p"?packSlots():(which?storeCap(which):0));
    h+=`<div class="row flat"><b style="color:#5d6a67">${used} of ${cap} slots</b><span></span></div>`;
    return h||`<div class="empt">nothing</div>`;
  };
  $("cacheL").innerHTML=side(G.pack,"p");
  $("cacheR").innerHTML=any?side(store,"c"):`<div class="empt">nothing in reach</div>`;
  if(both){ const sw=$("cacheswap"); if(sw) sw.onclick=()=>{ cacheView=atHome?"hold":"home"; drawCache(); }; }
  if(!any) return;
  const wire=el=>{
    el.onclick=()=>{
      const i=+el.dataset.i;
      if(el.dataset.w==="p"){
        const s=G.pack[i]; if(!s) return;
        const moved=cacheGive(s.id,s.n,which);
        if(!moved) { toast(atHome?"the store is full":"the hold is full",atHome?"Even walls run out of corners.":"Ten stacks and not one more."); return; }
        take(s.id,moved);
      } else {
        const s=store[i]; if(!s) return;
        const moved=give(s.id,s.n);
        if(!moved){ toast("pack is full","Nowhere to put it."); return; }
        cacheTake(s.id,moved,which);
      }
      drawCache(); syncHUD();
    };
  };
  ["cacheL","cacheR"].forEach(id=>{
    const el=$(id), rows=el.children;
    for(let i=0;i<rows.length;i++) if(rows[i].dataset&&rows[i].dataset.w) wire(rows[i]);
  });
}

/* ============================================================ ARTWORK PANEL */
function artCounts(){
  const c={imported:0,"art.js":0,"baked in":0,"art folder":0,code:0,clips:0};
  for(const k in ART_SPEC){
    const s=IMG[k]?(ARTSRC[k]||"art folder"):"code"; c[s]=(c[s]||0)+1;
    if(hasClips(k)) c.clips++;
  }
  return c;
}
function drawArt(){
  const c=artCounts(), all=Object.keys(ART_SPEC).length, drawn=all-c.code;
  const bit=(n,label)=>n?`<b>${n}</b> ${label}`:"";
  const line=[bit(c.imported,"imported"),bit(c["art.js"],"from art.js"),
              bit(c["art folder"],"from the art folder"),bit(c["baked in"],"baked in"),
              bit(c.clips,"with moving clips"),
              bit(c.code,"still drawn in code")].filter(Boolean).join(" · ");
  $("artnote").innerHTML=
    `<b>${drawn} of ${all}</b> sprites are yours. ${line}
     <br><br>
     <span class="artbtn" id="artimport">[ import art.js or PNGs ]</span>
     <span class="artbtn" id="artlook">[ re-read art.js from disk ]</span>
     <span class="artbtn" id="artexport">[ export one art.js ]</span>
     ${Object.keys(ART_IMPORTED).length?`<span class="artbtn" id="artforget">[ forget imported ]</span>`:""}
     <br><br>
     Three ways in, and they stack — imported art wins, then an <code>art.js</code> beside this file
     (or in an <code>art/</code> folder), then anything baked in, then the game's own drawing.
     <br>• <b>Sprite Shop:</b> save its <code>art.js</code> next to this file and press
       <i>re-read art.js from disk</i> — no reload needed.
     <br>• <b>Loose PNGs:</b> import them and they are remembered between sessions. Name them
       <code>tree.png</code> or <code>tile_tree.png</code>; either works.
     <br>• Ground tiles are square and tile edge to edge. Everything else is anchored at the bottom of
       its footprint, so a tall tree or foe just draws upward. Transparent backgrounds throughout.`;
  const groups=[["ground & scenery","tile_"],["people","player|ally|castaway|townie|down"],
                ["foes","foe_"],["animals","an_"],["what you build","build_|crop_"],["city fixtures","city_"],
                ["spirit, craft & shots","spirit|prop_|shot_"],["gatherables","node_"]];
  let h="";
  for(const [title,pat] of groups){
    const re=new RegExp("^("+pat+")");
    const keys=Object.keys(ART_SPEC).filter(k=>re.test(k));
    const mine=keys.filter(k=>IMG[k]).length;
    h+=`<div><h3>${title} — ${mine}/${keys.length}</h3>`;
    for(const k of keys){
      const o=artOpt(k), got=!!IMG[k]||hasClips(k), src=(!!IMG[k]||hasClips(k))?(ARTSRC[k]||"art folder"):"drawn in code";
      const cl=clipNames(k);
      h+=`<div class="row flat"><b style="${got?"":"color:#5d6a67"}">${ART_SPEC[k][0]}</b>
        <span class="${got?"ok2":"no2"}">${o.w}×${o.h} · ${src}${cl.length?" · "+cl.join("/"):""}</span></div>`;
    }
    h+=`</div>`;
  }
  $("artlist").innerHTML=h;
  const on=(id,fn)=>{ const el=$(id); if(el) el.onclick=fn; };
  on("artlook",()=>{ toast("looking again","Re-reading art.js."); reloadArtFile(()=>setTimeout(drawArt,300)); });
  on("artimport",()=>{ const f=$("artfile"); if(f&&f.click) f.click(); });
  on("artforget",()=>{ forgetArt(); toast("forgotten","Back to art.js, baked art and the drawn versions."); setTimeout(drawArt,250); });
  on("artexport",()=>{
    const t=exportArtJS();
    try{
      const b=new Blob([t],{type:"text/javascript"});
      const a=document.createElement("a");
      a.href=URL.createObjectURL(b); a.download="art.js"; a.click();
      toast("written","art.js — put it beside this file.");
    }catch(e){ toast("could not write it","Your browser would not allow the download."); }
  });
}
/* the file picker: an art.js, or any number of PNGs */
function wireArtInput(){
  const inp=$("artfile");
  if(!inp||!inp.addEventListener) return;
  inp.addEventListener("change",()=>{
    const files=Array.from(inp.files||[]);
    if(!files.length) return;
    const entries=[]; let left=files.length; const bad=[];
    const finish=()=>{
      if(--left>0) return;
      const r=importArt(entries);
      const miss=r.skipped.concat(bad);
      toast((r.n||r.clips)?"artwork in":"nothing matched",
        (r.n?`${r.n} sprite${r.n>1?"s":""} taken up${r.kept?" and remembered":""}. `:"")+
        (r.clips?`${r.clips} of them animated. `:"")+
        (miss.length?`No slot for: ${miss.slice(0,4).join(", ")}${miss.length>4?"…":""}`:""));
      inp.value="";
      setTimeout(drawArt,300);
    };
    for(const f of files){
      const rd=new FileReader();
      if(/\.js$/i.test(f.name)){
        rd.onload=()=>{ const p=parseArtJS(String(rd.result));
          if(p) entries.push({bulk:p}); else bad.push(f.name); finish(); };
        rd.onerror=()=>{ bad.push(f.name); finish(); };
        rd.readAsText(f);
      } else {
        const key=keyForFile(f.name);
        rd.onload=()=>{ if(key) entries.push({key,data:String(rd.result)}); else bad.push(f.name); finish(); };
        rd.onerror=()=>{ bad.push(f.name); finish(); };
        rd.readAsDataURL(f);
      }
    }
  });
}

/* ============================================================ CITY PANEL */
function drawCityPanel(){
  const list=allCities();
  const imported=CITY_IMPORTED.length, onDisk=cityFile().length;
  $("citynote").innerHTML=
    `<b>${list.length}</b> city${list.length===1?"":"ies"} loaded — ${CITY_BAKED.length} baked in,
     ${onDisk} from a city.js, ${imported} imported.
     <br><br>
     <span class="artbtn" id="cityimport">[ import city.js or json ]</span>
     <span class="artbtn" id="citylook">[ re-read city.js from disk ]</span>
     ${imported?`<span class="artbtn" id="cityforget">[ forget imported ]</span>`:""}
     <br><br>
     A city is data, not code. It arrives as one file that sets
     <code>window.CITY_IMPORT</code> (or <code>window.CITIES</code> for several), and this game is the only thing
     that has to understand it — see <code>CITY_FORMAT.md</code>. Districts load one at a time through gates, so a
     city can be several times the size of an island. Nothing hostile spawns in one.
     <br>Everything below is checked when it loads; anything with an error is refused and told why.`;
  let h="";
  for(const c of list){
    const v=validateCity(c);
    const st=G.cityState[c.id];
    h+=`<div class="rec ${v.ok?"ok":"no"}" ${v.ok?`data-c="${c.id}"`:""}>
      <b>${c.name||c.id} — ${c.__src}</b>
      <span>${c.sub||""}${c.sub?"<br>":""}
        ${v.stats.districts} district${v.stats.districts===1?"":"s"} ·
        ${v.stats.npcs} people · ${v.stats.gates} gates · ${v.stats.tiles} tiles
        ${c.story&&c.story.stages?`· ${c.story.stages.length} stages`:""}
        ${st?`<br>you are ${st.stage+1} stage${st.stage?"s":""} in`:""}
        ${v.errors.length?`<br><span style="color:var(--rust)">${v.errors.length} problem${v.errors.length>1?"s":""}:</span>
           <span style="color:#a4afab">${v.errors.slice(0,6).join(" · ")}${v.errors.length>6?" …":""}</span>`:""}
        ${v.warns.length?`<br><span style="color:var(--brass)">notes:</span>
           <span style="color:#8b9793">${v.warns.slice(0,4).join(" · ")}</span>`:""}
      </span>
      <div class="cost">${!v.ok?"will not load"
          :(c.minIsl||0)>G.isl?`not this early — ${(c.minIsl||0)+1} coasts behind you first`
          :!G.ports[c.id]?"you have no chart to it — they turn up on things that keep islands"
          :G.craft?"click to sail there from a shore":"needs a craft on the water"}</div></div>`;
  }
  if(!list.length) h=`<div class="empt">No cities loaded. Put a city.js beside this file, or import one.</div>`;
  $("citylist").innerHTML=h;
  const on=(id,fn)=>{ const el=$(id); if(el) el.onclick=fn; };
  on("cityimport",()=>{ const f=$("cityfile"); if(f&&f.click) f.click(); });
  on("citylook",()=>{ toast("looking again","Re-reading city.js."); reloadCityFile(()=>setTimeout(drawCityPanel,300)); });
  on("cityforget",()=>{ forgetCities(); toast("forgotten","Back to whatever is baked in or on disk."); setTimeout(drawCityPanel,200); });
  $("citylist").querySelectorAll(".rec[data-c]").forEach(el=>{
    el.onclick=()=>{
      const c=cityById(el.dataset.c);
      if((c.minIsl||0)>G.isl){ toast("not yet","You have not been out here long enough to have heard of it."); return; }
      if(!G.ports[c.id]){ toast("you cannot find it","You would need somebody's chart. They turn up on the things that keep islands."); return; }
      if(!shoreReady()){ toast("not from here","Stand on the sand at the water's edge with a craft."); return; }
      closePanels(); setSail("__city:"+el.dataset.c,0);
    };
  });
}
function wireCityInput(){
  const inp=$("cityfile");
  if(!inp||!inp.addEventListener) return;
  inp.addEventListener("change",()=>{
    const files=Array.from(inp.files||[]);
    if(!files.length) return;
    let left=files.length; const found=[]; const bad=[];
    const finish=()=>{
      if(--left>0) return;
      const r=importCities(found);
      let msg="";
      if(r.took.length) msg+=`${r.took.join(", ")} loaded${r.kept?" and remembered":""}. `;
      if(r.failed.length) msg+=r.failed.map(f=>`${f.name}: ${f.errors[0]}`).join(" · ");
      if(bad.length) msg+=` Could not read: ${bad.join(", ")}.`;
      toast(r.took.length?"city in":"nothing loaded",msg||"Nothing in those files.");
      inp.value="";
      setTimeout(drawCityPanel,250);
    };
    for(const f of files){
      const rd=new FileReader();
      rd.onload=()=>{ const p=parseCityJS(String(rd.result));
        if(p.list.length) found.push.apply(found,p.list); else bad.push(f.name+(p.err?" ("+p.err+")":""));
        finish(); };
      rd.onerror=()=>{ bad.push(f.name); finish(); };
      rd.readAsText(f);
    }
  });
}
/* ============================================================ NEAREST / INTERACT */
function findNearest(player){
  player=player||PL1;   // the captain, unless you are asking for somebody else
  // lower priority number wins; distance only breaks ties within a priority
  const cands=[];
  const add=(o,k,lab,d,prio)=>{ if(d<48) cands.push({o,k,lab,d,prio}); };
  for(const n of nodes) add(n,"node",NODES[n.k].n,dist(n.x,n.y,player.x,player.y),3);
  for(const n of neutrals){
    const d=dist(n.x,n.y,player.x,player.y);
    if(n.down) add(n,"down","get "+n.n.split(" ")[0]+" off the ground",d,0);
    else add(n,"cast",n.n.split(" ")[0]+" — "+n.role,d,2);
  }
  for(const n of townies){
    const d=dist(n.x,n.y,player.x,player.y);
    if(n.cnpc) add(n,"town",(n.cnpc.sign?"look at ":"")+n.n+(n.role?" — "+n.role:""),d,n.cnpc.sign?3:2);
    else add(n,"town",n.n.split(" ")[0]+(n.resident?" — staying here":n.trader?" — trade post":n.giver?" — a word":" — "+n.role),d,2);
  }
  // your own people are only in the way if they need something
  for(const m of party){
    const d=dist(m.x,m.y,player.x,player.y);
    if(m.down) add(m,"down","get "+m.n.split(" ")[0]+" off the ground",d,0);
    else if(ISL.home) add(m,"party","a word with "+m.n.split(" ")[0],d,4);
    else if(m.hp<m.mhp*.6&&(have("bandage")||have("herb"))) add(m,"party","patch "+m.n.split(" ")[0]+" up",d,4);
  }
  if(ISL.spirit&&!ISL.spirit.done) add(ISL.spirit,"spirit",ISL.spirit.n,dist(ISL.spirit.x,ISL.spirit.y,player.x,player.y),1);
  const tx=Math.floor(player.x/TILE), ty=Math.floor(player.y/TILE);
  for(const [a,b] of [[tx,ty],[tx+1,ty],[tx-1,ty],[tx,ty+1],[tx,ty-1]]){
    if(T(a,b)===GRAVE&&!G.dug["".concat(a,",",b)]) add({tx:a,ty:b},"grave","open the grave",20,5);
  }
  for(const g of gates){
    const d=dist(g.x,g.y,player.x,player.y);
    if(d<Math.max(48,g.r+26)) cands.push({o:g,k:"gate",lab:g.def.label||("through to "+g.def.to),d,prio:2});
  }
  for(const b of builds){
    const d=dist(b.x,b.y,player.x,player.y);
    let lab=BUILDS[b.k].n.toLowerCase();
    if(b.k==="plot") lab=b.seed?(b.ripe?"harvest the "+CROPS[b.seed].n:CROPS[b.seed].n+", coming up"):"plant the bed";
    if(b.k==="furnace"){ let out=0; for(const k in (b.out||{})) out+=b.out[k];
      lab=out?"take iron out of the furnace":(b.q&&b.q.length?"the furnace, working":"the furnace"); }
    if(d<Math.max(48,b.r+26)) cands.push({o:b,k:"build",lab,d,prio:b.k==="plot"&&b.ripe?1:2});
  }
  for(const a of animals){
    const d=dist(a.x,a.y,player.x,player.y);
    const A=ANIMALS[a.k];
    if(a.pet) add(a,"animal",A.n+(a.hp<a.mhp?" — hurt":""),d,4);
    else add(a,"animal",A.n+(a.fed?" — fed":""),d,3);
  }
  const cl=clearTarget();
  if(cl) cands.push({o:cl,k:"clear",lab:"clear the "+CLEARABLE[cl.t].n,d:Math.min(47,cl.d),prio:4});
  const camp=nearCamp();
  if(camp) add(camp,"camp",camp.k==="hull"?"the wreck's hold":"the "+camp.k+"'s hold",
               dist(camp.x,camp.y,player.x,player.y),3);
  cands.sort((a,b)=>a.prio-b.prio||a.d-b.d);
  if(cands.length) return cands[0];
  if(shoreReady()) return {o:null,k:"sail",lab:G.craft==="boat"?"put out in the boat":"put out on the raft"};
  return null;
}
G.dug={};
function interact(asP2){
  if(G.halt) return;
  if(asP2){ netInteractP2(); return; }
  if(G.mode!=="play") return;
  const n=nearest;
  if(!n){
    const t=T(Math.floor(player.x/TILE),Math.floor(player.y/TILE));
    if((t===SHOAL||t===ICE)&&G.trade&&G.trade.b==="shoal"){ fishShoal(); }
    return;
  }
  if(n.k==="node") gather(n.o);
  else if(n.k==="cast") talkCast(n.o);
  else if(n.k==="town") talkTown(n.o);
  else if(n.k==="party") talkParty(n.o);
  else if(n.k==="down") helpUp(n.o);
  else if(n.k==="spirit") talkSpirit(n.o);
  else if(n.k==="grave") digGrave(n.o);
  else if(n.k==="clear") clearTile(n.o);
  else if(n.k==="gate") gateTalk(n.o);
  else if(n.k==="build") buildTalk(n.o);
  else if(n.k==="animal") animalTalk(n.o);
  else if(n.k==="camp") togglePanel("cachep");
  else if(n.k==="sail") togglePanel("chart");
}
function fishShoal(){
  if(player.cd>0) return; player.cd=.9;
  if(rnd()<.55){ give(rnd()<.5?"berry":"fiber",1); toast("the shallows","Something in the weed. It counts as food.");}
  else toast("the shallows","Nothing but cold up to the knee.");
  syncHUD();
}
function gather(nd){
  const def=NODES[nd.k];
  const needOk=!def.needs||G.tools[def.needs]||(def.needs==="spear"&&wpnCls()==="spear");
  if(!needOk){
    toast("no tool for it",`The ${def.n} wants a ${def.needs==="spear"?"spear in hand — any spear":def.needs}.`); return;
  }
  let amt=def.amt;
  if(def.give==="wood"&&G.tools.hatchet) amt*=2;
  if(def.give==="wood"&&G.trade&&G.trade.b==="wood") amt+=1;
  if(def.give==="berry"&&G.trade&&G.trade.b==="food") amt+=1;
  if(packCount()>=packSlots()&&have(def.give)===0){
    toast("pack is full","Nowhere to put it. Drop something — shift-click in the pack.");
    return;
  }
  const got=give(def.give,amt);
  for(let i=0;i<7;i++) parts.push({x:nd.x,y:nd.y,vx:(rnd()-.5)*120,vy:(rnd()-.5)*120-30,
    life:.4+rnd()*.3,c:def.c,s:1.6+rnd()*2});
  sfx(220,.06,"triangle",.07);
  if(got<amt) toast("pack is full",`You leave ${amt-got} ${ITEMS[def.give].n} on the ground.`);
  else toast("gathered",`${amt} ${ITEMS[def.give].n}.`);
  if(got>0){ nd.uses--; if(nd.uses<=0) nodes=nodes.filter(o=>o!==nd); }
  syncHUD();
}
function digGrave(g){
  G.dug[g.tx+","+g.ty]=1;
  const r=rnd();
  if(r<.42){ give("bone",1); toast("the grave","A bone, and the strong sense of being counted.");}
  else if(r<.60){ give("glass",1); toast("the grave","Mistglass, laid on the chest like coin.");}
  else if(r<.68){ give("cloth",1); toast("the grave","Shroud cloth. It will patch a sail.");}
  else if(r<.72){ dropBroken(rnd()<.18?BRK_HIGH:BRK_IRON,"a grave"); }
  else if(r<.86){ toast("the grave","Empty. Filled in over nothing at all.");}
  else { toast("the grave","Something was waiting in it.");
    const a=rnd()*6.28; mkFoe("hollow",player.x+Math.cos(a)*60,player.y+Math.sin(a)*60); }
  for(let i=0;i<10;i++) parts.push({x:g.tx*TILE+14,y:g.ty*TILE+14,vx:(rnd()-.5)*90,vy:-40-rnd()*60,
    life:.6,c:"#6b5540",s:2});
  syncHUD();
}

/* ============================================================ PACK USE */
function useSlot(i){
  const s=G.pack[i]; if(!s) return;
  if(s.id==="wpn"){ equipWeapon(i); return; }
  if(s.id==="arm"){ equipArmor(i); return; }
  if(s.id==="brk"){ toast(itName(s),"Past mending. Research it at the craft panel."); return; }
  const it=ITEMS[s.id];
  if(!it.use){ toast(it.n,"Nothing to do with it but build."); return; }
  if(it.use==="heal"){
    if(G.hp>=G.mhp&&!(it.revive&&nearDown())){ toast("no need","You are as whole as this island is going to make you."); return; }
    if(it.revive&&nearDown()){ helpUp(nearDown()); take(s.id,1); return; }
    let v=it.v*(G.trade&&G.trade.b==="food"?1.4:1)*(G.trade&&G.trade.b==="medic"&&s.id==="bandage"?1.4:1);
    G.hp=Math.min(G.mhp,G.hp+v); take(s.id,1); toast("used",`${it.n} — ${Math.round(v)} body back.`);
    sfx(300,.1,"sine",.07);
  } else if(it.use==="tonic"){
    G.hp=Math.min(G.mhp,G.hp+it.v); G.vg=G.mvg; take(s.id,1);
    toast("used","Tonic. Body and vigor both.");
  } else if(it.use==="port"){
    if(readPort()) take(s.id,1);
  } else if(it.use==="sever"){
    severTalk(i); return;
  } else if(it.use==="mist"){
    if(G.mist>=mistCap()){ toast("no room","You cannot hold any more mist than you are holding."); return; }
    gainMist(it.v,"mistglass"); take(s.id,1);
  }
  syncHUD();
}
function dropSlot(i){
  const s=G.pack[i]; if(!s) return;
  toast("dropped",s.id==="wpn"||s.id==="arm"||s.id==="brk"?itName(s)+".":`${s.n} ${ITEMS[s.id].n}.`);
  G.pack[i]=null;
  packNorm();
}

/* ============================================================ CRAFT */
function doCraft(id){
  const r=allRecipes().find(x=>x.id===id); if(!r||!canPay(r.c)) return;
  if(r.tool==="ironhull"&&G.craft!=="boat"){ toast("no boat","Plate what, exactly?"); return; }
  pay(r.c);
  if(r.give) for(const k in r.give) give(k,r.give[k]);
  if(r.tool&&ARM[r.tool]){
    if(!giveGear(mkArmItem(r.tool))) return;
    toast("made",ARM[r.tool].n+". It goes on from the pack.");
    logit("made",ARM[r.tool].n+".");
    sfx(180,.12,"square",.08); syncHUD(); return;
  }
  if(r.tool){
    G.tools[r.tool]=1;
    if(r.tool==="ironhull"){ ISL.dests=null; }

    if(r.tool==="crucible"){ /* mist cap and thickness are read from G.tools */ }

    if(r.tool==="cord"){ player.takeMul*=1.18; }

    if(r.tool==="lantern"){ /* the chart opens up, and so do you */ }
    toast("made",r.n+".");
    logit("made",r.n+".");
  }
  if(r.craft){
    G.craft=r.craft;
    props.push({k:r.craft,x:player.x,y:player.y+10,a:rnd()*6.3});
    toast("made",r.craft==="boat"?"A boat. Small, but it is a boat.":"A raft. It will float, mostly.");
    logit("built",r.craft==="boat"?"A small boat, on the sand of "+ISL.name+".":"A raft, on the sand of "+ISL.name+".");
    ISL.dests=null;
  }
  sfx(180,.12,"square",.08);
  syncHUD();
}

/* ============================================================ MIST & REGARD */
function gainMist(n,why,raw){
  const gain=raw?n:n*mistMul();
  G.mistTotal+=gain;
  let left=gain, guard=0;
  // mist goes into whichever regard is furthest behind; nothing is banked while one can use it
  while(left>0&&guard++<40){
    let g=null,owner=null;
    for(const x of G.regards) if(x.rank<5&&(!g||x.rank<g.rank||(x.rank===g.rank&&x.xp<g.xp))) g=x;
    if(!g){                              // yours are finished growing: it runs on into theirs
      for(const m of party){
        if(!m.g||m.g.rank>=5) continue;
        if(!g||m.g.rank<g.rank||(m.g.rank===g.rank&&m.g.xp<g.xp)){ g=m.g; owner=m; }
      }
    }
    if(!g) break;
    const need=RANKCOST[g.rank+1]-g.xp;
    if(left>=need){ left-=need; g.xp=0; g.rank++; rankUp(g,owner); }
    else { g.xp+=left; left=0; }
  }
  if(left>0){
    const cap=mistCap(), room=cap-G.mist;
    G.mist=Math.min(cap,G.mist+left);
    if(left>room){
      G.spill=(G.spill||0)+(left-room);
      if(G.spill>60){ G.spill=0;
        toast("mist runs off you",G.regards.length?"Everything you carry is as grown as it gets.":"Nothing to spend it on. It goes back into the ground.");
      }
    }
  }
}
function rankUp(g,owner){
  const R=g.R, r=g.rank;
  if(owner){
    owner.mhp=Math.round(owner.mhp*1.12); owner.hp=owner.mhp;
    owner.dmg=Math.round(owner.dmg*1.14);
    toast(owner.n.split(" ")[0]+"'s regard grows",`${R.n} is rank ${r}.`);
    logit("rank "+r,`${R.n}, carried by ${owner.n}.`);
    sfx(430,.28,"sine",.07);
    syncHUD();
    return;
  }
  let txt=`${R.n} is rank ${r}.`;
  if(r===1) txt+=` ${R.act.n} is yours.`;
  else if(R.ups[r]) txt+=` ${R.ups[r]}`;
  toast("the regard grows",txt); logit("rank "+r,`${R.n} — ${txt}`);
  G.mvg+=5; G.mhp+=6; G.hp+=6; player.dmg+=1;
  sfx(520,.3,"sine",.09);
}
function regardProgress(g){
  if(!g) return {f:0,need:0};
  if(g.rank>=5) return {f:1,need:0};
  const need=RANKCOST[g.rank+1];
  return {f:clamp(g.xp/need,0,1),need};
}
/* a plan off something big: tier 2 from mini-bosses, 3 from guardians, 4 for emptying a Forsaken island */
function grantPlan(tier,src){
  let pool=Object.keys(PLANS).filter(k=>PLANS[k].tier===tier&&!G.plans[k]&&!OLD_WPN[k]);
  if(!pool.length) pool=Object.keys(PLANS).filter(k=>PLANS[k].tier<=tier&&!G.plans[k]&&!OLD_WPN[k]);
  if(!pool.length) pool=Object.keys(PLANS).filter(k=>!G.plans[k]&&!OLD_WPN[k]);
  if(!pool.length) return false;
  const id=pick(pool); G.plans[id]=1;
  toast("you can see how it was made",`${PLANS[id].n} — ${PLANS[id].d}`);
  logit("a plan",`${PLANS[id].n}, off ${src||"something that stopped moving"}.`);
  return true;
}
function acceptRegard(R,src){
  const g={R,rank:1,xp:0};
  G.regards.push(g);
  const slot=G.regards.length;
  const banked=G.mist; G.mist=0;
  if(banked>0) gainMist(banked,"banked",true);
  toast("you are regarded",`${R.n}, on ${slot===1?"Q":"F"}. It grows on mist, and mist comes off the dead.`);
  logit("regarded",`${R.n} — ${R.tier} tier, from ${src}. ${slot===1?"Q":"F"}.`);
  syncHUD();
}
function crewRegard(R,m,src){
  m.g={R,rank:1,xp:0};
  m.mhp=Math.round(m.mhp*1.25); m.hp=m.mhp; m.dmg=Math.round(m.dmg*1.3);
  interlude("It goes over your shoulder",m.n+" is regarded",
    `You step aside and it does not mind. It was never particular about which of you it landed on.<br><br>
     <b>${m.n}</b> — ${m.role} — takes <b>${R.n}</b>. ${R.pas.d}<br><br>
     They use ${R.act.n} themselves, when it occurs to them, which is not always when you would like.`,
    ()=>{ syncHUD(); });
  logit("regarded",`${m.n} took ${R.n} on ${ISL.name}.`);
}
function declineRegard(R){
  gainMist(240,"declined",true);
  toast("it lets go of you","The Regard comes apart into mist, and the mist does not hold a grudge.");
  logit("a regard, declined",`${R.n} was offered and turned down. It came apart into mist.`);
  syncHUD();
}
function grantRegard(src,tries){
  /* anything already on screen goes first — an interlude, or a conversation
     that is mid-sentence. say() would have overwritten a live dialogue and
     taken its onEnd with it, losing whatever that talk was about to give you. */
  const busy=interOn||interQ.length||G.mode==="dialog"
            ||$("ending").classList.contains("on")||$("title").classList.contains("on");
  if(busy){
    const t=(tries||0)+1;
    if(t>400){                                   // ~2 minutes: stop waiting, keep the value
      gainMist(240,"an offer that could not find you",true);
      logit("an offer, missed",`Something held a Regard out off ${src} and did not wait. It came apart into mist.`);
      return;
    }
    setTimeout(()=>grantRegard(src,t),300); return;
  }
  const taken={};
  for(const g of G.regards) taken[g.R.id]=1;
  for(const m of party) if(m.g) taken[m.g.R.id]=1;
  let pool=REGARDS.filter(R=>!taken[R.id]);
  if(!pool.length) pool=REGARDS;
  const R=pick(pool);

  const room=G.regards.length<MAXREGARDS;
  const open=party.filter(m=>!m.g&&!m.down);
  const lines=[
    `Something old takes an interest in you, briefly, the way a person glances at a tool before picking it up. It holds a <b>Regard</b> out — and holds it, waiting.`,
    `<b>${R.n}</b>, ${R.tier} tier, of ${R.of}.<br><br>
     <b>${R.pas.n}</b> — ${R.pas.d}<br>
     <b>${R.act.n}</b> — ${R.act.d}<br><br><i>${R.side}</i>`,
    room
      ? `It is offered, not imposed. A person carries two of these and no more.`
      : (open.length
          ? `You carry two already, and it knows that. It is looking past you, at ${open[0].n}.`
          : `You carry two already, and there is nobody walking with you to take a third.`)
  ];
  const opts=[];
  if(room) opts.push({t:"Take it.",fn:()=>acceptRegard(R,src)});
  if(!room&&open.length) opts.push({t:`Let ${open[0].n.split(" ")[0]} carry it.`,fn:()=>crewRegard(R,open[0],src)});
  opts.push({t:open.length&&room
      ? `Decline — it can go to ${open[0].n.split(" ")[0]}.`
      : "Decline. Let it come apart.",
    fn:()=>{ if(open.length) crewRegard(R,open[0],src); else declineRegard(R); }});
  if(open.length&&room) opts.push({t:"Decline for everyone.",fn:()=>declineRegard(R)});
  say(R.n,"an offer",lines,opts,null);
}
/* ============================================================ TALK: SPIRIT */
function talkSpirit(s){
  s.seen=1;
  if(s.stage===0){
    const t=s.task;
    const ask=t.k==="kill"
      ? `There is a thing on this island that stands up straighter than it should. Put it down and come back.`
      : `Bring ${t.n} ${ITEMS[t.id].n}. Lay it where you are standing now.`;
    say(s.n,"a spirit",[
      `<i>${s.line}</i>`,
      `It does not speak so much as arrange the idea in you, already finished.`,
      `<i>${ask}</i>`
    ],null,()=>{ s.stage=1; logit("a spirit",ask.replace(/<[^>]+>/g,"")); syncHUD(); });
    return;
  }
  const t=s.task;
  let done=false;
  if(t.k==="kill") done=!ISL.mini||foes.indexOf(ISL.mini)<0;
  else done=have(t.id)>=t.n;
  if(!done){
    say(s.n,"a spirit",[t.k==="kill"
      ? `<i>It is still standing. You would know.</i>`
      : `<i>${t.n} ${ITEMS[t.id].n}. You have ${have(t.id)}. It is not interested in nearly.</i>`]);
    return;
  }
  if(t.k==="bring") take(t.id,t.n);
  s.done=true; G.spiritsMet++;
  say(s.n,"a spirit",[
    `The thing in front of you settles, the way a held breath settles.`,
    `<i>It looks at you properly for the first time, and being looked at properly by this turns out to be a physical event.</i>`
  ],null,()=>grantRegard("a spirit on "+ISL.name));
}

/* ============================================================ TALK: CASTAWAY */
function joinUp(c){
  if(party.length>=3){ toast("too many","Three is already more than this sea usually allows."); return; }
  neutrals=neutrals.filter(o=>o!==c);
  c.joined=true; party.push(c);
  toast("joined",`${c.n}, ${c.role}.`);
  logit("joined",`${c.n} — ${c.role} — came with you off ${ISL.name}.`);
  syncHUD();
}
function talkCast(c){
  c.seen=1;
  const w=c.want;
  const first=c.asked===0; c.asked++;
  const open=first?[`<i>${c.line}</i>`,`${c.n}. ${c.role[0].toUpperCase()+c.role.slice(1)}, before the water got involved.`]
                  :[`${c.n} looks up. <i>"Still you."</i>`];
  if(w.k==="free"){
    say(c.n,c.role,open.concat([`<i>"I'm not going to pretend I have a better plan. Say the word and I'll walk behind you."</i>`]),
      [{t:"Come with me.",fn:()=>joinUp(c)},
       {t:"Stay here. It's safer.",fn:()=>toast(c.n.split(" ")[0],"Stays where they are, and watches you go.")}]);
  } else if(w.k==="item"){
    const ok=have(w.id)>=w.n;
    say(c.n,c.role,open.concat([`<i>"I'll come. I'm not walking anywhere on nothing though. ${w.n} ${ITEMS[w.id].n}, and I'm yours."</i>`]),
      [{t:ok?`Hand over ${w.n} ${ITEMS[w.id].n}.`:`${w.n} ${ITEMS[w.id].n} — you have ${have(w.id)}.`,off:!ok,
        fn:()=>{ take(w.id,w.n); joinUp(c); }},
       {t:"Not yet.",fn:()=>{}}]);
  } else if(w.k==="kills"){
    const ok=G.kills>=w.n;
    say(c.n,c.role,open.concat([`<i>"You'll get me killed. Show me you can put down what's out here and I'll change my mind."</i>`]),
      [{t:ok?`"I've put down ${G.kills}."`:`Slain ${G.kills} of ${w.n}.`,off:!ok,fn:()=>joinUp(c)},
       {t:"Fair enough.",fn:()=>{}}]);
  } else {
    const dead=!ISL.mini||foes.indexOf(ISL.mini)<0;
    say(c.n,c.role,open.concat([`<i>"There's a thing that walks the ridge and I have watched it work. Kill that and I'll follow you into the sea itself."</i>`]),
      [{t:dead?`"It's down."`:`${ISL.mini?ISL.mini.n:"It"} is still up.`,off:!dead,fn:()=>joinUp(c)},
       {t:"I'll come back.",fn:()=>{}}]);
  }
}
function talkParty(m){
  if(m.down){ helpUp(m); return; }
  say(m.n,m.role,[`<i>"${pick([
    "Still walking. Tell me when you want me in front.",
    "I keep hearing the ship. Not the sinking. The ordinary noise of it.",
    "Whatever you're building on that sand, build it faster.",
    "I'll hold. Go and do the thing you were going to do."])}"</i>`],
    [{t:"Here — take this.",off:!(have("bandage")||have("herb")),fn:()=>{
        const id=have("bandage")?"bandage":"herb";
        take(id,1); m.hp=Math.min(m.mhp,m.hp+(id==="bandage"?45:26));
        toast(m.n.split(" ")[0],"Patched up."); syncHUD(); }},
     {t:`Their gear — ${allyGearLine(m)}.`,fn:()=>gearTalk(m)},
     ISL.home
       ? {t:"Stay here and keep the place.",fn:()=>leaveHere(m)}
       : {t:"Keep close.",fn:()=>{}},
     {t:"Carry on.",fn:()=>{}}]);
}
function nearDown(){
  let best=null,bd=95;
  for(const m of party) if(m.down){ const d=dist(m.x,m.y,player.x,player.y); if(d<bd){ bd=d; best=m; } }
  for(const c of neutrals) if(c.down){ const d=dist(c.x,c.y,player.x,player.y); if(d<bd){ bd=d; best=c; } }
  return best;
}
function helpUp(m){
  if(!m||!m.down) return;
  const medic=G.trade&&G.trade.b==="medic";
  const useHerb=!medic&&!have("bandage")&&have("herb")>=2;
  if(!medic&&!have("bandage")&&!useHerb){
    toast("nothing to do it with","A bandage, two bitterroot, or a surgeon's hands. You have none of those.");
    return;
  }
  if(!medic){ if(have("bandage")) take("bandage",1); else take("herb",2); }
  m.down=0; m.bleed=0; m.hp=m.mhp*(medic?.55:useHerb?.30:.40);
  if(!m.joined&&m.want&&m.want.k!=="free"){ m.want={k:"free"}; m.asked=0;
    m.line="You did that for somebody you had no reason to do it for."; }
  toast(m.n.split(" ")[0]+" is up",medic?"Your hands know the job.":"Not well. Up.");
  logit("off the ground",m.n+" — on "+ISL.name+".");
  syncHUD();
}

/* ============================================================ TALK: TOWN */
function talkTown(n){
  if(n.cnpc) return cityTalk(n);
  if(n.resident) return residentTalk(n);
  if(n.trader) return tradePost(n);
  if(n.giver) return storyTalk(n);
  say(n.n,n.role,[`<i>"${n.line}"</i>`]);
}
function tradePost(n){
  const swaps=[
    {a:"stone",an:4,b:"cloth",bn:1},
    {a:"wood",an:5,b:"pitch",bn:1},
    {a:"bone",an:4,b:"glass",bn:1},
    {a:"ore",an:2,b:"cloth",bn:2}
  ];
  const opts=swaps.map(s=>{
    const ok=have(s.a)>=s.an&&!n.traded[s.a];
    return {t:`${s.an} ${ITEMS[s.a].n} → ${s.bn} ${ITEMS[s.b].n}${n.traded[s.a]?" (done)":""}`,off:!ok,
      fn:()=>{ take(s.a,s.an); give(s.b,s.bn); n.traded[s.a]=1;
               toast("traded",`${s.bn} ${ITEMS[s.b].n}.`); syncHUD(); }};
  });
  opts.push({t:"Nothing today.",fn:()=>{}});
  say(n.n,"trade post",[`<i>"I'll deal, but once each. I'm not a shop, I'm a man with a shed."</i>`],opts);
}
function storyTalk(n){
  const st=ISL.story;
  if(st.doneAll){ say(n.n,n.role,[`<i>"You're the reason there's a tomorrow worth having here. Go on before I get sentimental."</i>`]); return; }
  if(st.stage===0){
    say(n.n,n.role,[`<i>"${st.open}"</i>`,`<i>"${st.task}"</i>`],
      [{t:"I'll see to it.",fn:()=>{ st.stage=1; logit("the town asks",st.task); syncHUD();
          toast(st.give,st.task); }},
       {t:"I have my own problems.",fn:()=>{}}]);
    return;
  }
  // check the condition
  let done=false;
  if(st.need==="mini") done=!ISL.mini||foes.indexOf(ISL.mini)<0;
  else if(st.need==="rescue") done=!!(st.lost&&st.lost.joined&&dist(st.lost.x,st.lost.y,n.x,n.y)<120);
  else done=canPay(st.need);
  if(!done){ say(n.n,n.role,[`<i>"${st.mid}"</i>`]); return; }
  say(n.n,n.role,[`<i>"${st.close}"</i>`],null,()=>{
    if(typeof st.need==="object") pay(st.need);
    for(const k in st.rw) giveTell(k,st.rw[k]);
    gainMist(st.mist,"the town");
    st.doneAll=true;
    if(st.recruit&&party.length<3){
      const c=mkCastaway(n.x+30,n.y+10); c.want={k:"free"};
      c.n=pick(FIRST)+" "+n.n.split(" ")[1]; c.role="cousin's boy";
      neutrals.push(c);
      toast("somebody follows you out","The cousin's boy. He has decided, and nobody asked him.");
    } else toast(st.give,"Settled.");
    logit("settled",st.give+" — on "+ISL.name+".");
    syncHUD();
  });
}

/* ============================================================ REGARD PASSIVES & ACTIVES */
/* how much a rank is worth: damage and duration scale, reach barely does */
const rPw=g=>1+(g.rank-1)*.30;
const rReach=g=>1+(g.rank-1)*.06;

/* passives, read wherever they matter */
/* every point of vigor goes through here, so spending it can mean something */
function spendVg(n){
  G.vg=Math.max(0,G.vg-n);
  const g=hasPas("toltec");                    // Toltec: what you burn comes back as blood
  if(g&&n>0) G.hp=Math.min(G.mhp,G.hp+n*(.05+.02*g.rank));
}
function pasSpeed(){ const g=hasPas("mishaabooz"); return g?1+.10+.025*g.rank:1; }
function pasDashCost(){ return hasPas("mishaabooz")?.5:1; }
function pasRegen(){ const g=hasPas("drum"); return g?.7+.35*g.rank:0; }
function pasLevel(f){                         // the Levelling speaks out of slowed things now (castEchoes)
  return 1;
}
function pasCold(f){                          // Laurentide: what touches you goes stiff
  const g=hasPas("laurentide"); if(!g) return;
  f.slow=Math.max(f.slow||0,.9+.2*g.rank);
}
function pasEmber(f){                         // Ember: what you strike catches
  const g=hasPas("kettle"); if(!g) return;
  f.burn=Math.max(f.burn,.7+.25*g.rank);
}
function pasShadowCap(g){ return g.rank>=4?90:60; }   // Yūrei: how much the shadow can hold
function pasYurei(d,src){                     // Yūrei: the shadow stands into the blow
  const g=hasPas("yurei"); if(!g||!src||!src.r) return d;
  let frac=Math.min(.55,(src.r/70)*( .40+.06*g.rank+(g.rank>=4?.08:0) ));
  const held=d*frac;
  G.shadow=Math.min(pasShadowCap(g),G.shadow+held);
  if(rnd()<.6) parts.push({x:player.x+6,y:player.y+8,vx:(rnd()-.5)*40,vy:-30,life:.4,c:"#3a3f4a",s:2});
  return d-held;
}
function pasSawSlow(e){                       // Sawbones: the ether around you
  const g=hasPas("sawbones"); if(!g) return 1;
  const r=140+(g.rank>=2?30+8*g.rank:0);
  return dist(e.x,e.y,player.x,player.y)<r?(.86-.03*g.rank):1;
}
function pasToltecHit(d){                     // Toltec: a wound fills the lungs
  const g=hasPas("toltec"); if(!g) return;
  G.vg=Math.min(G.mvg,G.vg+d*.45);
  player.tolT=Math.max(player.tolT||0,g.rank>=3?1.8:1.1);
}
/* rank 3 of each regard makes its own status a wound */
function dmgMul(f){
  let m=1;
  const la=hasPas("laurentide"), tr=hasPas("trident"), la3=la&&la.rank>=3, tr3=tr&&tr.rank>=3;
  const ke=hasPas("kettle"), lm=hasPas("lamp"), sb=hasPas("sawbones");
  if(la3&&f.froze>0) m*=1.5;
  if(tr3&&f.empt) m*=1.5;
  if(ke&&ke.rank>=3&&f.burn>0) m*=1.4;
  if(lm&&lm.rank>=3&&f.lit>0) m*=1.5;
  if(sb&&sb.rank>=3&&pasSawSlow(f)<1) m*=1.33;      // slowed in the ether
  return m;
}

const AB={
  laurentide:(src,g,mult)=>{
    const pw=rPw(g)*mult, len=g.R.act.r*rReach(g), hold=2.0+.5*g.rank;
    zones.push({k:"cone",x:src.x,y:src.y,a:src.aim,len,life:.45,max:.45,c:"#9fc6d6"});
    let first=null;
    coneAt(src,len,.70,(f,d)=>{ f.froze=hold; hurtFoe(f,12*pw,2); if(!first) first=f; });
    if(g.rank>=5&&first){                        // it spreads to the next thing along
      let best=null,bd=130;
      for(const f of foes) if(f!==first&&f.froze<=0){ const d=dist(f.x,f.y,first.x,first.y); if(d<bd){bd=d;best=f;} }
      if(best){ best.froze=hold*.7; hurtFoe(best,8*pw,0);
        zones.push({k:"bolt",x1:first.x,y1:first.y,x2:best.x,y2:best.y,life:.3,max:.3,c:"#bcd8e4"}); }
    }
    sfx(760,.2,"sine",.07);
  },
  togo:(src,g,mult)=>{
    const r=g.R.act.r*rReach(g), hold=(1.4+.3*g.rank)*Math.min(1,mult+.4);
    const flash=(sx)=>{
      zones.push({k:"ring",x:sx.x,y:sx.y,r,life:.4,max:.4,c:"#e8f2f6"});
      ringAt(sx,r,(f,d)=>{ f.stun=Math.max(f.stun||0,hold); });
      for(let i=0;i<8;i++) parts.push({x:sx.x,y:sx.y,vx:(rnd()-.5)*160,vy:(rnd()-.5)*160,life:.3,c:"#e8f2f6",s:2});
    };
    flash(src);
    for(const w of petWolves()) if(w!==src) flash(w);
    sfx(880,.18,"sine",.07);
  },
  trident:(src,g,mult)=>{
    const pw=rPw(g)*mult, r=g.R.act.r*rReach(g), hold=(5+1.4*g.rank)*(mult>=1?1:.7);
    zones.push({k:"ring",x:src.x,y:src.y,r,life:.45,max:.45,c:"#c7bfa8"});
    ringAt(src,r,(f,d)=>{
      hurtFoe(f,Math.max(1,Math.round((8+3*g.rank)*pw)),2);
      if(foes.indexOf(f)<0) return;
      f.slow=Math.max(f.slow||0,hold); f.empt=1;
      if(g.rank>=5&&f.hp<f.mhp*.28&&f.k!=="boss"&&f.k!=="mini"){
        for(let i=0;i<8;i++) parts.push({x:f.x,y:f.y,vx:(rnd()-.5)*130,vy:(rnd()-.5)*130,life:.5,c:"#c7bfa8",s:2});
        killFoe(f);
      }
    });
    sfx(120,.28,"sine",.08);
  },
  mishaabooz:(src,g,mult)=>{
    const pw=rPw(g)*mult, len=g.R.act.r*rReach(g);
    let bx=src.x,by=src.y;
    for(let s=8;s<=len;s+=8){
      const x=src.x+Math.cos(src.aim)*s, y=src.y+Math.sin(src.aim)*s;
      if(x<TILE||y<TILE||x>TILE*(GW-1)||y>TILE*(GH-1)) break;
      if(free(x,y,src.r||11)){ bx=x; by=y; }
    }
    zones.push({k:"puff",x:src.x,y:src.y,life:.4,max:.4,c:"#cfc7ae"});
    // the place you left comes apart
    const ox=src.x, oy=src.y;
    for(const f of foes) if(dist(f.x,f.y,ox,oy)<72) hurtFoe(f,(g.rank>=3?26:16)*pw,10);
    src.x=bx; src.y=by;
    if(src===player) player.inv=g.rank>=2?.45:.28;
    zones.push({k:"puff",x:bx,y:by,life:.4,max:.4,c:"#cfc7ae"});
    if(g.rank>=4) for(const f of foes) if(dist(f.x,f.y,bx,by)<62) hurtFoe(f,14*pw,8);
    if(g.rank>=5&&src===player) player.cd=0;      // the second one is free of the first
    sfx(640,.13,"sine",.07);
  },
  drum:(src,g,mult)=>{
    const pw=rPw(g)*mult, r=g.R.act.r*rReach(g);
    const beat=(rr,dm)=>{
      zones.push({k:"ring",x:src.x,y:src.y,r:rr,life:.5,max:.5,c:"#d8b478"});
      ringAt(src,rr,(f,d)=>{
        hurtFoe(f,dm,16); f.slow=2.0+.2*g.rank;
        if(g.rank>=3) f.rcd=Math.max(f.rcd||0,1.6);
      });
    };
    beat(r,16*pw);
    if(g.rank>=4&&src===player) G.hp=Math.min(G.mhp,G.hp+6+2*g.rank);
    if(g.rank>=5) setTimeout(()=>{ if(G.mode==="play") beat(r*.8,12*pw); },340);
    G.shake=5; sfx(70,.3,"sine",.13);
  },
  kettle:(src,g,mult)=>{
    if(src!==player){                            // an ally just lights what is next to it
      for(const f of foes) if(dist(f.x,f.y,src.x,src.y)<90) f.burn=Math.max(f.burn,2.2);
      return;
    }
    G.coal=4.5+.9*g.rank;
    toast("banked coal","What comes close burns while it stays close.");
    sfx(340,.28,"sawtooth",.07);
  },
  lamp:(src,g,mult)=>{
    const pw=rPw(g)*mult, reach=g.R.act.r*rReach(g), rad=58+6*g.rank;
    const lx=src.x+Math.cos(src.aim)*reach, ly=src.y+Math.sin(src.aim)*reach;
    const life=3.5+.6*g.rank;
    zones.push({k:"lamp",x:lx,y:ly,r:rad,life,max:life,c:"#e8d79a",rank:g.rank});
    for(const f of foes) if(dist(f.x,f.y,lx,ly)<rad){ f.lit=life; hurtFoe(f,10*pw,0); }
    sfx(520,.25,"triangle",.07);
  },
  yurei:(src,g,mult)=>{
    const pw=rPw(g)*mult, len=g.R.act.r*rReach(g);
    const held=src===player?G.shadow:20;
    const dmg=(14+held*.85)*pw;
    const strike=()=>{
      const ex=src.x+Math.cos(src.aim)*len, ey=src.y+Math.sin(src.aim)*len;
      zones.push({k:"cut",x1:src.x,y1:src.y,x2:ex,y2:ey,life:.35,max:.35,c:"#2c313c"});
      zones.push({k:"puff",x:ex,y:ey,life:.4,max:.4,c:"#3a3f4a"});
      let fed=false;
      coneAt(src,len,.5,(f,d)=>{
        hurtFoe(f,dmg*dmgMul(f),8);
        if(f.hp<=0&&!fed){ fed=true;
          if(src===player){ G.hp=Math.min(G.mhp,G.hp+14+5*g.rank);
            toast("the shadow feeds","What it killed comes back to you as blood."); }
        }
      });
      sfx(90,.22,"sine",.09);
    };
    strike();
    if(g.rank>=5) setTimeout(()=>{ if(G.mode==="play") strike(); },260);
    if(src===player) G.shadow=g.rank>=3?G.shadow*.5:0;   // it comes home thinner
  },
  nanook:(src,g,mult)=>{
    if(src!==player){ src.rage=6; zones.push({k:"puff",x:src.x,y:src.y,life:.4,max:.4,c:"#dfe6ea"}); return; }
    G.nanookT=6+.5*g.rank;
    G.shake=6;
    toast("white hunger","Faster, longer, heavier — and no dashing until it lets go of you.");
    sfx(60,.5,"sawtooth",.11);
  },
  sawbones:(src,g,mult)=>{
    const pw=rPw(g)*mult, r=g.R.act.r*rReach(g);
    const amt=(20+7*g.rank)*(g.rank>=4?1.4:1)*mult;
    let healed=0;
    const mend=(cur,max)=>{ const h=Math.min(max-cur,amt); healed+=h; return cur+h; };
    if(src===player) G.hp=mend(G.hp,G.mhp);
    else if(src.hp!==undefined){ const h=Math.min(src.mhp-src.hp,amt); src.hp+=h; healed+=h; }
    for(const m of party) if(!m.down&&dist(m.x,m.y,src.x,src.y)<r){ const h=Math.min(m.mhp-m.hp,amt); m.hp+=h; healed+=h; }
    if(src!==player&&dist(player.x,player.y,src.x,src.y)<r) G.hp=mend(G.hp,G.mhp);
    zones.push({k:"ring",x:src.x,y:src.y,r,life:.5,max:.5,c:"#b8d8c0"});
    const dmg=Math.max(10*pw,healed*.65);
    ringAt(src,r,(f,d)=>{
      hurtFoe(f,dmg,10);
      if(g.rank>=5) f.stun=Math.max(f.stun||0,.7);
    });
    sfx(300,.25,"triangle",.08);
  },
  toltec:(src,g,mult)=>{
    const pw=rPw(g)*mult;
    const isP=src===player;
    const hpNow=isP?G.hp:src.hp, mhp=isP?G.mhp:src.mhp;
    const cut=Math.max(8,Math.round(mhp*(g.rank>=4?.08:.12)));
    if(hpNow<=cut+4){ if(isP) toast("not enough blood","The Offering would take more than you have."); return; }
    if(isP){ G.hp-=cut; G.hurtT=.25; } else src.hp-=cut;
    const dmg=(26+cut*(g.rank>=2?1.5:1.15))*pw;
    shots.push({x:src.x+Math.cos(src.aim)*(src.r+8),y:src.y+Math.sin(src.aim)*(src.r+8),
      vx:Math.cos(src.aim)*330,vy:Math.sin(src.aim)*330,dmg,r:9,life:1.4,
      c:"#9c2f3a",s:"boulder",mine:1,pierce:1,hitset:[],
      tol:{owner:isP?"player":"ally",cut,heal:g.rank>=5}});
    G.shake=4; sfx(110,.3,"sawtooth",.1);
  },
  ravenmocker:(src,g,mult)=>{
    const pw=rPw(g)*mult, len=g.R.act.r*(g.rank>=3?1.25:1);
    zones.push({k:"cone",x:src.x,y:src.y,a:src.aim,len,life:.5,max:.5,c:"#4a3a52"});
    coneAt(src,len,.65,(f,d)=>{
      f.pup=3.0; f.pupDmg=(18+6*g.rank)*pw; f.pupHold=g.rank>=4?1.5:1;
      f.pupTw=g.rank>=5;
    });
    sfx(180,.35,"sine",.08);
  },
  missionary:(src,g,mult)=>{
    const r=g.R.act.r*(g.rank>=2?1.25:1), kb=g.rank>=2?46:34;
    zones.push({k:"ring",x:src.x,y:src.y,r,life:.5,max:.5,c:"#e4ddc8"});
    ringAt(src,r,(f,d)=>{
      const a=Math.atan2(f.y-src.y,f.x-src.x);
      if(!f.big){ const nx=f.x+Math.cos(a)*kb, ny=f.y+Math.sin(a)*kb;
        if(free(nx,ny,f.r)){ f.x=nx; f.y=ny; } }
      if(g.rank>=4) f.stun=Math.max(f.stun||0,.8);
    });
    if(src===player){
      G.zeal=Math.max(G.zeal,g.rank>=5?2:1);
      toast("the sermon","The next blow lands twice — yours, or a Regard's.");
    }
    sfx(240,.3,"triangle",.09);
  }
};
function coneAt(src,len,ang,fn){
  for(const f of foes){
    const d=dist(f.x,f.y,src.x,src.y); if(d>len+f.r) continue;
    let a=Math.atan2(f.y-src.y,f.x-src.x)-src.aim;
    a=Math.atan2(Math.sin(a),Math.cos(a));
    if(Math.abs(a)<ang) fn(f,d);
  }
}
function ringAt(src,r,fn){
  for(const f of foes){ const d=dist(f.x,f.y,src.x,src.y); if(d<r+f.r) fn(f,d); }
}

function abil(slot){
  if(G.mode!=="play"||isPaused()) return;
  const g=G.regards[slot-1];
  if(!g){
    toast(slot===1?"nothing looks at you":"only one thing looks at you",
      slot===1?"No Regard yet. Find a spirit, or put down whatever guards an island."
             :"A second Regard would sit on F. You are carrying one.");
    return;
  }
  let cost=Math.round(g.R.act.cost*(g.rank>=4?.82:1));
  if(player.cd>0||player.roll>0) return;
  if(g.R.id==="nanook"){                        // White Hunger drinks the tank, not a price
    const min=20;
    if(G.vg<min){ toast("not enough vigor",`${g.R.act.n} wants at least ${min}, and then everything else.`); return; }
    cost=Math.max(min,Math.floor(G.vg*(g.rank>=4?.7:.85)));
  } else if(G.vg<cost){ toast("not enough vigor",`${g.R.act.n} wants ${cost}.`); return; }
  spendVg(cost); player.cd=.38;
  let mult=1;
  if(G.zeal>0&&g.R.id!=="missionary"){ mult=2; G.zeal--; }   // the Sermon doubles whatever comes next
  AB[g.R.id](player,g,mult);
  castEchoes(g,mult);
  syncHUD();
}
/* the world repeats you: Togo's team at rank 3, and everything the Trident has slowed */
function echoAim(from,skip){
  let best=null,bd=460;
  for(const f of foes){ if(f===skip||f.asleep) continue;
    const d=dist(f.x,f.y,from.x,from.y); if(d<bd){ bd=d; best=f; } }
  return best?Math.atan2(best.y-from.y,best.x-from.x):player.aim;
}
function castEchoes(g,mult){
  const tg=hasPas("togo");
  if(tg&&tg.rank>=3&&g.R.id!=="togo")
    for(const w of petWolves()){
      AB[g.R.id]({x:w.x,y:w.y,r:w.r,aim:echoAim(w,null)},g,.6*mult);
      zones.push({k:"puff",x:w.x,y:w.y,life:.3,max:.3,c:"#e8f2f6"});
    }
  const tr=hasPas("trident");
  if(tr&&g){
    const em=(tr.rank>=4?.85:.5)*mult;
    let n=0;
    for(const f of foes.slice()){
      if(n>=3) break;
      if(f.slow>0){ n++;
        AB[g.R.id]({x:f.x,y:f.y,r:f.r,aim:echoAim(f,f)},g,em);
        zones.push({k:"puff",x:f.x,y:f.y,life:.35,max:.35,c:"#c7bfa8"});
      }
    }
  }
}
function petWolves(){ return animals.filter(a=>a.pet&&a.k==="wolf"); }
function petCap(){ return hasPas("togo")?4:2; }
/* a follower carrying a regard uses it now and then, at half strength */
function allyCast(m,dt){
  if(!m.g) return;
  m.gcd=(m.gcd===undefined?3+rnd()*3:m.gcd)-dt;
  if(m.gcd>0) return;
  let near=false;
  for(const f of foes) if(dist(f.x,f.y,m.x,m.y)<150){ near=true; break; }
  if(!near) return;
  m.gcd=7+rnd()*4;
  m.aim=m.face||0;
  AB[m.g.R.id](m,m.g,.55);
  zones.push({k:"puff",x:m.x,y:m.y,life:.4,max:.4,c:"#a396c2"});
}
/* ============================================================ BUILDING */
const BUILD_LIMIT={hearth:1,plot:10,furnace:2,house:3,pen:2};
function whyNotBuild(kind){
  const B=BUILDS[kind];
  if(!B) return "Nothing like that.";
  if(!B.anywhere&&!ISL.home) return "Only on your own island. Lay a hearth first.";
  if(kind==="hearth"){
    if(ISL.home) return "There is already a fire here that does not go out.";
    if(HOME) return "You keep one island. Let the old fire out first, if you mean to move.";
    if(ISL.threat>=2) return "Nothing this island holds would let a fire stay lit.";
  }
  if(homeCount(kind)>=BUILD_LIMIT[kind]) return `${BUILD_LIMIT[kind]} is as many as you can keep up.`;
  const tx=Math.floor(player.x/TILE), ty=Math.floor(player.y/TILE), t=T(tx,ty);
  if(SOLID[t]||t===WATER||t===SHOAL||t===MIRE) return "Not on this ground.";
  for(const b of builds) if(dist(b.x,b.y,player.x,player.y)<B.r+20) return "Too close to what you have already put up.";
  for(const p of props) if(dist(p.x,p.y,player.x,player.y)<26) return "Stand clear of the boat.";
  return null;
}
function doBuild(kind){
  const why=whyNotBuild(kind);
  if(why){ toast("not here",why); return; }
  const B=BUILDS[kind];
  if(!canPay(B.c)){
    toast("not enough for it",Object.keys(B.c).map(k=>`${ITEMS[k].n} ${avail(k)}/${B.c[k]}`).join(" · "));
    return;
  }
  pay(B.c);
  const b={k:kind,x:player.x,y:player.y+4,r:B.r,ph:rnd()*6.3};
  if(kind==="furnace"){ b.q=[]; b.out={}; b.heat=0; }
  if(kind==="plot"){ b.seed=null; b.grow=0; b.ripe=false; }
  builds.push(b);
  if(kind==="hearth"){ claimHome(); }
  else if(ISL.home) saveHome();
  sfx(150,.16,"square",.08);
  toast("put up",B.n+".");
  logit("built",`${B.n} — ${ISL.name}.`);
  syncHUD();
}
function abandonHome(){
  HOME=null;
  ISL.home=false; ISL.cleared=false;
  G.home=[];                                    // what you left in the store stays in the store
  builds=builds.filter(b=>b.k!=="hearth");
  spawnT=6;
  toast("the fire is out","This is somebody else's island again. Whatever you left on it stays on it.");
  logit("abandoned",`${ISL.name} is not yours any more.`);
  syncHUD();
}

/* ---- the hearth ---- */
function hearthTalk(b){
  const opts=[
    {t:"Sit down for a while.",fn:()=>{
      G.hp=G.mhp; G.vg=G.mvg; player.chill=0;
      homeTick(150); G.clock+=150;
      toast("rested","Whole again, and a little of the day gone.");
      syncHUD(); }},
    {t:"The hold.",fn:()=>togglePanel("cachep")},
    {t:"What can be built.",fn:()=>togglePanel("craft")},
    {t:"Let the fire out.",fn:()=>{
      say("The hearth","",[`<i>Everything you have put up here stays here, and stops being yours.</i>`],
        [{t:"Let it out.",fn:abandonHome},{t:"No.",fn:()=>{}}]); }},
    {t:"Leave it.",fn:()=>{}}
  ];
  const res=(HOME&&HOME.residents)||[];
  say("The hearth",`${ISL.name} · ${builds.length} thing${builds.length===1?"":"s"} up · ${res.length} staying`,
    [`<i>The fire is where you laid it. It has not gone out and it is not going to.</i>`],opts);
}

/* ---- garden beds ---- */
function plotTalk(b){
  if(!b.seed){
    const opts=[];
    for(const sd in CROPS){
      const c=CROPS[sd], n=avail(sd);
      opts.push({t:n?`Plant ${c.n} — ${n} seed`:`${c.n} — no seed`,off:!n,fn:()=>{
        pay({[sd]:1}); b.seed=sd; b.grow=0; b.ripe=false;
        if(ISL.home) saveHome();
        toast("planted",c.n+".");
        syncHUD(); }});
    }
    opts.push({t:"Leave it turned.",fn:()=>{}});
    say("Turned ground","a garden bed",[`<i>Bare soil, waiting. It does not care what goes in it.</i>`],opts);
    return;
  }
  const c=CROPS[b.seed];
  if(!b.ripe){
    const pct=Math.min(99,Math.floor((b.grow/c.time)*100));
    say(c.n,"coming up",[`<i>${pct}% of the way there. It will keep going while you are away.</i>`],
      [{t:"Pull it up early.",fn:()=>{
          give(c.give,1); b.seed=null; b.grow=0; b.ripe=false;
          if(ISL.home) saveHome();
          toast("pulled early","One, and the bed is free."); syncHUD(); }},
       {t:"Leave it.",fn:()=>{}}]);
    return;
  }
  const hands=((HOME&&HOME.residents)||[]).length;
  const amt=c.amt+(hands?1:0);
  const got=give(c.give,amt);
  const seedBack=rnd()<.6?1:0;
  if(seedBack) give(b.seed,1);
  b.seed=null; b.grow=0; b.ripe=false;
  if(ISL.home) saveHome();
  for(let i=0;i<8;i++) parts.push({x:b.x,y:b.y,vx:(rnd()-.5)*110,vy:-40-rnd()*50,life:.5,c:c.c,s:2});
  sfx(300,.1,"triangle",.07);
  toast("harvested",`${got} ${ITEMS[c.give].n}${seedBack?", and seed back":""}${got<amt?" — the rest is on the ground":""}.`);
  syncHUD();
}

/* ---- the furnace ---- */
function furnaceTalk(b){
  const readyList=[];
  for(const k in (b.out||{})) if(b.out[k]>0) readyList.push(`${b.out[k]} ${ITEMS[k].n}`);
  const cooking=(b.q||[]).length;
  const opts=[];
  opts.push({t:`Cook 4 ore into iron — ore ${avail("ore")}/4, pitch ${avail("pitch")}/1`,
    off:!(avail("ore")>=4&&avail("pitch")>=1),fn:()=>{
      pay({ore:4,pitch:1}); b.q=b.q||[]; b.q.push({give:"ingot",n:4,t:55});
      if(ISL.home) saveHome();
      toast("in the furnace","Iron, once it has had long enough."); syncHUD(); }});
  opts.push({t:`Roast 3 meat — meat ${avail("meat")}/3`,off:avail("meat")<3,fn:()=>{
      pay({meat:3}); b.q=b.q||[]; b.q.push({give:"roast",n:3,t:60});
      if(ISL.home) saveHome();
      toast("on the fire","It will be worth eating shortly."); syncHUD(); }});
  if(readyList.length) opts.push({t:`Take out ${readyList.join(" and ")}.`,fn:()=>{
      for(const k in b.out){ if(b.out[k]>0){ giveTell(k,b.out[k]); b.out[k]=0; } }
      if(ISL.home) saveHome();
      toast("out of the furnace",readyList.join(", ")+"."); syncHUD(); }});
  opts.push({t:"Leave it burning.",fn:()=>{}});
  const state=cooking?`${cooking} thing${cooking>1?"s":""} in it`:readyList.length?"something ready":"cold and empty";
  say("The furnace",state,
    [`<i>Stone, ore-dust, and a heat that does not care what you put in it.</i>`+
     (readyList.length?`<br>Ready: ${readyList.join(", ")}.`:"")],opts);
}

/* ---- houses ---- */
function houseTalk(b){
  const res=(HOME&&HOME.residents)||[];
  const cap=1+homeCount("house");
  const opts=[
    {t:"Sleep.",fn:()=>{
      G.hp=G.mhp; G.vg=G.mvg; player.chill=0;
      for(const m of party) if(!m.down) m.hp=m.mhp;
      toast("slept","A night of it. The garden waited for you — things here only grow at your back.");
      syncHUD(); }},
    {t:"Leave it.",fn:()=>{}}
  ];
  say("The house",`${res.length} of ${cap} staying here`,
    [`<i>Four walls you cut and carried. It smells of pitch and it does not leak.</i>`],opts);
}

/* ---- the pen ---- */
function penTalk(b){
  const near={};
  for(const a of animals){
    if(a.pet||a.young) continue;
    if(dist(a.x,a.y,b.x,b.y)>200) continue;
    if(!ANIMALS[a.k].breeds) continue;
    near[a.k]=near[a.k]||[]; near[a.k].push(a);
  }
  const opts=[];
  for(const k in near){
    const fed=near[k].filter(a=>a.fed).length;
    const A=ANIMALS[k];
    opts.push({t:fed>=2?`Put the ${A.n}s together — both fed`:`${A.n}s — ${fed} of 2 fed on ${ITEMS[A.feed].n}`,
      off:fed<2||!!b.breeding,fn:()=>{
        let done=0;
        for(const a of near[k]){ if(a.fed&&done<2){ a.fed=0; done++; } }
        b.breeding=k; b.bt=170;
        if(ISL.home) saveHome();
        toast("in the pen","Give it time.");
        logit("the pen",`Two ${A.n}s, fed and penned.`); }});
  }
  if(b.breeding) opts.push({t:`Something is coming — ${Math.ceil(b.bt)} to go.`,off:true,fn:()=>{}});
  if(!opts.length) opts.push({t:"Nothing here to put in it.",off:true,fn:()=>{}});
  opts.push({t:"Leave it.",fn:()=>{}});
  say("The pen","fenced ground",
    [`<i>Wood and cord, waist high. Feed two of a kind and they will do the rest.</i>`],opts);
}
function buildTalk(b){
  if(b.k==="hearth") return hearthTalk(b);
  if(b.k==="plot")   return plotTalk(b);
  if(b.k==="furnace")return furnaceTalk(b);
  if(b.k==="house")  return houseTalk(b);
  if(b.k==="pen")    return penTalk(b);
}

/* ---- making room: on your own island you may take the scenery down ---- */
const CLEARABLE={};
CLEARABLE[TREE] ={n:"tree",  tool:"hatchet",give:"wood", amt:2,c:"#3a2b20"};
CLEARABLE[PINE] ={n:"pine",  tool:"hatchet",give:"wood", amt:2,c:"#28493d"};
CLEARABLE[SCRUB]={n:"scrub", tool:null,     give:"fiber",amt:1,c:"#54633f"};
CLEARABLE[ROCK] ={n:"rock",  tool:"pick",   give:"stone",amt:2,c:"#6d7377"};
CLEARABLE[VEIN] ={n:"ore vein",tool:"pick", give:"ore",  amt:2,c:"#b06a36"};
function clearTarget(){
  if(!ISL.home) return null;
  const tx=Math.floor(player.x/TILE), ty=Math.floor(player.y/TILE);
  let best=null,bd=1e9;
  for(let dy=-1;dy<=1;dy++)for(let dx=-1;dx<=1;dx++){
    const a=tx+dx, b=ty+dy, t=T(a,b);
    if(!CLEARABLE[t]) continue;
    const p=tc(a,b), d=dist(p.x,p.y,player.x,player.y);
    if(d<bd){ bd=d; best={tx:a,ty:b,t,d}; }
  }
  return best;
}
function clearTile(o){
  const C=CLEARABLE[o.t];
  if(!C) return;
  if(!ISL.home){ toast("not here","Only on your own island."); return; }
  if(C.tool&&!G.tools[C.tool]){ toast("no tool for it",`A ${C.tool==="hatchet"?"hand hatchet":"rock pick"}, at least.`); return; }
  if(player.cd>0) return;
  player.cd=.5; player.swing=.22; G.shake=3;
  setT(o.tx,o.ty,THEMES[ISL.theme].ground);
  if(o.ty*GW+o.tx<reach.length) reach[o.ty*GW+o.tx]=1;
  const p=tc(o.tx,o.ty);
  for(let i=0;i<10;i++) parts.push({x:p.x,y:p.y,vx:(rnd()-.5)*150,vy:-40-rnd()*70,life:.5,c:C.c,s:2});
  if(C.give) giveTell(C.give,C.amt);
  sfx(C.give==="stone"||C.give==="ore"?110:150,.14,"square",.08);
  collectSpots();
  saveHome();
  toast("cleared",`${C.n[0].toUpperCase()+C.n.slice(1)} down.${C.give?` ${C.amt} ${ITEMS[C.give].n}.`:""}`);
  syncHUD();
}

/* ============================================================ ANIMALS */
function petCount(){ let n=0; for(const a of animals) if(a.pet) n++; return n; }
function feedAnimal(a){
  const A=ANIMALS[a.k];
  if(!A.feed||avail(A.feed)<1) return;
  pay({[A.feed]:1});
  a.fed=1; a.tp=(a.tp||0)+1; a.wary=0;
  if(A.breeds) a.follow=30;                 // it walks after the hand that fed it, for a while
  for(let i=0;i<6;i++) parts.push({x:a.x,y:a.y,vx:(rnd()-.5)*80,vy:-30,life:.4,c:"#cfc7ae",s:2});
  toast(A.n,`Takes the ${ITEMS[A.feed].n} out of your hand${ANIMALS[a.k].breeds?" and will walk after you a while — lead it to a pen":""}.${ISL.home?" A pen will do the rest.":""}`);
  syncHUD();
}
function tameAnimal(a){
  const A=ANIMALS[a.k];
  if(!A.tame) return;
  if(petCount()>=petCap()){ toast(petCap()>2?"the team is full":"two is enough",
    `${petCap()} of them already walk with you.`); return; }
  if(avail("meat")<1&&avail("roast")<1){ toast("nothing to offer it","Meat. Nothing else interests it."); return; }
  pay(avail("roast")>0?{roast:1}:{meat:1});
  let ch=.22+.14*(a.tp||0)+(a.hp<a.mhp*.55?.22:0)+(ISL.home?.18:0);
  a.tp=(a.tp||0)+1;
  if(rnd()<ch){
    a.pet=true; a.fed=1; a.gx=a.x; a.gy=a.y; a.hp=a.mhp; G.tamed++;
    toast("it comes with you",`${A.n}. It walks a little behind you and slightly to the left.`);
    logit("tamed",`A ${A.n}, on ${ISL.name}.`);
    sfx(420,.25,"sine",.08);
  } else {
    toast("not yet","It takes the meat and backs off. Try again when it has had a few.");
    a.wary=2.5;
  }
  syncHUD();
}
function animalTalk(a){
  const A=ANIMALS[a.k];
  if(a.pet){
    const opts=[];
    if(a.hp<a.mhp&&(avail("meat")||avail("roast")))
      opts.push({t:"Feed it.",fn:()=>{ pay(avail("roast")?{roast:1}:{meat:1});
        a.hp=Math.min(a.mhp,a.hp+a.mhp*.5); toast(A.n,"Better."); syncHUD(); }});
    if(ISL.home) opts.push({t:"Leave it here.",fn:()=>{
      a.pet=false; a.fed=1; a.gx=a.x; a.gy=a.y;
      if(ISL.home) saveHome();
      toast(A.n,"Stays. It will be here."); syncHUD(); }});
    opts.push({t:"Get on, then.",fn:()=>{}});
    say(A.n,a.young?"young":"yours",[`<i>It looks up at you with the flat, total attention of something that has decided.</i>`],opts);
    return;
  }
  const opts=[];
  if(A.feed) opts.push({t:avail(A.feed)?`Offer ${ITEMS[A.feed].n}${a.fed?" (already fed)":""}`:`${ITEMS[A.feed].n} — you have none`,
    off:!avail(A.feed),fn:()=>feedAnimal(a)});
  if(A.tame) opts.push({t:`Try to bring it with you — meat ${avail("meat")+avail("roast")}`,
    off:!(avail("meat")+avail("roast")),fn:()=>tameAnimal(a)});
  opts.push({t:"Leave it alone.",fn:()=>{}});
  say(A.n,a.fed?"fed":"wild",
    [`<i>${a.k==="bear"?"It has not decided about you. That is the best you are getting."
      :a.k==="wolf"?"It watches your hands, not your face."
      :a.k==="deer"?"It has stopped chewing and has not started again."
      :"It is entirely occupied with something in the leaf litter."}</i>`],opts);
}
function hurtAnimal(a,d){
  a.hp-=d; a.hurt=.2; a.wary=4;
  for(let i=0;i<5;i++) parts.push({x:a.x,y:a.y,vx:(rnd()-.5)*150,vy:(rnd()-.5)*150,life:.3,c:"#7a3a2c",s:2});
  if(a.hp<=0) killAnimal(a);
}
function killAnimal(a){
  animals=animals.filter(o=>o!==a);
  const A=ANIMALS[a.k];
  corpses.push({x:a.x,y:a.y,r:a.r,k:"beast",t:0,max:.8,c:a.c});
  if(a.pet){
    G.deaths.push(A.n+", "+ISL.name);
    toast("dead",`Your ${A.n}. It did not back off when it should have.`);
    logit("dead",`The ${A.n} that walked with you — ${ISL.name}.`);
    syncHUD(); return;
  }
  const meat=Math.max(1,Math.round((a.meat||1)*(a.young?.5:1)));
  giveTell("meat",meat);
  if(rnd()<(a.k==="bear"?.9:a.k==="deer"?.5:.2)) giveTell("hide",a.k==="bear"?2:1);
  if(rnd()<.3&&a.k!=="squirrel") giveTell("bone",1);
  gainMist(a.mist||2,"an animal");
  toast("killed",`${A.n} — ${meat} meat.`);
  sfx(140,.14,"triangle",.08);
  syncHUD();
}
function updAnimals(dt){
  for(const a of animals.slice()){
    a.ph+=dt; a.hurt=Math.max(0,(a.hurt||0)-dt); a.cd=Math.max(0,(a.cd||0)-dt);
    a.swing=Math.max(0,(a.swing||0)-dt); a.wary=Math.max(0,(a.wary||0)-dt);
    if(a.hp<=0){ killAnimal(a); continue; }
    const pd=dist(a.x,a.y,player.x,player.y);
    if(a.pet){
      if(a.stag>0){                            // struck by something too big to argue with
        a.stag-=dt;
        let fl=null,fb=300;
        for(const f of foes){ if(f.asleep) continue; const d=dist(f.x,f.y,a.x,a.y); if(d<fb){ fb=d; fl=f; } }
        const ang=fl?Math.atan2(a.y-fl.y,a.x-fl.x):Math.atan2(player.y-a.y,player.x-a.x);
        moveEnt(a,Math.cos(ang),Math.sin(ang),a.sp*1.05,dt);
        a.face=ang;
        continue;
      }
      let tgt=null,bd=220;
      for(const f of foes){ if(f.asleep) continue; const d=dist(f.x,f.y,a.x,a.y); if(d<bd){ bd=d; tgt=f; } }
      if(tgt&&pd<380){
        if(bd>tgt.r+a.r+6) seekTo(a,tgt.x,tgt.y,a.sp*.85,dt,8);
        else { a.face=Math.atan2(tgt.y-a.y,tgt.x-a.x);
          if(a.cd<=0){ a.cd=.85; a.swing=.15;
            if(window.ART&&window.ART.hit) window.ART.hit(a);
            const tg=hasPas("togo");
            const wdmg=(a.dmg||10)+(a.k==="wolf"&&tg&&tg.rank>=5?wpnSt().dmg:0);
            hurtFoe(tgt,wdmg*dmgMul(tgt),4); sfx(210,.05,"square",.05); } }
      } else if(pd>76) seekTo(a,player.x,player.y,a.sp*(pd>240?.95:.7),dt,60);
      else { a.gx=player.x; a.gy=player.y; wanderEnt(a,dt,46,26); }
      continue;
    }
    // wild — but a fed thing follows the hand for a while
    if(a.follow>0&&!a.pet){
      a.follow-=dt;
      if(pd>72) seekTo(a,player.x,player.y,a.sp*.9,dt,64);
      else { a.gx=player.x; a.gy=player.y; wanderEnt(a,dt,50,26); }
      a.face=Math.atan2(player.y-a.y,player.x-a.x);
      continue;
    }
    if(a.flee){
      if((pd<160||a.wary>0)&&!a.fed){
        const ang=Math.atan2(a.y-player.y,a.x-player.x);
        a.face=ang; moveEnt(a,Math.cos(ang),Math.sin(ang),a.sp*(a.wary>0?1:.8),dt);
      } else { a.gx=a.gx||a.x; a.gy=a.gy||a.y; wanderEnt(a,dt,70,32); }
      continue;
    }
    const hunts=!ISL.home&&!a.fed&&
      (a.pack?pd<250:(a.hurt>0||a.wary>0?pd<300:pd<(a.territory||150)));
    if(hunts){
      if(pd>a.r+player.r+3) seekTo(a,player.x,player.y,a.sp,dt,8);
      else if(a.cd<=0){ a.cd=.95; a.face=Math.atan2(player.y-a.y,player.x-a.x); a.swing=.16; hurtPlayer(a.dmg,a); }
      for(const m of party) if(!m.down&&dist(m.x,m.y,a.x,a.y)<a.r+m.r+3&&a.cd<=0){ a.cd=1.1; hurtAlly(m,a.dmg*.7); }
    } else { a.gx=a.gx||a.x; a.gy=a.gy||a.y; wanderEnt(a,dt,80,30); }
  }
}

/* ============================================================ RESIDENTS */
function residentCap(){ return 1+homeCount("house"); }
function leaveHere(m){
  if(!ISL.home){ toast("not here","Only on your own island."); return; }
  const res=(HOME.residents=HOME.residents||[]);
  if(res.length>=residentCap()){
    toast("nowhere to put them",`A hearth and ${homeCount("house")} house${homeCount("house")===1?"":"s"} sleeps ${residentCap()}. Build another.`);
    return;
  }
  party=party.filter(o=>o!==m);
  m.resident=true; m.gx=m.x; m.gy=m.y;
  res.push(m); townies.push(m);
  saveHome();
  toast(m.n.split(" ")[0]+" stays",`On ${ISL.name}. They will be here.`);
  logit("stayed behind",`${m.n} — ${ISL.name}.`);
  syncHUD();
}
function rejoin(m){
  if(party.filter(x=>!x.beast).length>=3){ toast("too many","Three is already more than this sea usually allows."); return; }
  HOME.residents=(HOME.residents||[]).filter(o=>o!==m);
  townies=townies.filter(o=>o!==m);
  m.resident=false; m.joined=true; m.down=0; m.bleed=0;
  party.push(m);
  saveHome();
  toast(m.n.split(" ")[0]+" comes along","Picks up whatever they had put down.");
  syncHUD();
}
function residentTalk(m){
  const opts=[{t:"Come with me.",fn:()=>rejoin(m)},{t:"Keep the place standing.",fn:()=>{}}];
  say(m.n,m.role+" · staying here",
    [`<i>"${pick(["The beds are aired. Nothing has come up out of the water.",
      "I have been turning your soil. Somebody had to.",
      "It is quiet here. I had forgotten quiet was a thing that happened.",
      "There is iron in the furnace and I did not touch it."])}"</i>`],opts);
}

/* ============================================================ CHART SCRAPS */
/* scraps count wherever they are — pack, hold, hearth, or still owed.
   avail() only sees a store you are standing beside, which made the count
   flicker and put the course out of reach of anyone who had stowed them. */
function fragCount(){
  return have("frag")+cacheHave("frag","hold")+cacheHave("frag","home")
       +((G.owed&&G.owed.frag)||0);
}
function spendFrags(){
  let need=FRAGS_FOR_ELITE;
  if(G.owed&&G.owed.frag){ const t=Math.min(need,G.owed.frag); G.owed.frag-=t; need-=t; }
  if(need>0){ const t=Math.min(need,have("frag")); if(t>0){ take("frag",t); need-=t; } }
  if(need>0){ const t=Math.min(need,cacheHave("frag","hold")); if(t>0){ cacheTake("frag",t,"hold"); need-=t; } }
  if(need>0){ const t=Math.min(need,cacheHave("frag","home")); if(t>0){ cacheTake("frag",t,"home"); need-=t; } }
  return need===0;
}
/* ============================================================ CITIES
   A city is data, not code. It arrives as a plain object — baked in below, or
   from a city.js on disk, or imported through the Y panel — and this file is the
   only thing that has to understand it. See CITY_FORMAT.md for the schema. */

/* tile names a designer may use in a district legend */
const TILENAME={
  water:WATER, shallow:SHOAL, sand:SAND, grass:GRASS, moss:MOSS, dirt:DIRT, road:PATH,
  scree:SCREE, snow:SNOW, ice:ICE, mire:MIRE, tree:TREE, pine:PINE, scrub:SCRUB,
  rock:ROCK, cliff:CLIFF, wall:WALL, floor:FLOOR, door:DOOR, plank:PLANK, ruin:RUIN, grave:GRAVE
};
const CITY_BAKED=[];                       // the example city appends itself here
let CITY_IMPORTED=[];
const cityFile=()=>((typeof window!=="undefined"&&window.__CITYF)||[]);
function citySnap(){
  if(typeof window==="undefined") return;
  window.__CITYF=window.__CITYF||[];
  const push=c=>{ if(c&&c.id){ window.__CITYF=window.__CITYF.filter(x=>x.id!==c.id); window.__CITYF.push(c); } };
  if(window.CITY_IMPORT) push(window.CITY_IMPORT);
  if(Array.isArray(window.CITIES)) window.CITIES.forEach(push);
  window.CITY_IMPORT=null; window.CITIES=null;
}
function allCities(){
  const out={};
  for(const c of CITY_BAKED) out[c.id]=Object.assign({},c,{__src:"baked in"});
  for(const c of cityFile()) out[c.id]=Object.assign({},c,{__src:"city.js"});
  for(const c of CITY_IMPORTED) out[c.id]=Object.assign({},c,{__src:"imported"});
  return Object.keys(out).map(k=>out[k]);
}
function cityById(id){ return allCities().find(c=>c.id===id)||null; }

/* ---- turning a district's picture into ground ---- */
function parseDistrict(d){
  const rows=d.map||[];
  const h=rows.length, w=rows.reduce((a,r)=>Math.max(a,r.length),0);
  const cells=new Uint8Array(Math.max(1,w)*Math.max(1,h));
  const legend=d.legend||{};
  const bad={};
  for(let y=0;y<h;y++){
    const row=rows[y]||"";
    for(let x=0;x<w;x++){
      const ch=row[x]===undefined?" ":row[x];
      let name=legend[ch];
      if(name===undefined){ if(ch!==" ") bad[ch]=1; name=ch===" "?"water":"water"; }
      const t=TILENAME[name];
      cells[y*w+x]=(t===undefined?WATER:t);
    }
  }
  return {w,h,cells,bad:Object.keys(bad)};
}
function districtReach(P,sx,sy){
  const seen=new Uint8Array(P.w*P.h);
  if(sx<0||sy<0||sx>=P.w||sy>=P.h) return seen;
  const q=[sx,sy]; seen[sy*P.w+sx]=1;
  while(q.length){
    const y=q.pop(), x=q.pop();
    for(const [a,b] of [[x+1,y],[x-1,y],[x,y+1],[x,y-1]]){
      if(a<0||b<0||a>=P.w||b>=P.h) continue;
      if(seen[b*P.w+a]) continue;
      const t=P.cells[b*P.w+a];
      if(SOLID[t]||t===SHOAL) continue;
      seen[b*P.w+a]=1; q.push(a,b);
    }
  }
  return seen;
}

/* ---- validation: the whole point is that a designer never reads game code ---- */
function validateCity(c){
  const e=[],w=[];
  const add=(arr,m)=>{ if(arr.indexOf(m)<0) arr.push(m); };
  if(!c||typeof c!=="object") return {ok:false,errors:["not an object"],warns:[],stats:{}};
  if(!c.id) add(e,"no id");
  if(!c.name) add(e,"no name");
  if(!Array.isArray(c.districts)||!c.districts.length) add(e,"no districts");
  const ids={}, stats={districts:0,npcs:0,gates:0,tiles:0};
  const stageIds={};
  if(c.story&&Array.isArray(c.story.stages))
    for(const s of c.story.stages){ if(!s.id) add(e,"a story stage with no id"); else stageIds[s.id]=1; }
  for(const d of (c.districts||[])){
    if(!d.id){ add(e,"a district with no id"); continue; }
    if(ids[d.id]) add(e,`two districts share the id "${d.id}"`);
    ids[d.id]=1; stats.districts++;
    if(!Array.isArray(d.map)||!d.map.length){ add(e,`${d.id}: no map`); continue; }
    const P=parseDistrict(d);
    stats.tiles+=P.w*P.h;
    for(const ch of P.bad) add(e,`${d.id}: nothing in the legend for the character "${ch}"`);
    for(const k in (d.legend||{})) if(!(d.legend[k] in TILENAME))
      add(e,`${d.id}: legend "${k}" says "${d.legend[k]}", which is not a tile name`);
    if(P.w>MAX_W||P.h>MAX_H) add(e,`${d.id}: ${P.w}x${P.h} is larger than the ${MAX_W}x${MAX_H} limit`);
    if(P.w<24||P.h<18) add(w,`${d.id}: ${P.w}x${P.h} is small for a district`);
    const rag=d.map.filter(r=>r.length!==P.w).length;
    if(rag) add(w,`${d.id}: ${rag} rows are shorter than the widest one — they will be filled with water`);
    let R=null;
    if(!Array.isArray(d.spawn)) add(e,`${d.id}: no spawn [x,y]`);
    else {
      const [sx,sy]=d.spawn;
      if(sx<0||sy<0||sx>=P.w||sy>=P.h) add(e,`${d.id}: spawn ${sx},${sy} is off the map`);
      else if(SOLID[P.cells[sy*P.w+sx]]) add(e,`${d.id}: spawn ${sx},${sy} is inside something solid`);
      else R=districtReach(P,sx,sy);
    }
    const onmap=(x,y)=>x>=0&&y>=0&&x<P.w&&y<P.h;
    const check=(o,what)=>{
      if(!onmap(o.x,o.y)){ add(e,`${d.id}: ${what} at ${o.x},${o.y} is off the map`); return; }
      if(SOLID[P.cells[o.y*P.w+o.x]]) add(e,`${d.id}: ${what} at ${o.x},${o.y} is inside something solid`);
      else if(R&&!R[o.y*P.w+o.x]) add(e,`${d.id}: ${what} at ${o.x},${o.y} cannot be walked to from the spawn`);
    };
    for(const n of (d.npcs||[])){
      stats.npcs++;
      if(!n.id) add(e,`${d.id}: an npc with no id`);
      if(!n.name) add(w,`${d.id}: npc "${n.id}" has no name`);
      check(n,`npc "${n.id||"?"}"`);
      for(const k in (n.lines||{})){
        if(k==="default"||k.indexOf("flag:")===0) continue;
        if(!stageIds[k]) add(w,`${d.id}: npc "${n.id}" has lines for stage "${k}", which no story stage declares`);
      }
      for(const o of (n.options||[])){
        if(!o.t) add(e,`${d.id}: npc "${n.id}" has an option with no text`);
        if(o.stage&&!stageIds[o.stage]) add(e,`${d.id}: npc "${n.id}" advances to stage "${o.stage}", which does not exist`);
        if(o.goto&&!o.goto.district) add(e,`${d.id}: npc "${n.id}" has a goto with no district`);
        for(const bag of [o.need&&o.need.items,o.pay&&o.pay.items,o.give&&o.give.items])
          for(const it in (bag||{})) if(!ITEMS[it]) add(e,`${d.id}: npc "${n.id}" refers to an item "${it}" that does not exist`);
      }
      if(n.shop){
        const cur=n.shop.currency||"silver";
        if(cur!=="silver"&&cur!=="vial") add(e,`${d.id}: npc "${n.id}" trades in "${cur}" — only silver or vial`);
        for(const s of (n.shop.sells||[])) if(!ITEMS[s.id]) add(e,`${d.id}: npc "${n.id}" sells "${s.id}", which does not exist`);
        for(const s of (n.shop.buys||[]))  if(!ITEMS[s.id]) add(e,`${d.id}: npc "${n.id}" buys "${s.id}", which does not exist`);
      }
    }
    for(const g of (d.gates||[])){
      stats.gates++;
      check(g,`gate to "${g.to}"`);
      if(!g.to&&!g.exit) add(e,`${d.id}: a gate with no "to" and no "exit"`);
      if(g.to&&!(c.districts||[]).some(x=>x.id===g.to)) add(e,`${d.id}: gate points at "${g.to}", which is not a district`);
      if(g.to&&g.at){
        const t=(c.districts||[]).find(x=>x.id===g.to);
        if(t){ const TP=parseDistrict(t);
          if(g.at[0]<0||g.at[1]<0||g.at[0]>=TP.w||g.at[1]>=TP.h)
            add(e,`${d.id}: gate to "${g.to}" lands at ${g.at[0]},${g.at[1]}, which is off that district`);
          else if(SOLID[TP.cells[g.at[1]*TP.w+g.at[0]]])
            add(e,`${d.id}: gate to "${g.to}" lands inside something solid`);
        }
      }
    }
    for(const n of (d.nodes||[])) if(!NODES[n.k]) add(e,`${d.id}: a node of kind "${n.k}" does not exist`);
    for(const a of (d.animals||[])) if(!ANIMALS[a.k]) add(e,`${d.id}: an animal of kind "${a.k}" does not exist`);
  }
  if(c.start&&c.start.district&&!ids[c.start.district]) add(e,`start district "${c.start.district}" does not exist`);
  const anyExit=(c.districts||[]).some(d=>(d.gates||[]).some(g=>g.exit))||
                (c.districts||[]).some(d=>d.harbour);
  if(!anyExit) add(w,"no district is marked harbour:true and no gate has exit:true — you may not be able to put back out to sea");
  return {ok:!e.length,errors:e,warns:w,stats};
}

/* ---- import, remember, export ---- */
const CITY_STORE="castaway_cities_v1";
function saveCities(){ try{ localStorage.setItem(CITY_STORE,JSON.stringify(CITY_IMPORTED)); return true; }catch(err){ return false; } }
function restoreCities(){
  try{ const raw=localStorage.getItem(CITY_STORE); if(!raw) return 0;
    const a=JSON.parse(raw); CITY_IMPORTED=Array.isArray(a)?a:[]; return CITY_IMPORTED.length;
  }catch(err){ return 0; }
}
function forgetCities(){ CITY_IMPORTED=[]; try{ localStorage.removeItem(CITY_STORE); }catch(err){} }
function parseCityJS(text){
  const box={};
  try{ (new Function("window","self","globalThis","document",text))(box,box,box,{}); }
  catch(err){ return {list:[],err:String(err.message||err)}; }
  const list=[];
  if(box.CITY_IMPORT) list.push(box.CITY_IMPORT);
  if(Array.isArray(box.CITIES)) for(const c of box.CITIES) list.push(c);
  if(!list.length){
    try{ const j=JSON.parse(text); if(j&&j.id) list.push(j); else if(Array.isArray(j)) list.push.apply(list,j); }catch(err2){}
  }
  return {list,err:list.length?null:"nothing set window.CITY_IMPORT or window.CITIES"};
}
function importCities(list){
  const took=[],failed=[];
  for(const c of list){
    const v=validateCity(c);
    if(v.ok){ CITY_IMPORTED=CITY_IMPORTED.filter(x=>x.id!==c.id); CITY_IMPORTED.push(c); took.push(c.name||c.id); }
    else failed.push({name:c&&(c.name||c.id)||"a city",errors:v.errors});
  }
  const kept=saveCities();
  return {took,failed,kept};
}
function reloadCityFile(then){
  if(typeof document==="undefined"||!document.createElement){ citySnap(); if(then) then(); return; }
  const paths=["city/city.js","city.js"];
  let left=paths.length;
  const done=()=>{ if(--left<=0){ citySnap(); if(then) then(); } };
  for(const p of paths){
    const sc=document.createElement("script");
    sc.onload=done; sc.onerror=done;
    sc.src=p+"?t="+Date.now();
    if(document.body&&document.body.appendChild) document.body.appendChild(sc); else done();
  }
}

/* ---- state that survives leaving and coming back ---- */
function cstate(cid){
  const s=G.cityState[cid]=G.cityState[cid]||{stage:0,flags:{},done:{}};
  s.flags=s.flags||{}; s.done=s.done||{};
  return s;
}
function curStage(c,st){
  if(!c.story||!Array.isArray(c.story.stages)) return null;
  return c.story.stages[Math.min(st.stage,c.story.stages.length-1)]||null;
}
function setStage(c,st,id){
  if(!c.story||!Array.isArray(c.story.stages)) return;
  const i=c.story.stages.findIndex(s=>s.id===id);
  if(i<0||i<=st.stage) return;
  st.stage=i;
  const s=c.story.stages[i];
  toast(c.name,s.objective||"Something has moved.");
  logit(c.name,s.objective||("Stage: "+s.id));
}

/* ---- entering, and moving between districts ---- */
function mkCityNpc(c,n){
  const p=tc(n.x,n.y);
  return {x:p.x,y:p.y,gx:p.x,gy:p.y,r:11,n:n.name||n.id,role:n.role||"",
    face:rnd()*6.28,ph:rnd()*6.3,c:n.c||pick(["#5b6a6f","#6d5a48","#4f6157","#6a5566"]),
    cnpc:n,city:c.id,fixed:!!n.fixed,line:""};
}
function enterCity(cid,did,at){
  const c=cityById(cid);
  if(!c){ toast("no such place","That city is not loaded."); return; }
  const v=validateCity(c);
  if(!v.ok){
    toast("that city will not load",v.errors[0]);
    logit("city refused",(c.name||cid)+" — "+v.errors[0]);
    return;
  }
  const d=(did?c.districts.find(x=>x.id===did):null)
        ||(c.start&&c.start.district?c.districts.find(x=>x.id===c.start.district):null)
        ||c.districts[0];
  const P=parseDistrict(d);
  resizeWorld(P.w,P.h);
  grid.set(P.cells.subarray(0,GW*GH));
  const pets=animals.filter(a=>a.pet);
  nodes=[];foes=[];neutrals=[];townies=[];parts=[];zones=[];shots=[];corpses=[];props=[];
  builds=[];gates=[];animals=pets;
  ISL={theme:d.theme||"town",threat:0,name:c.name,sub:d.name||d.id,city:c.id,district:d.id,
       buildings:[],labels:d.labels||[],spirit:null,story:null,mini:null,boss:null,bigs:[],
       cleared:true,home:false,seen:0,dests:null,harbour:!!d.harbour};
  const sp=at||d.spawn||[2,2];
  const q=tc(sp[0],sp[1]);
  player.x=q.x; player.y=q.y;
  if(!free(player.x,player.y,player.r)){
    for(let rr=1;rr<14;rr++){ let done=false;
      for(let dy=-rr;dy<=rr&&!done;dy++)for(let dx=-rr;dx<=rr&&!done;dx++){
        const w=tc(sp[0]+dx,sp[1]+dy);
        if(free(w.x,w.y,player.r)){ player.x=w.x; player.y=w.y; done=true; }
      }
      if(done) break; }
  }
  player.inv=0; player.roll=0; player.cd=0;
  ISL.start=[Math.floor(player.x/TILE),Math.floor(player.y/TILE)];
  floodReach(ISL.start[0],ISL.start[1]); collectSpots();
  for(const n of (d.npcs||[])) townies.push(mkCityNpc(c,n));
  for(const g of (d.gates||[])){
    const gp=tc(g.x,g.y);
    gates.push({def:g,city:c.id,x:gp.x,y:gp.y,tx:g.x,ty:g.y,r:16,ph:rnd()*6.3});
  }
  for(const n of (d.nodes||[])) if(NODES[n.k]){
    const np=tc(n.x,n.y);
    nodes.push({tx:n.x,ty:n.y,x:np.x,y:np.y,k:n.k,uses:n.uses||1,ph:rnd()*6.3});
  }
  for(const p of (d.props||[])){ const pp=tc(p.x,p.y); props.push({k:p.k||"hull",x:pp.x,y:pp.y,a:0}); }
  for(const a of (d.animals||[])) if(ANIMALS[a.k]){
    const ap=tc(a.x,a.y);
    const an=mkAnimal(a.k,ap.x,ap.y,{fed:1,tame:0});
    if(a.name) an.n=a.name;
    if(a.tame) an.tp=3;
  }
  for(const m of party){ m.x=player.x+(rnd()*40-20); m.y=player.y+(rnd()*40-20); m.down=0; m.bleed=0; }
  for(const a of animals) if(a.pet){ a.x=player.x+(rnd()*46-23); a.y=player.y+(rnd()*46-23); a.gx=a.x; a.gy=a.y; }
  G.tint=d.tint||THEMES.town.tint;
  spawnT=1e9; G.mode="play";
  cstate(c.id); G.citiesSeen[c.id]=1;
  logit(c.name,(d.name||d.id)+".");
  syncHUD(); drawChart();
}
function goDistrict(cid,did,at,text){
  if(text) interlude(cityById(cid)?cityById(cid).name:"",did,text,()=>enterCity(cid,did,at));
  else enterCity(cid,did,at);
}

/* ---- needs, payment and effects, shared by npc options and gates ---- */
function bagLack(need){
  if(!need) return null;
  const miss=[];
  if(need.silver&&avail("silver")<need.silver) miss.push(`${need.silver} silver`);
  if(need.vial&&avail("vial")<need.vial) miss.push(`${need.vial} mist vial${need.vial>1?"s":""}`);
  for(const k in (need.items||{})) if(avail(k)<need.items[k]) miss.push(`${need.items[k]} ${ITEMS[k].n}`);
  if(need.flag&&!cstate(need.__cid||"").flags[need.flag]) miss.push("something you have not done");
  if(Array.isArray(need.flags)){
    const st=cstate(need.__cid||"");
    const n=need.flags.filter(f=>!st.flags[f]).length;
    if(n) miss.push(`${n} thing${n>1?"s":""} you have not done yet`);
  }
  if(need.stage&&need.__city){
    const c=cityById(need.__cid||""), st=cstate(need.__cid||"");
    if(c&&curStage(c,st)&&curStage(c,st).id!==need.stage) miss.push("this is not the moment");
  }
  return miss.length?("wants "+miss.join(", ")):null;
}
function bagPay(bag){
  if(!bag) return;
  const c={};
  if(bag.silver) c.silver=bag.silver;
  if(bag.vial) c.vial=bag.vial;
  for(const k in (bag.items||{})) c[k]=bag.items[k];
  if(Object.keys(c).length) pay(c);
}
function bagGive(bag){
  if(!bag) return;
  if(bag.silver) giveTell("silver",bag.silver);
  if(bag.vial) giveTell("vial",bag.vial);
  for(const k in (bag.items||{})) giveTell(k,bag.items[k]);
  if(bag.mist) gainMist(bag.mist,"a city",true);
}
function optKey(def,o){ return (def.id||def.name)+"|"+(o.t||"").slice(0,24); }
function doCityOption(c,st,def,o){
  const lack=bagLack(Object.assign({__cid:c.id},o.need||{}));
  if(lack){ toast("not yet",lack); return; }
  bagPay(o.pay);
  bagGive(o.give);
  if(o.flag) st.flags[o.flag]=1;
  if(o.clear) st.flags[o.clear]=0;
  if(o.once) st.done[optKey(def,o)]=1;
  if(o.stage) setStage(c,st,o.stage);
  const after=()=>{
    if(o.goto) goDistrict(c.id,o.goto.district,o.goto.at,o.goto.text);
    else if(o.ending) showEnding(o.ending);
    syncHUD();
  };
  if(o.say&&o.say.length) say(def.name||def.id,def.role||"",o.say.slice(),null,after);
  else after();
}
function cityTalk(t){
  const c=cityById(t.city); if(!c) return;
  const st=cstate(c.id), def=t.cnpc, stage=curStage(c,st);
  let lines=null;
  for(const k in (def.lines||{}))
    if(k.indexOf("flag:")===0&&st.flags[k.slice(5)]) lines=def.lines[k];
  if(!lines&&stage&&def.lines&&def.lines[stage.id]) lines=def.lines[stage.id];
  if(!lines&&def.lines) lines=def.lines.default;
  lines=(lines||[`<i>They look at you the way people look at weather.</i>`]).slice();
  const opts=[];
  for(const o of (def.options||[])){
    if(o.once&&st.done[optKey(def,o)]) continue;
    if(o.onStage&&(!stage||stage.id!==o.onStage)) continue;
    if(o.afterStage&&(!stage||c.story.stages.findIndex(s=>s.id===o.afterStage)>st.stage)) continue;
    if(o.needFlag&&!st.flags[o.needFlag]) continue;
    if(o.hideFlag&&st.flags[o.hideFlag]) continue;
    const lack=bagLack(Object.assign({__cid:c.id},o.need||{}));
    opts.push({t:o.t+(lack?` — ${lack}`:""),off:!!lack,fn:()=>doCityOption(c,st,def,o)});
  }
  if(def.shop){
    const cur=def.shop.currency||"silver";
    opts.push({t:cur==="vial"?"Ask about the other trade.":"Trade.",fn:()=>shopTalk(t,def.shop)});
  }
  opts.push({t:"Leave it there.",fn:()=>{}});
  say(def.name||t.n,def.role||"",lines,opts);
}
function shopTalk(t,shop){
  const cur=shop.currency||"silver", curN=ITEMS[cur].n;
  const opts=[];
  for(const s of (shop.sells||[])){
    const n=s.n||1, price=s.price||1;
    const can=avail(cur)>=price;
    opts.push({t:`Buy ${n} ${ITEMS[s.id].n} — ${price} ${curN}`,off:!can,fn:()=>{
      pay({[cur]:price}); giveTell(s.id,n);
      toast("bought",`${n} ${ITEMS[s.id].n} for ${price} ${curN}.`); syncHUD(); }});
  }
  for(const s of (shop.buys||[])){
    const n=s.n||1, price=s.price||1;
    const can=avail(s.id)>=n;
    opts.push({t:`Sell ${n} ${ITEMS[s.id].n} — ${price} ${curN}`,off:!can,fn:()=>{
      pay({[s.id]:n}); giveTell(cur,price);
      toast("sold",`${n} ${ITEMS[s.id].n} for ${price} ${curN}.`); syncHUD(); }});
  }
  opts.push({t:"Not today.",fn:()=>{}});
  say(t.cnpc.name||t.n,shop.title||(cur==="vial"?"the quiet trade":"trade"),
    [`<i>${shop.line||(cur==="vial"
      ? "The counter he uses for this is not the counter he uses for everything else."
      : "Prices are prices. He says the number and then waits.")}</i>`],opts);
}
function gateTalk(g){
  const d=g.def;
  if(d.exit){
    if(!G.craft){ toast("no craft","You have nothing on the water to leave in."); return; }
    togglePanel("chart"); return;
  }
  const c=cityById(g.city), st=cstate(g.city);
  const lack=bagLack(Object.assign({__cid:g.city},d.need||{}));
  if(lack){ toast(d.label||"the gate",(d.refuse||"Not this way.")+" "+lack); return; }
  if(d.needFlag&&!st.flags[d.needFlag]){ toast(d.label||"the gate",d.refuse||"It is shut to you."); return; }
  if(d.pay) bagPay(d.pay);
  goDistrict(g.city,d.to,d.at,d.text);
}
function cityObjective(){
  if(!ISL.city) return null;
  const c=cityById(ISL.city); if(!c) return null;
  const st=cstate(c.id), s=curStage(c,st);
  if(!s) return null;
  return {title:(c.story&&c.story.title)||c.name,text:s.objective||"",hint:s.hint||""};
}
/* the next port you have heard of but cannot yet find */
function unknownPort(){
  for(const c of allCities()) if((c.minIsl||0)<=G.isl&&!G.ports[c.id]) return c;
  return null;
}
function readPort(){
  const c=unknownPort();
  if(!c){ toast("nothing new on it","Every coast drawn on this one is a coast you can already steer for."); return false; }
  G.ports[c.id]=1;
  interlude("A chart, of sorts",c.name,
    `Soundings in one hand and a coastline in another, and a course off the sea route drawn in a third that has
     pressed hard enough to score the vellum.<br><br>
     <b>${c.name}</b> — ${c.sub||"a place with people on it"}. ${c.chartNote||""}<br><br>
     <i>It is on your chart now. It was not before.</i>`,
    ()=>{ syncHUD(); drawChart(); });
  logit("a chart",`${c.name} — you know where it is now.`);
  return true;
}
function cityChartRows(){
  const rows=[];
  for(const c of allCities()){
    if((c.minIsl||0)>G.isl) continue;
    if(!G.ports[c.id]) continue;              // you need a chart to it before it is a place you can steer for
    if(ISL.city===c.id) continue;
    const st=G.cityState[c.id];
    const been=!!G.citiesSeen[c.id];
    rows.push({id:c.id,name:c.name,sub:c.sub||"",been,stage:st?st.stage:0,
      note:c.chartNote||(been?"You have walked it.":"You have heard of it.")});
  }
  return rows;
}

/* ============================================================ MARIE ISLAND
   The example city, baked in. It is only data: everything here could equally
   arrive from a city.js on disk or through the Y panel, and the game would not
   know the difference. See CITY_FORMAT.md. */
CITY_BAKED.push(
{
 "id": "marie",
 "name": "Marie Island",
 "sub": "a trading settlement on the Sea of Specters",
 "chartNote": "Forty people, a trade post, and one dead navigator. A long crossing west of the sea route.",
 "minIsl": 2,
 "currency": {
  "silver": "silver",
  "vial": "mist vial"
 },
 "start": {
  "district": "docks"
 },
 "arrive": "The fog opens on a pier with three men standing on it who do not wave.<br><br>Forty people, a trade post, a hall, and no engine anywhere on the island. It is the fifth day of November weather and the crossing east closes when the ice does.<br><br><i>Nothing here will try to kill you. That is not the same as nothing being wrong.</i>",
 "story": {
  "title": "The dead navigator",
  "stages": [
   {
    "id": "arrive",
    "objective": "Ask the dockhand what happened here, then get up the road into town.",
    "hint": "Silas Marbury, on the quay."
   },
   {
    "id": "ask",
    "objective": "Three people in town know a piece of it. Northrup on the street, Vosbury in the trade post, the widow who keeps the hall.",
    "hint": "Then ask Northrup who moved the body."
   },
   {
    "id": "east",
    "objective": "Somebody has been past the trail with a spade. Go east and look at the old ground yourself.",
    "hint": "The apprentice outside the lodge saw the spade."
   },
   {
    "id": "north",
    "objective": "Nobody has asked the Hendricks girl anything. She is up the north trail with four dogs.",
    "hint": "Then find Captain Holl on the quay."
   },
   {
    "id": "hall",
    "objective": "Get the town into the meeting hall and name somebody. You get one room and one go at it.",
    "hint": "Widow Vane opens the hall."
   },
   {
    "id": "done",
    "objective": "It is finished. The crossing closes when the ice does — put back out from the docks.",
    "hint": "The chart is at the water's edge."
   }
  ]
 },
 "districts": [
  {
   "id": "docks",
   "name": "The Docks",
   "harbour": true,
   "theme": "town",
   "w": 92,
   "tint": "rgba(48,58,66,.16)",
   "legend": {
    "~": "water",
    "-": "shallow",
    ".": "sand",
    ",": "grass",
    "\"": "scrub",
    "T": "tree",
    "P": "pine",
    "o": "rock",
    "#": "wall",
    "_": "floor",
    "+": "door",
    "=": "plank",
    ":": "road",
    "%": "ruin",
    "t": "grave",
    "m": "mire",
    "M": "moss",
    "d": "dirt",
    "i": "ice"
   },
   "map": [
    "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~",
    "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~",
    "~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T~~~",
    "~~~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT,\",,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,o,,,,,,,,,,,\",o,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,\",,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--ddddd,,,,,,,,,,,,,,,,\",o,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,T,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~-dd\"dd,,##########,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~-ddddd,,#________#,o,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--ddddd,,#________#,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,\",,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,#________#,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,\",,,,,,,~~~",
    "~~~~~~~~~============ddddd,,#________#,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,~~~",
    "~~~~~~~~~============ddddd,,####,,,###,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,T\",,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,\",\",,,,,,,,,,,,,,,,,,,,,,,,,o,,,,,o,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~--..ddddd,,,,,,,,,,,,,,,,,,,,###########,,,,,,,\"o,,,,,,,,,,,,\",,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~--..,,ddddd,,,,,,,,,,,,,,,\",,,,#_________#,\",,,,,,,,,,,,T,,,\",,,,,,,\",,,,T~~~",
    "~~~~~~~~~~~~~~~~--..,d\"ddd,,,,,,,,,,,,,,,,,,,,#_________#,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..,ddddd,,,,,,,,,,,,,,,,,,,,#_________#,,,,,,,,,,,,,,,,,,,,,,o,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~--..,,ddddd,,,,,,,o,,,,,,,,,,,,#_________#,,,,\",,,,,,\",,\",,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..,ddddd,,,\",,,,,,,,,,,,,,,,#_________#,,,,,,,,,\"T,,,,,,,,,\",,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--ddddd,,,,,,,,,,,,,,,,,,,,####,,,####,,,,,\",,,,,,,,,,,,,,,,,,T,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..\"ddddd,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,\"\",,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,,,,,,o,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,T,,,TT,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~-,ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,o,,,,,,,,,,,T~~~",
    "~~~~~~===============ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,T,,,,,~~~",
    "~~~~~~===============ddddd:::::::::::::::::::::::::::::::::,::::,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd:::::::::::::::::::::::::::::::::,::::,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,\",,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd,,,,,,,,,,,o,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~-ddddd,,,,,,,,\",,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~-ddddd,,\",,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,o,,,,,,,,T,,,T,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--ddddd,,,,,,,,,,,,,,\",,,,o,,,\",,,,,,,,,,,,,,,,,,T,,,,\",,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,T,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~-ddddd,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,T,,,,,~~~",
    "~~~~~~~~~~~~~~~~~--..ddddd,,,,####+####,,,,o##,,,,,###o,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,o,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,#_______#,,,,,#________#,,,,,,,,,,,,,,,,,,,T\",,,\",,,,,,,,,,~~~",
    "~~~~~~~~~~~==========ddddd,,,,#_______#,,,,,#________#,,,o,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~==========ddd\"d,,,,#_______#\",,,,#________#,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,#_______#,,,,,#________#,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,\",~~~",
    "~~~~~~~~~~~~~~~~~--..ddddd,,,,#########,,,,,##########,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,o~~~",
    "~~~~~~~~~~~~~~~~--..,ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,o,,,,,,,,,,,,,,,,,,,o,,,,,~~~",
    "~~~~~~~~~~~~~~~~~--..ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~--..ddddd,,,,,,,,,,,,,,,,\",,,,,,,,\",,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--.ddddd,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,o~~~",
    "~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,o,o,,,,,,,,,T,,,,,T,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,T,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~--..,\",,,,,\",,,,,,,,,,,,,,,,,,,\",,,,,,,,,,\",,\",,,,,,,,,,,,,,,,,,,,,,,,,~~~",
    "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~",
    "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
   ],
   "spawn": [
    23,
    31
   ],
   "labels": [
    {
     "x": 33,
     "y": 15,
     "t": "BOAT SHEDS"
    },
    {
     "x": 34,
     "y": 44,
     "t": "NET LOFT"
    },
    {
     "x": 51,
     "y": 23,
     "t": "HARBOURMASTER"
    },
    {
     "x": 49,
     "y": 44,
     "t": "CREW CABIN"
    }
   ],
   "npcs": [
    {
     "id": "silas",
     "name": "Silas Marbury",
     "role": "dockhand",
     "x": 24,
     "y": 20,
     "c": "#5d7480",
     "lines": {
      "default": [
       "You came in on that. Whatever that is.",
       "Marie Island. Forty of us, less the one we buried nothing of. You'll want to keep your hands where people can see them for a day or two."
      ],
      "arrive": [
       "You came in on that. Whatever that is.",
       "Six days back we had a man in the water east of the sheds. Ansel Thorpe. Left side of his head opened up.",
       "The hunters are calling it ice."
      ],
      "ask": [
       "Still here. Good. Nobody's told me to stop being useful yet."
      ],
      "hall": [
       "The hall, is it. I'll come. I've carried enough of this island to be owed a chair."
      ],
      "done": [
       "It's quieter. I don't know that quieter is the same as better."
      ]
     },
     "options": [
      {
       "t": "Who have they settled on?",
       "onStage": "arrive",
       "once": true,
       "say": [
        "The Hendricks girl, up in the north woods.",
        "Her father took a hatchet to a village up the coast when she was small, so the sum writes itself. Nobody here does the sum twice.",
        "<i>He says it without any particular feeling, the way a man reads a weight off a scale.</i>"
       ],
       "flag": "blame",
       "stage": "ask"
      },
      {
       "t": "Ice doesn't hit a man once and stop.",
       "onStage": "arrive",
       "say": [
        "No.",
        "I said that in the hall on the second day. Tolus Brann looked at the floor until I stopped."
       ]
      },
      {
       "t": "Where was he found?",
       "needFlag": "blame",
       "say": [
        "Shallows, east of the sheds. Walk down and look at it yourself. It's forty feet of water that wouldn't drown a dog."
       ]
      }
     ]
    },
    {
     "id": "kegg",
     "name": "Sawyer Kegg",
     "role": "ice fisher",
     "x": 22,
     "y": 45,
     "c": "#6b6f63",
     "lines": {
      "default": [
       "There's a drum out past the shallows. Two nights running.",
       "I've told four people. Four people have told me it's ice moving."
      ],
      "hall": [
       "You're the one calling the hall? Then I'll say it in front of everybody and they can look at the floor together."
      ],
      "done": [
       "Drum's still going. Nobody's asked me about it since."
      ]
     },
     "options": [
      {
       "t": "What does it sound like?",
       "once": true,
       "say": [
        "Like somebody walking behind you and stopping when you stop.",
        "Not fast. It has a gap in it where the next beat should be, and the gap is the part I don't like."
       ],
       "flag": "drum"
      }
     ]
    },
    {
     "id": "bindon",
     "name": "Karl Bindon",
     "role": "greenhorn",
     "x": 33,
     "y": 18,
     "c": "#7d6a58",
     "lines": {
      "default": [
       "You're off the water too? Good. Everyone here talks like the water is a person.",
       "Our captain's in the crew cabin down the quay. He'll want to know what you saw coming in, and he'll ask twice."
      ]
     },
     "options": [
      {
       "t": "How long have you been here?",
       "once": true,
       "say": [
        "Nine days. I've spent six of them being told the same thing by different people.",
        "<i>He looks at the boat sheds rather than at you while he says it.</i>"
       ]
      }
     ]
    },
    {
     "id": "holl",
     "name": "Captain Ernest Holl",
     "role": "a captain, not from here",
     "x": 49,
     "y": 41,
     "c": "#a8442a",
     "lines": {
      "default": [
       "You'll be wanting off this island. So will I. The ice comes in before either of us manages it."
      ],
      "east": [
       "You've been up the trail. Then you've seen the ground out there."
      ],
      "north": [
       "The girl talked to you. She hasn't talked to anyone else in six days."
      ],
      "hall": [
       "The hall, then. I'll hold the door and Cundy will do the talking until it's worth being direct."
      ],
      "done": [
       "It held. Barely. Get off this island before the ice closes the crossing."
      ]
     },
     "options": [
      {
       "t": "Can this be put in front of the whole town?",
       "onStage": "north",
       "once": true,
       "say": [
        "It can. Widow Vane keeps the hall and she'll open it for an accusation faster than for a funeral.",
        "Get everything you have straight first. You'll get one room and one go at it."
       ],
       "stage": "hall"
      }
     ]
    },
    {
     "id": "cundy",
     "name": "George Cundy",
     "role": "first mate",
     "x": 47,
     "y": 41,
     "c": "#b0a48c",
     "lines": {
      "default": [
       "Forty people and one dead navigator. Everybody on this island knows which forty-first name is missing off the list.",
       "Don't ask me who. Ask them, and watch which ones answer fast."
      ]
     },
     "options": [
      {
       "t": "Fast is bad?",
       "once": true,
       "say": [
        "Fast means they've said it before. To themselves, mostly.",
        "<i>He goes back to the rope in his hands without waiting to see whether that helped.</i>"
       ]
      }
     ]
    },
    {
     "id": "harbour",
     "name": "The harbourmaster",
     "role": "keeps the crossings book",
     "x": 51,
     "y": 27,
     "c": "#6c7a86",
     "lines": {
      "default": [
       "Nothing sails out of here until the weather says so, and the weather has stopped saying anything.",
       "Losten is the nearest place with more than a hundred people in it. A long crossing east, and only two on this island could ever do it in weather. Now there's one."
      ]
     },
     "options": [
      {
       "t": "Who is the one?",
       "once": true,
       "say": [
        "Enric Kell. He hunts, and he crosses. He's out past the trail more days than he's in a bed.",
        "Thorpe was the other."
       ],
       "flag": "crossings"
      }
     ]
    },
    {
     "id": "nets",
     "name": "A woman mending nets",
     "role": "",
     "x": 24,
     "y": 36,
     "c": "#7f8f6d",
     "lines": {
      "default": [
       "Mind the third pier. Two boards gone and nobody's replacing them until spring."
      ]
     }
    },
    {
     "id": "sign_shallows",
     "name": "The shallows east of the sheds",
     "role": "turned water",
     "x": 22,
     "y": 49,
     "c": "#8b9793",
     "lines": {
      "default": [
       "<i>Forty feet of water that would not drown a dog. There is a line in the gravel where something heavy was dragged in and let go.</i>",
       "<i>Whatever came in here came in from the land.</i>"
      ]
     },
     "options": [
      {
       "t": "Look at the gravel properly.",
       "once": true,
       "say": [
        "<i>Two grooves, a boot's width apart, running down from the tree line and stopping at the water.</i>",
        "<i>Nobody drags themselves in feet first.</i>"
       ],
       "flag": "dragged"
      }
     ],
     "sign": true
    }
   ],
   "gates": [
    {
     "x": 62,
     "y": 31,
     "to": "town",
     "at": [
      5,
      38
     ],
     "label": "up the road into town",
     "text": "The road runs a half mile inland between two rows of stacked wood, and every window you pass has somebody in it."
    },
    {
     "x": 21,
     "y": 30,
     "exit": true,
     "label": "put back out to sea"
    }
   ],
   "props": [
    {
     "x": 19,
     "y": 31,
     "k": "hull"
    }
   ],
   "nodes": [
    {
     "x": 27,
     "y": 22,
     "k": "log"
    },
    {
     "x": 28,
     "y": 24,
     "k": "drift"
    },
    {
     "x": 26,
     "y": 45,
     "k": "drift"
    },
    {
     "x": 30,
     "y": 36,
     "k": "reeds"
    },
    {
     "x": 56,
     "y": 26,
     "k": "stones"
    }
   ]
  },
  {
   "id": "town",
   "name": "The Town",
   "theme": "town",
   "tint": "rgba(52,44,30,.14)",
   "legend": {
    "~": "water",
    "-": "shallow",
    ".": "sand",
    ",": "grass",
    "\"": "scrub",
    "T": "tree",
    "P": "pine",
    "o": "rock",
    "#": "wall",
    "_": "floor",
    "+": "door",
    "=": "plank",
    ":": "road",
    "%": "ruin",
    "t": "grave",
    "m": "mire",
    "M": "moss",
    "d": "dirt",
    "i": "ice"
   },
   "map": [
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT",
    "TT,,,,,\",,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,TT",
    "TT,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,,,,T,,,,T,,,,,,,,,,,,,T,,,T,,,\",,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,T,,,,,\",,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,TT",
    "TT,,T,,,,,,,,,,,,,,,\",,,\",,\",,,,,,T,,,,,,,T,,,,,,,,,,,T,::::,,,,,,,,,\",,,,,,,o\",,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,\",,TT",
    "TT,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,T,,,,TT",
    "TT,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,T,,,,,,,,,,,,,,,T::::,,,,,,,,,,\",,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,TT",
    "TT,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,TT",
    "TT,T,,,,,T,,,,,,,,,,,,,,,,,T,,,,,,,T,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,T,,,,,,,T,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,\",,TT,,,::::,,,,,,,,T,\",,,,,,T,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,TT",
    "TT,,,,,,,,,T,,,,,,,,,,,,T,,,,,,o,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,T,,,,,,,,,,,,,T,\",,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,TT",
    "TT,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,::::,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,T,,,,,,TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,:,,,,,T,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,T,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,T,,o,,,,,:\",,,,,,,,,,T,,,,,,,,,,,,,T,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,T,,,,,,,,,,,T,,,,,:,,,,,,,,,T,,,,T,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TTT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,T\",,,,,,,,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,T,,,:,,,,,,,,T,,,,,,\",,,,,,,,,,,,,T,,T,,,,,,,,,,,,,,,,TT,,,,,,TT",
    "TT,,T,,,,,,,,,,,,,,,,,\",\",,\",,,,,,,\",,,,,,,,,,,,,,,,T,,,,,:,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,\",,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,\",,,,,,\",,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,o,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,######:#########,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,#_____:________#,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,#_____:________#,,,o,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\"#_____:________#,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,############,,,,,,,,,,##########,,,\",,#_____:________#,,\",,,,,,,,,############,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,\",,,,,,,#__________#,,,,,,,,,,#________#,,,,,,#_____:________#,,,,,,,,,,,,#__________#,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,#__________#\",,,,,,,,,#________#,,,,,,#_____:________#,,,,,,,,,,,,#__________#,,,\",,##########\",,,,,,,TT",
    "TT,,,,,,,,,,,,#__________#,,,,,,,,,,#________#,,,,,,#_____:________#,,,,,,,,,,,,#__________#,,,,,,#________#,,,,,,,,TT",
    "TT,,,,,,,,,,,,#__________#,,,,,,,,,,#________#,,,,,,######:,,,######\",,,,,,,,,,,#__________#,,,,,,#________#,,,,,,,,TT",
    "TT,,,,,,,\",o,,#__________#,,,,,,,,,,#________#,,\",,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,#__________#,,,,,,#________#,,,,,,,,TT",
    "TT,,,,,,,,,,,,#__________#,,,,,,,,,,####,,,###,,,,,,dddddd:ddddddd,,,,,,,,,,,,,,#__________#,,,,,,#________#,,,,,,,,TT",
    "TT,,,,,,,,,,\",#####,,,####,,,,,,,,,,,,,,,,,,,,,\",,,,dddddd:ddddddd,,,,,,,,,,,,,,#####,,,####,,,,,,####,,,###,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,:,\",,,,,,,,,,,,,,,,\",:,,,,,,,,,,dddddd:d:ddddd,,,,,,,,,,,,\",,,,,,,:,,,,,,,,,,,,,,,,:,,,o,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,:,,\",,,,,,,,,,,,,,,,,:,,,,,,,,,,dddddd:d:ddddd,,,,,,,,,,,,,,\",,,,,:,,,,,,,,,,,,,,,,:,,,,,,,,,,,,TT",
    "TT,:::::,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,dddddd:d:ddddd,,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,o:,,,,,,:::::,TT",
    "TT,:::::::::::::::::::::::::::::::::::::::::::::::::dddddd:d:ddddd:::::::::::::::::::::::::::::::::::::::::::::::::,TT",
    "TT,:::::::::::::::::::::::::::::::::::::::::::::::::dddddddd:ddddd:::::::::::::::::::::::::::::::::::::::::::::::::,TT",
    "TT,:::::::::::::::::::::::::::::::::::::::::::::::::dddddddd:d:ddd:::::::::::::::::::::::::::::::::::::::::::::::::,TT",
    "TT,:::::,\",,,,,,,,,,,:,,,,,,,,,,,,,,,,,:,,,,,,,,,,,\"dddddddddd:ddd\"\",,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,o,,,,,,,,,:::::,TT",
    "TT,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,dddddddddd:ddd,,,,,,,,,,,,,,,,,,,o,:,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,\",:,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,dddddddddd:ddd,,,,,,,,,,,,,,oo,,,,,:,,,,,,,\",,,,,,,,,,,,,,o,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,:,,,\",,,,o,,,ddddddd\"dd:ddd,,,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,\",,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,:,,,,,o,,,,,,,,,,o,,,,,,,:,,,,,,,,,,,o,,,,,\",,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,:,,,,,\",,\",,,,,,,,,,,,,,,:,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,\",,,,####,:,###,,,,,,,,,\",,,:,,,,,,,,,,,,,,,,,,,\",,:,,o,,,,\",,,,,,,,,,,#####:####,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,#________#,,,,,,,,#####+#####,,,,,,,,,o,#####,,,####,,,,,,,,,,,,,,#________#,,,,o,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,#________#,,,,,,,,#_________#,,,,,,,,\",o#__________#,,,,,,,,,,,,,,#________#,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,#________#,,,,,,,,#_________#,,,,,,,,,,,#__________#,,,,,,,,,,,,,,#________#,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,#________#,,,,,\",,#_________#,,,,,,,,,,,#__________#,,,,,,,,,o,,,,#________#,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,#________#,,,,,,,,#_________#,,,,,,,,,,,#__________#,,,,,,,,,,,,,,#________#,,,,,,,,,,,,,,,,,,,,,,o,TT",
    "TT,,,,,,,,,,,,,,##########,,,,,,,,#_________#,,,,,,,,,,,#__________#,,,,,,,,,,,,,,##########,,,,,,,,,,,,,\",,,,,,,,,,TT",
    "TT,,,,,\",,,,,,,,,,,,,,,\",,,,,,,,,,###########,,,,,,,,,,,#__________#,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,############,o,,,,,,,,,,,,,,,,,,,,,,,o,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,o,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,o,,,,,\",,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,o,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,,,T,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,T,,,,T,,,,T,,,,,,,T,,,,,T,,,,T,,,,T,,,,,,,,T,,,,,,,,,,,,,,,,,T,,,,,TT",
    "TT,,,,,,,,,,,,,,T,\",,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,T,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,T,,,\"\",,,,,,,TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,T,,,,,,\",,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,T,,,,,T,,,,,,,,,TT,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TTT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,o,,,o,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT,,,,,,,,T,,,,,,,,,,,,,T,TT",
    "TT,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,T,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,T,,,,,\",,,\",,,,,\",,,,,,,,,,,,,,,,T,,,,,,,,,,,,,T,,,,,,,,,,,,,,TT,,,,,,,,T,,,TT,,,,,,,,,,T,,,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T\",,,,,,,,,,,,,,,,o,,,,,,,,,\",,,,,,,,,,,,,,,,,,,,,,,o,,o,,,,,,,,,,,,,,,,,,,,T,,,,,,T,TT",
    "TT,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,\",,,,,,,T,,,,,T,,,\",,,\",T,,,,,o,,T,,o,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,TT",
    "TT,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,To,,\",T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,\"T,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,,,,,,,,,,\",,,,,,,,,,,,,\",,,,,,,,,,,,,,,,T,,,TT,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,,T,,,,,,,,,,,,,,,,,,,,,,,T,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,T,,,T,,,T,,,,\",,,,,\",,,,,,,,,,,,,,,T,,,,,\",,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,TT",
    "TT,,,,\",,,,\",,,,,,,,,,,,,T,,,,,,,,,,T,,,,,,,,,,,,,,,,,,,,,,,,\",T,,,,,,,,,,,,\",,T,,,,,T,,,,,,,,,T,,,,,,,,,T,,,,T,,,,,TT",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT"
   ],
   "spawn": [
    5,
    38
   ],
   "labels": [
    {
     "x": 20,
     "y": 29,
     "t": "VOSBURY & SON"
    },
    {
     "x": 41,
     "y": 29,
     "t": "BAKEHOUSE"
    },
    {
     "x": 60,
     "y": 26,
     "t": "MEETING HALL"
    },
    {
     "x": 86,
     "y": 29,
     "t": "HUNTERS' LODGE"
    },
    {
     "x": 103,
     "y": 30,
     "t": "WINTER STORE"
    },
    {
     "x": 21,
     "y": 49,
     "t": "THORPE HOUSE"
    },
    {
     "x": 39,
     "y": 50,
     "t": "COOPERAGE"
    },
    {
     "x": 62,
     "y": 50,
     "t": "THE LONG ROOM"
    },
    {
     "x": 87,
     "y": 49,
     "t": "SMITHY"
    }
   ],
   "npcs": [
    {
     "id": "leon",
     "name": "Leon Northrup",
     "role": "rookie merchant",
     "x": 50,
     "y": 38,
     "c": "#c9a24a",
     "lines": {
      "default": [
       "You're the one off the boat. Nobody comes in this month, so you're either lost or you're trouble."
      ],
      "ask": [
       "You're the one off the boat. Then you'll understand why the island's in a state.",
       "Ansel Thorpe read charts. Ansel Thorpe was the only man here who could take a boat to Losten in weather.",
       "Six days back they found him in the shallows. They're calling it ice, and they've settled on the Hendricks girl."
      ],
      "east": [
       "Four people asked me this morning if I'd seen a copper kettle. None of them could tell me whose kettle."
      ],
      "hall": [
       "Tomorrow in the hall? I'll stand up front. I'm the only one here who can read a ledger out loud."
      ],
      "done": [
       "I keep books. I'll be putting this one down exactly as it happened, and I'll be the only one who does."
      ]
     },
     "options": [
      {
       "t": "What has the town lost besides a man?",
       "onStage": "ask",
       "once": true,
       "say": [
        "Inventory. A whole winter of it.",
        "Four people asked me about a copper kettle and not one of them could name whose it was. Two of them own one.",
        "When a town misplaces its own things in one season, that isn't forgetting. I keep books. I know what forgetting looks like on paper."
       ],
       "flag": "forgotten"
      },
      {
       "t": "So who moved him?",
       "need": {
        "flags": [
         "ledger",
         "laidout",
         "forgotten"
        ]
       },
       "once": true,
       "say": [
        "Somebody who could carry a grown man from the trees to the water and lay a knife out afterwards.",
        "The ground east of here, past the trail. The old burial plot, by the broken tower. Somebody has been out there with a spade and it isn't the widow.",
        "<i>He says the word spade and then looks up the road as if he has just heard it himself.</i>"
       ],
       "stage": "east"
      }
     ]
    },
    {
     "id": "raymond",
     "name": "Raymond Vosbury",
     "role": "merchant",
     "x": 20,
     "y": 34,
     "c": "#8a6f4b",
     "lines": {
      "default": [
       "Sit, sit- there's rum in it if you're civil. Not much else, mind."
      ],
      "ask": [
       "Thorpe. Yes. Terrible. Terrible. He kept my shipping book for me, the runs and the weights, since my son-",
       "Since my son came home and didn't stay long."
      ],
      "east": [
       "Kell brings back nothing worth a ledger line and I sign for it anyway. Don't look at me like that. I sign it because it's easier than being asked twice."
      ],
      "hall": [
       "You'll want the hall, then. I'll open my mouth in it, which is more than I've managed in a week."
      ],
      "done": [
       "Whatever had hold of my head- it came in through a purchase. I paid for it. Remember that when the story gets told."
      ]
     },
     "options": [
      {
       "t": "May I see the shipping book?",
       "onStage": "ask",
       "once": true,
       "say": [
        "You may see what's left of it.",
        "Three leaves gone. The last three Losten crossings, cut out with a blade- look at the edge of it.",
        "I did not do that, and I cannot remember who I let near it."
       ],
       "flag": "ledger"
      },
      {
       "t": "Who was crossing to Losten this winter?",
       "needFlag": "ledger",
       "say": [
        "Kell. Thorpe before him. The pages that are gone are the pages with their weights on."
       ]
      }
     ],
     "shop": {
      "currency": "silver",
      "title": "Vosbury & Son",
      "line": "He says the number and then waits, and he does not fill the silence.",
      "sells": [
       {
        "id": "wood",
        "n": 6,
        "price": 9
       },
       {
        "id": "cloth",
        "n": 3,
        "price": 12
       },
       {
        "id": "pitch",
        "n": 2,
        "price": 14
       },
       {
        "id": "fiber",
        "n": 6,
        "price": 8
       },
       {
        "id": "bandage",
        "n": 2,
        "price": 11
       },
       {
        "id": "herb",
        "n": 2,
        "price": 8
       }
      ],
      "buys": [
       {
        "id": "hide",
        "n": 1,
        "price": 7
       },
       {
        "id": "bone",
        "n": 2,
        "price": 4
       },
       {
        "id": "meat",
        "n": 2,
        "price": 6
       },
       {
        "id": "glass",
        "n": 1,
        "price": 18
       },
       {
        "id": "ore",
        "n": 2,
        "price": 9
       },
       {
        "id": "ingot",
        "n": 1,
        "price": 22
       }
      ]
     }
    },
    {
     "id": "cordelia",
     "name": "Widow Cordelia Vane",
     "role": "keeps the meeting hall",
     "x": 60,
     "y": 31,
     "c": "#6c7a86",
     "lines": {
      "default": [
       "Boots. Off. This floor is the only clean thing on Marie Island."
      ],
      "ask": [
       "Boots. Off.",
       "You want the dead man. I laid him out on that table, so I'll tell you what the hunters won't.",
       "There was no lake in him. A man who drowns comes up full. Ansel came up dry as a hymn book."
      ],
      "east": [
       "Old ground east of here, past the trail. Missionaries, traders, and whoever the sea wanted.",
       "We don't bury anymore- the sea won't hold what's put into it. Somebody has been out there with a spade regardless."
      ],
      "north": [
       "You went and asked her. Six days and you're the first."
      ],
      "hall": [
       "You can have the hall. You'll have it packed, too. Nothing fills a room here like an accusation."
      ],
      "done": [
       "I've laid out nine people on that table in eleven years. He's the only one I've had to argue about."
      ]
     },
     "options": [
      {
       "t": "How was he laid out when you got him?",
       "onStage": "ask",
       "once": true,
       "say": [
        "Cold through, and the wound had stopped bleeding a good while before the water got him. He was dead on land.",
        "Then somebody laid the girl's knife behind the sheds like a place setting. Fresh snow under it and none on top.",
        "Tell me what kind of murderer keeps a knife dry for a week and then loses it tidy."
       ],
       "flag": "laidout"
      },
      {
       "t": "Open the hall. I know who carried him.",
       "need": {
        "flags": [
         "dug",
         "alisa",
         "ledger",
         "laidout",
         "forgotten"
        ]
       },
       "onStage": "hall",
       "once": true,
       "say": [
        "Then say the name in front of them and don't say it twice.",
        "<i>She is already reaching for the door.</i>"
       ],
       "flag": "ready"
      },
      {
       "t": "Name Enric Kell in front of the town.",
       "needFlag": "ready",
       "once": true,
       "say": [
        "Kell.",
        "<i>The room does not gasp. It goes quiet in the specific way a room goes quiet when everybody has been waiting to be allowed.</i>",
        "Tolus Brann stands up and says it was ice. Northrup reads the three cut pages out of the ledger- the weights, the crossings, the two names on them.",
        "Kell gets as far as the door. Cundy is standing in it.",
        "<i>They put him in the boat sheds with two hunters on the door. Somebody remembers whose kettle it was. Then somebody else does.</i>"
       ],
       "give": {
        "silver": 90,
        "vial": 2,
        "mist": 140
       },
       "flag": "solved",
       "stage": "done"
      },
      {
       "t": "Name the Hendricks girl.",
       "needFlag": "ready",
       "once": true,
       "say": [
        "Her.",
        "<i>Nobody argues. That is the part that stays with you.</i>",
        "They take her down to the sheds and Togo has to be dragged off the door.",
        "<i>Two days later a copper kettle turns up in the bakehouse with nobody's name on it, and by then the crossing is closed.</i>"
       ],
       "give": {
        "silver": 30
       },
       "flag": "wrong",
       "stage": "done"
      }
     ]
    },
    {
     "id": "nettie",
     "name": "Nettie Boyington",
     "role": "twelve years old",
     "x": 44,
     "y": 38,
     "c": "#b98a9a",
     "lines": {
      "default": [
       "Are you staying? Nobody stays."
      ],
      "ask": [
       "I'm not supposed to talk to you. Everyone says that and then talks to you anyway."
      ],
      "east": [
       "I had a doll with a blue dress. I know I had it because I remember being angry about it.",
       "I don't remember what I was angry about."
      ],
      "hall": [
       "Mother says I'm not going in the hall. I'm going in the hall."
      ],
      "done": [
       "I remembered the doll. It was in the box under the bed the whole time.",
       "That's worse, I think. Nobody moved it."
      ]
     },
     "options": [
      {
       "t": "What else has gone missing?",
       "once": true,
       "say": [
        "Mr Vosbury's son's coat. The kettle. A knife off the bakehouse table.",
        "Grown-ups say I'm making it up, and then they ask me if I've seen the kettle.",
        "<i>She counts the list off on her fingers and gets to four before she stops and starts again.</i>"
       ],
       "flag": "kettle"
      }
     ]
    },
    {
     "id": "jean",
     "name": "Jean Farsue",
     "role": "baker",
     "x": 41,
     "y": 33,
     "c": "#a8763f",
     "lines": {
      "default": [
       "Bread on the second and fifth day. It is the fifth day of nothing, so no."
      ],
      "east": [
       "You have been talking to Northrup. He asked me about the shipping records. I asked him first."
      ],
      "done": [
       "I am not baking much. I have not been baking much since October. You may write that down if you are writing things down."
      ]
     },
     "options": [
      {
       "t": "Why do you want the shipping records?",
       "once": true,
       "say": [
        "Because I can read them and because two names on them cross to Losten and one of those names is in the water.",
        "I am Catal. When something goes wrong on an island like this, the arithmetic reaches me by the fourth day. I would rather have paper in my hand when it does."
       ],
       "flag": "baker"
      }
     ],
     "shop": {
      "currency": "silver",
      "title": "the bakehouse",
      "line": "There is flour on the counter and nothing on the shelf.",
      "sells": [
       {
        "id": "roast",
        "n": 2,
        "price": 10
       },
       {
        "id": "berry",
        "n": 4,
        "price": 6
       },
       {
        "id": "herb",
        "n": 1,
        "price": 5
       }
      ],
      "buys": [
       {
        "id": "meat",
        "n": 3,
        "price": 8
       },
       {
        "id": "berry",
        "n": 6,
        "price": 5
       }
      ]
     }
    },
    {
     "id": "tolus",
     "name": "Tolus Brann",
     "role": "head hunter",
     "x": 86,
     "y": 34,
     "c": "#8d6a4f",
     "lines": {
      "default": [
       "Ice. It was ice. You'll hear four other stories and they'll all be from people who have never pulled a body out of anything."
      ],
      "east": [
       "Kell hunts with me. Kell has hunted with me for nine years.",
       "<i>He says the number twice, which is once more than a man usually says a number.</i>"
      ],
      "hall": [
       "I'll be in the hall. I'll be saying ice."
      ],
      "done": [
       "Nine years.",
       "I'm the one who signed for his powder. Put that in whatever you're keeping."
      ]
     },
     "options": [
      {
       "t": "Ice hit him once and stopped?",
       "once": true,
       "say": [
        "Ice does what it likes.",
        "<i>He does not look up from the rifle across his knees, and he does not put it down either.</i>"
       ]
      }
     ]
    },
    {
     "id": "anpo",
     "name": "Anpo",
     "role": "hunter's apprentice",
     "x": 88,
     "y": 36,
     "c": "#7f8f6d",
     "lines": {
      "default": [
       "I'm not supposed to be the one who tells you things."
      ],
      "east": [
       "Kell went east four nights this week and came back with nothing worth a ledger line.",
       "He took a spade the second night. I saw it against the sled and I didn't ask, because you don't."
      ],
      "hall": [
       "If it goes wrong in the hall, get behind the table. That's all I know about halls."
      ],
      "done": [
       "Tolus hasn't spoken since. He's cleaning the same rifle he cleaned yesterday."
      ]
     },
     "options": [
      {
       "t": "A spade, east, at night.",
       "onStage": "east",
       "once": true,
       "say": [
        "Four nights. And he goes past the trail, not along it. Nobody goes past the trail.",
        "The old ground is out there. So is the tower that came down in the war."
       ],
       "flag": "spade"
      }
     ]
    },
    {
     "id": "boy",
     "name": "A boy with a bucket",
     "role": "",
     "x": 30,
     "y": 38,
     "c": "#9a927f",
     "lines": {
      "default": [
       "Everyone's in the hall or on the way to it. There's nothing else to be in."
      ]
     }
    },
    {
     "id": "women",
     "name": "Two women outside the long room",
     "role": "",
     "x": 62,
     "y": 46,
     "c": "#6a5566",
     "lines": {
      "default": [
       "- and I said to her, if the sea won't hold them, then what was the point of the ground.",
       "<i>They stop when they see you and start again about the weather.</i>"
      ]
     }
    },
    {
     "id": "store",
     "name": "The winter store",
     "role": "short of everything",
     "x": 103,
     "y": 34,
     "c": "#5b6a6f",
     "lines": {
      "default": [
       "Winter merchants are three weeks late. We've stopped setting the place for them."
      ]
     },
     "shop": {
      "currency": "silver",
      "title": "the winter store",
      "line": "Half the shelves have been turned to face the wall.",
      "sells": [
       {
        "id": "cloth",
        "n": 4,
        "price": 15
       },
       {
        "id": "pitch",
        "n": 3,
        "price": 18
       },
       {
        "id": "stone",
        "n": 8,
        "price": 7
       }
      ],
      "buys": [
       {
        "id": "wood",
        "n": 8,
        "price": 6
       },
       {
        "id": "fiber",
        "n": 8,
        "price": 6
       },
       {
        "id": "frag",
        "n": 1,
        "price": 40
       }
      ]
     }
    },
    {
     "id": "sign_thorpe",
     "name": "The Thorpe house",
     "role": "boarded from outside",
     "x": 21,
     "y": 45,
     "c": "#7d6a58",
     "lines": {
      "default": [
       "<i>Boards across the door, nailed from this side. A chart is still pinned in the window, and the pin has been moved recently enough to leave two holes.</i>",
       "<i>Whoever boarded it did it in daylight, in front of everybody.</i>"
      ]
     },
     "options": [
      {
       "t": "Look at the chart in the window.",
       "once": true,
       "say": [
        "<i>The crossing to Losten is drawn on it three times in three different hands, and the last hand is not the one that drew the coast.</i>",
        "<i>Somebody has been learning the route off a dead man's window.</i>"
       ],
       "flag": "window"
      }
     ],
     "sign": true
    }
   ],
   "gates": [
    {
     "x": 5,
     "y": 38,
     "to": "docks",
     "at": [
      62,
      31
     ],
     "label": "back down to the docks"
    },
    {
     "x": 112,
     "y": 38,
     "to": "trail",
     "at": [
      3,
      34
     ],
     "label": "east, past the last house",
     "text": "The houses stop. The trail keeps going for another mile and a half, and the ground on either side of it gets softer the further east you go."
    },
    {
     "x": 58,
     "y": 6,
     "to": "woods",
     "at": [
      46,
      62
     ],
     "label": "north, up into the woods",
     "text": "The road becomes a sled track and then becomes two ruts in old snow. Somewhere ahead of you a dog starts barking and three others take it up."
    }
   ],
   "nodes": [
    {
     "x": 24,
     "y": 41,
     "k": "nettle"
    },
    {
     "x": 70,
     "y": 41,
     "k": "stones"
    },
    {
     "x": 48,
     "y": 60,
     "k": "log"
    }
   ]
  },
  {
   "id": "trail",
   "name": "East, Past the Trail",
   "theme": "forest",
   "tint": "rgba(24,44,34,.18)",
   "legend": {
    "~": "water",
    "-": "shallow",
    ".": "sand",
    ",": "grass",
    "\"": "scrub",
    "T": "tree",
    "P": "pine",
    "o": "rock",
    "#": "wall",
    "_": "floor",
    "+": "door",
    "=": "plank",
    ":": "road",
    "%": "ruin",
    "t": "grave",
    "m": "mire",
    "M": "moss",
    "d": "dirt",
    "i": "ice"
   },
   "map": [
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT..--~~~~~~~~",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT..--~~~~~~~~",
    "TT,,,TT,,,,,T,TT,,\",,,,,,TT,,,,,T,,TTT,,T,TT,,,T,,TT,T,,,T,T,T,\"T,\",T,,,T,T,,,,,T,,,,,,T..--~~~~~~~~",
    "TTTTTT,\"TT,,,,,TTT,,,,T,,ToT,,TT,T,\",,,,,T,,,,,,T,\",T,,T,,T,,To,T,,TT,,,T,,,T,,,,T,T,T\",..--~~~~~~~~",
    "TT,,TT,,T,T,,,,T\",T,\",,T\",,,T\",TT,,TTT,,T,,,TT,,,TT,,TT,,TTT,TT,\",,T,,,,,,,T,TT,\",,,,T,,..--~~~~~~~~",
    "TT,,,T,,,T,To,,,T,T,,T,,T\",TT,T,,T\",TT,,,T,TT,,,,,,\",T,\",,,,TTT,T,,TT,T,T,T,,,,T,,,,,,,\"..--~~~~~~~~",
    "TT,\"T,,,,\"\"T,,T,,T,,,,,,,T,TT,T,,,,,,,,,TTTTTTTT,T,TTT,T,,,,,,T,,,TT,,,T,T,,\"T,,,TT,,,\",..--~~~~~~~~",
    "TT\",,\",,,,T,,,,\",T,T,T,,,,,,,,T,TT,,,,,,,TT,,,,T,TTTT,,\"T,,,,,T,TT,,,,T,TT,,,,,T,,,,,,,,..--~~~~~~~~",
    "TT,TTTTT,,,,,,TT,T,T,T,TT,,,,,,\"T,\",T,,,,,,,,,,,\",TT,,T,,T,,,T,TT,T,TTTT,,o,,,,,T,,TTT,,..--~~~~~~~~",
    "TT,ToTT,TT,T,,T,,,TT,,,T,T,,,,,,,,,,\",,,,,,,TT,TTT,,T,,,,,,,T,T,T,,,TT,,TT,,,,,,TTT,,\",T..--~~~~~~~~",
    "TTTT,,TT,,T,,TTT,,T,,TTTT\"T,,,,,,T,T,T,T,TT,,T,,,,,\",,,T,,,T,T,T,,,,,,T,TTT,\",,,,,T,,\",,..--~~~~~~~~",
    "TT,\",T,,TTT,,,,\",,TT,,,,,T,,,T,T,T\",,,,TT,,TTTT,,,,TT,,,,TTT,T,T,,,T,TTT,,,,,,,,T,,T,,,T..--~~~~~~~~",
    "TT,,,TTT,\",,,,,,,T,,TT,,TT,,,TTT,,,,,,,T,T,,T,,,,,,TTT,,T,TTT\",\",T,,,T,,,T,,TT,T,TT,,\",T..--~~~~~~~~",
    "TT\",,,T,T\",,,,,,T,,,,,TT,,T,,,TT,,,T,,,T,,T,TT,TTT,,,,,,,,T,T,,T,TT,\",,T,,\",T,,,,,TT,,,,..--~~~~~~~~",
    "TT\",T\"TTTT,,,,,TT,T,\",,,,T,,,,\",TT,,T,,,,T,T,,T,,,,,,,,,,,TTT,,,\",,T,,T,T,,T,T,,,T,TT,,T..--~~~~~~~~",
    "TT,,,,T,T,,,,T,,,,,,,,,TT,,,,,To,T,TT,T,,,,,,,T,TTT,,,,,T\"o,,T,,T,,,,,,,T\",,,,,,,,,T,,,,..--~~~~~~~~",
    "TTTTT,,,,,,,T,,,,,,,,T,,,,TT,,T,TT,,,,,,T,,,,,,,,,T\"\"TT,,TTT,,T,,TT,,T,,%%%%ddd%%%T,\"TT,..--~~~~~~~~",
    "TTT,,T,,,,,TTTTTT,,T\",,,,,,\",,T,,,T,T,TT,,,,,,,,TT,TT,,T,T,TT,,TTT,TTT,,%%%%ddd%%%,,,T,,..--~~~~~~~~",
    "TT,T,,,,,,T,,T,,,,TTTTT,\",T,,T,T,T,T,,,,,TTTTT,,,,,,,T,,,,,,,\",,,T,,TTT,%%MMMMMM%%,\",,,,..--~~~~~~~~",
    "TTT,,T,T,,\"TT,,,,TT,,,T\"T,TT,,,,TT,T\",,T,,T,,,,,,,T,,,,,,,,,TT,T,\",TT,,T%%MMMMMM%%,T,,TT..--~~~~~~~~",
    "TTT,,T,,T,,,,T,,,,T,,TTT,T,T,,T,,TTT,,T,,T,T,oTT,,,,,T,T,T\"T,,,,T,oT,TT,%%MMMMMM%%,,T,,,..--~~~~~~~~",
    "TT,T,,TTT,T,TT,,,,,,,,,,T,,\",T,,\",T,,,T,,,TTT,,TTT,,T,T,,TTT\",,,T\"T,T,TT%%MMMMMM%%,,,,TT..--~~~~~~~~",
    "TTT,T\",T,,T,TT,,,\"T,T,,,,,TT,,,,,,,,\",,,,T,,,,,,,,,,,T,,,,,TTT,,,,,,,T,,%%MMMMMM%%T,,T,\"..--~~~~~~~~",
    "TT,,TTT,TT,,\"T,,,T,,,,T,\",,,,,,T,,,,,TT,o,,T,T,,,,,T,,,,TT,T,TT,T,T,,,,,%%MMMMMM%%,,TT,,..--~~~~~~~~",
    "TT,,T,,T,TT,T,,,T,,,,,,,,TT,,TT,TT,TT\",To,T,,TT,,T,TTT,,,oT,,T,,,T,T,,,,%%%%%%%%%%,,T,,,..--~~~~~~~~",
    "TT,,,,\",T,,,,T,,T,,TTT,,,,,\",\"TT,TT,T\"T,o,TT,,,T,,\",T,T\"TT,T,,,TT,T,,,,,%%%%MM,%%%,\",T,,..--~~~~~~~~",
    "TT,,,,,,o,,,,,,T,,,T,,,,,TT,,,,,,,,,T,,,,,T,,,,,T,,,,,,,T,,,,,,,,,,,,,T,,T\",,:,TT,T,,,T,..--~~~~~~~~",
    "TT,T\",,,T,,,T,T,,,,,,T,,T,,,T,T,,,,,,,TTT,,,,T,,,,,T,,,T,,,,,T,,,T,To,,\",TTT,:,T,,,T,,\",..--~~~~~~~~",
    "TT,T,,T,,,\",,TT,,,,,TT,T,,,T,,,\",,,TTTTT,,,,,,TTTT,T,TT,T,,,,,T,\",,TT,,,T,,,T:,,T,,TT,,,..--~~~~~~~~",
    "TT,,T,,,,T,,TTTT,,,,,T,T,T,,T,,,TTT,T,,,TTT,,,TTT,,\",TT,T,,\",,,,TTTTT,T,T,,TT:,T,,,,,,,,..--~~~~~~~~",
    "TTT,,,,,TTT,,,TT,,T,,TT,T,,TT,TTT,,,,T,T,,TT,,,,,,TT,TTT\",,,,,,,,,,TT,,T,,T,T:\"T,\"T,,,,T..--~~~~~~~~",
    "TT,TT,T,,,T,,,,,TT,,,T,T,T,:::::::::::::,TT,T,TT,,TT,,T,,,,T,,,,TTT,T,,TT,,o,:,,T,,,,,,,..--~~~~~~~~",
    "TTT,T,TT,,,,,,:::::::::::::::::::::::::::::::::::::::,T,T,T,,\",,,,,,T,,T,,,,,:,oT\",TTT,\"..--~~~~~~~~",
    "TTd:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::,TT,T,,,T,T:,:::::::,T..--~~~~~~~~",
    "TTd::::::::::::::::::::::::,,,,,,,T,,TT,::::::::::::::::::::::::::::::::::::::::::::::T,..--~~~~~~~~",
    "TTd:::::::::::T,,\"T,,,,,,,,,,,,,,,,,TT,TTTTT,,T,T,T,,:::::::::::::::::::::::::::::::::,,..--~~~~~~~~",
    "TT,,,T,,T,,T,,,TT,,TT,,,,,\"T,T,,,T,,,,,T,,T,,,,,,TT,,,T,,,T,,T,,,T:::::::::::::TTTTTT,,o..--~~~~~~~~",
    "TT,,,T,T,T\"T,T,,,,,,T,,TT,,,,T,TT,,,T,,TT,,,T,,,TTT,,,,,,,,,,,,TT,T,TT,,,T:TTTT\"T,,o,,T,..--~~~~~~~~",
    "TTT,,,T,,,,,,TTTT,,,o,,,,,,T\"T,,TT,,,,T,,,,,,,\",T,,,,,,,,,TT,TT,T,TTTT,,TT:,,,,TT,TT,,,,..--~~~~~~~~",
    "TT,,,,TT\",,TTT,,T,,,T,T,,,,,,T,,\",T,T,T,,,,TT,,,,T,,T,,T,T,\",TT,T,,,,TT,T,:,,,,T,,,,TTTT..--~~~~~~~~",
    "TTT,T,,,,,,T,,,,,,,,o,TT,,,,,,,,,,,,\"TT,,,TTT,,T,T,T,,TTT,,,,,T,,,T,,,,,,,:T,,,T,,TT\"T,,..--~~~~~~~~",
    "TTT,,T,,,,T,,,TTT,,\",,T,,,,TT,,T,,TT,,T,,T,,,,\",,,T,,TTTT,,,,T,TTT,T,,,,\"\":,T,T,,,,,,TT,..--~~~~~~~~",
    "TTT,,TT,,T,,T,,TT,,,,T,,,,T\"T,T,,,,,,,,T,,,,TTT,T,T,,,,T,,,,,,TT,,\",,,T,,\":T,,,,T,,,T,,,..--~~~~~~~~",
    "TT,T,,T,,,,,,,T,,TT,,,,T,T,,,,T,,,T,TT,TTT,,\"T,TTT,,\"T,,TTToT,,,,,,\",T,,,,:TTTT,,,\"\"T,T,..--~~~~~~~~",
    "TT,T,TTT,,T,TT,,\",T,,,T,T,,,,TT\",,,,T,,,,,T,T,T,,TT,T,,TT,T\"TT,,MMMMMMtMMM:tMttMMMMM,TTT..--~~~~~~~~",
    "TTTT,T,\",,,T,,,,,,,,,,,TT,,T,,,TTT,,T,,,,T,TT,,TT,,,,T,,,,,,,,,,MtMMMMMMMMMMMMMMMMtM,TT,..--~~~~~~~~",
    "TT,T,,,,,,T,T,,T,T,,TTT,,,T,,,,,,T,T\",,,,,,,,T,,,T,,T,TT,,T,To,,MMMMtMMMMMMMMMMMMMMM,TT\"..--~~~~~~~~",
    "TT,,,,,,,,,,To,,TT,,,,\",TT,,,,,T,,,T,\",,T,,,,T,,,TT,,TT,,,,,,T,\"MMMtMMMMMMMMMMMMMMMM,T,,..--~~~~~~~~",
    "TT,,,,,,,,,,,T,,,,,T,,,T,\",,T,,T,\"TT,T,To,,TTTT,,TTT,T,T,,,T,,T,MMMMMMMMMMMMMMMMMMMM,,,,..--~~~~~~~~",
    "TTTT,,,\"T,,T,,T,,T,,,,T,,,T,TTT,,,,,T,,,,\"T,,,,,,TTTTTTT,T,TTT,,MtMMMMMMMMMMMMMMMMMM,,,T..--~~~~~~~~",
    "TT,,,,o,,,T,,,\"o,,,,,TTT,,T,TTT,,,,,T,,\",,,T,,TT,,T\",,TT,\",,\",,TMMMMMMMtMtMMMMMMMMMMT,,,..--~~~~~~~~",
    "TT\"T,,,,,T,,\"T,,\",,,TT,,,T,,,,,,T,T,,,TT,,,T,,TT,TT,,,,,,,,,,,T,MMoMMtMMMM,MMMtMoMMM,,TT..--~~~~~~~~",
    "TTT,,,,,\",,TT\",TT,\"TT,,,TT\",,TTTTTT,,T,T,,,T,,,,,o,T,,,,,,,,\",,,MMMMMMMMMMMMMtMMMMMtTT,T..--~~~~~~~~",
    "TTT\",,TT,TT,,,T,,m,,,T,\",,,\",o,,,,,,,,,,TT,,TTTTm,,,T,\",,T,,,,,TMMMMMMMMMMMMMMoMMMMMm,,,..--~~~~~~~~",
    "TTT,,,,,,TT,,T,,,,TT,,,\",T,T,,TT,,T,T,,T\",T,TT,T,T,,,,T,TT,TT,T,MMmMMMMMMMMMMMMMMMMM,,,,..--~~~~~~~~",
    "TTT,,TTT,,TT,TTT,T,,TT,T,,,,,,,T,,,,,T,,,,T,To,,TT,T,,,,T,T\",,,,MMMMMMMMMMMMMMMMMMMM,,,T..--~~~~~~~~",
    "TT,,,,T,T,,T\",,,TT,,,,TT,,TT,,T,,,T,,,,T,,TT,,,T,,T,TT,T,,T,\"\"TTMMMMMMMMMmtMMMtMMtMo,,,,..--~~~~~~~~",
    "TT,,,,,T,,T\",,,,,,T,,,,T,T,,,T,T\",,m,T,,T,T,,,,TT,T,,,,TTT,,,TT,MMMMMMMMMMMMMMMMMMMt,T,,..--~~~~~~~~",
    "TTTT,,,,T,,T,,,T,T,,,TT,,,,,,,TT,T,,\",,T,oT,,T,,,m,,T,Tm,TTT,,,TMtMMMMMMmMMMMtMMMMMm,,,,..--~~~~~~~~",
    "TT,,T,,m,,,,T,,,T,,,T,T,,,T,,,T,,,,,,,,,T,m,T,,,TTT,T,,,,o,T,,,,MmMMMMtMMMMMMMMMMMMM,,,,..--~~~~~~~~",
    "TT,,T,,,,,T,,\"T,TTT,T,\"T,,,T,T,,,T\"T,,,\",,T\",,,,,mTTT,,,,\",T,T,,,,\"TT,,,TT,,,mTT,,TT,\"T,..--~~~~~~~~",
    "TT,\",,,,TTTTT,TTT,,,,,TT,,T,T,,,,,,T,T,,,,TT,,TmTT,,,,,,,,,T,TTT,,,,T,TT,,,TTT,,T,T,,T,,..--~~~~~~~~",
    "TTTTT,,mT,TT,,T,,,,,,,,TT,,T,,,T,oT,,,,T,,,,,T,,,,,T,,m,TTT,,T,,,T\",T,T,,T,TT,,,,,,\"TTT,..--~~~~~~~~",
    "TTTTT,,,,,\",T,,T\",,,,,,,\",T,Tm,,,T,,,,,T,,mT,,,TT,T,,Tm,T,T,,TTT,T,,,T,T,TT,\"T,T,TmT,,,,..--~~~~~~~~",
    "TT,TT,T,,T,,TT,T\"TTTT,,,,,,T,TT,\",T,,T,,,,TT,,,,Tm,,TTT,,,T,,T,\"TT\"m,TT,TT,T,,,T,T,,,,,,..--~~~~~~~~",
    "TT,TTT,,T,T\",TTT,,,,,,,TT,,,,,T,TTT\",,,T,,TT,,\"T,,,,,T,,T,,,,T,TTTT,,T,,,mT,T,,,,,T,T,,,..--~~~~~~~~",
    "TT,TT,,,T,,,\",,T\",,,,To,T,TT,TT,,T,\"T,,,,,,TT,,,,,,,TTT,\",,,,TTT,T,,T,,TT,\",,,T,TT,TT,TT..--~~~~~~~~",
    "TTT\",,T,,,,T,,,,TTT,,T,T\",,T,,,,,T\"T,,m,T,,T,,\",TT,T,\",T,,,\"T,,o,\",,,,,T,TT,,,T,,,T,,,,,..--~~~~~~~~",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT..--~~~~~~~~",
    "TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT..--~~~~~~~~"
   ],
   "spawn": [
    3,
    34
   ],
   "labels": [
    {
     "x": 77,
     "y": 21,
     "t": "THE TOWER"
    },
    {
     "x": 74,
     "y": 47,
     "t": "OLD GROUND"
    }
   ],
   "npcs": [
    {
     "id": "kell",
     "name": "Enric Kell",
     "role": "hunter",
     "x": 74,
     "y": 50,
     "c": "#8f2f1f",
     "lines": {
      "default": [
       "You're a long way past the trail for somebody who came in yesterday."
      ],
      "east": [
       "You're a long way past the trail.",
       "There's nothing out here. Old ground, a tower that fell in the war, and me, because the deer come through at dusk.",
       "<i>There is fresh earth on his boots and none on the spade leaning behind him.</i>"
      ],
      "hall": [
       "The hall. Yes. I'll be at the back where I can hear it properly."
      ],
      "done": [
       "-"
      ]
     },
     "options": [
      {
       "t": "You've been digging.",
       "onStage": "east",
       "say": [
        "I've been setting snares.",
        "Snares want a hole. Holes want a spade. You'll want to be careful how many words you put next to each other out here."
       ]
      },
      {
       "t": "What do you bring back from Losten?",
       "needFlag": "crossings",
       "once": true,
       "say": [
        "Whatever the crossing is worth carrying.",
        "Vosbury signs for it and doesn't read it. You'd be surprised what a man will sign for twice."
       ],
       "flag": "metkell"
      }
     ],
     "shop": {
      "currency": "vial",
      "title": "the other trade",
      "line": "He does not take the vials out where the light is.",
      "sells": [
       {
        "id": "glass",
        "n": 3,
        "price": 1
       },
       {
        "id": "frag",
        "n": 1,
        "price": 2
       },
       {
        "id": "ore",
        "n": 6,
        "price": 1
       }
      ],
      "buys": []
     }
    },
    {
     "id": "jeren",
     "name": "Jeren",
     "role": "hunter's apprentice",
     "x": 30,
     "y": 34,
     "c": "#96968a",
     "lines": {
      "default": [
       "Trail's the trail. Don't go off it and you'll be at the shore by dark."
      ],
      "east": [
       "Kell's out past the old ground. He'll tell you the deer come through there. The deer do not come through there.",
       "I've hunted this island since I was nine. There is nothing on that side worth four nights."
      ]
     },
     "options": [
      {
       "t": "Why does nobody go past the trail?",
       "once": true,
       "say": [
        "Because the ground out there was a burial plot before the rule changed, and because the tower came down on it.",
        "And because the sea won't hold what's put into it, so what got put in the ground is all that's left of anybody."
       ]
      }
     ]
    },
    {
     "id": "sign_tower",
     "name": "The broken tower",
     "role": "came down in the war",
     "x": 77,
     "y": 26,
     "c": "#6a6a63",
     "lines": {
      "default": [
       "<i>Stone in a ring, and the lamp housing on its side in the middle of it with sixty years of moss in the glass.</i>",
       "<i>Somebody has cleared the moss off one shelf inside, and set three things on it, and taken them away again. The rings they left are dry.</i>"
      ]
     },
     "options": [
      {
       "t": "Look at the shelf.",
       "once": true,
       "say": [
        "<i>Three rings in the dust. Two the size of a jar. One the size of a copper kettle.</i>",
        "<i>Nobody carries a kettle two miles past a trail for a snare line.</i>"
       ],
       "flag": "shelf"
      }
     ],
     "sign": true
    },
    {
     "id": "sign_dug",
     "name": "Turned earth",
     "role": "the old burial ground",
     "x": 70,
     "y": 52,
     "c": "#5d6a62",
     "lines": {
      "default": [
       "<i>Four holes, none of them finished, and one that is. The finished one is the length of a forearm and there is nothing in it now.</i>",
       "<i>The spoil is stacked tidily beside each hole. Whoever did this was not in a hurry and did not expect to be interrupted.</i>"
      ]
     },
     "options": [
      {
       "t": "Go through the spoil.",
       "once": true,
       "say": [
        "<i>Bone, old, and cloth gone the colour of the mud. And under the last heap, glass: a jar with a finger of grey in it that leans toward you when you think about it.</i>",
        "<i>It is the same grey as what the winter merchants carry. Nobody on this island has any business having it.</i>"
       ],
       "give": {
        "items": {
         "vial": 1
        }
       },
       "flag": "dug",
       "stage": "north"
      }
     ],
     "sign": true
    }
   ],
   "gates": [
    {
     "x": 3,
     "y": 34,
     "to": "town",
     "at": [
      112,
      38
     ],
     "label": "back west into town"
    },
    {
     "x": 77,
     "y": 17,
     "to": "woods",
     "at": [
      80,
      20
     ],
     "label": "north along the ridge",
     "need": {
      "flags": [
       "dug"
      ]
     },
     "refuse": "There is no reason to go up there yet.",
     "text": "A deer track along the ridge, and it is the only thing on this island that has been walked recently by something with four legs."
    }
   ],
   "nodes": [
    {
     "x": 20,
     "y": 30,
     "k": "log"
    },
    {
     "x": 44,
     "y": 38,
     "k": "nettle"
    },
    {
     "x": 58,
     "y": 30,
     "k": "root"
    },
    {
     "x": 86,
     "y": 40,
     "k": "drift"
    },
    {
     "x": 68,
     "y": 56,
     "k": "bones"
    },
    {
     "x": 80,
     "y": 56,
     "k": "bones"
    }
   ]
  },
  {
   "id": "woods",
   "name": "The North Woods",
   "theme": "snow",
   "tint": "rgba(140,168,180,.13)",
   "legend": {
    "~": "water",
    "-": "shallow",
    ".": "sand",
    ",": "grass",
    "\"": "scrub",
    "T": "tree",
    "P": "pine",
    "o": "rock",
    "#": "wall",
    "_": "floor",
    "+": "door",
    "=": "plank",
    ":": "road",
    "%": "ruin",
    "t": "grave",
    "m": "mire",
    "M": "moss",
    "d": "dirt",
    "i": "ice"
   },
   "map": [
    "PPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPP",
    "PPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPP",
    "PP,P,,PPPP,,,,,PP,,P,,,,P,,PPPPP,,PP,PP,,,P,,PP,,P,\",,P,,\",PPP,P\",\"PPPPP\",P,,P,,,,P,,,P,,PP,P,PP",
    "PPPPP,,,,,,,PPP,,,P,,,,PP,PP,,PP,PP,,PPP,,PPPP,,P,,PPP,,PP,,PPP,P,\"\"P,,,P,,PP\",P,,,P\"PPPP,,,,TPP",
    "PP\",,P,P,,,P\"P,P,,,,,,,,,\",P,,,PPPTP,P,,,,PP,,,P,,,,,\",,PP,PPP,PP,,,PP,T,,,,,PPP,,P,,,\"PP,,P,,PP",
    "PPP,P,PP,,P,,,P,,,,P,,,P,,,PPP,,,PPPPP,P,,,,P,PPP,,,TP,,,,PP,,,,PP,P,,,,P,P,,P,,P,,,,,,,,,PTP,PP",
    "PPPPo,,P,,PPPP,,,,,\",\",,P,,,PPPP,P,,PPP,,P,,\"P,,,,,P,PPPPP,PPP,P,P,P,,,,,,P,,,PP\"P,,P,,,,P,P,PPP",
    "PP\",,,P,,PPP,,,P\",P,P,,P,,P,\"PPPP,,,,P,,,PP,,P,,PP,,\"\"P,,P,PP,,P,\"PPP,,,PP,,P,PP,\"P,,PP,P,,PPPPP",
    "PPPP,,,PP,,,,PPP,PPP,,,P,P,,,,,P,P\",PPP,P,,,PP,PPP,,P,P,,PP,,,,,\",,P,,,,,P,,\",o\",,,T\"PP,\"P,\",PPP",
    "PP,,P,P,,PP\",,,P,,,,,,,,P,,,P,,,,P,,,,PP,,PPP,,P,P,,P,,PPP\",,oP,,P,P,,,,PP,P,PP,T,,,P,P,,,,,,PPP",
    "PP,,,PP,,,,P,\",PPPP,,P,PPP,P,,,,P,,TP,PP,,P,,,,,P,,PP,,P,,PP,,,,PP\",,,,PPPPPP,PP,,P,PP,,,,,P,,PP",
    "PPP,P,,,,T,P,,,,,PP,,PP,P,,,,P,PoP,,PP,P,,P,,\",:::P,,P,P,P,,,,,P,P,P,,P,PP,,\",,,PPP,PP\",P,,P,PPP",
    "PPPP,,,,P,,,PP,,,,,,P,,PP,,P,,,PP,,PP,P,,P,,,PP:::,P,P,PP,,,PPPP,,PPP,,PP,TPP,,P,PP,PPP,P,,,,,PP",
    "PP,PP,P,,,,,,P,,,P,,,,,,PP,\"PPPT,\",,,PPP,PP,P,,:::P,,PPP,PP,,P,P,,P,,,P,,,P,,P,PPP,,PP,,,P,,PPPP",
    "PPPP,,,P,,P,P,,,PP,,,,,PP,,,,PP,,,,P,,,P############\",P,PP,P,PPP,,,P,TPPPP,,,,,,,,\"PPP,P,PPP,,PP",
    "PP,,,,,PP,PPP\",,P,,PT,,,P,PPPP,,P,,PP,P,#__________#P,P,,PP,,,PPP,P,\",,P,\",P,,P,P,,P,,,,,,PP,,PP",
    "PPP,,PPP\"P,\",T,,P,PP,,PPPPPP,\"PPPP,P\"P,,#__________#PP,P,PPP,,P,P,PP,,,PP,PPPP,PP,P,PP,,,P,,\",PP",
    "PPPP,,P,\",,TP\"PPPP,,,PP,,PPP,,,P,,,,PP,T#__________#\"PPPP\",,PPP,PT,PP,,,PPPP,\"PP\"PP,P,P,,P,,,,PP",
    "PP,\",,P,,,,,,PPP,,,,P,TPP,PPP,,,,,\",,,P,#__________#,P,PPP,,P,,\",P,,,P,P,,,\",P,PP,P,PP,,,PPPPPPP",
    "PP,P,,PP,P,,,TP,,,PP,,,P,,,,,,,PP,,,,\",P#__________#,P,,P,PP,PP,,P,,P,PPPP\",TPP,d,,\",,,P,P\"P,,PP",
    "PP,,P,P,PP,,PP,PP,P,TP,P,P,P,P,,PPP,PP,,#__________#P,P,P,:::::::::::::::::::::::,,,,,,PPP,PP,PP",
    "PPPP,,,,P,PP,,,PP,P,PPP,PP,P\",,,,,,,,P,P#####,,,####P,,,P,:,,P\",PPPPPPP,,P,,,,,dd,,P,PP,P,PP,PPP",
    "PP,P,P,,,,PPP,P,T,,,,,,,P,,PP,,,P,P,,\",,,,,,P,,,:::,\",P\"PP:,,,,,,P,,,,,,PP,\",PPP,PTP,PTPPPP,,,PP",
    "PP,P,P,\"P,,,P,,,P,PP\"P,PP,,,PPPPP,,PPPPP,,,P,,,,:::,,,,,,P:,P,PPPP\",,P,PPPP,PP,,,PP,PPP,,,,,,PPP",
    "PPPPP,PP,PP,,,,,,,\"P,,P,,,,PPP,PPPP,dddddddddddddddddddddd:,,PP,,,,PP,P,,,,P,,,PP,,PP,,PP,PP,PPP",
    "PPPPPP,P,P,,P\",,P,,,,,PPP,P,,,PP,,,Pdddddddddddddddddddddd:P,P,,,\"PPP,P,PPP,,,,,P,P,,,,P,P,,,,PP",
    "PPP,,\"PPP,,,PPP,P\"\"PP,PP,P,,,P,,\",,Pdddddddddddodddddddddd:PP,,\"PP,P,,,,,P,\"P,P,,PP,,P\"\",,,,\"PPP",
    "PP,P,,,,\",\",P,P,,P\"P,P,,PP,TP,,PP,,,ddddddddddddddddddddddP,,,,P,,,P,P,,,,P,,,PPPP,P,,,P,PP,,PPP",
    "PP\"PP,P,\",P,P,,,P,P,PP,,PP,,,,,,P,,Pdddddddddddddddddddddd,,,,P,,P,P\"P,PPPPP,,,,,,,PP,P,,PPP,,PP",
    "PPPPP,,,PP\"P,,,P,,,PP,,P,,PP,,P,,P,PddddddddddTdddddddddddPPPPP,,,,P\",,,,P,,P,T,,,P,P,,,,,,,P,PP",
    "PP,PP,PPP,,,,,,P,,P\",\"PP\"P,PP,,PPPP,dddddddddddddddddddddd,,\"P,,P,,,PP,\",,PP,,P,P,,,P,,P,PP,P,PP",
    "PPPPPP,P,,P,P,P,,PP,,,PPP,,P,,,PP,,,oddddTdddddddddddddddd,,,PP,P,,\",,PP,,PPP,PPPPP,,P,P,,,P,PPP",
    "PP,,P,,,PTP,,,P\",,,PPPP,P,,PP,,P,TP,dddddddddddddddddddddd,,\",,,PP,,,,PP,,,,,PPPPPPTP,,P\",,P,PPP",
    "PPPPP,P,P,,P,PP,PP,PP,,,P,,,P,PP,P,,dddddddddddddddddddddd,,,PP,,PPPP,o,P,,P\"P,,PTPP,P,T,,,,PPPP",
    "PPP,,,,PT,P,P,,,P,P,PP,P,,,,P,P\",,,,,\",,P,,,\",,:::\",P,,,,P,,,,P,,P,,,PPP,,,P,,P,P,P,,\"PP,P,,,PPP",
    "PPP,\"P,,P,,,PP\"P,PPP,,PPPPP,,P,P,,,P,P,,,,,,P,T:::P,,P,,,\",PP,Po,P,P,,\",,,,,,P,,,,,,PP,\"PPP,,,PP",
    "PP,P,,,P,,,P,P,,,,,P,P,,PPPP,PP,,\",,P,,,P,,,,PP:::PP,P,P,,,P,PP\",,,,,,P,,,,PP,,PP,,,PP,,PP,P,PPP",
    "PP\",,PPP,,P,PP,,,P,\",P,P,\"P,,,P\",,,P,,,P,P,P,,,:::,P,,,,\",,,,P\"P,P,,PP,P,,P,,,,,P,,,PP,P,,,P\"oPP",
    "PP,,,P,,P\",,P,P,P,,,,P,\"PPP,,PPPPPP,,,,P,P,P,P,:::P,,,,,,,,P,,P,P,,P,,P,,,,T\"P,\"P,,,PPPP,,PP,,PP",
    "PP,\",PP,,,PP,PP,PPP\",,PP,PPPP,,,,,,,,P,P,,,PPP,:::,,,,\",\",P,,PP,,P,PP,,P,P,,P,,P,,,,,,,,PP,PTPPP",
    "PP,P,P,P,PPPPPiiiiiiiiiiiiiiP,,P,PP,,P,,P,PP\",P:::P,,,,P,PPP,P,PP,,,P\"PPPP,PP,,,PPPPPP,PPPP,,,PP",
    "PP,P,PP,\"P,,,PiiiiiiiiiiiiiiPPP,P,,PP,P,P,,,,,P:::,PP,P,\"P,,,,P,P,PP,P,,PP,,,,,,,,,PP,P,,,,P,PPP",
    "PP,PPP,P,,\"PPPiiiiiiiiiiiiiiP\"PPT\",PP,PP,,P,\",,:::P,,P,PP,P,,P,P,P,,,,P,,PP,PP,,T,PP,,,,P,,,P,PP",
    "PPP,,,PP,P,,,,iiiiiiiiiiiiii\"PP,,,,,,P,PP,\",P,,:::,,\"\"P\"PP,P,,P,,P\",PPP,,,,,\"\",,,PP,,,P,PPP\",PPP",
    "PPPPPP,,,P,,P,iiiiiiiiiiiiiiP,,,,,,\",,,P,P,,,,:::P,PPP,,PPP,,,,,,,,,,PP,,P,P,,,,P,PP,P,,P,,PP,PP",
    "PP,P,,PPP,,,,,iiiiiiiiiiiiiiTP,\",,,,,,,,,,,,,,:::,,PPP,P,P,,PPPPP,,,\",,,,,P,PPP,PPP,,P,P,,,PP,PP",
    "PPPP\"PP,PP,,P,iiiiiiiiiiiiiiP,P,P,P,PPP,PP,,,,:::,,P,PP,P,P,,PP,P,,PP,,,P,,,,P,,,P,,,P,,PPTP,,PP",
    "PPP,,,,P,,,,P,iiiiiiiiiiiiiiP,,PP,PPP,,,PP,,P,:::PP,,,,,P,,PP,,,P,,,,,,,P,,,PPP,,PPP,P,,P,\"P,,PP",
    "PP,,,,P\"\"PP,PPiiiiiiiiiiiiii\",,,,,P,P,,\",,,,P,:::,P,oP,,P\"P,P,PPP,,,P,,PP,P,P,,,,PP\"P,,P,,,PPPPP",
    "PPPPP,,,P,,PP,,P,,,,PP,P\",,,,,PP,,,PP,P,,,PP,,:::P,P,P,,,PP,,,,o,P,,,,,PP,,,\",,,,PPP,P,PPPP,P,PP",
    "PP,,P\",P,\",P,,PPP,P,,P,,,,P,,PP,PP,,,PP,,,,PPP:::,,P,P,,,,,,,,,P,PP,,,,,,P,,,\"P,,PPP,,,,,P,,P\"PP",
    "PPP,,,,,,P,\"P,,,,PPPP,,,,PP,,PP,,P,P,,,,PPPP,P:::,,P\"PP,,,P,,,,,P,P,,P,P,,,,PP,,,P,PPPPP,PP,P,PP",
    "PP,P,,,,,,P,PPP,,,,,PP,PT,\",P,PP,,,,P,PP,P,PPP:::,,,PPo,,,,P,,,P,PP,P,,,,P,,\",,P,,P,,\",,,,,PPPPP",
    "PP,P,P,,P,PP,P,P,,,P,,PP,,P,,,P,,P,PP,PP,PP,PP:::PPP\",P,P,PP,P,PP,PP,,,,,,,\",,,PPP,P,P,,\",,,,\"PP",
    "PP,,,P,,P,,P,PP,,,P,,,,,\",,P,P\",PP,PP,PP,,,,,,:::,,\"P,P,,,PP,P\"TP,,P,P,P,\"PPP,P,P,PPP,PP,PPP,,PP",
    "PPP,P,,,,,PPP,P,PPPP,,PP,,,P,P,,P,P,,,,,,P,,,:::,PPPP,P,,,,,,,P\",\"PPP,P,,,P,P,,TP,,,PP,P,PPPP,PP",
    "PP,,,,\",P,,,P,,,,,PT,,P,PP,PP,,\",,PP,,,,\"PP,,:::P,,P,P,,,P,P,\",,P,,P,,,P,,,P,P,,,,,,PPP,,,,P,,PP",
    "PP,,,,\"P\"P,,,PP,P,P,P,,,PP,,,PPTP,,PP,P,PPPPP:::,,,,PPP,P,,,\"P,o,,PP,\"P,P,,PPPP\",,,,,PP,,,P,,PPP",
    "PP,PPP,PP,,,,,,P,PP,PP,,,PP,P,,,,,,P,,,,,TP,T:::PP,,\"PPP\"P,\"PP,,,,,PP,,,,PP,,,,,P,,P,,PP\",,PoPPP",
    "PP,\",P,P,,P,PPP,,P,,P,,,PP,,P,,PP,P,PP\",P,,PP:::P,P,PP\",PP,,,,,,P,P,,P,,PPTPPP,,\",,P,P,,o,,P,,PP",
    "PPP,P,,PP,,,P,T,,,PP,,,P,P,,PPPP,,P,PP,,,PP,,:::\"P,P,PP,,,P,,,,,,P,P,,PP,\"PP,,P,,P,,,P,,P,PPP,PP",
    "PPPP\"PP,,P,,,,PP,PPPP\"\"PP,,,\"P,,,,,,,,,,,,P,,:::,,PP,,,P,,,PP,P,PP,,PP,PP,,,P,,P,,,,,,PPP,P,P,PP",
    "PP,,P,PP,,P,PP\"PP,,P,,P,P,PP,,P,,\",,,PP,,P,,,:::,PP,PPPPP,P,,,P,,,,,,P,,P,P,,,,P,P,,P,,PPPPPPPPP",
    "PP,,P,PP,,,PPP,,,,o,,,P,,P,,,,,,,PP\"P,,,P\"P,P,,,,,PPP,,P,,,PP,P,PP,PP,P,,P,,PPP,PPP,,PP,,,,P,PPP",
    "PPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPP",
    "PPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPP"
   ],
   "spawn": [
    46,
    62
   ],
   "labels": [
    {
     "x": 46,
     "y": 17,
     "t": "HENDRICKS CABIN"
    }
   ],
   "npcs": [
    {
     "id": "alisa",
     "name": "Alisa Hendricks",
     "role": "winter merchant",
     "x": 46,
     "y": 23,
     "c": "#c8ccd2",
     "lines": {
      "default": [
       "Stop there. The dogs will decide about you before I do."
      ],
      "north": [
       "Stop there. Togo has already decided and he is usually right.",
       "Six days and you're the first one up the trail. The rest of them sent a boy with a note.",
       "It was my knife. It has been my knife for eleven years and it was in the sled box on the second day, and on the seventh it was behind the boat sheds in the snow."
      ],
      "hall": [
       "They're opening the hall? Then I'll come down and stand in it. I'd rather be looked at than talked about."
      ],
      "done": [
       "-"
      ],
      "flag:wrong": [
       "-"
      ]
     },
     "options": [
      {
       "t": "Where were you the night he died?",
       "onStage": "north",
       "once": true,
       "say": [
        "Here. Twelve miles of nothing between me and the sheds, and four dogs who bark at weather.",
        "Ask what else was here. Ask why my father's name gets said in a room I'm not in.",
        "<i>She does not raise her voice for any of it, which is somehow worse than if she had.</i>"
       ],
       "flag": "alisa"
      },
      {
       "t": "Somebody out east has a jar of grey.",
       "needFlag": "dug",
       "once": true,
       "say": [
        "Then somebody out east has been buying off a winter merchant, and it wasn't me.",
        "We carry them sealed and we count them at both ends. One goes missing, it went missing on purpose.",
        "Kell rode out with our sled twice in October. He asked twice about the count."
       ],
       "flag": "count"
      }
     ],
     "shop": {
      "currency": "silver",
      "title": "the sled box",
      "line": "She counts everything out loud, once, and does not count it again.",
      "sells": [
       {
        "id": "vial",
        "n": 1,
        "price": 120
       },
       {
        "id": "hide",
        "n": 3,
        "price": 14
       },
       {
        "id": "meat",
        "n": 4,
        "price": 12
       }
      ],
      "buys": [
       {
        "id": "glass",
        "n": 2,
        "price": 30
       },
       {
        "id": "hide",
        "n": 2,
        "price": 9
       }
      ]
     }
    },
    {
     "id": "sign_cabin",
     "name": "The Hendricks cabin",
     "role": "dog yard",
     "x": 46,
     "y": 22,
     "c": "#8b8578",
     "lines": {
      "default": [
       "<i>Four dogs, a sled turned on its side against the wall, and a box with a brass count-plate screwed to the lid.</i>",
       "<i>The plate has numbers stamped down it in pairs. The last pair has been scratched at, not cut out. Scratched at, as if by somebody who could not read them.</i>"
      ]
     },
     "sign": true
    }
   ],
   "animals": [
    {
     "x": 42,
     "y": 27,
     "k": "wolf",
     "name": "Togo",
     "tame": 1
    },
    {
     "x": 52,
     "y": 27,
     "k": "wolf",
     "name": "Kettle",
     "tame": 1
    },
    {
     "x": 39,
     "y": 30,
     "k": "wolf",
     "name": "Moss",
     "tame": 1
    },
    {
     "x": 50,
     "y": 31,
     "k": "wolf",
     "name": "the small one",
     "tame": 1
    }
   ],
   "gates": [
    {
     "x": 46,
     "y": 62,
     "to": "town",
     "at": [
      58,
      6
     ],
     "label": "south, back down to the town"
    },
    {
     "x": 80,
     "y": 20,
     "to": "trail",
     "at": [
      77,
      17
     ],
     "label": "east along the ridge"
    }
   ],
   "nodes": [
    {
     "x": 30,
     "y": 36,
     "k": "log"
    },
    {
     "x": 62,
     "y": 40,
     "k": "log"
    },
    {
     "x": 22,
     "y": 52,
     "k": "root"
    },
    {
     "x": 70,
     "y": 30,
     "k": "stones"
    }
   ]
  }
 ]
});
/* ============================================================ AUDIO */
let AC=null;
function sfx(f,dur,type,vol){
  try{
    AC=AC||new (window.AudioContext||window.webkitAudioContext)();
    if(AC.state==="suspended") AC.resume();
    const o=AC.createOscillator(), g=AC.createGain();
    o.type=type||"square"; o.frequency.value=f;
    g.gain.value=vol||.08; o.connect(g); g.connect(AC.destination);
    o.frequency.exponentialRampToValueAtTime(Math.max(40,f*.5),AC.currentTime+dur);
    g.gain.exponentialRampToValueAtTime(.0008,AC.currentTime+dur);
    o.start(); o.stop(AC.currentTime+dur);
  }catch(e){}
}

/* ============================================================ COMBAT */
function melee(){
  if(wpnRanged()){ playerShot(); return; }
  if(G.mode!=="play"||isPaused()||player.cd>0||player.roll>0) return;
  /* a swing costs breath now. Empty lungs still swing, but badly. */
  const nk=hasPas("nanook"), white=G.nanookT>0;
  const winded=G.vg<4;
  spendVg(Math.min(G.vg,4));
  player.cd=(winded?wpnSt().cd*1.5:wpnSt().cd)*(white?.6:1); player.swing=.18;
  if(window.ART&&window.ART.hit) window.ART.hit(player);   // plays the attack clip if one was drawn
  sfx(160,.06,"square",.11);
  let reach=wpnSt().reach, dmg=wpnSt().dmg+(G.trade&&G.trade.b==="arm"?4:0);
  if(nk){                                      // Nanook: the long arm
    reach+=10+3*nk.rank; dmg+=3+2*nk.rank;
    if(G.hp<G.mhp*(nk.rank>=3?.45:.35)) dmg=Math.round(dmg*1.3);
  }
  if(white){ reach+=30; dmg=Math.round(dmg*1.5); }
  if(winded) dmg=Math.round(dmg*.5);
  const tg5=hasPas("togo");
  if(tg5&&tg5.rank>=5) dmg=Math.round(dmg*(1+.25*petWolves().length));
  const zealed=G.zeal>0;                       // the Sermon doubles the blow that lands, not the one that misses
  if(zealed) dmg*=2;
  const mi=hasPas("missionary");
  let hit=false;
  for(const f of foes){
    const d=dist(player.x,player.y,f.x,f.y);
    if(d>reach+f.r) continue;
    let a=Math.atan2(f.y-player.y,f.x-player.x)-player.aim;
    a=Math.atan2(Math.sin(a),Math.cos(a));
    if(Math.abs(a)<1.15){
      let out=dmg*dmgMul(f);
      if(zealed&&mi&&mi.rank>=3) out+=(f.armor||0);   // the doubled strike is not argued with
      hurtFoe(f,out,5);
      pasEmber(f); hit=true;
    }
  }
  for(const a of animals){
    if(a.pet) continue;
    const d=dist(player.x,player.y,a.x,a.y);
    if(d>reach+a.r) continue;
    let ang=Math.atan2(a.y-player.y,a.x-player.x)-player.aim;
    ang=Math.atan2(Math.sin(ang),Math.cos(ang));
    if(Math.abs(ang)<1.15){ hurtAnimal(a,dmg); hit=true; }
  }
  if(hit){ G.hitstop=.045; if(zealed) G.zeal--; }
}
function dodge(){
  if(G.mode!=="play"||isPaused()||player.roll>0||player.dashcd>0) return;
  if(G.nanookT>0){ toast("no dashing","White Hunger wants you standing your ground."); return; }
  const cost=Math.max(3,Math.round(12*pasDashCost()*(player.dashCost||1)));
  if(G.vg<cost){ toast("no vigor for it",`A dash costs ${cost}.`); return; }
  spendVg(cost); player.roll=.3; player.inv=.36; player.dashcd=.42;
  // dash the way you are moving if you are moving, otherwise the way you are looking
  let mx=(K.KeyD||K.ArrowRight?1:0)-(K.KeyA||K.ArrowLeft?1:0);
  let my=(K.KeyS||K.ArrowDown?1:0)-(K.KeyW||K.ArrowUp?1:0);
  if(!mx&&!my&&(TC.mx||TC.my)){ mx=TC.mx; my=TC.my; }
  player.rollDir=(mx||my)?Math.atan2(my,mx):player.aim;
  for(let i=0;i<8;i++) parts.push({x:player.x,y:player.y,vx:-Math.cos(player.rollDir)*90*rnd(),
    vy:-Math.sin(player.rollDir)*90*rnd(),life:.35,c:"#cfc7ae",s:2});
  sfx(210,.1,"sine",.06);
}
function hurtFoe(f,dmg,kb){
  const tr=hasPas("trident");
  const pierce=(tr&&tr.rank>=3&&f.empt)?1:0;      // an emptied thing does not resist
  const d=Math.max(1,dmg-(pierce?0:(f.armor||0)));
  f.hp-=d; f.hit=.14; f.asleep=false;
  const a=Math.atan2(f.y-player.y,f.x-player.x);
  if(kb&&!f.big){ const nx=f.x+Math.cos(a)*kb, ny=f.y+Math.sin(a)*kb;
    if(free(nx,ny,f.r)){ f.x=nx; f.y=ny; } }
  for(let i=0;i<5;i++) parts.push({x:f.x,y:f.y,vx:(rnd()-.5)*170,vy:(rnd()-.5)*170,
    life:.28+rnd()*.3,c:f.k==="drifter"?"#c8d2d6":"#5c3a2e",s:1.5+rnd()*2});
  if(f.hp<=0) killFoe(f);
}
function killFoe(f){
  if(f.dead) return;                             // a thing pays out once
  f.dead=1;
  foes=foes.filter(o=>o!==f);
  G.kills++;
  corpses.push({x:f.x,y:f.y,r:f.r,k:f.k,t:0,max:f.k==="boss"?1.7:f.k==="mini"?1.2:.7,c:f.c});
  gainMist(f.mist*((f.pupMark||f.pup>0)&&f.pupTw?2:1),f.k);
  /* Raven Mocker: now and then the years of the dying settle into you */
  const rm=hasPas("ravenmocker");
  if(rm){
    const ch=(.025+.012*rm.rank)*((f.k==="mini"||f.k==="boss")?5:1);
    if(rnd()<ch){
      G.mhp+=1; G.hp=Math.min(G.mhp,G.hp+1);
      toast("eaten years","Something of it stays with you. +1 body, for good.");
      logit("eaten years","+1 body, off a "+f.n+".");
    }
  }
  if(f.drop) for(const k in f.drop){
    const p=f.drop[k];
    const n=p>=1?Math.floor(p)+(rnd()<p-Math.floor(p)?1:0):(rnd()<p?1:0);
    if(n>0) give(k,n);
  }
  if(f.k==="boss"&&rnd()<.4) dropBroken(BRK_HIGH,f.n);
  else if(f.k==="mini"&&rnd()<.25) dropBroken(rnd()<.25?BRK_HIGH:BRK_IRON,f.n);
  if(f.k!=="boss"&&f.k!=="mini"){
    // small chance of something worth keeping
    if(rnd()<.09){ const t=pick(["glass","bandage","herb","cloth","ore","tonic","berry"]);
      giveTell(t,1); toast("something on it",ITEMS[t].n+"."); }
    if(rnd()<.015) grantPlan(2,"a "+f.n);
  }
  if(f.k==="mini"){
    grantPlan(2,f.n);
  }
  /* the things that keep islands carry pilots' charts — minis and guardians both */
  if((f.k==="mini"||f.k==="boss")&&G.isl>=2&&unknownPort()
     &&have("port")<1&&cacheHave("port","hold")<1&&cacheHave("port","home")<1){
    const got=give("port",1);
    if(got) toast("folded in its coat","A pilot's chart. Somebody carried this before it did — use it and you will know where the place is.");
    else { readPort();                          // no room to carry it: read it where you stand
      toast("no room to carry it","A pilot's chart, read on the spot. The place is on your chart now."); }
  }
  /* the scrap first, so a full pack cannot eat it and its line goes in the
     log where an interlude cannot bury it */
  if(f.k==="boss"&&ISL.threat>=2){
    const want=ISL.threat>=3?2:1;
    giveKeep("frag",want);
    const fc=fragCount();
    const where=have("frag")>=want?"":" Stowed — there was no room on you for it.";
    toast("a corner of a chart",(fc>=FRAGS_FOR_ELITE
      ? "Four scraps. They agree on a place, and the place is not on any chart."
      : `${fc} of ${FRAGS_FOR_ELITE}. The coastline on it is not one you have walked.`)+where);
    logit("a corner of a chart",
      `Off ${f.n} on ${ISL.name}. ${fc} of ${FRAGS_FOR_ELITE} scraps. ${
        fc>=FRAGS_FOR_ELITE?"They agree on a place. A boat and five coasts behind you and the chart will show it."
                           :"The coastline on it is not one you have walked."}`);
  }
  if(f.k==="boss"){
    G.bossKills++;
    grantPlan(3,f.n);
    G.shake=14;
    logit("put down",f.n+" — on "+ISL.name+".");
    interlude("It stops",f.n+" comes apart",
      "The shape lets go of whatever it was holding itself in, and the island gets quieter by one thing. "+
      "There is mist all over the ground where it stood, and it goes into you without being asked.",
      ()=>{ if(rnd()<.5) grantRegard("what guarded "+ISL.name); else { gainMist(70,"boss"); toast("mist","Heavy with it. No Regard in it, though."); } });
  } else if(f.k==="mini"){
    G.shake=9; logit("put down",f.n+".");
    toast("down",f.n+" is down.");
  }
  /* the thing falls, and then the island goes quiet — in that order */
  if((f.k==="mini"||f.k==="boss")&&ISL.bigs) checkCleared();
  sfx(f.k==="hollow"?110:130,.16,"triangle",.09);
  syncHUD();
}
function checkCleared(){
  if(!ISL.bigs||!ISL.bigs.length||ISL.cleared) return;
  for(const f of ISL.bigs) if(foes.indexOf(f)>=0) return;
  ISL.cleared=true;
  const t=ISL.threat;
  if(t>=3){
    G.broken++;
    grantPlan(4,"a Broken island");
    grantPlan(4,"a Broken island");
    gainMist(900,"the Broken",true);
    giveTell("frag",1);
    interlude("It is over","Nothing on "+ISL.name+" is standing",
      "The last of it comes apart and the fog goes with it — not thinning, exactly. Lifting off, the way a hand lifts "+
      "off a held-down thing.<br><br>What is underneath is only an island. Rock, a beach, some trees that were always "+
      "trees. You have been walking through the wrong sea for a long time and this is the first ground that has not "+
      "been trying to keep you.<br><br><i>From here the water goes somewhere. The chart at the shore will show you.</i>",
      ()=>syncHUD());
    logit("broken",`${ISL.name} — emptied. The sea route is open from here.`);
    return;
  }
  if(t===2){
    grantPlan(4,"an emptied Forsaken island");
    gainMist(260,"clearing");
    interlude("Nothing left standing",ISL.name+" is empty",
      "Everything on this island that was keeping it is on the ground. The quiet afterwards is not a relief — it is the "+
      "specific silence of a place that has been holding something in for a long time and has just let go of it.<br><br>"+
      "You know how to make something you did not know how to make an hour ago.",
      ()=>syncHUD());
  } else if(t===1){
    grantPlan(3,"an emptied Haunted island");
    gainMist(120,"clearing");
    toast("nothing left standing",ISL.name+" has nothing keeping it any more.");
  } else {
    gainMist(50,"clearing");
    toast("nothing left standing","A quiet island, and now quieter.");
  }
  logit("cleared",ISL.name+" — everything that kept it is down.");
}
function hurtPlayer(d,src){
  if(player.inv>0||G.mode!=="play") return;
  d*=player.takeMul||1;
  const mi=hasPas("missionary");
  if(mi&&src&&src.r) d*=1.5;                   // a blow from a body lands harder on faith
  d=pasYurei(d,src);                           // the shadow takes its share first
  d=Math.max(1,d-(player.armorOff?0:(player.armor||0)));
  G.hp-=d; player.inv=.55; G.shake=7; G.hurtT=.35; sfx(120,.12,"square",.09);
  pasToltecHit(d);                             // the wound fills the lungs
  if(G.hp<=0){
    G.hp=0; G.mode="pause";
    if(G.wrecks>=2) showEnding("drowned");
    else wreckAgain();
  }
  syncHUD();
}
function hurtAlly(m,d){
  if(m.down) return;
  m.hp-=d; m.hurt=.22;
  for(let i=0;i<4;i++) parts.push({x:m.x,y:m.y,vx:(rnd()-.5)*130,vy:(rnd()-.5)*130,life:.3,c:"#9c3f22",s:2});
  if(m.hp<=0){
    m.hp=0; m.down=1; m.bleed=34;
    if(m.joined){ toast(m.n.split(" ")[0]+" is down","Bandage, or a surgeon's hands, and be quick."); }
  }
  syncHUD();
}
function killAlly(m){
  party=party.filter(o=>o!==m);
  neutrals=neutrals.filter(o=>o!==m);
  G.deaths.push(m.n+", "+ISL.name);
  logit("dead",`${m.n} — ${m.role} — on ${ISL.name}.`);
  toast("dead",m.n+". There was not time.");
  corpses.push({x:m.x,y:m.y,r:m.r,k:"person",t:0,max:1.1,c:m.c});
  syncHUD();
}

/* ============================================================ PROJECTILES */
function fireShot(from,tx,ty,rng){
  const a=Math.atan2(ty-from.y,tx-from.x)+(rnd()-.5)*.10;
  shots.push({x:from.x+Math.cos(a)*(from.r+6),y:from.y+Math.sin(a)*(from.r+6),
    vx:Math.cos(a)*rng.sp,vy:Math.sin(a)*rng.sp,dmg:rng.dmg,r:6,life:2.6,
    c:rng.c,s:rng.s,drain:rng.drain,chill:rng.chill,foe:1});
  sfx(rng.s==="mist"?420:260,.12,rng.s==="mist"?"sine":"square",.06);
}
function updShots(dt){
  for(let i=shots.length-1;i>=0;i--){
    const s=shots[i];
    s.life-=dt; s.x+=s.vx*dt; s.y+=s.vy*dt;
    if(s.life<=0||solidAt(s.x,s.y)){
      for(let k=0;k<5;k++) parts.push({x:s.x,y:s.y,vx:(rnd()-.5)*120,vy:(rnd()-.5)*120,
        life:.3,c:s.c,s:2});
      shots.splice(i,1); continue;
    }
    if(s.foe){
      if(dist(s.x,s.y,player.x,player.y)<player.r+s.r){
        if(hasPas("missionary")){              // nothing thrown can touch you
          for(let k=0;k<7;k++) parts.push({x:s.x,y:s.y,vx:(rnd()-.5)*150,vy:(rnd()-.5)*150,life:.35,c:"#e4ddc8",s:2});
          sfx(600,.08,"triangle",.05);
          shots.splice(i,1); continue;
        }
        hurtPlayer(s.dmg);
        if(s.drain) G.vg=Math.max(0,G.vg-10);
        if(s.chill) player.chill=1.4;
        shots.splice(i,1); continue;
      }
      let hit=false;
      for(const m of party) if(!m.down&&dist(s.x,s.y,m.x,m.y)<m.r+s.r){ hurtAlly(m,s.dmg*.8); hit=true; break; }
      if(!hit) for(const c of neutrals) if(!c.down&&dist(s.x,s.y,c.x,c.y)<c.r+s.r){ hurtAlly(c,s.dmg*.8); hit=true; break; }
      if(hit){ shots.splice(i,1); continue; }
    } else if(s.mine){                         // something of yours, going the other way
      let spent=false;
      for(const f of foes){
        if(s.hitset&&s.hitset.indexOf(f)>=0) continue;
        if(dist(s.x,s.y,f.x,f.y)<f.r+s.r){
          hurtFoe(f,s.dmg*dmgMul(f),12);
          for(let k=0;k<6;k++) parts.push({x:f.x,y:f.y,vx:(rnd()-.5)*160,vy:(rnd()-.5)*160,life:.35,c:s.c,s:2});
          if(f.hp<=0&&s.tol&&s.tol.heal&&s.tol.owner==="player")
            G.hp=Math.min(G.mhp,G.hp+s.tol.cut);          // the blood comes back
          if(s.pierce){ if(s.hitset) s.hitset.push(f); }
          else { spent=true; break; }
        }
      }
      if(!spent) for(const a of animals){
        if(a.pet) continue;
        if(dist(s.x,s.y,a.x,a.y)<a.r+s.r){ hurtAnimal(a,s.dmg); spent=true; break; }
      }
      if(spent){ shots.splice(i,1); continue; }
    }
  }
}

/* ============================================================ AI HELPERS */
function moveEnt(e,ax,ay,sp,dt){
  const nx=e.x+ax*sp*dt, ny=e.y+ay*sp*dt;
  if(free(nx,e.y,e.r)) e.x=nx; else e.y+=ay*sp*dt*.6;
  if(free(e.x,ny,e.r)) e.y=ny; else e.x+=ax*sp*dt*.6;
}
function seekTo(e,tx,ty,sp,dt,stop){
  if(e.p2) return 0;                       // somebody real is steering that one
  const d=dist(e.x,e.y,tx,ty); if(d<(stop||6)) return d;
  const a=Math.atan2(ty-e.y,tx-e.x);
  e.face=a; moveEnt(e,Math.cos(a),Math.sin(a),sp,dt);
  return d;
}
function wanderEnt(e,dt,rad,sp){
  e.wt=(e.wt||0)-dt;
  if(e.wt<=0||dist(e.x,e.y,e.tx||e.x,e.ty||e.y)<10){
    e.wt=1.4+rnd()*2.6;
    const a=rnd()*6.28, d=20+rnd()*rad;
    e.tx=clamp(e.gx+Math.cos(a)*d,TILE*2,TILE*(GW-2));
    e.ty=clamp(e.gy+Math.sin(a)*d,TILE*2,TILE*(GH-2));
  }
  seekTo(e,e.tx,e.ty,sp,dt,8);
}
function spawnEdge(){
  for(let i=0;i<70;i++){
    const a=rnd()*6.28, d=460+rnd()*300;
    const x=player.x+Math.cos(a)*d, y=player.y+Math.sin(a)*d;
    const tx=Math.floor(x/TILE), ty=Math.floor(y/TILE);
    if(!isReach(tx,ty)) continue;
    if(free(x,y,12)) return {x,y};
  }
  return null;
}

/* ============================================================ UPDATE */
function update(dt){
  if(NET.role==="guest"){ netGuestFrame(dt); return; }
  const halted=isPaused();
  $("paused").classList[halted&&G.mode==="play"?"add":"remove"]("on");
  if((G.mode==="play"||(NET.on&&G.mode==="dialog"))&&!halted){
    /* --- player --- */
    let mx=(K.KeyD||K.ArrowRight?1:0)-(K.KeyA||K.ArrowLeft?1:0);
    let my=(K.KeyS||K.ArrowDown?1:0)-(K.KeyW||K.ArrowUp?1:0);
    if(!mx&&!my&&(TC.mx||TC.my)){ mx=TC.mx; my=TC.my; }
    let sp=player.sp;
    if(slowAt(player.x,player.y)&&!(G.trade&&G.trade.b==="shoal")) sp*=.62;
    sp*=pasSpeed()*player.spdMul;
    if(player.tolT>0) sp*=1.22;                       // blood-debt speed
    if(G.nanookT>0&&hasPas("nanook")&&hasPas("nanook").rank>=5) sp*=1.15;
    if(player.roll>0){ mx=Math.cos(player.rollDir); my=Math.sin(player.rollDir); sp=370;
      if(rnd()<dt*40) parts.push({x:player.x,y:player.y,vx:0,vy:0,life:.25,c:"#a9b4b0",s:2}); }
    else if(mx||my){ const m=Math.hypot(mx,my); mx/=m; my/=m; }
    if((mx||my)&&G.mode==="play") moveEnt(player,mx,my,sp,dt);
    player.cd=Math.max(0,player.cd-dt); player.swing=Math.max(0,player.swing-dt);
    player.roll=Math.max(0,player.roll-dt); player.inv=Math.max(0,player.inv-dt);
    player.dashcd=Math.max(0,player.dashcd-dt);
    G.vg=Math.min(G.mvg,G.vg+7*dt);
    const rg=pasRegen();
    if(rg>0){
      G.hp=Math.min(G.mhp,G.hp+rg*dt);
      for(const m of party) if(!m.down&&dist(m.x,m.y,player.x,player.y)<170) m.hp=Math.min(m.mhp,m.hp+rg*dt);
    }
    G.shake=Math.max(0,G.shake-dt*22); G.hurtT=Math.max(0,G.hurtT-dt);
    /* anything a full pack could not take comes to you as soon as it can */
    if(G.owed){
      G.owedT=(G.owedT||0)-dt;
      if(G.owedT<=0){ G.owedT=1.5; payOwed(); }
    }
    G.coal=Math.max(0,(G.coal||0)-dt);
    G.nanookT=Math.max(0,(G.nanookT||0)-dt);
    player.tolT=Math.max(0,(player.tolT||0)-dt);
    nearest=findNearest();
    prompt=nearest?nearest.lab:"";
    netHostTick(dt);
    saveTick(dt);
    // banked coal burns what comes close
    if(G.coal>0){
      const kg=hasPas("kettle"), rr=(kg&&kg.rank>=2)?96:78;
      for(const f of foes) if(dist(f.x,f.y,player.x,player.y)<rr) f.burn=Math.max(f.burn,2.2);
      if(rnd()<dt*22) parts.push({x:player.x+(rnd()*20-10),y:player.y+(rnd()*20-10),
        vx:0,vy:-40,life:.5,c:"#e0a141",s:2});
    }
    G.clock+=dt;
    if(ISL.home) homeTick(dt,false,true);
    updAnimals(dt);
    /* --- foes and what they throw --- */
    for(const f of foes.slice()) updFoe(f,dt);
    updShots(dt);
    player.chill=Math.max(0,(player.chill||0)-dt);
    /* --- allies --- */
    for(const m of party.slice()) updAlly(m,dt);
    /* --- neutrals & townies --- */
    for(const c of neutrals.slice()){
      if(c.down){ c.bleed-=dt; if(c.bleed<=0) killAlly(c); continue; }
      if(dist(c.x,c.y,player.x,player.y)<300) c.seen=1;
      let tgt=null,bd=170;
      for(const f of foes){ const d=dist(f.x,f.y,c.x,c.y); if(d<bd){ bd=d; tgt=f; } }
      if(tgt){ c.cd=Math.max(0,c.cd-dt);
        if(bd>tgt.r+c.r+10) seekTo(c,tgt.x,tgt.y,c.sp*.7,dt,10);
        else if(c.cd<=0){ c.cd=1.1; hurtFoe(tgt,c.dmg*.7,3); c.swing=.16; } }
      else wanderEnt(c,dt,60,32);
    }
    for(const t of townies){
      if(t.cnpc&&t.cnpc.sign) continue;               // a signpost does not go anywhere
      t.ph+=dt*.7;
      const pd=dist(t.x,t.y,player.x,player.y);
      if(pd<80) t.face=Math.atan2(player.y-t.y,player.x-t.x);
      else if(t.cnpc) { if(dist(t.x,t.y,t.gx,t.gy)>4) seekTo(t,t.gx,t.gy,20,dt,3); }
      else wanderEnt(t,dt,50,26); }
    /* --- spirit / boss seen flags --- */
    if(ISL.spirit&&dist(ISL.spirit.x,ISL.spirit.y,player.x,player.y)<300) ISL.spirit.seen=1;
    if(ISL.mini&&dist(ISL.mini.x,ISL.mini.y,player.x,player.y)<340) ISL.mini.seen=1;
    if(ISL.boss&&dist(ISL.boss.x,ISL.boss.y,player.x,player.y)<380) ISL.boss.seen=1;
    /* --- ambient spawns --- */
    const cap=[4,8,12,13][ISL.threat]+Math.floor(G.isl*.5);
    spawnT-=dt;
    if(ISL.home||ISL.city) spawnT=1e9;    // nothing grows back at home, and nothing lives in a city
    if(spawnT<=0){
      spawnT=([9.5,5.4,3.0,2.1][ISL.threat]+rnd()*3)/(G.tools.lantern?2:1);
      const live=foes.filter(f=>f.k!=="boss"&&f.k!=="mini").length;
      if(live<cap){ const p=spawnEdge(); if(p) mkFoe(pick(pool()),p.x,p.y); }
    }
    dawnPulse+=dt;
  }
  /* --- particles, zones, corpses (always tick so the world settles) --- */
  for(let i=parts.length-1;i>=0;i--){
    const p=parts[i]; p.life-=dt;
    if(p.life<=0){ parts.splice(i,1); continue; }
    p.x+=(p.vx||0)*dt; p.y+=(p.vy||0)*dt; if(p.vy!==undefined) p.vy+=90*dt;
  }
  for(let i=zones.length-1;i>=0;i--){
    const z=zones[i]; z.life-=dt;
    if(z.k==="fire"&&G.mode==="play"){
      for(const f of foes) if(dist(f.x,f.y,z.x,z.y)<z.r) f.burn=Math.max(f.burn,1.6);
    }
    if(z.life<=0) zones.splice(i,1);
  }
  for(let i=corpses.length-1;i>=0;i--){ corpses[i].t+=dt; if(corpses[i].t>corpses[i].max) corpses.splice(i,1); }
  // camera
  const tx=clamp(player.x-VW/2,0,Math.max(0,GW*TILE-VW)), ty=clamp(player.y-VH/2,0,Math.max(0,GH*TILE-VH));
  cam.fx=(cam.fx===undefined?tx:cam.fx)+(tx-(cam.fx===undefined?tx:cam.fx))*Math.min(1,dt*7);
  cam.fy=(cam.fy===undefined?ty:cam.fy)+(ty-(cam.fy===undefined?ty:cam.fy))*Math.min(1,dt*7);
  cam.x=Math.round(cam.fx); cam.y=Math.round(cam.fy);   // whole pixels: tiles and things on them agree
  if(toastT>0){ toastT-=dt; if(toastT<=0) $("toast").style.opacity=0; }
  $("prompt").style.opacity=(prompt&&G.mode==="play")?1:0;
  if(prompt) $("prompt").textContent="E — "+prompt;
  syncBars();
  hudT-=dt;
  if(hudT<=0){ hudT=.2; if(G.mode==="play") syncHUD(); }
}

function updFoe(f,dt){
  f.t+=dt; f.hit=Math.max(0,f.hit-dt);
  f.stun=Math.max(0,f.stun-dt); f.froze=Math.max(0,f.froze-dt);
  f.slow=Math.max(0,(f.slow||0)-dt); f.cd=Math.max(0,f.cd-dt);
  f.lit=Math.max(0,(f.lit||0)-dt);
  if(f.burn>0){ f.burn-=dt; f.hp-=11*dt; if(rnd()<dt*14) parts.push({x:f.x,y:f.y,vx:0,vy:-50,life:.4,c:"#e0a141",s:2});
    if(f.hp<=0){ killFoe(f); return; } }
  if(f.empt&&f.stun<=0&&!(f.slow>0)) f.empt=0;
  /* Raven Mocker's strings: dragging for a while, then pulled tight */
  if(f.pup>0){
    f.pup-=dt;
    f.slow=Math.max(f.slow||0,.3);
    if(rnd()<dt*8) parts.push({x:f.x,y:f.y-f.r-4,vx:0,vy:-24,life:.3,c:"#4a3a52",s:1.5});
    if(f.pup<=0){
      f.stun=f.pupHold||1;
      const dm=f.pupDmg||16;
      f.pupMark=f.pupTw?1:0; f.pupDmg=0;
      zones.push({k:"burst",x:f.x,y:f.y,life:.35,max:.35,c:"#4a3a52"});
      hurtFoe(f,dm,0);
      if(foes.indexOf(f)<0) return;            // the strings finished it
    }
  }
  const pd=dist(f.x,f.y,player.x,player.y);
  if(f.asleep){
    if(pd<250){ f.asleep=false; G.shake=8;
      toast(f.k==="boss"?"it has your scent":"something stands up",f.n+".");
      logit(f.k==="boss"?"awake":"awake",f.n+" on "+ISL.name+".");
      sfx(60,.6,"sine",.13); }
    return;
  }
  if(f.froze>0||f.stun>0) return;
  // pick a target: nearest of player and standing allies
  let tx=player.x,ty=player.y,tobj=null,bd=pd;
  for(const m of party) if(!m.down){ const d=dist(f.x,f.y,m.x,m.y); if(d<bd-30){ bd=d; tx=m.x; ty=m.y; tobj=m; } }
  for(const c of neutrals) if(!c.down){ const d=dist(f.x,f.y,c.x,c.y); if(d<bd-60){ bd=d; tx=c.x; ty=c.y; tobj=c; } }
  let sp=f.sp*(f.slow>0?.45:1)*pasSawSlow(f);
  let a=Math.atan2(ty-f.y,tx-f.x);
  if(f.k==="drifter") a+=Math.sin(f.t*3)*.6;
  // throwing / spitting
  const rng=FOES[f.k]&&FOES[f.k].rng;
  if(rng){
    f.rcd=(f.rcd===undefined?rng.cd*rnd():f.rcd)-dt;
    const clear=losClear(f.x,f.y,tx,ty);
    if(bd<rng.d&&bd>70&&clear&&f.rcd<=0){
      f.rcd=rng.cd*(.8+rnd()*.5); f.aim=1.0; fireShot(f,tx,ty,rng);
    }
    f.aim=Math.max(0,(f.aim||0)-dt);
    if(rng.hold&&bd<rng.hold&&clear) a+=Math.PI;         // back off and keep throwing
    else if(f.aim>0) sp*=.35;                            // brief pause on the throw
  }
  if(f.blink){
    f.bcd=(f.bcd===undefined?2+rnd()*3:f.bcd)-dt;
    if(f.bcd<=0&&bd>140&&bd<420){
      f.bcd=4+rnd()*3;
      const a2=Math.atan2(ty-f.y,tx-f.x);
      for(let s=bd-90;s>60;s-=12){
        const nx2=f.x+Math.cos(a2)*s, ny2=f.y+Math.sin(a2)*s;
        if(free(nx2,ny2,f.r)){
          zones.push({k:"puff",x:f.x,y:f.y,life:.35,max:.35,c:"#7a6a86"});
          f.x=nx2; f.y=ny2;
          zones.push({k:"puff",x:f.x,y:f.y,life:.35,max:.35,c:"#7a6a86"});
          sfx(300,.14,"sine",.06);
          break;
        }
      }
    }
  }
  // big ones charge
  if(f.k==="mini"||f.k==="boss"){
    if(f.wind>0) sp*=.15;
    f.dash=Math.max(0,(f.dash||0)-dt);
    f.chg=(f.chg||(f.k==="boss"?6:4.4))-dt;
    if(f.chg<=0&&bd<420){ f.chg=(f.k==="boss"?6.5:4.8)+rnd()*2; f.dash=.55; f.da=a; sfx(100,.2,"sawtooth",.1); }
    if(f.dash>0){ a=f.da; sp*=2.6; }
    if(f.k==="boss"){
      f.pulse-=dt; f.sum-=dt; f.wind=Math.max(0,(f.wind||0)-dt);
      const frac=f.hp/f.mhp;
      if(f.pulse<=0&&!f.wind){
        // wind up: show where the ring will be solid and where the gaps are
        f.pulse=3.8-1.2*(1-frac);
        f.arms=frac<.45?4:3;
        f.rot=rnd()*6.28;
        f.wedge=(6.283/f.arms)*(frac<.45?.60:.52);
        f.rad=210;
        f.wind=.78; f.winding=1; f.fired=false;
        zones.push({k:"tell",x:f.x,y:f.y,r:f.rad,arms:f.arms,rot:f.rot,wedge:f.wedge,
                    life:.78,max:.78,c:"#9c3f22",follow:f});
        sfx(340,.5,"sine",.06);
      }
      if(f.winding&&f.wind<=0&&!f.fired){
        f.fired=true; f.winding=0;
        zones.push({k:"arms",x:f.x,y:f.y,r:f.rad,arms:f.arms,rot:f.rot,wedge:f.wedge,
                    life:.5,max:.5,c:"#9c3f22"});
        const caught=(ex,ey)=>{
          const d=dist(ex,ey,f.x,f.y);
          if(d>f.rad||d<8) return false;
          if(!losClear(f.x,f.y,ex,ey)) return false;      // get behind something
          let ang=Math.atan2(ey-f.y,ex-f.x)-f.rot;
          const seg=6.283/f.arms;
          ang=((ang%seg)+seg)%seg;
          return ang<f.wedge;
        };
        if(caught(player.x,player.y)) hurtPlayer(16);
        for(const m of party) if(!m.down&&caught(m.x,m.y)) hurtAlly(m,12);
        let flung=0;
        for(const a of animals) if(a.pet&&caught(a.x,a.y)){ a.stag=10; a.wary=0; flung++; }
        if(flung) toast("the pack is thrown","The light goes through them and they want no more of it for a while.");
        G.shake=9; sfx(70,.34,"sine",.13);
      }
      if(f.sum<=0&&frac<.75){ f.sum=9.5;
        for(let i=0;i<2;i++){ const p=spawnEdge()||{x:f.x+40,y:f.y};
          mkFoe(pick(pool()),f.x+(rnd()*80-40),f.y+(rnd()*80-40)); }
        toast("it calls","Something answers from the trees.");
      }
    } else if(f.k==="mini"){
      f.sum=(f.sum||7)-dt;
      if(f.sum<=0&&f.hp/f.mhp<.7){ f.sum=11; mkFoe(pick(pool()),f.x+30,f.y+20); }
      f.vol=(f.vol===undefined?4:f.vol)-dt;
      if(f.vol<=0&&bd>110&&bd<420&&losClear(f.x,f.y,tx,ty)){
        f.vol=5.5+rnd()*2;
        const base=Math.atan2(ty-f.y,tx-f.x);
        for(let i=-1;i<=1;i++){
          const a=base+i*.26;
          fireShot(f,f.x+Math.cos(a)*200,f.y+Math.sin(a)*200,
                   {d:400,dmg:11,sp:250,cd:0,c:"#cfc7ae",s:"stone"});
        }
      }
    }
  }
  moveEnt(f,Math.cos(a),Math.sin(a),sp,dt);
  // contact damage
  const lvl=pasLevel(f);
  if(pd<f.r+player.r+3&&f.cd<=0){
    f.cd=.75/lvl;
    if(window.ART&&window.ART.hit) window.ART.hit(f);
    hurtPlayer(f.dmg*lvl,f); pasCold(f);
    if(f.drain) G.vg=Math.max(0,G.vg-12);
    if(f.chill) player.chill=1.6;
  }
  if(tobj&&dist(f.x,f.y,tobj.x,tobj.y)<f.r+tobj.r+3&&f.cd<=0){ f.cd=.85; hurtAlly(tobj,f.dmg*.8); }
}

function updAlly(m,dt){
  m.ph+=dt; m.hurt=Math.max(0,(m.hurt||0)-dt); m.swing=Math.max(0,(m.swing||0)-dt);
  m.rage=Math.max(0,(m.rage||0)-dt);
  if(m.g&&!m.down) allyCast(m,dt);
  if(m.down){ m.bleed-=dt; if(m.bleed<=0) killAlly(m); return; }
  m.cd=Math.max(0,m.cd-dt);
  const eth=pasSawSlow(m);                     // the surgeon's ether does not pick sides
  let tgt=null,bd=230;
  for(const f of foes){ const d=dist(f.x,f.y,m.x,m.y); if(d<bd&&!f.asleep){ bd=d; tgt=f; } }
  const away=dist(m.x,m.y,player.x,player.y);
  const wpn=allyWeapon(m), st=wpn?wpn.st:null;
  /* a medic goes where the blood is */
  const bnd=allyBandages(m);
  if(bnd&&m.cd<=0){
    let pat=null,pd2=520,ratio;
    if(G.hp/G.mhp<.5){ pat="captain"; pd2=dist(m.x,m.y,player.x,player.y); }
    for(const o of party) if(!o.down&&o.hp/o.mhp<.5){ const d=dist(m.x,m.y,o.x,o.y); if(d<pd2){ pd2=d; pat=o; } }
    if(m.hp/m.mhp<.5&&(!pat||pd2>0)) { pat=m; pd2=0; }
    if(pat){
      const px=pat==="captain"?player.x:pat.x, py=pat==="captain"?player.y:pat.y;
      if(pd2>26){ seekTo(m,px,py,m.sp*1.2*eth,dt,20); return; }
      bnd.n--; if(bnd.n<=0){ const ci=m.carry.indexOf(bnd); if(ci>=0) m.carry[ci]=null; }
      m.cd=1.2;
      if(pat==="captain"){ G.hp=Math.min(G.mhp,G.hp+40); toast(m.n.split(" ")[0],"Binds your arm while you are still using it."); }
      else { pat.hp=Math.min(pat.mhp,pat.hp+40); if(pat!==m) toast(m.n.split(" ")[0],"Patches "+pat.n.split(" ")[0]+" up."); }
      syncHUD(); return;
    }
  }
  /* fear is a kind of sense */
  if(m.hp<m.mhp*.35&&tgt){
    const a2=Math.atan2(m.y-tgt.y,m.x-tgt.x)*.5+Math.atan2(player.y-m.y,player.x-m.x)*.5;
    const ang=bd<140?Math.atan2(m.y-tgt.y,m.x-tgt.x):Math.atan2(player.y-m.y,player.x-m.x);
    moveEnt(m,Math.cos(ang),Math.sin(ang),m.sp*1.15*eth,dt);
    m.face=ang;
    return;
  }
  /* the captain's vicinity is where the crew belongs */
  if(away>260){
    seekTo(m,player.x,player.y,m.sp*1.3*eth,dt,60);
    return;
  }
  if(tgt&&away<330){
    const reach=st&&!WPN_RANGED[WPN[wpn.w].cls]?st.reach*.9:null;
    const ranged=st&&WPN_RANGED[WPN[wpn.w].cls];
    if(ranged&&bd<st.reach&&bd>60&&losClear(m.x,m.y,tgt.x,tgt.y)){
      m.face=Math.atan2(tgt.y-m.y,tgt.x-m.x);
      if(m.cd<=0){ m.cd=st.cd*1.25; m.swing=.14; allyShot(m,tgt,st); }
      if(bd<100){ const ang=Math.atan2(m.y-tgt.y,m.x-tgt.x); moveEnt(m,Math.cos(ang),Math.sin(ang),m.sp*.7,dt); }
    }
    else if(bd>(reach||tgt.r+m.r+8)) seekTo(m,tgt.x,tgt.y,m.sp*eth,dt,10);
    else { m.face=Math.atan2(tgt.y-m.y,tgt.x-m.x);
      if(m.cd<=0){ m.cd=st?st.cd*1.15:.95; m.swing=.16;
        if(window.ART&&window.ART.hit) window.ART.hit(m);
        hurtFoe(tgt,(st?st.dmg:m.dmg)*(m.rage>0?1.5:1)*dmgMul(tgt),4); sfx(190,.05,"square",.06); } }
  } else {
    const a=(party.indexOf(m))*2.1;
    seekTo(m,player.x-Math.cos(player.aim)*34+Math.cos(a)*26,player.y-Math.sin(player.aim)*34+Math.sin(a)*26,
           (away>200?m.sp*1.25:m.sp)*eth,dt,26);
  }
}

/* ============================================================ ENDING */
function showEnding(kind){
  G.mode="ending"; closePanels(); $("dbox").classList.remove("on");
  const bits=[];
  bits.push(`<p>${G.name}, ${G.trade.n}. ${G.isl+1} island${G.isl?"s":""} walked. ${G.kills} things put down. ${Math.floor(G.mist)} mist taken off them.</p>`);
  bits.push(`<p class="small">Regards: ${G.regards.length?G.regards.map(g=>`${g.R.n} (${g.R.tier}, rank ${g.rank})`).join("; "):"none — nothing ever looked at you long enough"}.
    Spirits met: ${G.spiritsMet}. Guardians put down: ${G.bossKills}. Wrecks survived: ${G.wrecks}.</p>`);
  if(G.deaths.length) bits.push(`<p class="small">Left behind: ${G.deaths.join("; ")}.</p>`);
  else bits.push(`<p class="small">Nobody who followed you died, which is rarer out here than a Regard.</p>`);
  bits.push(`<p class="small">${HOME?`Your island: ${HOME.ISL?HOME.ISL.name:"claimed"}, ${(HOME.builds||[]).length} things standing, ${(HOME.residents||[]).length} staying on it.`:"You never laid a hearth anywhere."}
    ${G.tamed?` Tamed: ${G.tamed}.`:""}${G.bred?` Bred: ${G.bred}.`:""}${G.broken?` Broken islands emptied: ${G.broken}.`:""}</p>`);
  if(kind==="route"){
    $("ettl").textContent="The sea route";
    bits.unshift(`<p>With the Broken place quiet behind you the fog finally has an edge, and the edge has a gap in it
      the width of a boat. You go through it. The water on the other side is ordinary water — it moves with the wind,
      it has a horizon, and nothing under it is paying attention to you.</p>
      <p>Somewhere behind you an island keeps a fire you laid, and whatever you left standing on it goes on standing.</p>`);
  }
  else if(kind==="drowned"){
    $("ettl").textContent="The sea keeps you";
    bits.unshift(`<p>The third time under, the water does not bother giving the light back. There is a long grey moment
      that feels like being read rather than drowned, and then the sea route goes on without you.</p>`);
  } else {
    $("ettl").textContent="The chart runs out";
    bits.unshift(`<p>East of the last island the fog stops being weather and starts being a wall, and the boat goes into it
      because you point it there. Somewhere behind you the sea route closes like a book.</p>`);
  }
  $("etxt").innerHTML=bits.join("");
  $("ending").classList.add("on");
}

/* ============================================================ INPUT */
const K={};
const TC={on:false,mx:0,my:0,id:null,cx:0,cy:0};
const ADVW=()=>TC.on?"tap ›":"e ›";
addEventListener('keydown',e=>{
  const ae=document.activeElement;
  if(ae&&(ae.tagName==="INPUT"||ae.tagName==="TEXTAREA")) return;
  if(["KeyW","KeyA","KeyS","KeyD","Space","KeyE","KeyQ","KeyF","KeyR","KeyC","KeyJ","KeyM","KeyH","KeyK","KeyP","KeyY","Tab",
      "Digit1","Digit2","Digit3","Digit4","Digit5","Digit6","Digit7","Digit8",
      "ArrowUp","ArrowDown","ArrowLeft","ArrowRight"].includes(e.code)) e.preventDefault();
  if(K[e.code]) return; K[e.code]=true;
  if($("title").classList.contains("on")){ if(NET.role==="guest") return; if(e.code==="KeyE"||e.code==="Enter"||e.code==="Space") beginGame(); return; }
  if($("inter").classList.contains("on")){ if(e.code==="KeyE"||e.code==="Enter"||e.code==="Space") closeInter(); return; }
  if($("ending").classList.contains("on")) return;
  if(NET.role==="guest"&&netGuestKey(e)) return;
  if(e.code==="Escape"){ if(G.mode==="dialog") closeDialog(); else closePanels(); return; }
  if(G.mode==="dialog"){ if(e.code==="KeyE"||e.code==="Space"||e.code==="Enter") advanceDialog(); return; }
  if(e.code==="KeyP"){ netPause(); return; }
  if(anyPanel()==="cachep"&&(e.code==="KeyE")){ closePanels(); return; }
  if(e.code==="Tab") togglePanel("pack");
  else if(e.code==="KeyK") togglePanel("artp");
  else if(e.code==="KeyY") togglePanel("cityp");
  else if(e.code==="KeyC") togglePanel("craft");
  else if(e.code==="KeyR") togglePanel("regard");
  else if(e.code==="KeyM") togglePanel("chart");
  else if(e.code==="KeyJ") togglePanel("logp");
  else if(e.code==="KeyE") interact();
  else if(e.code==="KeyQ") abil(1);
  else if(e.code==="KeyF") abil(2);
  else if(e.code==="KeyH"){ const m=nearDown(); if(m) helpUp(m); }
  else if(e.code==="Space") dodge();
  else if(e.code.startsWith("Digit")){ const i=+e.code.slice(5)-1; if(i>=0&&i<8){ useSlot(i); if($("pack").classList.contains("on")) drawPack(); } }
});
addEventListener('keyup',e=>{K[e.code]=false;});
cv.addEventListener('mousemove',e=>{
  const b=cv.getBoundingClientRect();
  const mx=(e.clientX-b.left)*(VW/b.width), my=(e.clientY-b.top)*(VH/b.height);
  const me=(NET.role==="guest"&&NET.me)?NET.me:player;
  me.aim=Math.atan2(my-(me.y-cam.y), mx-(me.x-cam.x));
  if(NET.role==="guest") NET.aim=me.aim;
});
cv.addEventListener('mousedown',e=>{ if(e.button===0){ if(NET.role==="guest") netPress("atk"); else melee(); } });
$("idle").addEventListener('click',closeInter);
$("inter").addEventListener('click',closeInter);
/* ============================================================ TOUCH
   A left-thumb stick that appears wherever the thumb lands, and buttons for
   everything the right hand did with a mouse. All of it routes through the
   same rules the keyboard follows, so host, guest, title and dialogue behave
   identically. None of this exists on a machine with a fine pointer. */
(function(){
  TC.on=("ontouchstart" in window)||(window.matchMedia&&matchMedia("(pointer:coarse)").matches);
  if(!TC.on) return;
  document.body.classList.add("touch");
  $("idle").textContent="tap to go on";

  /* the page itself never moves. Scrolling lives inside open panels and cards. */
  const scrollOK=el=>!!(el&&el.closest&&el.closest(".panel.on,.full.on,#dopts,textarea"));
  document.addEventListener("touchmove",e=>{ if(!scrollOK(e.target)) e.preventDefault(); },{passive:false});
  document.addEventListener("gesturestart",e=>e.preventDefault());
  document.addEventListener("dblclick",e=>e.preventDefault());
  document.addEventListener("contextmenu",e=>{
    if(e.target&&e.target.closest&&!e.target.closest("textarea,input")) e.preventDefault();
  });
  addEventListener("touchend",()=>{ try{
    AC=AC||new (window.AudioContext||window.webkitAudioContext)();
    if(AC.state==="suspended") AC.resume();
  }catch(e){} });

  /* a synthetic key walks through every rule the real one does */
  const vkey=code=>{
    dispatchEvent(new KeyboardEvent("keydown",{code}));
    dispatchEvent(new KeyboardEvent("keyup",{code}));
  };

  /* ---- the stick: born under the thumb, dies when it lifts ---- */
  const lay=$("tlayer"), base=$("tstick"), nub=$("tnub");
  const R=54, DEAD=.3;
  const inStage=t=>{ const b=lay.getBoundingClientRect();
    return {x:t.clientX-b.left,y:t.clientY-b.top}; };
  function setStick(t){
    const p=inStage(t);
    let dx=p.x-TC.cx, dy=p.y-TC.cy;
    const d=Math.hypot(dx,dy);
    if(d>R){ dx=dx/d*R; dy=dy/d*R; }
    nub.style.transform=`translate(${dx}px,${dy}px)`;
    if(d<R*DEAD){ TC.mx=0; TC.my=0; return; }
    const m=Math.hypot(dx,dy)||1; TC.mx=dx/m; TC.my=dy/m;
    const me=(NET.role==="guest"&&NET.me)?NET.me:player;
    me.aim=Math.atan2(TC.my,TC.mx);
    if(NET.role==="guest") NET.aim=me.aim;
  }
  lay.addEventListener("touchstart",e=>{
    e.preventDefault();
    if(TC.id!==null) return;
    const t=e.changedTouches[0], p=inStage(t);
    TC.id=t.identifier; TC.cx=p.x; TC.cy=p.y;
    base.style.left=p.x+"px"; base.style.top=p.y+"px";
    nub.style.transform="translate(0px,0px)";
    base.classList.add("on");
  },{passive:false});
  lay.addEventListener("touchmove",e=>{
    e.preventDefault();
    for(const t of e.changedTouches) if(t.identifier===TC.id) setStick(t);
  },{passive:false});
  const drop=e=>{
    for(const t of e.changedTouches) if(t.identifier===TC.id){
      TC.id=null; TC.mx=0; TC.my=0; base.classList.remove("on");
    }
  };
  lay.addEventListener("touchend",drop);
  lay.addEventListener("touchcancel",drop);

  /* ---- striking: the button aims itself at the nearest live thing ---- */
  function aimNearest(){
    const rng=wpnRanged()?340:180;
    let best=null,bd=rng;
    for(const f of foes){ const d=dist(player.x,player.y,f.x,f.y); if(d<bd){bd=d;best=f;} }
    for(const a of animals){ if(a.pet) continue;
      const d=dist(player.x,player.y,a.x,a.y); if(d<bd){bd=d;best=a;} }
    if(best) player.aim=Math.atan2(best.y-player.y,best.x-player.x);
  }
  function doAtk(){
    if(NET.role==="guest"){ netPress("atk"); return; }
    aimNearest(); melee();
  }
  let holdT=null;
  function press(id,fn,rep){
    const el=$(id); if(!el) return;
    el.addEventListener("touchstart",e=>{
      e.preventDefault(); e.stopPropagation(); fn();
      if(rep){ clearInterval(holdT); holdT=setInterval(fn,140); }
    },{passive:false});
    const up=e=>{ e.preventDefault(); if(rep){ clearInterval(holdT); holdT=null; } };
    el.addEventListener("touchend",up);
    el.addEventListener("touchcancel",up);
  }
  press("tatk",doAtk,true);
  press("tdash",()=>vkey("Space"));
  press("tuse",()=>vkey("KeyE"));
  press("tq",()=>vkey("KeyQ"));
  press("tf",()=>vkey("KeyF"));
  press("th",()=>vkey("KeyH"));
  press("tm_pack",()=>vkey("Tab"));
  press("tm_craft",()=>vkey("KeyC"));
  press("tm_rg",()=>vkey("KeyR"));
  press("tm_map",()=>vkey("KeyM"));
  press("tm_log",()=>vkey("KeyJ"));
  press("tm_pause",()=>vkey("KeyP"));
  if(document.documentElement.requestFullscreen){
    press("tm_fs",()=>{
      if(document.fullscreenElement) document.exitFullscreen();
      else document.documentElement.requestFullscreen().catch(()=>{});
    });
  } else $("tm_fs").style.display="none";

  /* ---- a tap on a belt slot uses it, same as its number key ---- */
  $("belt").addEventListener("touchstart",e=>{
    let s=e.target; while(s&&s!==$("belt")&&!(s.classList&&s.classList.contains("slot"))) s=s.parentNode;
    if(!s||s===$("belt")) return;
    e.preventDefault();
    const i=Array.prototype.indexOf.call($("belt").children,s);
    if(i>=0&&i<8) vkey("Digit"+(i+1));
  },{passive:false});

  /* ---- a tap on the dialogue box turns the page ---- */
  $("dbox").addEventListener("touchstart",e=>{
    if(e.target&&e.target.closest&&e.target.closest("#dopts")) return;
    e.preventDefault();
    if(NET.role==="guest"&&NET.gd){ netDlgAdvance(); return; }
    if(G.mode==="dialog") advanceDialog();
  },{passive:false});

  /* ---- every panel grows a way out ---- */
  for(const p of PANELS){
    const x=document.createElement("div");
    x.className="pclose"; x.textContent="\u2715";
    x.addEventListener("touchstart",ev=>{ ev.preventDefault(); ev.stopPropagation(); closePanels(); },{passive:false});
    x.addEventListener("click",()=>closePanels());
    $(p).appendChild(x);
  }
})();
/* ============================================================ RENDER */
function tileColor(t,x,y){
  const h=(tileHash(x,y)%10)/10;
  switch(t){
    case WATER: return h<.5?"#0d1c22":"#0a171d";
    case SHOAL: return h<.5?"#17323a":"#153039";
    case SAND:  return h<.5?"#b5a684":"#ab9c7b";
    case GRASS: return h<.5?"#3f5540":"#3a4f3b";
    case MOSS:  return h<.5?"#4a5c3f":"#445639";
    case DIRT:  return h<.5?"#6a5c48":"#645643";
    case PATH:  return h<.5?"#8b8570":"#847e69";
    case SCREE: return h<.5?"#787066":"#716a60";
    case SNOW:  return h<.5?"#cdd6d3":"#c4cecb";
    case ICE:   return h<.5?"#9fb4bd":"#95abb4";
    case MIRE:  return h<.5?"#33403a":"#2d3a34";
    case TREE:  return "#33452f";
    case PINE:  return "#2a3d33";
    case SCRUB: return h<.5?"#4f5b3f":"#495439";
    case ROCK:  return "#6d7377";
    case CLIFF: return h<.5?"#585d61":"#51565a";
    case VEIN:  return "#6b5a4a";
    case WALL:  return "#3b2f26";
    case FLOOR: return h<.5?"#6f5a44":"#67533f";
    case DOOR:  return "#a8763f";
    case PLANK: return h<.5?"#5b4a38":"#54452f";
    case RUIN:  return "#6a6a63";
    case GRAVE: return "#7c8a80";
    default: return "#3f5540";
  }
}
/* tall things — scenery, nodes, props, builds — are queued here each frame and
   drawn y-sorted alongside people, so you can actually walk behind a bush
   instead of being smeared on top of it */
const tallQ=[];
const TALLSCEN={};
TALLSCEN[TREE]=1;TALLSCEN[PINE]=1;TALLSCEN[SCRUB]=1;TALLSCEN[ROCK]=1;TALLSCEN[CLIFF]=1;TALLSCEN[VEIN]=1;
function drawWorld(){
  tallQ.length=0;
  /* a custom district smaller than the screen leaves canvas outside the grid —
     paint it sea-dark every frame so nothing stale survives out there */
  if(GW*TILE<VW||GH*TILE<VH){ ctx.fillStyle="#0a171d"; ctx.fillRect(-10,-10,VW+20,VH+20); }
  const x0=Math.max(0,Math.floor(cam.x/TILE)), y0=Math.max(0,Math.floor(cam.y/TILE));
  const x1=Math.min(GW-1,Math.ceil((cam.x+VW)/TILE)), y1=Math.min(GH-1,Math.ceil((cam.y+VH)/TILE));
  const now=performance.now();
  for(let y=y0;y<=y1;y++)for(let x=x0;x<=x1;x++){
    const t=T(x,y), sx=Math.round(x*TILE-cam.x), sy=Math.round(y*TILE-cam.y);
    const artK=TILEART[t];
    const im=artK?((window.ART&&window.ART.tile&&window.ART.tile(artK,x,y))||IMG[artK]):null;
    if(im){
      if(FLATART[t]){
        /* ground under everything, even flat art — a transparent bush must never
           show last frame's pixels through its corners */
        const bt=THEMES[ISL.theme].ground, bk=TILEART[bt];
        const bim=(bk&&bk!==artK)?((window.ART&&window.ART.tile&&window.ART.tile(bk,x,y))||IMG[bk]):null;
        if(bim) ctx.drawImage(bim,sx,sy,TILE,TILE);
        else { ctx.fillStyle=tileColor(THEMES[ISL.theme].ground,x,y); ctx.fillRect(sx,sy,TILE,TILE); }
        ctx.drawImage(im,sx,sy,TILE,TILE);
      }
      else {
        const bt=THEMES[ISL.theme].ground, bk=TILEART[bt];
        const bim=bk?((window.ART&&window.ART.tile&&window.ART.tile(bk,x,y))||IMG[bk]):null;
        if(bim) ctx.drawImage(bim,sx,sy,TILE,TILE);
        else { ctx.fillStyle=tileColor(bt,x,y); ctx.fillRect(sx,sy,TILE,TILE); }
        const o=artOpt(artK);
        tallQ.push({y:(y+1)*TILE,im,dx:sx+TILE/2-o.w/2+o.ox,dy:sy+TILE-o.h+o.oy,w:o.w,h:o.h});
      }
      continue;
    }
    ctx.fillStyle=tileColor(t,x,y); ctx.fillRect(sx,sy,TILE,TILE);
    if(TALLSCEN[t]){ tallQ.push({y:(y+1)*TILE,scen:t,tx:x,ty:y,sx,sy}); continue; }
    if(t===WATER&&(x+y+Math.floor(now/900))%11===0){
      ctx.fillStyle="rgba(150,180,190,.10)"; ctx.fillRect(sx,sy+TILE/2,TILE,2); }
    if(t===SHOAL&&(x*3+y+Math.floor(now/700))%7===0){
      ctx.fillStyle="rgba(190,215,220,.12)"; ctx.fillRect(sx+3,sy+TILE/2,TILE-6,2); }
    if(t===PATH){ ctx.fillStyle="rgba(255,255,255,.07)"; ctx.fillRect(sx,sy+TILE-3,TILE,3); }
    if(t===MIRE){ ctx.fillStyle="rgba(120,150,130,.10)"; ctx.beginPath();
      ctx.ellipse(sx+14,sy+16,9,4,0,0,6.3); ctx.fill(); }
    if(t===ICE){ ctx.strokeStyle="rgba(255,255,255,.12)"; ctx.lineWidth=1; ctx.beginPath();
      ctx.moveTo(sx+4,sy+20); ctx.lineTo(sx+18,sy+6); ctx.stroke(); }
    if(t===SCREE){ ctx.fillStyle="rgba(0,0,0,.16)";
      for(let i=0;i<3;i++){ const jx=(tileHash(x+i,y)%20), jy=(tileHash(y,x+i)%20);
        ctx.fillRect(sx+jx*1.2,sy+jy*1.2,3,2); } }
    if(t===SAND&&(tileHash(x,y)%9)===0){ ctx.fillStyle="rgba(255,255,255,.10)"; ctx.fillRect(sx+7,sy+13,10,2); }
    if(t===GRAVE){ ctx.fillStyle="#5d6a62"; ctx.fillRect(sx+11,sy+7,5,14); ctx.fillRect(sx+7,sy+11,13,4); }
    if(t===WALL){ ctx.fillStyle="rgba(0,0,0,.25)"; ctx.fillRect(sx,sy+TILE-5,TILE,5);
      ctx.fillStyle="rgba(255,255,255,.05)"; ctx.fillRect(sx,sy,TILE,3); }
    if(t===RUIN){ ctx.fillStyle="rgba(0,0,0,.22)"; ctx.fillRect(sx,sy+TILE-6,TILE,6);
      ctx.fillStyle="rgba(255,255,255,.06)"; ctx.fillRect(sx+2,sy+2,TILE-4,3); }
    if(t===DOOR){ ctx.fillStyle="#6b4a24"; ctx.fillRect(sx+3,sy+4,TILE-6,TILE-4); }
    if(t===PLANK){ ctx.fillStyle="rgba(0,0,0,.20)"; ctx.fillRect(sx,sy+9,TILE,2); ctx.fillRect(sx,sy+20,TILE,2); }
  }
  // labels a city district asked for
  if(ISL.labels&&ISL.labels.length){
    ctx.font="10px 'Courier New', monospace"; ctx.textAlign="center";
    for(const l of ISL.labels){
      const sx=l.x*TILE+14-cam.x, sy=l.y*TILE+14-cam.y;
      if(sx<-160||sx>VW+160||sy<-90||sy>VH+90) continue;
      ctx.fillStyle="rgba(233,226,207,.55)"; ctx.fillText(l.t,sx,sy);
    }
    ctx.textAlign="left";
  }
  // building plates
  if(ISL.buildings.length){
    ctx.font="10px 'Courier New', monospace"; ctx.textAlign="center";
    for(const b of ISL.buildings){
      const sx=b.x*TILE+b.w*TILE/2-cam.x, sy=b.y*TILE+b.h*TILE/2-cam.y;
      if(sx<-160||sx>VW+160||sy<-90||sy>VH+90) continue;
      ctx.fillStyle="rgba(233,226,207,.5)"; ctx.fillText(b.n.toUpperCase(),sx,sy);
    }
    ctx.textAlign="left";
  }
}
/* the code-drawn tall things, one tile at a time, from the sorted pass */
function drawTallScen(t,x,y,sx,sy){
  if(t===ROCK||t===CLIFF){
      const jx=(tileHash(x,y)%5)-2;
      ctx.fillStyle=t===CLIFF?"#454a4e":"#4f5457";
      ctx.beginPath(); ctx.moveTo(sx+2+jx,sy+TILE); ctx.lineTo(sx+14+jx,sy+3);
      ctx.lineTo(sx+26+jx,sy+TILE); ctx.closePath(); ctx.fill();
      ctx.fillStyle="rgba(255,255,255,.07)"; ctx.beginPath();
      ctx.moveTo(sx+14+jx,sy+3); ctx.lineTo(sx+20+jx,sy+16); ctx.lineTo(sx+9+jx,sy+16); ctx.closePath(); ctx.fill();
    }
  if(t===VEIN){
      ctx.fillStyle="#4f5457"; ctx.beginPath(); ctx.moveTo(sx+2,sy+TILE);
      ctx.lineTo(sx+14,sy+4); ctx.lineTo(sx+26,sy+TILE); ctx.closePath(); ctx.fill();
      ctx.fillStyle="#b06a36";
      for(let i=0;i<3;i++){ const jy=8+i*5; ctx.fillRect(sx+9+((tileHash(x,y+i)%6)),sy+jy,4,2); }
    }
  if(t===SCRUB){ ctx.fillStyle="rgba(20,30,24,.28)"; ctx.beginPath();
      ctx.ellipse(sx+14,sy+20,8,4,0,0,6.3); ctx.fill();
      ctx.fillStyle="#54633f"; ctx.beginPath(); ctx.arc(sx+14,sy+15,7,0,6.3); ctx.fill();
      ctx.fillStyle="#455233"; ctx.beginPath(); ctx.arc(sx+11,sy+13,4,0,6.3); ctx.fill(); }
  if(t===TREE||t===PINE){
      const jx=(tileHash(x,y)%7)-3, jy=(tileHash(y,x)%5)-2, snowy=ISL.theme==="snow";
      ctx.fillStyle="rgba(10,18,14,.32)"; ctx.beginPath();
      ctx.ellipse(sx+14+jx,sy+22+jy,11,5,0,0,6.3); ctx.fill();
      ctx.fillStyle="#3a2b20"; ctx.fillRect(sx+12+jx,sy+15+jy,4,10);
      if(t===PINE||snowy){
        ctx.fillStyle="#1e3a31"; ctx.beginPath();
        ctx.moveTo(sx+14+jx,sy-5+jy); ctx.lineTo(sx+25+jx,sy+20+jy); ctx.lineTo(sx+3+jx,sy+20+jy); ctx.closePath(); ctx.fill();
        ctx.fillStyle="#28493d"; ctx.beginPath();
        ctx.moveTo(sx+14+jx,sy+0+jy); ctx.lineTo(sx+22+jx,sy+15+jy); ctx.lineTo(sx+6+jx,sy+15+jy); ctx.closePath(); ctx.fill();
        if(snowy){ ctx.fillStyle="rgba(215,225,222,.72)"; ctx.beginPath();
          ctx.moveTo(sx+14+jx,sy-5+jy); ctx.lineTo(sx+19+jx,sy+6+jy); ctx.lineTo(sx+9+jx,sy+6+jy); ctx.closePath(); ctx.fill(); }
      } else {
        ctx.fillStyle="#2f4a2c"; ctx.beginPath(); ctx.arc(sx+14+jx,sy+11+jy,11,0,6.3); ctx.fill();
        ctx.fillStyle="#3b5b34"; ctx.beginPath(); ctx.arc(sx+11+jx,sy+8+jy,7,0,6.3); ctx.fill();
        ctx.fillStyle="rgba(0,0,0,.16)"; ctx.beginPath(); ctx.arc(sx+18+jx,sy+15+jy,5,0,6.3); ctx.fill();
      }
    }

}
function drawProp(p){
    const sx=p.x-cam.x, sy=p.y-cam.y;
    if(sx<-80||sx>VW+80||sy<-80||sy>VH+80) return;
    if(spr("prop_"+p.k,sx,sy+14,1,undefined,0,p)) return;
    ctx.save(); ctx.translate(sx,sy); ctx.rotate(p.a*.2);
    ctx.fillStyle="rgba(10,16,18,.30)"; ctx.beginPath(); ctx.ellipse(0,6,22,7,0,0,6.3); ctx.fill();
    if(p.k==="hull"){
      ctx.fillStyle="#4a3a2b"; ctx.beginPath();
      ctx.moveTo(-26,2); ctx.quadraticCurveTo(0,-16,26,2); ctx.quadraticCurveTo(0,10,-26,2); ctx.fill();
      ctx.strokeStyle="#6b5540"; ctx.lineWidth=2; ctx.beginPath(); ctx.moveTo(-8,-6); ctx.lineTo(-2,-24); ctx.stroke();
    } else {
      ctx.fillStyle=p.k==="boat"?"#5c4832":"#6b5540";
      for(let i=-2;i<=2;i++){ ctx.fillRect(-20,i*5-2,40,4); }
      if(p.k==="boat"){ ctx.strokeStyle="#8b9793"; ctx.lineWidth=2;
        ctx.beginPath(); ctx.moveTo(0,-2); ctx.lineTo(0,-26); ctx.stroke();
        ctx.fillStyle="rgba(233,226,207,.65)"; ctx.beginPath();
        ctx.moveTo(1,-24); ctx.lineTo(15,-6); ctx.lineTo(1,-6); ctx.closePath(); ctx.fill(); }
    }
    ctx.restore();
  }
function drawProps(){ for(const p of props) drawProp(p); }
function drawBuild(b){
    const sx=b.x-cam.x, sy=b.y-cam.y, t=performance.now()/1000;
    if(sx<-120||sx>VW+120||sy<-120||sy>VH+120) return;
    ctx.fillStyle="rgba(10,16,18,.30)"; ctx.beginPath();
    ctx.ellipse(sx,sy+4,b.r*.95,b.r*.4,0,0,6.3); ctx.fill();
    const drew=spr("build_"+b.k,sx,sy+6,1,undefined,0,b);
    if(!drew){
      if(b.k==="hearth"){
        ctx.fillStyle="#5b5450";
        for(let i=0;i<7;i++){ const a=i*.9;
          ctx.beginPath(); ctx.arc(sx+Math.cos(a)*14,sy+Math.sin(a)*7,4.5,0,6.3); ctx.fill(); }
        ctx.fillStyle="#3a2b20";
        ctx.fillRect(sx-8,sy-6,16,4); ctx.fillRect(sx-2,sy-12,4,10);
      } else if(b.k==="plot"){
        ctx.fillStyle="#4a3a2a"; ctx.beginPath(); ctx.ellipse(sx,sy,b.r,b.r*.62,0,0,6.3); ctx.fill();
        ctx.strokeStyle="rgba(0,0,0,.22)"; ctx.lineWidth=2;
        for(let i=-1;i<=1;i++){ ctx.beginPath();
          ctx.moveTo(sx-b.r*.8,sy+i*5); ctx.lineTo(sx+b.r*.8,sy+i*5); ctx.stroke(); }
      } else if(b.k==="furnace"){
        ctx.fillStyle="#5a5450"; ctx.beginPath();
        ctx.moveTo(sx-14,sy); ctx.lineTo(sx-10,sy-30); ctx.lineTo(sx+10,sy-30); ctx.lineTo(sx+14,sy); ctx.closePath(); ctx.fill();
        ctx.fillStyle="#20191a"; ctx.fillRect(sx-6,sy-13,12,13);
      } else if(b.k==="house"){
        ctx.fillStyle="#4a3a2b"; ctx.fillRect(sx-24,sy-32,48,32);
        ctx.fillStyle="#3a2d22"; ctx.beginPath();
        ctx.moveTo(sx-30,sy-30); ctx.lineTo(sx,sy-54); ctx.lineTo(sx+30,sy-30); ctx.closePath(); ctx.fill();
        ctx.fillStyle="#20191a"; ctx.fillRect(sx-6,sy-18,12,18);
        ctx.fillStyle="rgba(224,161,65,.55)"; ctx.fillRect(sx+10,sy-26,8,8);
      } else if(b.k==="pen"){
        ctx.strokeStyle="#6b5540"; ctx.lineWidth=3;
        for(let i=0;i<10;i++){ const a=i*.628;
          const px=sx+Math.cos(a)*b.r*1.5, py=sy+Math.sin(a)*b.r*.8;
          ctx.beginPath(); ctx.moveTo(px,py); ctx.lineTo(px,py-12); ctx.stroke(); }
        ctx.beginPath(); ctx.ellipse(sx,sy-6,b.r*1.5,b.r*.8,0,0,6.3); ctx.stroke();
      }
    }
    // a fire that does not go out, and smoke off the furnace
    if(b.k==="hearth"||b.k==="furnace"){
      const fy=b.k==="hearth"?sy-8:sy-10;
      for(let i=0;i<3;i++){
        const fl=(Math.sin(t*7+i*2+b.ph)+1)/2;
        ctx.fillStyle=`rgba(${224-i*30},${140-i*20},${50},${.5+fl*.4})`;
        ctx.beginPath(); ctx.arc(sx+(i-1)*4,fy-fl*7,3.5-i,0,6.3); ctx.fill();
      }
      if((b.k==="furnace"&&b.q&&b.q.length)||b.k==="hearth"){
        for(let i=0;i<2;i++){
          const p2=((t*.5+i*.5+b.ph)%1);
          ctx.fillStyle=`rgba(150,150,150,${.18*(1-p2)})`;
          ctx.beginPath(); ctx.arc(sx+Math.sin(p2*6+b.ph)*7,fy-16-p2*34,4+p2*7,0,6.3); ctx.fill();
        }
      }
    }
    // what is growing in the bed
    if(b.k==="plot"&&b.seed){
      const c=CROPS[b.seed], f=Math.min(1,(b.grow||0)/c.time);
      const sc=.35+f*.65;
      if(!sprC("crop_"+b.seed,sx,sy-8*sc,sc)){
        ctx.fillStyle=c.c;
        for(let i=-1;i<=1;i++){
          ctx.beginPath();
          ctx.ellipse(sx+i*7,sy-6-10*sc,3.5*sc+1.5,7*sc+2,i*.2,0,6.3); ctx.fill();
        }
      }
      if(b.ripe){
        const gl=(Math.sin(t*2.4+b.ph)+1)/2;
        ctx.strokeStyle=`rgba(224,161,65,${.25+gl*.35})`; ctx.lineWidth=1;
        ctx.beginPath(); ctx.arc(sx,sy-8,b.r+5,0,6.3); ctx.stroke();
      }
    }
    if(b.k==="furnace"){
      let out=0; for(const k in (b.out||{})) out+=b.out[k];
      if(out){ const gl=(Math.sin(t*2.4)+1)/2;
        ctx.strokeStyle=`rgba(224,161,65,${.3+gl*.4})`; ctx.lineWidth=1;
        ctx.beginPath(); ctx.arc(sx,sy-14,b.r+6,0,6.3); ctx.stroke(); }
    }
  }
function drawBuilds(){ for(const b of builds) drawBuild(b); }
function drawGates(){
  for(const g of gates){
    const sx=g.x-cam.x, sy=g.y-cam.y, t=performance.now()/1000;
    if(sx<-90||sx>VW+90||sy<-90||sy>VH+90) continue;
    ctx.fillStyle="rgba(10,16,18,.28)"; ctx.beginPath(); ctx.ellipse(sx,sy+6,20,7,0,0,6.3); ctx.fill();
    if(!spr("city_gate",sx,sy+8,1,undefined,0,g)){
      ctx.fillStyle="#4a3a2b";
      ctx.fillRect(sx-18,sy-34,5,42); ctx.fillRect(sx+13,sy-34,5,42);
      ctx.fillStyle="#3a2d22"; ctx.fillRect(sx-22,sy-40,44,7);
      if(g.def.exit){
        ctx.strokeStyle="rgba(199,191,168,.5)"; ctx.lineWidth=2;
        ctx.beginPath(); ctx.moveTo(sx,sy-33); ctx.lineTo(sx,sy-12); ctx.stroke();
        ctx.fillStyle="rgba(233,226,207,.55)"; ctx.beginPath();
        ctx.moveTo(sx+1,sy-31); ctx.lineTo(sx+13,sy-18); ctx.lineTo(sx+1,sy-18); ctx.closePath(); ctx.fill();
      }
    }
    const near=nearest&&nearest.k==="gate"&&nearest.o===g;
    const gl=(Math.sin(t*2+g.ph)+1)/2;
    ctx.strokeStyle=`rgba(224,161,65,${near?.55:.18+gl*.14})`; ctx.lineWidth=1;
    ctx.beginPath(); ctx.arc(sx,sy,24,0,6.3); ctx.stroke();
    if(near){
      ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
      ctx.fillStyle="rgba(233,226,207,.85)";
      ctx.fillText((g.def.label||g.def.to||"out").toUpperCase(),sx,sy-46); ctx.textAlign="left";
    }
  }
}
function drawSign(o){
  const sx=o.x-cam.x, sy=o.y-cam.y;
  ctx.fillStyle="rgba(10,16,18,.28)"; ctx.beginPath(); ctx.ellipse(sx,sy+5,10,4,0,0,6.3); ctx.fill();
  ctx.fillStyle="#4a3a2b"; ctx.fillRect(sx-2,sy-20,4,24);
  ctx.fillStyle="#5b4a38"; ctx.fillRect(sx-11,sy-30,22,12);
  ctx.strokeStyle="rgba(0,0,0,.3)"; ctx.lineWidth=1; ctx.strokeRect(sx-11,sy-30,22,12);
  ctx.fillStyle="rgba(199,191,168,.55)";
  ctx.fillRect(sx-8,sy-26,16,1.5); ctx.fillRect(sx-8,sy-23,11,1.5);
  const hl=nearest&&nearest.o===o;
  if(hl){ ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(233,226,207,.85)"; ctx.fillText(o.n.toUpperCase(),sx,sy-36); ctx.textAlign="left"; }
}
function drawAnimal(a){
  const sx=a.x-cam.x, sy=a.y-cam.y, r=a.r;
  if(sx<-90||sx>VW+90||sy<-90||sy>VH+90) return;
  ctx.fillStyle="rgba(10,16,18,.30)"; ctx.beginPath();
  ctx.ellipse(sx,sy+r*.7,r*1.15,r*.42,0,0,6.3); ctx.fill();
  const sc=a.young?.68:1;
  if(!spr("an_"+a.k,sx,sy+r*.7,sc,a.hurt>0?.7:1,0,a)){
    const wob=Math.sin(a.ph*(a.k==="squirrel"?12:6))*1.5;
    ctx.fillStyle=a.hurt>0?"#e9e2cf":a.c;
    if(a.k==="squirrel"){
      ctx.beginPath(); ctx.ellipse(sx,sy-4*sc,6*sc,5*sc,0,0,6.3); ctx.fill();
      ctx.strokeStyle=a.c; ctx.lineWidth=3*sc;
      ctx.beginPath(); ctx.moveTo(sx-5*sc,sy-4*sc);
      ctx.quadraticCurveTo(sx-13*sc,sy-10*sc,sx-8*sc,sy-17*sc); ctx.stroke();
    } else {
      // a body and four legs, roughly
      const bw=r*1.35*sc, bh=r*.72*sc;
      ctx.beginPath(); ctx.ellipse(sx,sy-r*.55*sc+wob*.3,bw,bh,0,0,6.3); ctx.fill();
      ctx.strokeStyle=a.c; ctx.lineWidth=2.6*sc;
      for(let i=-1;i<=1;i+=2)for(let j=-1;j<=1;j+=2){
        ctx.beginPath();
        ctx.moveTo(sx+i*bw*.6,sy-r*.3*sc);
        ctx.lineTo(sx+i*bw*.6+j*Math.sin(a.ph*7+i)*2,sy+r*.6*sc); ctx.stroke();
      }
      const hx=sx+Math.cos(a.face||0)*bw*.95, hy=sy-r*.75*sc+Math.sin(a.face||0)*4;
      ctx.fillStyle=a.hurt>0?"#e9e2cf":a.c;
      ctx.beginPath(); ctx.ellipse(hx,hy,r*.46*sc,r*.36*sc,0,0,6.3); ctx.fill();
      if(a.k==="deer"){                       // antlers
        ctx.strokeStyle="#c9bda2"; ctx.lineWidth=2;
        for(let i=-1;i<=1;i+=2){ ctx.beginPath();
          ctx.moveTo(hx+i*3,hy-4*sc); ctx.lineTo(hx+i*7,hy-14*sc);
          ctx.moveTo(hx+i*5,hy-9*sc); ctx.lineTo(hx+i*11,hy-11*sc); ctx.stroke(); }
      }
      if(a.k==="wolf"||a.k==="bear"){          // eyes that are looking at you
        ctx.fillStyle=a.pet?"#d8b478":"#c25334";
        ctx.beginPath(); ctx.arc(hx+2,hy-1,1.8,0,6.3); ctx.fill();
      }
    }
  }
  if(a.swing>0){ ctx.strokeStyle="rgba(233,226,207,.6)"; ctx.lineWidth=2;
    ctx.beginPath(); ctx.arc(sx,sy-r*.4,r*1.5,(a.face||0)-.6,(a.face||0)+.6); ctx.stroke(); }
  if(a.pet){ ctx.strokeStyle="rgba(216,180,120,.35)"; ctx.lineWidth=1;
    ctx.beginPath(); ctx.arc(sx,sy+r*.7,r*.8,0,6.3); ctx.stroke(); }
  if(a.fed&&!a.pet){ ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(127,143,109,.8)"; ctx.fillText("fed",sx,sy-r*1.7); ctx.textAlign="left"; }
  if(a.hp<a.mhp){
    ctx.fillStyle="rgba(8,14,15,.8)"; ctx.fillRect(sx-14,sy-r*1.5,28,3);
    ctx.fillStyle=a.pet?"#8fb07a":"#c25334"; ctx.fillRect(sx-14,sy-r*1.5,28*clamp(a.hp/a.mhp,0,1),3);
  }
}
function drawNode(n){
    const sx=n.x-cam.x, sy=n.y-cam.y;
    if(sx<-40||sx>VW+40||sy<-40||sy>VH+40) return;
    const d=NODES[n.k], on=nearest&&nearest.k==="node"&&nearest.o===n;
    ctx.fillStyle="rgba(10,16,18,.26)"; ctx.beginPath(); ctx.ellipse(sx,sy+5,9,3.5,0,0,6.3); ctx.fill();
    if(sprC("node_"+n.k,sx,sy,1,undefined,0,n)){
      if(on){ ctx.strokeStyle="rgba(224,161,65,.55)"; ctx.lineWidth=1;
        ctx.beginPath(); ctx.arc(sx,sy,17,0,6.3); ctx.stroke(); }
      return;
    }
    ctx.fillStyle=d.c;
    if(n.k==="log"||n.k==="drift"){ ctx.save(); ctx.translate(sx,sy); ctx.rotate(n.ph);
      ctx.fillRect(-11,-3,22,6); ctx.restore(); }
    else if(n.k==="reeds"||n.k==="nettle"){ ctx.strokeStyle=d.c; ctx.lineWidth=2;
      for(let i=-2;i<=2;i++){ ctx.beginPath(); ctx.moveTo(sx+i*3,sy+4);
        ctx.lineTo(sx+i*4+Math.sin(n.ph+i)*3,sy-10); ctx.stroke(); } }
    else if(n.k==="bush"){ ctx.fillStyle="#3f5236"; ctx.beginPath(); ctx.arc(sx,sy-2,9,0,6.3); ctx.fill();
      ctx.fillStyle=d.c; for(let i=0;i<4;i++) { const a=n.ph+i*1.6;
        ctx.beginPath(); ctx.arc(sx+Math.cos(a)*5,sy-2+Math.sin(a)*5,2,0,6.3); ctx.fill(); } }
    else if(n.k==="root"){ ctx.strokeStyle=d.c; ctx.lineWidth=2;
      ctx.beginPath(); ctx.moveTo(sx,sy+4); ctx.lineTo(sx,sy-8); ctx.stroke();
      ctx.fillStyle=d.c; ctx.beginPath(); ctx.arc(sx-4,sy-8,3,0,6.3); ctx.arc(sx+4,sy-9,3,0,6.3); ctx.fill(); }
    else if(n.k==="seep"){ ctx.fillStyle="#1b1512"; ctx.beginPath(); ctx.ellipse(sx,sy,10,6,0,0,6.3); ctx.fill();
      ctx.fillStyle="rgba(224,161,65,.20)"; ctx.beginPath(); ctx.ellipse(sx-2,sy-1,4,2,0,0,6.3); ctx.fill(); }
    else if(n.k==="glass"){ const p=(Math.sin(performance.now()/420+n.ph)+1)/2;
      ctx.fillStyle="rgba(178,174,198,"+(.5+p*.5)+")"; ctx.beginPath();
      ctx.moveTo(sx,sy-10); ctx.lineTo(sx+6,sy); ctx.lineTo(sx,sy+8); ctx.lineTo(sx-6,sy); ctx.closePath(); ctx.fill(); }
    else if(n.k==="bones"){ ctx.fillStyle=d.c; ctx.fillRect(sx-9,sy,18,3);
      ctx.fillRect(sx-3,sy-7,4,12); ctx.beginPath(); ctx.arc(sx+8,sy-4,4,0,6.3); ctx.fill(); }
    else if(n.k==="wreck"){ ctx.fillStyle=d.c; ctx.save(); ctx.translate(sx,sy); ctx.rotate(n.ph*.4);
      ctx.fillRect(-10,-7,20,14); ctx.restore();
      ctx.strokeStyle="rgba(0,0,0,.3)"; ctx.strokeRect(sx-10,sy-7,20,14); }
    else { // stones, chips, vein-chunks
      ctx.beginPath(); ctx.arc(sx-4,sy+1,5,0,6.3); ctx.arc(sx+4,sy-2,6,0,6.3); ctx.fill();
      if(n.k==="chips"||n.k==="vein"){ ctx.fillStyle="#b06a36";
        ctx.beginPath(); ctx.arc(sx+4,sy-2,2.4,0,6.3); ctx.fill(); } }
    if(on){ ctx.strokeStyle="rgba(224,161,65,.55)"; ctx.lineWidth=1;
      ctx.beginPath(); ctx.arc(sx,sy,15,0,6.3); ctx.stroke(); }
  }
function drawNodes(){ for(const n of nodes) drawNode(n); }
function figure(o,tag,col,scale,key){
  const s=scale||1, sx=o.x-cam.x, sy=o.y-cam.y, r=o.r*s;
  ctx.fillStyle="rgba(10,16,18,.32)"; ctx.beginPath(); ctx.ellipse(sx,sy+r*.95,r*1.05,r*.42,0,0,6.3); ctx.fill();
  const bob=Math.sin((o.ph||0)*2)*1.2;
  if(key&&spr(key,sx,sy+r*1.05,s,undefined,0,o)){
    if(o.swing>0&&!hasClips(key)){ ctx.strokeStyle="rgba(233,226,207,.7)"; ctx.lineWidth=3; ctx.beginPath();
      ctx.arc(sx,sy,r*2.1,(o.face||0)-.7,(o.face||0)+.7); ctx.stroke(); }
    if(o.hurt>0){ ctx.fillStyle="rgba(156,63,34,"+o.hurt*2+")"; ctx.beginPath(); ctx.arc(sx,sy,r*1.4,0,6.3); ctx.fill(); }
    if(tag){ ctx.font="9px 'Courier New', monospace"; ctx.textAlign="center";
      ctx.fillStyle="rgba(233,226,207,.82)"; ctx.fillText(tag,sx,sy-r*2.2); ctx.textAlign="left"; }
    if(o.mhp&&o.hp<o.mhp){
      ctx.fillStyle="rgba(8,14,15,.8)"; ctx.fillRect(sx-16,sy-r*1.95,32,3);
      ctx.fillStyle=o.down?"#a24a2c":"#8fb07a"; ctx.fillRect(sx-16,sy-r*1.95,32*clamp(o.hp/o.mhp,0,1),3);
    }
    return;
  }
  ctx.fillStyle=col||o.c||"#54626b";
  ctx.beginPath();
  if(ctx.roundRect) ctx.roundRect(sx-r*.7,sy-r+bob,r*1.4,r*2,3); else ctx.rect(sx-r*.7,sy-r+bob,r*1.4,r*2);
  ctx.fill();
  ctx.fillStyle="#e6dcc6"; ctx.beginPath(); ctx.arc(sx,sy-r*1.05+bob,r*.5,0,6.3); ctx.fill();
  const f=o.face!==undefined?o.face:0;
  ctx.strokeStyle="rgba(20,24,26,.7)"; ctx.lineWidth=2; ctx.beginPath();
  ctx.moveTo(sx,sy-r*1.05+bob); ctx.lineTo(sx+Math.cos(f)*r*.75,sy-r*1.05+bob+Math.sin(f)*r*.75); ctx.stroke();
  if(o.swing>0){ ctx.strokeStyle="rgba(233,226,207,.7)"; ctx.lineWidth=3; ctx.beginPath();
    ctx.arc(sx,sy,r*2.1,f-.7,f+.7); ctx.stroke(); }
  if(o.hurt>0){ ctx.fillStyle="rgba(156,63,34,"+o.hurt*2+")"; ctx.beginPath(); ctx.arc(sx,sy,r*1.4,0,6.3); ctx.fill(); }
  if(tag){ ctx.font="9px 'Courier New', monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(233,226,207,.82)"; ctx.fillText(tag,sx,sy-r*2.2); ctx.textAlign="left"; }
  if(o.mhp&&o.hp<o.mhp){
    ctx.fillStyle="rgba(8,14,15,.8)"; ctx.fillRect(sx-16,sy-r*1.95,32,3);
    ctx.fillStyle=o.down?"#a24a2c":"#8fb07a"; ctx.fillRect(sx-16,sy-r*1.95,32*clamp(o.hp/o.mhp,0,1),3);
  }
}
function drawDown(m){
  const sx=m.x-cam.x, sy=m.y-cam.y;
  ctx.fillStyle="rgba(10,16,18,.30)"; ctx.beginPath(); ctx.ellipse(sx,sy+4,15,5,0,0,6.3); ctx.fill();
  if(sprC("down",sx,sy,1)){
    const p0=(Math.sin(performance.now()/220)+1)/2;
    ctx.fillStyle="rgba(156,63,34,"+(.25+p0*.35)+")"; ctx.beginPath(); ctx.arc(sx,sy,20,0,6.3); ctx.fill();
    ctx.font="9px 'Courier New', monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(224,161,65,.9)"; ctx.fillText(Math.ceil(m.bleed)+"s",sx,sy-20); ctx.textAlign="left";
    return;
  }
  ctx.fillStyle=m.c; ctx.beginPath(); ctx.ellipse(sx,sy,14,7,0,0,6.3); ctx.fill();
  ctx.fillStyle="#e6dcc6"; ctx.beginPath(); ctx.arc(sx-13,sy-2,5,0,6.3); ctx.fill();
  const p=(Math.sin(performance.now()/220)+1)/2;
  ctx.fillStyle="rgba(156,63,34,"+(.35+p*.45)+")"; ctx.beginPath(); ctx.arc(sx,sy,20,0,6.3); ctx.fill();
  ctx.font="9px 'Courier New', monospace"; ctx.textAlign="center";
  ctx.fillStyle="rgba(224,161,65,.9)"; ctx.fillText(Math.ceil(m.bleed)+"s",sx,sy-20); ctx.textAlign="left";
}
function drawFoe(f){
  const sx=f.x-cam.x, sy=f.y-cam.y, r=f.r;
  if(sx<-90||sx>VW+90||sy<-90||sy>VH+90) return;
  ctx.fillStyle="rgba(10,16,18,.34)"; ctx.beginPath(); ctx.ellipse(sx,sy+r*.9,r*1.1,r*.45,0,0,6.3); ctx.fill();
  const wob=Math.sin(f.t*(f.k==="wretch"?9:5))*2, big=f.k==="boss"||f.k==="mini";
  let drew=false;
  if(spr("foe_"+f.k,sx,sy+r*.95,1,f.hit>0?.75:1,0,f)) drew=true;
  ctx.fillStyle=f.hit>0?"#e9e2cf":f.c;
  if(drew){
    if(f.asleep&&big){ ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
      ctx.fillStyle="rgba(139,151,147,.75)"; ctx.fillText("still",sx,sy-r*2.9); ctx.textAlign="left"; }
  } else
  if(f.k==="drifter"){
    ctx.globalAlpha=.72;
    for(let i=0;i<3;i++){ ctx.beginPath();
      ctx.arc(sx+Math.sin(f.t*2+i)*5,sy-i*5,r-i*2,0,6.3); ctx.fill(); }
    ctx.globalAlpha=1;
  } else if(f.k==="kelp"){
    ctx.beginPath(); ctx.ellipse(sx,sy,r*1.3,r*.75,0,0,6.3); ctx.fill();
    ctx.strokeStyle=f.c; ctx.lineWidth=2;
    for(let i=-1;i<=1;i+=2){ ctx.beginPath(); ctx.moveTo(sx+i*7,sy+4); ctx.lineTo(sx+i*9,sy+11); ctx.stroke(); }
    ctx.fillStyle="#2a3f33"; ctx.beginPath(); ctx.arc(sx+Math.cos(f.face||0)*r,sy-2,r*.5,0,6.3); ctx.fill();
  } else if(f.k==="brine"){
    ctx.beginPath(); ctx.ellipse(sx,sy,r*1.15,r*.9,0,0,6.3); ctx.fill();
    ctx.fillStyle="rgba(190,230,225,.20)"; ctx.beginPath(); ctx.ellipse(sx-2,sy-4,r*.7,r*.45,0,0,6.3); ctx.fill();
    ctx.strokeStyle=f.c; ctx.lineWidth=3;
    for(let i=-1;i<=1;i++){ ctx.beginPath(); ctx.moveTo(sx+i*8,sy-r*.6);
      ctx.quadraticCurveTo(sx+i*16,sy-r*1.5,sx+i*9+Math.sin(f.t*2+i)*4,sy-r*1.9); ctx.stroke(); }
  } else if(f.k==="counter"){
    ctx.beginPath();
    if(ctx.roundRect) ctx.roundRect(sx-r*.42,sy-r*1.9+wob*.3,r*.84,r*2.5,3); else ctx.rect(sx-r*.42,sy-r*1.9,r*.84,r*2.5);
    ctx.fill();
    ctx.fillStyle="#0d1416"; ctx.beginPath(); ctx.arc(sx,sy-r*1.9+wob*.3,r*.4,0,6.3); ctx.fill();
    ctx.strokeStyle="rgba(224,216,189,.55)"; ctx.lineWidth=1;
    const cn=3+Math.floor((f.t*2)%5);
    for(let i=0;i<cn;i++){ ctx.beginPath(); ctx.moveTo(sx-9+i*5,sy-r*2.5); ctx.lineTo(sx-9+i*5,sy-r*2.9); ctx.stroke(); }
  } else if(f.k==="lampless"){
    ctx.beginPath(); ctx.ellipse(sx,sy-r*.4,r*.8,r*1.35,0,0,6.3); ctx.fill();
    ctx.fillStyle="rgba(10,8,14,.85)"; ctx.beginPath(); ctx.arc(sx,sy-r*1.4,r*.5,0,6.3); ctx.fill();
    ctx.strokeStyle="rgba(163,150,194,.4)"; ctx.lineWidth=1;
    ctx.beginPath(); ctx.arc(sx,sy-r*.6,r*1.6+Math.sin(f.t*3)*2,0,6.3); ctx.stroke();
  } else if(f.k==="rimeling"){
    ctx.beginPath(); ctx.moveTo(sx,sy-r*1.5); ctx.lineTo(sx+r*.8,sy-r*.2);
    ctx.lineTo(sx+r*.4,sy+r*.9); ctx.lineTo(sx-r*.4,sy+r*.9); ctx.lineTo(sx-r*.8,sy-r*.2);
    ctx.closePath(); ctx.fill();
    ctx.fillStyle="rgba(255,255,255,.3)"; ctx.beginPath();
    ctx.moveTo(sx,sy-r*1.5); ctx.lineTo(sx+r*.35,sy-r*.5); ctx.lineTo(sx-r*.35,sy-r*.5); ctx.closePath(); ctx.fill();
  } else if(f.k==="gaunt"){
    const hh=r*2.3;
    ctx.beginPath();
    if(ctx.roundRect) ctx.roundRect(sx-r*.4,sy-hh*.55+wob*.4,r*.8,hh,3); else ctx.rect(sx-r*.4,sy-hh*.55,r*.8,hh);
    ctx.fill();
    ctx.fillStyle="#0d1416"; ctx.beginPath(); ctx.arc(sx,sy-hh*.55+wob*.4,r*.38,0,6.3); ctx.fill();
    ctx.strokeStyle=f.c; ctx.lineWidth=2;
    ctx.beginPath(); ctx.moveTo(sx-r*.4,sy-hh*.2); ctx.lineTo(sx-r*1.5,sy-r*.3+Math.sin(f.t*7)*5); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(sx+r*.4,sy-hh*.2); ctx.lineTo(sx+r*1.5,sy-r*.3-Math.sin(f.t*7)*5); ctx.stroke();
  } else if(f.k==="crag"){
    ctx.beginPath(); ctx.moveTo(sx-r,sy+r*.8); ctx.lineTo(sx-r*.6,sy-r);
    ctx.lineTo(sx+r*.7,sy-r*.9); ctx.lineTo(sx+r,sy+r*.8); ctx.closePath(); ctx.fill();
    ctx.fillStyle="rgba(255,255,255,.08)"; ctx.fillRect(sx-r*.5,sy-r*.7,r*.8,r*.5);
  } else if(f.k==="floe"){
    ctx.beginPath(); ctx.moveTo(sx,sy-r*1.2); ctx.lineTo(sx+r,sy); ctx.lineTo(sx,sy+r);
    ctx.lineTo(sx-r,sy); ctx.closePath(); ctx.fill();
    ctx.fillStyle="rgba(255,255,255,.22)"; ctx.beginPath();
    ctx.moveTo(sx,sy-r*1.2); ctx.lineTo(sx+r*.5,sy-r*.2); ctx.lineTo(sx-r*.4,sy-r*.2); ctx.closePath(); ctx.fill();
  } else if(big){
    // a heavy body, and something on top of it
    ctx.beginPath(); ctx.ellipse(sx,sy+wob*.3,r*.95,r*1.15,0,0,6.3); ctx.fill();
    ctx.fillStyle="rgba(0,0,0,.25)"; ctx.beginPath(); ctx.ellipse(sx,sy+r*.4,r*.8,r*.5,0,0,6.3); ctx.fill();
    ctx.strokeStyle=f.hit>0?"#e9e2cf":"#cfc7ae"; ctx.lineWidth=2.4;
    const spread=f.k==="boss"?5:3;
    for(let i=0;i<spread;i++){
      const a=-1.9+i*(3.8/(spread-1||1));
      ctx.beginPath(); ctx.moveTo(sx,sy-r*.8);
      ctx.quadraticCurveTo(sx+Math.cos(a)*r*1.3,sy-r*1.9,sx+Math.cos(a)*r*1.9,sy-r*2.4-Math.sin(f.t)*2);
      ctx.stroke();
    }
    ctx.fillStyle="#0a1012"; ctx.beginPath(); ctx.arc(sx,sy-r*.5,r*.36,0,6.3); ctx.fill();
    if(f.asleep){ ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
      ctx.fillStyle="rgba(139,151,147,.75)"; ctx.fillText("still",sx,sy-r*2.9); ctx.textAlign="left"; }
  } else {
    // hollow / wretch / husk: standing shapes
    const h=f.k==="wretch"?r*1.5:f.k==="husk"?r*2.1:r*1.9;
    ctx.beginPath();
    if(ctx.roundRect) ctx.roundRect(sx-r*.62,sy-h*.55+wob*.3,r*1.24,h,3);
    else ctx.rect(sx-r*.62,sy-h*.55,r*1.24,h);
    ctx.fill();
    ctx.fillStyle="#0d1416"; ctx.beginPath(); ctx.arc(sx,sy-h*.55+wob*.3,r*.44,0,6.3); ctx.fill();
    ctx.strokeStyle=f.c; ctx.lineWidth=2;
    const sw=Math.sin(f.t*6)*4;
    ctx.beginPath(); ctx.moveTo(sx-r*.6,sy-h*.1); ctx.lineTo(sx-r*1.1,sy+r*.4+sw); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(sx+r*.6,sy-h*.1); ctx.lineTo(sx+r*1.1,sy+r*.4-sw); ctx.stroke();
  }
  if(f.froze>0){ ctx.fillStyle="rgba(180,220,235,.32)"; ctx.beginPath(); ctx.arc(sx,sy,r*1.5,0,6.3); ctx.fill();
    ctx.strokeStyle="rgba(220,240,250,.6)"; ctx.lineWidth=1; ctx.strokeRect(sx-r,sy-r*1.4,r*2,r*2.6); }
  if(f.stun>0){ ctx.font="11px Georgia,serif"; ctx.textAlign="center";
    ctx.fillStyle="rgba(199,191,168,.9)"; ctx.fillText("·  ·  ·",sx,sy-r*1.9); ctx.textAlign="left"; }
  if(f.burn>0){ for(let i=0;i<2;i++){ ctx.fillStyle="rgba(224,161,65,"+(.3+rnd()*.4)+")";
    ctx.beginPath(); ctx.arc(sx+(rnd()*r-r/2),sy-r+(rnd()*r-r/2),2.5,0,6.3); ctx.fill(); } }
  if(f.slow>0){ ctx.strokeStyle="rgba(216,180,120,.35)"; ctx.lineWidth=1;
    ctx.beginPath(); ctx.arc(sx,sy,r*1.7,0,6.3); ctx.stroke(); }
  if(f.hp<f.mhp&&!f.asleep&&!big){
    ctx.fillStyle="rgba(8,14,15,.8)"; ctx.fillRect(sx-16,sy-r*1.75,32,3);
    ctx.fillStyle="#c25334"; ctx.fillRect(sx-16,sy-r*1.75,32*clamp(f.hp/f.mhp,0,1),3);
  }
}
function drawSpirit(s){
  const sx=s.x-cam.x, sy=s.y-cam.y, t=performance.now()/1000;
  if(sx<-90||sx>VW+90||sy<-90||sy>VH+90) return;
  if(sprC("spirit",sx,sy-10,1,.72+Math.sin(t*1.6+s.ph)*.2,0,s)){
    ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(199,191,168,.8)"; ctx.fillText(s.done?"quiet":"regarding",sx,sy-46); ctx.textAlign="left";
    return;
  }
  ctx.globalAlpha=.55+Math.sin(t*1.6+s.ph)*.18;
  ctx.fillStyle="#a396c2";
  for(let i=0;i<4;i++){
    const a=t*.6+i*1.57, r=16+i*4;
    ctx.beginPath(); ctx.ellipse(sx+Math.cos(a)*6,sy-i*7+Math.sin(a)*4,r*.5,r*.32,a,0,6.3); ctx.fill();
  }
  ctx.globalAlpha=1;
  ctx.strokeStyle="rgba(163,150,194,.5)"; ctx.lineWidth=1;
  ctx.beginPath(); ctx.arc(sx,sy,34+Math.sin(t*2)*4,0,6.3); ctx.stroke();
  ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
  ctx.fillStyle="rgba(199,191,168,.8)"; ctx.fillText(s.done?"quiet":"regarding",sx,sy-42); ctx.textAlign="left";
}
function drawShots(){
  for(const s of shots){
    const sx=s.x-cam.x, sy=s.y-cam.y;
    if(sprC("shot_"+s.s,sx,sy,1,undefined,0,s)) continue;
    ctx.fillStyle=s.c;
    if(s.s==="mist"){ ctx.globalAlpha=.8; ctx.beginPath(); ctx.arc(sx,sy,6,0,6.3); ctx.fill(); ctx.globalAlpha=1; }
    else if(s.s==="shard"){ ctx.save(); ctx.translate(sx,sy); ctx.rotate(Math.atan2(s.vy,s.vx));
      ctx.beginPath(); ctx.moveTo(7,0); ctx.lineTo(-5,4); ctx.lineTo(-5,-4); ctx.closePath(); ctx.fill(); ctx.restore(); }
    else { const r=s.s==="boulder"?8:5; ctx.beginPath(); ctx.arc(sx,sy,r,0,6.3); ctx.fill();
      ctx.fillStyle="rgba(0,0,0,.25)"; ctx.beginPath(); ctx.arc(sx+1,sy+1,r*.5,0,6.3); ctx.fill(); }
    if(rnd()<.4) parts.push({x:s.x,y:s.y,vx:0,vy:0,life:.18,c:s.c,s:1.5});
  }
}
function drawZones(){
  for(const z of zones){
    const f=clamp(z.life/z.max,0,1), sx=(z.follow?z.follow.x:z.x)-cam.x, sy=(z.follow?z.follow.y:z.y)-cam.y;
    ctx.globalAlpha=f;
    if(z.k==="tell"||z.k==="arms"){
      const seg=6.283/z.arms;
      const telling=z.k==="tell";
      for(let i=0;i<z.arms;i++){
        const a0=z.rot+i*seg, a1=a0+z.wedge;
        ctx.globalAlpha=telling?(.10+(1-f)*.26):(f*.55);
        ctx.fillStyle=z.c;
        ctx.beginPath(); ctx.moveTo(sx,sy);
        ctx.arc(sx,sy,z.r*(telling?1:(1-f*.25)),a0,a1); ctx.closePath(); ctx.fill();
        ctx.globalAlpha=telling?.5:f;
        ctx.strokeStyle=z.c; ctx.lineWidth=telling?1:3;
        ctx.beginPath(); ctx.moveTo(sx,sy); ctx.arc(sx,sy,z.r,a0,a1); ctx.closePath(); ctx.stroke();
      }
      ctx.globalAlpha=1;
      continue;
    }
    if(z.k==="ring"){ ctx.strokeStyle=z.c; ctx.lineWidth=3+f*4;
      ctx.beginPath(); ctx.arc(sx,sy,z.r*(1-f*.35),0,6.3); ctx.stroke(); }
    else if(z.k==="cone"){ ctx.fillStyle=z.c; ctx.globalAlpha=f*.4;
      ctx.beginPath(); ctx.moveTo(sx,sy);
      ctx.arc(sx,sy,z.len,z.a-.72,z.a+.72); ctx.closePath(); ctx.fill(); }
    else if(z.k==="bolt"||z.k==="cut"){ ctx.strokeStyle=z.c; ctx.lineWidth=z.k==="cut"?7*f:3;
      ctx.beginPath(); ctx.moveTo(z.x1-cam.x,z.y1-cam.y); ctx.lineTo(z.x2-cam.x,z.y2-cam.y); ctx.stroke(); }
    else if(z.k==="puff"){ ctx.strokeStyle=z.c; ctx.lineWidth=2;
      ctx.beginPath(); ctx.arc(sx,sy,26*(1-f),0,6.3); ctx.stroke(); }
    else if(z.k==="burst"){ ctx.fillStyle=z.c; ctx.globalAlpha=f*.6;
      ctx.beginPath(); ctx.arc(sx,sy,40*(1-f)+8,0,6.3); ctx.fill(); }
    else if(z.k==="fire"){ ctx.fillStyle="rgba(156,63,34,.18)";
      ctx.beginPath(); ctx.arc(sx,sy,z.r,0,6.3); ctx.fill();
      for(let i=0;i<2;i++){ ctx.fillStyle="rgba(224,161,65,"+(.2+rnd()*.3)+")";
        ctx.beginPath(); ctx.arc(sx+(rnd()*z.r-z.r/2),sy+(rnd()*z.r-z.r/2),3,0,6.3); ctx.fill(); } }
    else if(z.k==="lamp"){
      const g=ctx.createRadialGradient(sx,sy,4,sx,sy,z.r);
      g.addColorStop(0,"rgba(232,215,154,.42)"); g.addColorStop(1,"rgba(232,215,154,0)");
      ctx.fillStyle=g; ctx.beginPath(); ctx.arc(sx,sy,z.r,0,6.3); ctx.fill();
      ctx.strokeStyle="rgba(232,215,154,.5)"; ctx.lineWidth=1;
      ctx.beginPath(); ctx.arc(sx,sy,z.r,0,6.3); ctx.stroke();
    }
    else if(z.k==="mark"){ ctx.strokeStyle=z.c; ctx.lineWidth=2; ctx.globalAlpha=.6;
      const p=(Math.sin(performance.now()/300)+1)/2;
      ctx.beginPath(); ctx.arc(sx,sy,14+p*6,0,6.3); ctx.stroke();
      ctx.beginPath(); ctx.arc(sx,sy,4,0,6.3); ctx.stroke(); }
    ctx.globalAlpha=1;
  }
}
function drawCorpses(){
  for(const c of corpses){
    const t=c.t/c.max, sx=c.x-cam.x, sy=c.y-cam.y, e=1-Math.pow(1-t,2);
    ctx.globalAlpha=1-t;
    ctx.fillStyle=c.k==="person"?"#8b9793":"#cfc7ae";
    ctx.beginPath(); ctx.ellipse(sx,sy+c.r*e*.8,c.r*(1+e*.7),c.r*(1-e*.7),0,0,6.3); ctx.fill();
    ctx.strokeStyle="rgba(199,191,168,"+(1-t)*.7+")"; ctx.lineWidth=3*(1-t);
    ctx.beginPath(); ctx.arc(sx,sy,c.r+e*(c.k==="boss"?200:c.k==="mini"?110:48),0,6.3); ctx.stroke();
    if(c.k==="boss"){ ctx.strokeStyle="rgba(156,63,34,"+(1-t)+")"; ctx.lineWidth=6*(1-t);
      ctx.beginPath(); ctx.arc(sx,sy,e*300,0,6.3); ctx.stroke(); }
    ctx.globalAlpha=1;
  }
}
function render(){
  if(window.ART){
    const w={snow:.8,outcrop:.7,mountain:.55,forest:.3,town:.35}[ISL.theme];
    window.ART.wind=(w===undefined?.4:w)+(ISL.threat>=2?.15:0);
  }
  ctx.save();
  if(G.shake>0) ctx.translate((rnd()-.5)*G.shake,(rnd()-.5)*G.shake);
  drawWorld();
  drawGates();
  drawCorpses();
  drawZones();
  drawShots();
  if(ISL.spirit) drawSpirit(ISL.spirit);
  // everything with a foot on the ground, back to front: people, foes, and the
  // tall furniture of the island, so a person can stand behind a bush properly
  const ppl=[];
  for(const q of tallQ) ppl.push({o:{y:q.y},k:"tall",q});
  for(const p of props) ppl.push({o:{y:p.y+14},k:"prop",it:p});
  for(const b of builds) ppl.push({o:{y:b.y+10},k:"build",it:b});
  for(const n of nodes) ppl.push({o:{y:n.y+10},k:"node",it:n});
  for(const t of townies) ppl.push({o:t,k:"t"});
  for(const c of neutrals) ppl.push({o:c,k:"c"});
  for(const m of party) ppl.push({o:m,k:"p"});
  for(const f of foes) ppl.push({o:f,k:"f"});
  for(const a of animals) ppl.push({o:a,k:"a"});
  ppl.push({o:player,k:"me"});
  ppl.sort((a,b)=>a.o.y-b.o.y);
  for(const e of ppl){
    if(e.k==="tall"){
      const q=e.q;
      if(q.im) ctx.drawImage(q.im,q.dx,q.dy,q.w,q.h);
      else drawTallScen(q.scen,q.tx,q.ty,q.sx,q.sy);
    }
    else if(e.k==="prop") drawProp(e.it);
    else if(e.k==="build") drawBuild(e.it);
    else if(e.k==="node") drawNode(e.it);
    else if(e.k==="f") drawFoe(e.o);
    else if(e.k==="a") drawAnimal(e.o);
    else if(e.k==="me"){
      ctx.globalAlpha=player.inv>0?.62:1;
      figure(player,null,"#7c5a3c",1.05,"player");
      ctx.globalAlpha=1;
      if(player.swing>0&&!hasClips("player")){    // a drawn attack clip does this itself
        ctx.strokeStyle="rgba(233,226,207,.8)"; ctx.lineWidth=3;
        ctx.beginPath(); ctx.arc(player.x-cam.x,player.y-cam.y,player.reach*.72,player.aim-.8,player.aim+.8); ctx.stroke(); }
      if(G.coal>0){ ctx.strokeStyle="rgba(224,161,65,.30)"; ctx.lineWidth=2;
        const kg2=hasPas("kettle");
        ctx.beginPath(); ctx.arc(player.x-cam.x,player.y-cam.y,(kg2&&kg2.rank>=2)?96:78,0,6.3); ctx.stroke(); }
    }
    else if(e.o.cnpc&&e.o.cnpc.sign) drawSign(e.o);
    else if(e.o.down) drawDown(e.o);
    else {
      const o=e.o;
      const hl=nearest&&nearest.o===o;
      figure(o,hl?o.n.split(" ")[0]:null,o.c,1,e.k==="p"?"ally":e.k==="t"?"townie":"castaway");
      if(e.k==="c"&&!hl){ ctx.font="9px 'Courier New',monospace"; ctx.textAlign="center";
        ctx.fillStyle="rgba(127,143,109,.8)"; ctx.fillText("?",o.x-cam.x,o.y-o.r*2.4-cam.y); ctx.textAlign="left"; }
    }
  }
  // particles
  for(const p of parts){ ctx.globalAlpha=clamp(p.life*2,0,1); ctx.fillStyle=p.c;
    ctx.fillRect(p.x-cam.x,p.y-cam.y,p.s,p.s); }
  ctx.globalAlpha=1;
  ctx.restore();
  // theme tint
  ctx.fillStyle=G.tint; ctx.fillRect(0,0,VW,VH);
  // vignette
  const vg=ctx.createRadialGradient(VW/2,VH/2,VH*.42,VW/2,VH/2,VH*.92);
  vg.addColorStop(0,"rgba(0,0,0,0)"); vg.addColorStop(1,"rgba(0,0,0,.55)");
  ctx.fillStyle=vg; ctx.fillRect(0,0,VW,VH);
  if(G.hurtT>0){ ctx.fillStyle="rgba(156,63,34,"+G.hurtT*.5+")"; ctx.fillRect(0,0,VW,VH); }
  if(player.chill>0){ ctx.fillStyle="rgba(150,200,220,"+Math.min(.22,player.chill*.14)+")"; ctx.fillRect(0,0,VW,VH); }
  if(G.mode==="play"&&isPaused()){ ctx.fillStyle="rgba(5,8,10,.55)"; ctx.fillRect(0,0,VW,VH); }
  // boss bar
  const big=foes.find(f=>(f.k==="boss"||f.k==="mini")&&!f.asleep);
  if(big){
    const w=VW*.46, x=(VW-w)/2, y=VH-34;
    ctx.fillStyle="rgba(8,14,15,.85)"; ctx.fillRect(x-1,y-1,w+2,10);
    ctx.fillStyle=big.k==="boss"?"#9c3f22":"#b07a2c";
    ctx.fillRect(x,y,w*clamp(big.hp/big.mhp,0,1),8);
    ctx.font="10px 'Courier New',monospace"; ctx.textAlign="center";
    ctx.fillStyle="rgba(233,226,207,.85)";
    ctx.fillText(big.n.toUpperCase(),VW/2,y-6); ctx.textAlign="left";
  }
}

/* ============================================================ BOOT */
function beginGame(){
  $("title").classList.remove("on");
  interQ=[]; interOn=false; interThen=null; $("inter").classList.remove("on");
  closePanels(); $("dbox").classList.remove("on"); G.mode="play";
  G.name=pick(FIRST)+" "+pick(LAST);
  G.trade=pick(TRADES);
  G.mhp=100; G.hp=100; G.mvg=60; G.vg=60;
  if(G.trade.b==="body"){ G.mhp=122; G.hp=122; }
  if(G.trade.b==="arm") player.dmg+=2;
  packInit(); give("herb",1); give("bandage",1);
  G.isl=0; G.kills=0; G.mist=0; G.regards=[]; G.lampT=0;
  G.tools={}; G.plans={}; G.craft=null; G.log=[]; G.deaths=[]; G.dug={}; G.wrecks=0;
  G.cache=[]; G.home=[]; G.mistTotal=0; G.halt=false; G.spill=0; G.regards=[];
  G.shadow=0; G.nanookT=0; G.zeal=0;
  G.clock=0; G.broken=0; G.elites=0; G.tamed=0; G.bred=0;
  G.bossKills=0; G.spiritsMet=0; G.rescued=0; G.owed={}; G.coal=0; G.hitstop=0;
  G.brokenRouteShown=false;
  HOME=null; builds=[]; animals=[]; gates=[];
  G.cityState={}; G.citiesSeen={}; G.ports={};
  player.armor=0; player.dmg=15; player.reach=46; player.dash=0; player.dashcd=0; player.chill=0;
  player.spdMul=1; player.takeMul=1; player.dashCost=1; player.armorOff=0; player.tolT=0; packSel=-1;
  logit("ashore",`${G.name}, ${G.trade.n}. ${G.trade.d}`);
  newIsland(rollTheme(true),0);
  interlude("Ashore","You are "+G.name,
    `${G.trade.d} None of that is much use to you now.<br><br>
     The ship is not on the water any more and neither is anybody you sailed with. There is sand, there is fog,
     and there is an island behind the fog with something on it.<br><br>
     <i>Gather. Build. Get off it before it decides about you.</i>`,
    ()=>{ syncHUD(); });
}
$("start").onclick=beginGame;
$("again").onclick=()=>{ $("ending").classList.remove("on"); beginGame(); };

let last=performance.now();
function loop(now){
  let dt=Math.min(.05,(now-last)/1000); last=now;
  if(G.hitstop>0){ G.hitstop-=dt; dt*=.16; }
  if(G.mode!=="title") update(dt);
  render();
  requestAnimationFrame(loop);
}
/* ============================================================ THE CROSSING
   Two players, two houses, one sea. No server: the host's game is the real
   one, the guest steers the second castaway in it. The two browsers speak
   WebRTC straight across; the connection is made by trading two pasted codes.
   Transport is abstracted so a test can wire two games together with a pipe. */
const PL1=player;
const NET={
  on:false, role:"solo", host:false, ch:null, pc:null,
  me:null,                 // guest: the entity I steer (my copy of the second castaway)
  p2:null,                 // host: the second castaway in the live party
  rin:{mx:0,my:0,aim:0},   // host: the guest's held input
  ev:[],                   // host: edge-triggered presses from the guest
  aim:0, nidc:1, snapT:0, inT:0, dlgFor:0,
  pend:null, pendEnd:null, gd:null, worldReady:false,
  send(o){ try{ if(NET.ch&&NET.ch.readyState==="open") NET.ch.send(JSON.stringify(o)); }catch(err){} },
  attach(fn){ NET.ch={readyState:"open",send:fn}; },   // for tests: a bare pipe
};

/* ---- small helpers ---- */
function b64grid(){
  let out="",i=0; const n=GW*GH;
  while(i<n){ out+=String.fromCharCode.apply(null,grid.subarray(i,Math.min(n,i+4096))); i+=4096; }
  return btoa(out);
}
function unb64grid(b){
  const raw=atob(b), a=new Uint8Array(raw.length);
  for(let i=0;i<raw.length;i++) a[i]=raw.charCodeAt(i);
  return a;
}
function r1(v){ return Math.round(v*10)/10; }

/* ---- the second castaway (host side) ---- */
function netSpawnP2(){
  if(NET.p2) return NET.p2;
  const m=mkCastaway(player.x+26,player.y+10);
  m.p2=true; m.n="the second castaway"; m.role="steered from the other house";
  m.joined=true; m.g=null; m.down=0;
  m.mhp=G.mhp; m.hp=m.mhp; m.dmg=15; m.reach=46; m.sp=142; m.aim=0;
  m.vg=60; m.mvg=60; m.cd=0; m.roll=0; m.rollDir=0; m.dashcd=0; m.inv=0; m.swing=0;
  m.nid=1;
  if(party.indexOf(m)<0) party.push(m);
  NET.p2=m;
  return m;
}

/* ---- host: steer, swing, dash, reach for things ---- */
function p2melee(){
  const m=NET.p2; if(!m||m.down||m.cd>0||m.roll>0) return;
  const winded=m.vg<4; m.vg=Math.max(0,m.vg-Math.min(m.vg,4));
  m.cd=winded?.5:.32; m.swing=.18;
  sfx(160,.06,"square",.09);
  let reach=m.reach, dmg=m.dmg;
  if(m.g&&m.g.R.id==="nanook"){ reach+=10+3*m.g.rank; dmg+=3+2*m.g.rank; }
  if(winded) dmg=Math.round(dmg*.5);
  let hit=false;
  for(const f of foes){
    const d=dist(m.x,m.y,f.x,f.y); if(d>reach+f.r) continue;
    let a=Math.atan2(f.y-m.y,f.x-m.x)-m.aim; a=Math.atan2(Math.sin(a),Math.cos(a));
    if(Math.abs(a)<1.15){ hurtFoe(f,dmg*dmgMul(f),5); hit=true; }
  }
  for(const a of animals){
    if(a.pet) continue;
    const d=dist(m.x,m.y,a.x,a.y); if(d>reach+a.r) continue;
    let ang=Math.atan2(a.y-m.y,a.x-m.x)-m.aim; ang=Math.atan2(Math.sin(ang),Math.cos(ang));
    if(Math.abs(ang)<1.15){ hurtAnimal(a,dmg); hit=true; }
  }
  if(hit) G.hitstop=.045;
}
function p2dodge(){
  const m=NET.p2; if(!m||m.down||m.roll>0||m.dashcd>0) return;
  if(m.vg<8){ NET.send({e:"toast",h:"no vigor for it",t:"A dash costs 8."}); return; }
  m.vg-=8; m.roll=.3; m.inv=.36; m.dashcd=.42;
  m.rollDir=(NET.rin.mx||NET.rin.my)?Math.atan2(NET.rin.my,NET.rin.mx):m.aim;
}
function p2abil(){
  const m=NET.p2; if(!m||m.down) return;
  if(!m.g){ NET.send({e:"toast",h:"nothing regards you yet",t:"When one is offered, the captain can pass it your way."}); return; }
  if(m.vg<14){ NET.send({e:"toast",h:"no vigor for it",t:"It wants 14."}); return; }
  m.vg-=14;
  try{ AB[m.g.R.id](m,m.g,.8); }catch(err){}
  NET.send({e:"toast",h:m.g.R.act.n,t:"—"});
}
function p2use(i){
  const m=NET.p2; if(!m) return;
  const sl=G.pack[i]; if(!sl) return;
  const id=sl.id;
  const heal={berry:8,fish:14,roast:26,bandage:35,tonic:40}[id];
  if(heal!==undefined){
    take(id,1);
    m.hp=Math.min(m.mhp,m.hp+heal);
    if(id==="tonic") m.vg=Math.min(m.mvg,m.vg+30);
    NET.send({e:"toast",h:ITEMS[id].n,t:"Better."});
  } else NET.send({e:"toast",h:ITEMS[id].n,t:"Only the captain can use that."});
}
function netInteractP2(){
  const m=NET.p2; if(!m||m.down) return;
  const n=findNearest(m);
  if(!n) return;
  NET.dlgFor=1;
  try{
    if(n.k==="node") gather(n.o);
    else if(n.k==="cast") talkCast(n.o);
    else if(n.k==="town") talkTown(n.o);
    else if(n.k==="spirit") talkSpirit(n.o);
    else if(n.k==="down") helpUp(n.o);
    else if(n.k==="grave") digGrave(n.o);
    else if(n.k==="clear") clearTile(n.o);
    else if(n.k==="gate") gateTalk(n.o);
    else if(n.k==="build") buildTalk(n.o);
    else if(n.k==="animal") animalTalk(n.o);
    else if(n.k==="camp") NET.send({e:"panel",p:"cachep"});
    else if(n.k==="sail") NET.send({e:"toast",h:"the crossing",t:"The captain weighs anchor. Stand by the craft together."});
  } finally { NET.dlgFor=0; }
}

/* ---- host: the frame ---- */
function netHostTick(dt){
  if(!NET.host) return;
  const m=NET.p2; if(!m) return;
  /* consume the guest's presses */
  while(NET.ev.length){
    const e=NET.ev.shift();
    if(e==="atk") p2melee();
    else if(e==="dash") p2dodge();
    else if(e==="ab") p2abil();
    else if(e==="E") interact(true);
    else if(typeof e==="number") p2use(e);
  }
  m.aim=NET.rin.aim||m.aim;
  m.cd=Math.max(0,m.cd-dt); m.swing=Math.max(0,m.swing-dt);
  m.roll=Math.max(0,m.roll-dt); m.inv=Math.max(0,m.inv-dt);
  m.dashcd=Math.max(0,m.dashcd-dt);
  m.vg=Math.min(m.mvg,m.vg+7*dt);
  if(!m.down){
    let mx=NET.rin.mx,my=NET.rin.my,sp=m.sp;
    if(slowAt(m.x,m.y)) sp*=.62;
    if(m.roll>0){ mx=Math.cos(m.rollDir); my=Math.sin(m.rollDir); sp=370; }
    else if(mx||my){ const h=Math.hypot(mx,my); mx/=h; my/=h; }
    if(mx||my) moveEnt(m,mx,my,sp,dt);
  }
  /* snapshots go out at ~15 a second */
  NET.snapT-=dt;
  if(NET.snapT<=0){ NET.snapT=1/15; netSnap(); }
}
function netSnap(){
  for(const f of foes) if(!f.nid) f.nid=++NET.nidc;
  for(const a of animals) if(!a.nid) a.nid=++NET.nidc;
  for(const c of party) if(!c.nid) c.nid=++NET.nidc;
  for(const c of neutrals) if(!c.nid) c.nid=++NET.nidc;
  for(const t of townies) if(!t.nid) t.nid=++NET.nidc;
  const m=NET.p2;
  NET.send({s:1,
    G:{hp:G.hp,mhp:G.mhp,mist:G.mist,halt:G.halt?1:0},
    P:[r1(player.x),r1(player.y),r1(player.aim),r1(player.roll),r1(player.swing),r1(player.inv)],
    Q:m?{x:r1(m.x),y:r1(m.y),hp:Math.round(m.hp),mhp:m.mhp,vg:Math.round(m.vg),mvg:m.mvg,
         down:m.down?1:0,roll:r1(m.roll),g:m.g?m.g.R.id:null,rank:m.g?m.g.rank:0}:null,
    F:foes.map(f=>[f.nid,f.k,r1(f.x),r1(f.y),Math.round(f.hp),f.mhp,f.r,f.hit>0?1:0,f.asleep?1:0]),
    A:animals.map(a=>[a.nid,a.k,r1(a.x),r1(a.y),Math.round(a.hp),a.mhp,a.pet?1:0,a.young?1:0]),
    M:party.filter(c=>!c.p2).map(c=>[c.nid,r1(c.x),r1(c.y),Math.round(c.hp),c.mhp,c.down?1:0,c.n]),
    C:neutrals.map(c=>[c.nid,r1(c.x),r1(c.y),c.down?1:0,c.n,c.role||""]),
    T:townies.map(t=>[t.nid,r1(t.x),r1(t.y),t.n]),
    S:shots.map(sh=>[r1(sh.x),r1(sh.y),sh.c,sh.s||"dot",sh.mine?1:0]),
    Z:zones.map(z=>[z.k,r1(z.x),r1(z.y),z.r||0,z.arms||0,r1(z.rot||0),r1(z.wedge||0),r1(z.life),r1(z.max),z.c]),
    N:nodes.map(nd=>[nd.tx,nd.ty,nd.k,nd.uses]),
    pack:G.pack, tools:Object.keys(G.tools),
  });
}
function netWorld(){
  if(!NET.host||!NET.on||!NET.runOn) return;
  NET.worldReady=true;
  NET.send({e:"world",gw:GW,gh:GH,grid:b64grid(),
    theme:ISL.theme,name:ISL.name,sub:ISL.sub||"",threat:ISL.threat,tint:G.tint,
    home:ISL.home?1:0,city:ISL.city||null,
    N:nodes.map(nd=>[nd.tx,nd.ty,nd.k,nd.uses]),
    B:builds.map(b=>[b.k,r1(b.x),r1(b.y),b.seed||null,b.ripe?1:0]),
    R:props.map(pp=>[pp.k,r1(pp.x),r1(pp.y)]),
    GA:gates.map(g=>[r1(g.x),r1(g.y),(g.def&&g.def.label)||""]),
    sx:r1(player.x),sy:r1(player.y)});
  netSpawnP2();
}

/* ---- dialogue across the water ---- */
function netSendDlg(name,sub,lines,opts,onEnd){
  NET.pend=opts||null; NET.pendEnd=onEnd||null;
  NET.send({e:"dlg",name,sub:sub||"",lines,
    opts:opts?opts.map(o=>({t:o.t,off:o.off?1:0})):null});
}
function netShowDlg(d){
  NET.gd={lines:d.lines,i:0,opts:d.opts};
  $("dname").innerHTML=`${d.name}${d.sub?` <b>${d.sub}</b>`:""}`;
  $("dbox").classList.add("on");
  netDlgLine();
}
function netDlgLine(){
  const gd=NET.gd; if(!gd) return;
  $("dtext").innerHTML=gd.lines[gd.i]||"";
  const last=gd.i>=gd.lines.length-1;
  $("dopts").classList.remove("on"); $("dopts").innerHTML="";
  $("dmore").textContent=last?(gd.opts?"— pick one —":ADVW()):ADVW();
  if(last&&gd.opts){
    $("dopts").classList.add("on");
    gd.opts.forEach((o,i)=>{
      const b=document.createElement("button");
      b.innerHTML=o.t; b.disabled=!!o.off;
      b.onclick=()=>{ if(o.off) return; netDlgClose(); NET.send({e:"opt",i}); };
      $("dopts").appendChild(b);
    });
  }
}
function netDlgAdvance(){
  const gd=NET.gd; if(!gd) return;
  if(gd.i<gd.lines.length-1){ gd.i++; netDlgLine(); return; }
  if(gd.opts) return;
  netDlgClose(); NET.send({e:"dend"});
}
function netDlgClose(){
  NET.gd=null;
  $("dbox").classList.remove("on"); $("dopts").classList.remove("on");
}

/* ---- pause, both sides of the water ---- */
function netPause(){
  if(NET.role==="guest"){ NET.send({e:"p"}); return; }
  G.halt=!G.halt;
  if(G.halt) toast("paused","Press P again."+(NET.on?" It holds for both of you.":""));
  if(NET.on) NET.send({e:"pause",on:G.halt?1:0});
}

/* ---- guest: input ---- */
function netPress(k){ NET.pressQ=NET.pressQ||[]; NET.pressQ.push(k); }
function netGuestKey(e){
  if(NET.gd){
    if(e.code==="KeyE"||e.code==="Enter"||e.code==="Space"){ netDlgAdvance(); return true; }
    if(e.code==="Escape"){ if(!NET.gd.opts){ netDlgClose(); NET.send({e:"dend"}); } return true; }
    return true;
  }
  if(e.code==="KeyE"){ if(!anyPanel()) netPress("E"); else closePanels(); return true; }
  if(e.code==="Space"){ netPress("dash"); return true; }
  if(e.code==="KeyQ"||e.code==="KeyF"){ netPress("ab"); return true; }
  if(e.code==="KeyH"){ netPress("E"); return true; }
  if(e.code.startsWith("Digit")){ const i=+e.code.slice(5)-1; if(i>=0&&i<8) netPress(i); return true; }
  return false;   // panels, movement keys, escape fall through
}

/* ---- guest: the frame ---- */
function netGuestFrame(dt){
  const me=NET.me;
  $("paused").classList[G.halt&&NET.worldReady?"add":"remove"]("on");
  if(!NET.worldReady||!me) return;
  if(!G.halt&&!me.down){
    let mx=(K.KeyD||K.ArrowRight?1:0)-(K.KeyA||K.ArrowLeft?1:0);
    let my=(K.KeyS||K.ArrowDown?1:0)-(K.KeyW||K.ArrowUp?1:0);
    if(!mx&&!my&&(TC.mx||TC.my)){ mx=TC.mx; my=TC.my; }
    NET.rin.mx=mx; NET.rin.my=my;
    let sp=me.sp*(slowAt(me.x,me.y)?.62:1);
    if(me.roll>0) sp=370;
    if(mx||my){ const h=Math.hypot(mx,my); moveEnt(me,mx/h,my/h,sp,dt); }
  }
  /* the host's word on where I am, folded in gently */
  if(NET.q){
    const k=Math.min(1,dt*(dist(me.x,me.y,NET.q.x,NET.q.y)>90?20:4));
    me.x+=(NET.q.x-me.x)*k; me.y+=(NET.q.y-me.y)*k;
    me.hp=NET.q.hp; me.mhp=NET.q.mhp; me.down=NET.q.down; me.roll=NET.q.roll;
    G.hp=NET.q.hp; G.mhp=NET.q.mhp; G.vg=NET.q.vg; G.mvg=NET.q.mvg;
  }
  /* everything else drifts toward the last snapshot */
  for(const f of foes){ if(f.__tx!==undefined){ const k=Math.min(1,dt*10);
    f.x+=(f.__tx-f.x)*k; f.y+=(f.__ty-f.y)*k; } f.t=(f.t||0)+dt; f.hit=Math.max(0,(f.hit||0)-dt); }
  for(const a of animals){ if(a.__tx!==undefined){ const k=Math.min(1,dt*10);
    a.x+=(a.__tx-a.x)*k; a.y+=(a.__ty-a.y)*k; } a.t=(a.t||0)+dt; }
  for(const c of party){ if(c.__tx!==undefined){ const k=Math.min(1,dt*10);
    c.x+=(c.__tx-c.x)*k; c.y+=(c.__ty-c.y)*k; } }
  for(const z of zones){ z.life-=dt; }
  zones=zones.filter(z=>z.life>0);
  for(const pp of parts){ pp.life-=dt; pp.x+=pp.vx*dt; pp.y+=pp.vy*dt; }
  parts=parts.filter(pp=>pp.life>0);
  /* my own camera */
  cam.x=me.x-VW/2; cam.y=me.y-VH/2;
  const mw=GW*TILE,mh=GH*TILE;
  if(mw>VW) cam.x=Math.max(0,Math.min(mw-VW,cam.x)); else cam.x=(mw-VW)/2;
  if(mh>VH) cam.y=Math.max(0,Math.min(mh-VH,cam.y)); else cam.y=(mh-VH)/2;
  nearest=findNearest(me);
  prompt=nearest?nearest.lab:"";
  $("prompt").style.opacity=prompt?1:0;
  if(prompt) $("prompt").textContent="E — "+prompt;
  /* held input goes across at ~30 a second */
  NET.inT-=dt;
  if(NET.inT<=0){
    NET.inT=1/30;
    const pq=NET.pressQ||[]; NET.pressQ=[];
    NET.send({i:[NET.rin.mx,NET.rin.my,Math.round((NET.aim||0)*100)/100],q:pq});
  }
  hudT=(hudT||0)+dt; if(hudT>.25){ hudT=0; syncHUD(); }
}

/* ---- message handling, both roles ---- */
function netMsg(str){
  let d; try{ d=JSON.parse(str); }catch(err){ return; }
  if(NET.host){
    if(d.i){ NET.rin.mx=d.i[0]; NET.rin.my=d.i[1]; NET.rin.aim=d.i[2];
      if(d.q) for(const k of d.q) NET.ev.push(k); }
    else if(d.e==="opt"){ const o=NET.pend&&NET.pend[d.i]; NET.pend=null;
      const f=NET.pendEnd; NET.pendEnd=null; if(o&&!o.off&&o.fn) o.fn(); else if(f) f(); }
    else if(d.e==="dend"){ NET.pend=null; const f=NET.pendEnd; NET.pendEnd=null; if(f) f(); }
    else if(d.e==="p") netPause();
    else if(d.e==="hello"){ netWorld(); }
    return;
  }
  /* guest */
  if(d.s){
    if(d.G){ G.mist=d.G.mist; G.halt=!!d.G.halt; }
    player.x=d.P[0]; player.y=d.P[1]; player.aim=d.P[2];
    player.roll=d.P[3]; player.swing=d.P[4]; player.inv=d.P[5];
    NET.q=d.Q;
    syncByNid(foes,d.F,(f,r)=>{ f.k=r[1]; f.__tx=r[2]; f.__ty=r[3]; f.hp=r[4]; f.mhp=r[5]; f.r=r[6];
      if(r[7]) f.hit=.14; f.asleep=!!r[8];
      if(f.x===undefined){ f.x=r[2]; f.y=r[3]; f.t=rnd()*9; f.ph=rnd()*6; f.face=1; f.n=FOES[f.k]?FOES[f.k].n:f.k; } });
    syncByNid(animals,d.A,(a,r)=>{ a.k=r[1]; a.__tx=r[2]; a.__ty=r[3]; a.hp=r[4]; a.mhp=r[5]; a.pet=!!r[6]; a.young=!!r[7];
      if(a.x===undefined){ a.x=r[2]; a.y=r[3]; a.t=rnd()*9; a.ph=rnd()*6; a.face=1; a.n=ANIMALS[a.k]?ANIMALS[a.k].n:a.k; a.r=(ANIMALS[a.k]&&ANIMALS[a.k].r)||10; } });
    syncByNid(party,d.M,(c,r)=>{ c.__tx=r[1]; c.__ty=r[2]; c.hp=r[3]; c.mhp=r[4]; c.down=!!r[5]; c.n=r[6];
      if(c.x===undefined){ c.x=r[1]; c.y=r[2]; c.r=11; c.aim=0; c.role=""; c.joined=true; } },true);
    syncByNid(neutrals,d.C,(c,r)=>{ c.x=r[1]; c.y=r[2]; c.down=!!r[3]; c.n=r[4]; c.role=r[5]; c.r=11; });
    syncByNid(townies,d.T,(t,r)=>{ t.x=r[1]; t.y=r[2]; t.n=r[3]; t.r=11; });
    shots=d.S.map(rr=>({x:rr[0],y:rr[1],vx:0,vy:0,c:rr[2],s:rr[3],mine:rr[4]}));
    zones=d.Z.map(rr=>({k:rr[0],x:rr[1],y:rr[2],r:rr[3],arms:rr[4],rot:rr[5],wedge:rr[6],life:rr[7],max:rr[8],c:rr[9]}));
    if(d.N){
      const ph={}; for(const nd of nodes) ph[nd.tx+":"+nd.ty]=nd.ph;
      nodes=d.N.map(rr=>{ const q=tc(rr[0],rr[1]);
        return {tx:rr[0],ty:rr[1],x:q.x,y:q.y,k:rr[2],uses:rr[3],ph:ph[rr[0]+":"+rr[1]]??rnd()*6.3}; });
    }
    G.pack=d.pack; G.tools={}; for(const t of d.tools) G.tools[t]=1;
    if(anyPanel()==="pack") drawPack();
    return;
  }
  if(d.e==="world"){
    resizeWorld(d.gw,d.gh);
    grid.set(unb64grid(d.grid).subarray(0,GW*GH));
    ISL={theme:d.theme,name:d.name,sub:d.sub,threat:d.threat,home:!!d.home,city:d.city,
         buildings:[],labels:[],spirit:null,story:null,mini:null,boss:null,bigs:[],cleared:false,seen:0};
    G.tint=d.tint;
    nodes=d.N.map(rr=>{ const q=tc(rr[0],rr[1]); return {tx:rr[0],ty:rr[1],x:q.x,y:q.y,k:rr[2],uses:rr[3],ph:rnd()*6.3}; });
    builds=d.B.map(rr=>({k:rr[0],x:rr[1],y:rr[2],seed:rr[3],ripe:!!rr[4],r:(BUILDS[rr[0]]&&BUILDS[rr[0]].r)||14}));
    props=d.R.map(rr=>({k:rr[0],x:rr[1],y:rr[2],a:0}));
    gates=d.GA.map(rr=>({x:rr[0],y:rr[1],def:{label:rr[2]},r:16,ph:rnd()*6.3}));
    foes=[]; animals=[]; party=[]; neutrals=[]; townies=[]; parts=[]; shots=[]; zones=[]; corpses=[];
    if(!NET.me) NET.me={x:d.sx,y:d.sy,r:11,aim:0,roll:0,inv:0,swing:0,down:0,sp:142,cd:0,n:"you"};
    NET.me.x=d.sx; NET.me.y=d.sy;
    NET.worldReady=true;
    $("title").classList.remove("on");
    G.mode="play";
    $("islname").innerHTML=d.name+(d.sub?` — ${d.sub}`:"");
    toast(d.name,"You wade in beside the captain.");
    return;
  }
  if(d.e==="dlg"){ netShowDlg(d); return; }
  if(d.e==="toast"){ toast(d.h,d.t); return; }
  if(d.e==="log"){ logit(d.h,d.t); return; }
  if(d.e==="inter"){ interlude(d.h,d.sub,d.body,null); return; }
  if(d.e==="pause"){ G.halt=!!d.on; return; }
  if(d.e==="panel"){ togglePanel(d.p); return; }
}
function syncByNid(arr,rows,apply,keepLocal){
  const seen={};
  for(const r of rows){
    seen[r[0]]=1;
    let o=arr.find(x=>x.nid===r[0]);
    if(!o){ o={nid:r[0]}; arr.push(o); }
    apply(o,r);
  }
  for(let i=arr.length-1;i>=0;i--) if(!seen[arr[i].nid]&&!(keepLocal&&arr[i].p2)) arr.splice(i,1);
}

/* ---- guest draws itself beside the captain ---- */
const _renderNet=render;
render=function(){
  _renderNet();
  if(NET.role==="guest"&&NET.me&&NET.worldReady&&G.mode==="play"){
    const me=NET.me;
    ctx.save(); ctx.globalAlpha=me.inv>0?.62:1;
    try{ figure(me,null,"#5a6f7c",1.05,"player"); }
    catch(err){ ctx.fillStyle="#5a6f7c"; ctx.beginPath();
      ctx.arc(me.x-cam.x,me.y-cam.y,me.r,0,6.3); ctx.fill(); }
    ctx.restore();
  }
};

/* ---- what the host says, the guest hears ---- */
const _toastNet=toast, _logNet=logit, _interNet=interlude;
toast=function(h,t){ _toastNet(h,t); if(NET.host) NET.send({e:"toast",h,t}); };
logit=function(h,t){ _logNet(h,t); if(NET.host) NET.send({e:"log",h,t}); };
interlude=function(h,sub,body,fn){ _interNet(h,sub,body,fn);
  if(NET.host) NET.send({e:"inter",h,sub,body}); };

/* ---- island changes carry the guest along ---- */
for(const fname of ["newIsland","enterCity","loadHome","wakeAtHome"]){
  const f=window[fname]||eval(fname);
  const wrapped=function(){ const r=f.apply(this,arguments); if(NET.host&&NET.on) netWorld(); return r; };
  eval(fname+"=wrapped");
}
const _beginNet=beginGame;
beginGame=function(){ NET.runOn=true; _beginNet.apply(this,arguments); if(NET.host&&NET.on) netWorld(); };

/* ---- the handshake: two pasted codes, no server ---- */
function netShowFlow(step){ $("netflow").style.display="block"; $("netbtns").style.display="none"; $("netstep").textContent=step; }
function netRTC(){
  return new RTCPeerConnection({iceServers:[{urls:"stun:stun.l.google.com:19302"}]});
}
function netWireChannel(ch){
  NET.ch=ch;
  ch.onopen=()=>{
    NET.on=true;
    $("netstat").textContent="— connected.";
    if(NET.role==="guest"){ NET.send({e:"hello"}); $("netstep").textContent="Connected. The captain starts the game."; }
    else { $("netstep").textContent="Connected. Wake up on the sand when ready."; if(G.mode!=="title") netWorld(); }
  };
  ch.onmessage=ev=>netMsg(ev.data);
  ch.onclose=()=>{ $("netstat").textContent="— the line went slack."; toast("the crossing","The other house went quiet."); };
}
/* the code is ready when the sea has finished naming your addresses — or when
   we get tired of waiting for it. Either way something usable goes in the box. */
function netEmitCode(pc){
  let done=false;
  const emit=()=>{
    if(done||!pc.localDescription) return;
    done=true;
    $("netout").value=btoa(JSON.stringify(pc.localDescription));
    $("netstat").textContent="— code ready. Copy the top box.";
  };
  pc.onicecandidate=e=>{ if(!e.candidate) emit(); };
  pc.onicegatheringstatechange=()=>{ if(pc.iceGatheringState==="complete") emit(); };
  setTimeout(emit,2500);                       // whatever has been gathered by now will do
}
async function netHostStart(){
  NET.role="host"; NET.host=true;
  netShowFlow("Copy this code to your brother. Paste his answer below.");
  $("netout").value=""; $("netout").placeholder="gathering the code — a moment…";
  $("netstat").textContent="— gathering…";
  const pc=NET.pc=netRTC();
  netWireChannel(pc.createDataChannel("sea"));
  netEmitCode(pc);
  const offer=await pc.createOffer(); await pc.setLocalDescription(offer);
}
async function netJoinStart(){
  NET.role="guest"; NET.host=false;
  netShowFlow("Paste the captain's code below, then send back the code that appears.");
  $("netout").value=""; $("netout").placeholder="your answer code will appear here after you paste his";
  const pc=NET.pc=netRTC();
  pc.ondatachannel=e=>netWireChannel(e.channel);
  netEmitCode(pc);
}
async function netUseCode(){
  const raw=$("netin").value.trim(); if(!raw) return;
  let desc; try{ desc=JSON.parse(atob(raw)); }catch(err){ $("netstat").textContent="— that code did not read."; return; }
  try{
    await NET.pc.setRemoteDescription(desc);
    if(NET.role==="guest"){
      const ans=await NET.pc.createAnswer(); await NET.pc.setLocalDescription(ans);
      $("netstat").textContent="— code taken. Your answer is gathering — send the top box back.";
    } else {
      $("netstat").textContent="— code taken. Waiting on the sea…";
    }
  }catch(err){ $("netstat").textContent="— "+err.message; }
}
if($("nethost")){ $("nethost").onclick=netHostStart; $("netjoin").onclick=netJoinStart; $("netgo").onclick=netUseCode; }
if($("start")){ const _sb=$("start");
  const _sbc=_sb.onclick;
  _sb.onclick=()=>{ if(NET.role==="guest"){ $("netstat").textContent="— the captain starts it; you will be pulled in."; return; } beginGame(); };
}


/* ============================================================ ARMS & THE RUN
   Weapons are things now, not habits: each one is made once, rolls what it
   rolls, and goes in a hand or a pack slot. Blueprints sharpen with practice.
   Broken finds off the big dead teach you their shape. And a run can be put
   down and picked back up, which the sea has historically not allowed. */

/* ---- what can be made, and what it swings like ----
   numbers are [middle, spread]. cls: spear reaches, sword bites, axe ends
   arguments slowly, bow and gun do it from over there. */
const WPN={
  oar:       {n:"the oar",              cls:"spear",tier:0,dmg:[9,0],  reach:[56,0], cd:[.42,0],
              d:"Flat, heavy, and the only thing the sea let you keep."},
  prim_spear:{n:"Primitive spear",      cls:"spear",tier:1,c:{wood:2,stone:1,fiber:1},dmg:[14,3],reach:[66,6],cd:[.34,.03],
              d:"A point on a pole. Most of what a spear ever needed to be."},
  prim_club: {n:"Primitive club",       cls:"axe",  tier:1,c:{wood:3,stone:1},dmg:[21,4],reach:[40,4],cd:[.62,.05],
              d:"Weight, gripped. Slow, and then very final."},
  prim_bow:  {n:"Primitive bow",        cls:"bow",  tier:1,c:{wood:2,fiber:3},dmg:[17,3],reach:[300,25],cd:[.64,.05],
              d:"Bent wood and cord. It argues at a distance."},
  iron_spear:{n:"Iron spear",           cls:"spear",tier:2,c:{ingot:2,wood:3,fiber:1},dmg:[24,4],reach:[74,6],cd:[.32,.03],
              d:"The point holds now."},
  iron_sword:{n:"Iron sword",           cls:"sword",tier:2,c:{ingot:3,hide:1},dmg:[30,5],reach:[46,4],cd:[.30,.03],
              d:"Short reach, quick, and it means it."},
  iron_axe:  {n:"Iron axe",             cls:"axe",  tier:2,c:{ingot:3,wood:2},dmg:[40,6],reach:[44,4],cd:[.78,.06],
              d:"You commit to a swing like this. So does whatever it lands on."},
  hunt_bow:  {n:"Hunting bow",          cls:"bow",  tier:2,c:{wood:3,fiber:3,hide:2},dmg:[27,4],reach:[330,25],cd:[.58,.04],
              d:"Hide-backed. It carries."},
  flint_gun: {n:"Flintlock",            cls:"gun",  tier:2,c:{ingot:3,wood:2,pitch:1},dmg:[34,7],reach:[360,25],cd:[.95,.08],
              d:"Loud, slow, and settles most questions with the first word."},
  long_iron: {n:"Long iron",            cls:"spear",tier:3,c:{ingot:5,wood:4,glass:1},dmg:[36,5],reach:[92,8],cd:[.34,.03],
              d:"Made for keeping something exactly one spear-length away forever."},
  mist_blade:{n:"Mist-tempered blade",  cls:"sword",tier:3,c:{ingot:4,glass:3,hide:2},dmg:[46,6],reach:[50,4],cd:[.28,.02],
              d:"Quenched in mistglass water. It keeps taking after it stops cutting."},
  grave_axe: {n:"Gravedigger's axe",    cls:"axe",  tier:3,c:{ingot:5,bone:6,glass:2},dmg:[62,8],reach:[46,4],cd:[.92,.07],
              d:"Whoever swung it before you did not dig graves for a living."},
  still_bow: {n:"Stillwater bow",       cls:"bow",  tier:3,c:{wood:4,glass:3,fiber:4},dmg:[41,5],reach:[380,25],cd:[.53,.04],
              d:"It does not creak. Nothing about it makes any sound at all."},
  sea_musket:{n:"Sea-pattern musket",   cls:"gun",  tier:3,c:{ingot:6,glass:2,pitch:2},dmg:[54,9],reach:[420,25],cd:[1.05,.08],
              d:"Navy work, off a navy that isn't on any chart either."},
};
const WPN_RANGED={bow:1,gun:1};
ITEMS.wpn={n:"weapon",d:"A made thing.",stack:1};
ITEMS.arm={n:"armour",d:"A worn thing.",stack:1};
ITEMS.brk={n:"broken weapon",d:"Past mending, not past reading.",stack:1};
const WPN_CLSN={spear:"spear",sword:"sword",axe:"axe",bow:"bow",gun:"gun"};
/* old tool-weapons folded into the new system; their recipes and plans retire */
const OLD_WPN={ironspear:1,mistblade:1,longiron:1,gutter:1};

/* ---- armour: one piece to a body part ---- */
const ARM={
  hidecap:{n:"Hide cap",         slot:"head", mhp:14},
  ironhelm:{n:"Iron helm",       slot:"head", mhp:22,armor:4},
  vest:   {n:"Hide vest",        slot:"chest",mhp:25},
  harness:{n:"Bone-plate harness",slot:"chest",mhp:40,armor:3},
  coat:   {n:"Warden's coat",    slot:"chest",mhp:55,armor:5},
  plate:  {n:"Scale plate",      slot:"chest",mhp:70,armor:10},
  ballast:{n:"Ballast plate",    slot:"chest",mhp:90,armor:8,spd:.75,mvg:-15},
  shoes:  {n:"Hobnail shoes",    slot:"legs", armor:3,spd:1.10},
  fogfoot:{n:"Fogfoot wraps",    slot:"legs", spd:1.25,dashCost:.5,armorOff:1},
};
const ARM_SLOTN={head:"head",chest:"chest",legs:"legs"};

/* ---- rolling a weapon: practice narrows the spread and lifts the middle ---- */
function rollStat(pair,m,lift,inverted){
  const f=1/(1+.3*m);
  const drift=Math.min(m*lift,lift*8);
  let v=pair[0]+(inverted?-drift:drift)+(rnd()*2-1)*pair[1]*f;
  return Math.round(v*100)/100;
}
function rollWeapon(id){
  const W=WPN[id], m=G.bp[id]||0;
  return {id:"wpn",n:1,w:id,st:{
    dmg:Math.max(1,Math.round(rollStat(W.dmg,m,.45))),
    reach:Math.max(20,Math.round(rollStat(W.reach,m,.6))),
    cd:Math.max(.18,rollStat(W.cd,m,.006,true)),
  }};
}
function mkArmItem(id){ return {id:"arm",n:1,a:id}; }
function mkBroken(id){ return {id:"brk",n:1,w:id}; }

/* ---- names and descriptions the pack can print ---- */
function itName(s){
  if(!s) return "—";
  if(s.id==="wpn") return WPN[s.w].n;
  if(s.id==="arm") return ARM[s.a].n;
  if(s.id==="brk") return "Broken "+WPN[s.w].n.toLowerCase();
  return ITEMS[s.id].n;
}
function itDesc(s){
  if(s.id==="wpn"){ const st=s.st;
    return `${WPN_CLSN[WPN[s.w].cls]} — ${st.dmg} damage, ${WPN_RANGED[WPN[s.w].cls]?st.reach+" range":st.reach+" reach"}, ${st.cd}s a swing.`; }
  if(s.id==="arm"){ const A=ARM[s.a], b=[];
    if(A.mhp) b.push("+"+A.mhp+" body"); if(A.armor) b.push("+"+A.armor+" armour");
    if(A.spd&&A.spd!==1) b.push(Math.round((A.spd-1)*100)+"% speed"); if(A.mvg) b.push(A.mvg+" vigor");
    if(A.dashCost) b.push("dashes cost half"); if(A.armorOff) b.push("armour counts for nothing");
    return `${A.slot} — ${b.join(", ")||"it fits"}.`; }
  if(s.id==="brk") return "Past mending, not past reading. Research it at the craft panel.";
  return ITEMS[s.id].d;
}
function itUsable(s){
  if(!s) return "";
  if(s.id==="wpn"||s.id==="arm") return "equip";
  if(s.id==="brk") return "";
  return ITEMS[s.id].use?"use":"";
}

/* ---- carry a made thing home ---- */
function giveGear(inst){
  for(let i=0;i<packSlots();i++) if(!G.pack[i]){ G.pack[i]=inst; return true; }
  const st=nearStores&&nearStores();
  if(st&&st.hold){ for(let i=0;i<G.cache.length;i++) if(!G.cache[i]){ G.cache[i]=inst; toast("stowed","No room on you. It went in the hold."); return true; } }
  toast("no room for it","Both hands and every pocket. Drop something first.");
  return false;
}

/* ---- equipping: one weapon in hand, one piece to a part ---- */
function equipWeapon(i){
  const s=G.pack[i]; if(!s||s.id!=="wpn") return;
  const old=G.wpn;
  G.pack[i]=old&&old.w!=="oar"?old:null;      // the oar goes over your shoulder, not in a slot
  if(old&&old.w==="oar") G.oarStowed=old;
  G.wpn=s; packNorm();
  toast("in hand",`${WPN[s.w].n}. ${itDesc(s)}`);
  syncHUD();
}
function equipArmor(i){
  const s=G.pack[i]; if(!s||s.id!=="arm") return;
  const A=ARM[s.a], slot=A.slot;
  const old=G.armor[slot];
  if(old) armApply(ARM[old.a],-1);
  G.pack[i]=old||null;
  G.armor[slot]=s;
  armApply(A,1);
  packNorm();
  toast("worn",`${A.n}, on the ${slot}.`);
  syncHUD();
}
function armApply(A,dir){
  if(A.mhp){ G.mhp+=A.mhp*dir; G.hp=Math.min(G.hp+(dir>0?A.mhp:0),G.mhp); G.hp=Math.min(G.hp,G.mhp); }
  if(A.armor) player.armor+=A.armor*dir;
  if(A.spd) player.spdMul*= dir>0?A.spd:1/A.spd;
  if(A.mvg){ G.mvg=Math.max(20,G.mvg+A.mvg*dir); G.vg=Math.min(G.vg,G.mvg); }
  if(A.dashCost) player.dashCost*= dir>0?A.dashCost:1/A.dashCost;
  if(A.armorOff) player.armorOff=dir>0?1:0;
  G.mhp=Math.max(20,G.mhp); G.hp=Math.max(1,Math.min(G.hp,G.mhp));
}

/* ---- the hand you fight with ---- */
function wpnSt(){ return G.wpn?G.wpn.st:{dmg:player.dmg,reach:player.reach,cd:.32}; }
function wpnCls(){ return G.wpn?WPN[G.wpn.w].cls:"spear"; }
function wpnRanged(){ return !!WPN_RANGED[wpnCls()]; }

/* ---- loosing something at a distance ---- */
function playerShot(){
  if(G.mode!=="play"||isPaused()||player.cd>0||player.roll>0) return;
  const st=wpnSt(), gun=wpnCls()==="gun";
  const cost=gun?7:5;
  if(G.vg<cost){ toast("no vigor for it",`It wants ${cost}.`); return; }
  spendVg(cost);
  player.cd=st.cd; player.swing=.14;
  let sdmg=st.dmg;
  const tg5s=hasPas("togo");
  if(tg5s&&tg5s.rank>=5) sdmg=Math.round(sdmg*(1+.25*petWolves().length));
  const a=player.aim+(rnd()-.5)*(gun?.05:.08);
  const sp=gun?620:390;
  shots.push({x:player.x+Math.cos(a)*(player.r+8),y:player.y+Math.sin(a)*(player.r+8),
    vx:Math.cos(a)*sp,vy:Math.sin(a)*sp,dmg:sdmg,r:5,life:st.reach/sp,
    c:gun?"#e0d6b8":"#cfc7ae",s:gun?"ball":"arrow",mine:1});
  sfx(gun?110:320,gun?.16:.09,gun?"square":"triangle",gun?.11:.06);
}

/* ---- the broken and the learned ---- */
const BRK_IRON=["iron_spear","iron_sword","iron_axe","hunt_bow","flint_gun"];
const BRK_HIGH=["long_iron","mist_blade","grave_axe","still_bow","sea_musket"];
function dropBroken(pool,src){
  const cand=pool.filter(w=>G.bp[w]===undefined);
  const id=cand.length?pick(cand):pick(pool);
  if(giveGear(mkBroken(id))){
    toast("broken, but legible",`A broken ${WPN[id].n.toLowerCase()}, off ${src}. Research it at the craft panel.`);
    logit("a broken thing",`${WPN[id].n}, off ${src}.`);
  }
}
function researchBroken(i){
  const s=G.pack[i]; if(!s||s.id!=="brk") return;
  const id=s.w;
  G.pack[i]=null; packNorm();
  if(G.bp[id]===undefined){
    G.bp[id]=0;
    toast("you can see how it was made",`${WPN[id].n} — the blueprint is yours now.`);
    logit("a blueprint",`${WPN[id].n}, off a broken one.`);
  } else {
    G.bp[id]+=1;
    toast("nothing new in it","You knew this one. Taking it apart steadied your hand anyway.");
  }
  syncHUD();
}
function craftWeapon(id){
  const W=WPN[id]; if(!W||G.bp[id]===undefined||!canPay(W.c)) return;
  pay(W.c);
  const inst=rollWeapon(id);
  G.bp[id]+=1;
  if(giveGear(inst)){
    toast("made",`${W.n}. ${itDesc(inst)}`);
    logit("made",`${W.n} — ${inst.st.dmg} damage.`);
  }
  sfx(180,.12,"square",.08); syncHUD();
}

/* ---- crew gear: two hands and a back ---- */
function allyGearLine(m){
  const bits=[];
  for(const c of (m.carry||[])) if(c) bits.push(itName(c)+(c.id==="bandage"?" ×"+c.n:""));
  if(m.armorItem) bits.push(ARM[m.armorItem.a].n);
  return bits.length?bits.join(", "):"nothing but their hands";
}
function allyWeapon(m){
  for(const c of (m.carry||[])) if(c&&c.id==="wpn") return c;
  return null;
}
function allyBandages(m){
  for(const c of (m.carry||[])) if(c&&c.id==="bandage") return c;
  return null;
}
function gearTalk(m){
  m.carry=m.carry||[null,null];
  const opts=[];
  /* hand things over */
  for(let i=0;i<packSlots();i++){
    const s=G.pack[i]; if(!s) continue;
    if(s.id==="wpn"||s.id==="bandage"){
      const free=m.carry[0]?(m.carry[1]?-1:1):0;
      opts.push({t:`Hand over: ${itName(s)}${s.id==="bandage"?" ×"+Math.min(3,s.n):""}`,off:free<0,fn:()=>{
        if(s.id==="bandage"){ const take=Math.min(3,s.n);
          const held=allyBandages(m);
          if(held){ held.n+=take; } else m.carry[free]={id:"bandage",n:take};
          s.n-=take; if(s.n<=0) G.pack[i]=null; packNorm();
        } else { m.carry[free]=s; G.pack[i]=null; packNorm(); }
        toast(m.n.split(" ")[0],"Takes it without a word."); gearTalk(m); }});
    }
    if(s.id==="arm"){
      opts.push({t:`Strap on them: ${itName(s)}`,off:!!m.armorItem,fn:()=>{
        m.armorItem=s; G.pack[i]=null; packNorm();
        const A=ARM[s.a];
        m.armor=(m.armor||0)+(A.armor||0); m.mhp+=(A.mhp||0); m.hp+=(A.mhp||0);
        toast(m.n.split(" ")[0],"Wears it."); gearTalk(m); }});
    }
  }
  /* take things back */
  for(let ci=0;ci<2;ci++){ const c=m.carry[ci]; if(!c) continue;
    opts.push({t:`Take back: ${itName(c)}${c.id==="bandage"?" ×"+c.n:""}`,fn:()=>{
      if(c.id==="bandage") give("bandage",c.n); else giveGear(c);
      m.carry[ci]=null; gearTalk(m); }});
  }
  if(m.armorItem) opts.push({t:`Take back: ${ARM[m.armorItem.a].n}`,fn:()=>{
    const A=ARM[m.armorItem.a];
    m.armor=Math.max(0,(m.armor||0)-(A.armor||0)); m.mhp-=(A.mhp||0); m.hp=Math.min(m.hp,m.mhp);
    giveGear(m.armorItem); m.armorItem=null; gearTalk(m); }});
  opts.push({t:"That's all.",fn:()=>{}});
  say(m.n,"carrying: "+allyGearLine(m),
    [`<i>They hold their hands out flat, the way you would for anything the sea might want back.</i>
      <br>Two hands — a weapon makes them dangerous, bandages make them a medic. The back takes one piece of armour.`],opts);
}
function allyShot(m,tgt,st){
  const a=Math.atan2(tgt.y-m.y,tgt.x-m.x)+(rnd()-.5)*.09;
  shots.push({x:m.x+Math.cos(a)*(m.r+8),y:m.y+Math.sin(a)*(m.r+8),
    vx:Math.cos(a)*380,vy:Math.sin(a)*380,dmg:st.dmg*.85,r:5,life:st.reach/380,
    c:"#cfc7ae",s:"arrow",mine:1});
  sfx(300,.08,"triangle",.05);
}

/* ---- the severance rite: a Regard cut out and handed on, paid in blood ---- */
function severTalk(slotI){
  if(!G.regards.length){ toast("nothing to cut","No Regard sits in you. The fetish has no interest in your meat alone."); return; }
  const crew=party.filter(m=>!m.p2&&!m.down);
  if(crew.length<2){
    toast("the fetish is quiet","It wants a hand to take the Regard and a life to pay for the cut. You do not have both walking with you.");
    return;
  }
  say("The severance fetish","glass, bone, and a decision",
    [`<i>It is warm, which it has no business being. It wants to know which one comes out.</i>`],
    G.regards.map((g,gi)=>({t:`Cut out: ${g.R.n} — rank ${g.rank}`,fn:()=>severWho(slotI,gi)}))
      .concat([{t:"Put it away.",fn:()=>{}}]));
}
function severWho(slotI,gi){
  const g=G.regards[gi];
  const crew=party.filter(m=>!m.p2&&!m.down);
  say("The severance fetish","who takes it",
    [`<i>${g.R.n}, held out on the flat of your hand like a wet coal. Somebody has to close their fingers on it.</i>`],
    crew.map(m=>({t:`${m.n} takes it${m.g?" — already carries one":""}`,off:!!m.g,
      fn:()=>severPay(slotI,gi,m)}))
      .concat([{t:"Nobody. Put it away.",fn:()=>{}}]));
}
function severPay(slotI,gi,recv){
  const g=G.regards[gi];
  const price=party.filter(m=>!m.p2&&!m.down&&m!==recv);
  say("The severance fetish","the price",
    [`<i>The cut does not close on its own. The fetish names its price the way the sea names hers: somebody.</i>`],
    price.map(m=>({t:`Give it ${m.n}.`,fn:()=>severDo(slotI,gi,recv,m)}))
      .concat([{t:"No. Not at that price.",fn:()=>{}}]));
}
function severDo(slotI,gi,recv,sac){
  const g=G.regards.splice(gi,1)[0];
  recv.g={R:g.R,rank:g.rank,xp:g.xp||0};
  party=party.filter(m=>m!==sac);
  G.deaths.push(sac.n+", given to the fetish");
  corpses.push({x:sac.x,y:sac.y,r:sac.r||11,k:"cast",t:0,max:1.2,c:"#7c5a3c"});
  for(let i=0;i<14;i++) parts.push({x:sac.x,y:sac.y,vx:(rnd()-.5)*160,vy:-30-rnd()*90,life:.7,c:"#c7bfa8",s:2});
  take("sever",1);
  toast("the cut is made",`${g.R.n} sits in ${recv.n.split(" ")[0]} now. ${sac.n.split(" ")[0]} paid for it.`);
  logit("severance",`${g.R.n} cut out and given to ${recv.n}. ${sac.n} was the price.`);
  interlude("The Severance",g.R.n+", handed on",
    `The fetish grows cold in your hand, which is the only receipt you get.<br><br>
     ${recv.n} stands differently now — the way you stood when ${g.R.n.toLowerCase().startsWith("the")?g.R.n:"the "+g.R.n} first looked at you.
     ${sac.n} does not stand at all. Whatever the fetish took, it took it all at once, and quietly.<br><br>
     <i>You will bury what is left, or you will not. The fetish does not care either way.</i>`,null);
  sfx(80,.6,"sine",.12);
  syncHUD();
}

/* ============================================================ THE SAVED RUN */
const RUN_STORE="castaway_run1";
function packRegards(list){ return (list||[]).map(g=>({id:g.R.id,rank:g.rank,xp:g.xp||0})); }
function unpackRegards(list){
  return (list||[]).map(o=>{ const R=REGARDS.find(r=>r.id===o.id); return R?{R,rank:o.rank,xp:o.xp}:null; })
    .filter(Boolean);
}
function packAllies(list){
  return (list||[]).filter(m=>!m.p2).map(m=>({n:m.n,role:m.role,x:r1(m.x),y:r1(m.y),hp:Math.round(m.hp),mhp:m.mhp,
    dmg:m.dmg,sp:m.sp,r:m.r,joined:!!m.joined,down:m.down?1:0,bleed:m.bleed||0,
    g:m.g?{id:m.g.R.id,rank:m.g.rank,xp:m.g.xp||0}:null,
    carry:m.carry||null,armorItem:m.armorItem||null,armor:m.armor||0,
    want:m.want||null,line:m.line||"",asked:m.asked||0}));
}
function unpackAllies(rows){
  return (rows||[]).map(o=>{ const m=Object.assign({ph:rnd()*6,cd:0,swing:0,hurt:0,rage:0,face:1,aim:0},o);
    if(o.g){ const R=REGARDS.find(r=>r.id===o.g.id); m.g=R?{R,rank:o.g.rank,xp:o.g.xp}:null; }
    return m; });
}
function saveRun(){
  if(NET.on&&!NET.host) return;               // the guest's run lives in the captain's house
  if(G.mode==="title"||G.mode==="ending") return;
  try{
    const gg=Object.assign({},G);
    gg.regards=packRegards(G.regards);
    gg.halt=false; gg.hitstop=0; gg.shake=0; gg.hurtT=0; gg.mode="play";
    const hom=HOME?Object.assign({},HOME):null;
    if(hom){ hom.grid=null; hom.gridB=(function(){ let out="",i2=0; const gr=HOME.grid, n2=gr.length;
      while(i2<n2){ out+=String.fromCharCode.apply(null,gr.subarray?gr.subarray(i2,Math.min(n2,i2+4096)):gr.slice(i2,i2+4096)); i2+=4096; }
      return btoa(out); })();
      hom.residents=(HOME.residents||[]).map(m=>Object.assign({},m)); }
    const isl=Object.assign({},ISL);
    isl.story=null; isl.boss=null; isl.mini=null; isl.bigs=[]; isl.spirit=ISL.spirit?JSON.parse(JSON.stringify(ISL.spirit)):null;
    const data={v:1,when:Date.now(),
      G:gg, player:Object.assign({},player), ISL:isl,
      GW,GH,grid:b64grid(),
      nodes:nodes.map(n=>({tx:n.tx,ty:n.ty,x:n.x,y:n.y,k:n.k,uses:n.uses})),
      builds:builds.map(b=>Object.assign({},b)),
      props:props.map(p=>({k:p.k,x:p.x,y:p.y,a:p.a||0})),
      gates:gates.map(g=>({x:g.x,y:g.y,tx:g.tx,ty:g.ty,def:g.def?JSON.parse(JSON.stringify(g.def)):null})),
      foes:foes.map(f=>({k:f.k,x:r1(f.x),y:r1(f.y),hp:Math.round(f.hp),asleep:f.asleep?1:0,boss:ISL.boss===f?1:0,mini:ISL.mini===f?1:0})),
      animals:animals.map(a=>({k:a.k,x:r1(a.x),y:r1(a.y),hp:Math.round(a.hp),pet:a.pet?1:0,fed:a.fed?1:0,young:a.young?1:0,tp:a.tp||0})),
      party:packAllies(party),
      neutrals:JSON.parse(JSON.stringify(neutrals)),
      townies:JSON.parse(JSON.stringify(townies)),
      HOME:hom};
    localStorage.setItem(RUN_STORE,JSON.stringify(data));
    return true;
  }catch(err){ G.saveErr=err.message; return false; }
}
function hasRun(){ try{ return !!localStorage.getItem(RUN_STORE); }catch(err){ return false; } }
function clearRun(){ try{ localStorage.removeItem(RUN_STORE); }catch(err){} }
function loadRun(){
  let data; try{ data=JSON.parse(localStorage.getItem(RUN_STORE)); }catch(err){ return false; }
  if(!data||data.v!==1) return false;
  Object.assign(G,data.G);
  G.regards=unpackRegards(data.G.regards);
  G.armor=G.armor||{head:null,chest:null,legs:null}; G.bp=G.bp||{};
  Object.assign(player,data.player);
  resizeWorld(data.GW,data.GH);
  grid.set(unb64grid(data.grid).subarray(0,GW*GH));
  ISL=Object.assign({story:null,boss:null,mini:null,bigs:[]},data.ISL);
  nodes=data.nodes.map(n=>Object.assign({ph:rnd()*6.3},n));
  builds=data.builds.map(b=>Object.assign({},b));
  props=data.props.map(p=>Object.assign({},p));
  gates=data.gates.map(g=>Object.assign({r:16,ph:rnd()*6.3},g));
  foes=[]; parts=[]; zones=[]; shots=[]; corpses=[];
  for(const fr of data.foes){
    let f=null;
    if(fr.k==="boss"||fr.k==="mini") f=mkBig(fr.k,fr.x,fr.y);
    else if(FOES[fr.k]) f=mkFoe(fr.k,fr.x,fr.y);
    if(f){ f.hp=fr.hp; f.asleep=!!fr.asleep;
      if(fr.boss) ISL.boss=f; if(fr.mini) ISL.mini=f; }
  }
  animals=data.animals.map(a=>{ const o=mkAnimal(a.k,a.x,a.y,{hp:a.hp}); if(o){ o.pet=!!a.pet; o.fed=!!a.fed; o.young=!!a.young; o.tp=a.tp; } return o; }).filter(Boolean);
  party=unpackAllies(data.party);
  neutrals=data.neutrals||[]; townies=data.townies||[];
  HOME=data.HOME||null;
  if(HOME&&HOME.gridB){ HOME.grid=unb64grid(HOME.gridB); delete HOME.gridB; }
  floodReach(...(ISL.start||[Math.floor(GW/2),Math.floor(GH/2)]));
  collectSpots();
  G.tint=THEMES[ISL.theme]?THEMES[ISL.theme].tint:G.tint;
  G.mode="play"; G.halt=false;
  spawnT=ISL.home?1e9:spawnT;
  $("title").classList.remove("on");
  packSel=-1;
  toast("the run picks back up",ISL.name+". Where you put it down.");
  logit("continued","The run, from where it was left.");
  syncHUD(); drawChart&&drawChart();
  return true;
}
let SAVE_T=0;
function saveTick(dt){
  if(NET.on&&!NET.host) return;
  SAVE_T+=dt;
  if(SAVE_T>=20){ SAVE_T=0; saveRun(); }
}

/* ---- transitions save themselves; endings clean up after ---- */
for(const fname of ["newIsland","enterCity","loadHome","wakeAtHome"]){
  const f=eval(fname);
  const wrapped2=function(){ const r=f.apply(this,arguments); saveRun(); return r; };
  eval(fname+"=wrapped2");
}
const _endGear=showEnding;
showEnding=function(kind){ clearRun(); return _endGear.apply(this,arguments); };

const _beginGear=beginGame;
beginGame=function(){
  clearRun();
  G.bp={prim_spear:0,prim_bow:0,prim_club:0};
  G.wpn={id:"wpn",n:1,w:"oar",st:{dmg:9,reach:56,cd:.42}};
  G.armor={head:null,chest:null,legs:null};
  const r=_beginGear.apply(this,arguments);
  return r;
};

/* ---- the title remembers ---- */
if($("contbtn")){
  const cb=$("contbtn");
  const show=()=>{ cb.style.display=hasRun()?"inline-block":"none"; };
  show();
  cb.onclick=()=>{ if(NET.role==="guest") return;
    if(!loadRun()) { toast("nothing kept","The save did not read. The sea got into it."); show(); } };
}


artSnap(); citySnap();                      // in case the header scripts were stripped
const restored=restoreArt();
const restoredCities=restoreCities();
wireArtInput(); wireCityInput();
loadArt();
if(restored) logit("artwork",restored+" imported sprite"+(restored>1?"s":"")+" remembered from last time.");
if(restoredCities) logit("cities",restoredCities+" imported cit"+(restoredCities>1?"ies":"y")+" remembered from last time.");
newNoise(); layTerrain("forest");           // something behind the title card
cam.x=GW*TILE/2-VW/2; cam.y=GH*TILE/2-VH/2;
syncHUD();
requestAnimationFrame(loop);
</script>
</body>
</html>
