# kryvka_kostky
[kryvka_kostka.html](https://github.com/user-attachments/files/32948125/kryvka_kostka.html)
<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Hrací kostka</title>
<style>
:root{
  --bg:#e6eee8;--felt:#2a6b57;--felt-dark:#1c4d3e;--panel:#fbfaf5;--ink:#1d2622;--muted:#5d6b64;
  --die:#fbf8ee;--pip:#1d2622;--brass:#c9962f;--line:#cfd9d2;
  box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){
  --bg:#0f1a16;--felt:#1f5242;--felt-dark:#143a2f;--panel:#17231e;--ink:#eef2ec;--muted:#98a89f;--die:#f4f0e2;--line:#2b3b34}}
:root[data-theme="dark"]{--bg:#0f1a16;--felt:#1f5242;--felt-dark:#143a2f;--panel:#17231e;--ink:#eef2ec;--muted:#98a89f;--die:#f4f0e2;--line:#2b3b34}
*{box-sizing:border-box}
html,body{margin:0}
body{background:var(--bg);color:var(--ink);font-family:"Trebuchet MS","Segoe UI",Verdana,sans-serif;min-height:100%;
  display:flex;justify-content:center;padding:20px 14px}
main{width:100%;max-width:520px;display:flex;flex-direction:column;gap:16px}
h1{margin:0;font-size:26px;letter-spacing:.2px}
.sub{margin:2px 0 0;color:var(--muted);font-size:14px}
.table{background:radial-gradient(ellipse at center,var(--felt) 0%,var(--felt-dark) 100%);border-radius:20px;
  border:6px solid var(--brass);padding:28px 16px;min-height:220px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:18px}
.dice{display:flex;gap:18px;flex-wrap:wrap;justify-content:center}
.die{width:96px;height:96px;background:var(--die);border-radius:18px;padding:12px;display:grid;
  grid-template:repeat(3,1fr)/repeat(3,1fr);box-shadow:0 6px 0 rgba(0,0,0,.28),0 12px 18px rgba(0,0,0,.25)}
.die.rolling{animation:shake .12s infinite}
@keyframes shake{0%{transform:rotate(-8deg) translateY(-4px)}50%{transform:rotate(7deg) translateY(2px)}100%{transform:rotate(-5deg) translateY(-3px)}}
.pip{width:100%;height:100%;max-width:19px;max-height:19px;margin:auto;background:var(--pip);border-radius:50%}
.die[data-v="1"] .pip{background:var(--pip1,#b83a2a)}
.total{color:#fff;font-size:18px;min-height:24px;text-shadow:0 1px 2px rgba(0,0,0,.4)}
.controls{display:flex;gap:10px;align-items:stretch}
.seg{display:flex;border:2px solid var(--line);border-radius:12px;overflow:hidden;background:var(--panel)}
.seg button{border:0;background:transparent;color:var(--ink);padding:0 16px;font:inherit;font-size:16px;cursor:pointer;min-height:52px}
.seg button[aria-pressed="true"]{background:var(--brass);color:#1d1606;font-weight:bold}
#roll{flex:1;min-height:52px;border:0;border-radius:12px;background:var(--ink);color:var(--bg);font:inherit;font-size:18px;font-weight:bold;cursor:pointer}
#roll:active{transform:translateY(1px)}
#roll:disabled{opacity:.6;cursor:wait}
button:focus-visible{outline:3px solid var(--brass);outline-offset:2px}
.panel{background:var(--panel);border:2px solid var(--line);border-radius:16px;padding:16px}
h2{margin:0 0 10px;font-size:16px}
.hist{display:flex;flex-wrap:wrap;gap:8px;min-height:34px}
.chip{background:var(--bg);border:1px solid var(--line);border-radius:999px;padding:4px 12px;font-size:14px}
.empty{color:var(--muted);font-size:14px}
.bars{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;align-items:end;height:90px}
.bar{display:flex;flex-direction:column;align-items:center;justify-content:flex-end;height:100%;gap:4px;font-size:13px;color:var(--muted)}
.bar i{display:block;width:100%;background:var(--felt);border-radius:6px 6px 0 0;min-height:3px;transition:height .25s}
.foot{display:flex;justify-content:space-between;align-items:center;color:var(--muted);font-size:13px}
.foot button{background:none;border:0;color:var(--muted);text-decoration:underline;font:inherit;cursor:pointer;padding:6px}
.pick{display:flex;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:10px}
.pick:last-child{margin-bottom:0}
.pick span.l{width:70px;font-size:14px;color:var(--muted)}
.sw{width:36px;height:36px;border-radius:50%;border:3px solid var(--line);cursor:pointer;padding:0}
.sw.sq{border-radius:10px}
.sw[aria-pressed="true"]{border-color:var(--ink);box-shadow:0 0 0 2px var(--panel) inset}
@media (prefers-reduced-motion:reduce){.die.rolling{animation:none}.bar i{transition:none}}
</style>
</head>
<body>
<main>
  <header>
    <h1>Hrací kostka</h1>
    <p class="sub">Klikni na Hodit nebo stiskni mezerník.</p>
  </header>

  <section class="table" aria-live="polite">
    <div class="dice" id="dice"></div>
    <div class="total" id="total">Zatím jsi nehodil.</div>
  </section>

  <div class="controls">
    <div class="seg" role="group" aria-label="Počet kostek">
      <button aria-pressed="true" data-n="1">1</button>
      <button aria-pressed="false" data-n="2">2</button>
      <button aria-pressed="false" data-n="3">3</button>
    </div>
    <button id="roll">Hodit kostkou</button>
  </div>

  <section class="panel">
    <h2>Poslední hody</h2>
    <div class="hist" id="hist"><span class="empty">Tady se objeví tvoje hody.</span></div>
  </section>

  <section class="panel">
    <h2>Kolikrát padla která strana</h2>
    <div class="bars" id="bars"></div>
  </section>

  <section class="panel">
    <h2>Barvy</h2>
    <div class="pick"><span class="l">Stůl</span><div class="pick" id="pt" style="margin:0"></div></div>
    <div class="pick"><span class="l">Kostky</span><div class="pick" id="pd" style="margin:0"></div></div>
  </section>

  <div class="foot"><span id="count">Hodů: 0</span><button id="reset">Vynulovat</button></div>
</main>

<script>
const PIPS={1:[5],2:[1,9],3:[1,5,9],4:[1,3,7,9],5:[1,3,5,7,9],6:[1,3,4,6,7,9]};
let n=1,busy=false,hist=[],stats=[0,0,0,0,0,0],rolls=0;
const $=id=>document.getElementById(id);
const rnd=()=>Math.floor(Math.random()*6)+1;
const reduce=matchMedia('(prefers-reduced-motion: reduce)').matches;

function face(el,v){
  el.dataset.v=v;el.setAttribute('aria-label','Kostka: '+v);
  el.innerHTML=PIPS[v].map(c=>`<span class="pip" style="grid-area:${Math.ceil(c/3)}/${(c-1)%3+1}"></span>`).join('');
}
function build(){
  $('dice').innerHTML='';
  for(let i=0;i<n;i++){const d=document.createElement('div');d.className='die';face(d,6);$('dice').appendChild(d)}
}
function render(){
  $('hist').innerHTML=hist.length?hist.slice(0,10).map(h=>`<span class="chip">${h.join(' + ')}${h.length>1?' = '+h.reduce((a,b)=>a+b):''}</span>`).join(''):'<span class="empty">Tady se objeví tvoje hody.</span>';
  const max=Math.max(1,...stats);
  $('bars').innerHTML=stats.map((s,i)=>`<div class="bar"><span>${s}</span><i style="height:${s/max*60}px"></i><span>${i+1}</span></div>`).join('');
  $('count').textContent='Hodů: '+rolls;
}
function roll(){
  if(busy)return;busy=true;$('roll').disabled=true;
  const dice=[...document.querySelectorAll('.die')];
  const result=dice.map(rnd);
  const finish=()=>{
    dice.forEach((d,i)=>{d.classList.remove('rolling');face(d,result[i])});
    result.forEach(v=>{stats[v-1]++;rolls++});
    hist.unshift(result);
    const sum=result.reduce((a,b)=>a+b);
    $('total').textContent=n>1?`Součet: ${sum}`:`Padlo ${sum}`;
    render();busy=false;$('roll').disabled=false;
  };
  if(reduce){finish();return}
  dice.forEach(d=>d.classList.add('rolling'));
  $('total').textContent='Kostky se valí…';
  let t=0;const tick=()=>{
    dice.forEach(d=>face(d,rnd()));
    if(++t<10)setTimeout(tick,60+t*14);else finish();
  };tick();
}
document.querySelectorAll('.seg button').forEach(b=>b.onclick=()=>{
  if(busy)return;n=+b.dataset.n;
  document.querySelectorAll('.seg button').forEach(x=>x.setAttribute('aria-pressed',x===b));
  build();$('total').textContent='Zatím jsi nehodil.';
});
$('roll').onclick=roll;
$('reset').onclick=()=>{hist=[];stats=[0,0,0,0,0,0];rolls=0;render();$('total').textContent='Zatím jsi nehodil.'};
addEventListener('keydown',e=>{if(e.code==='Space'&&document.activeElement.tagName!=='BUTTON'){e.preventDefault();roll()}});
const TABLES=[["Zelená","#2a6b57","#1c4d3e","#c9962f"],["Modrá","#2f5d8c","#1f4166","#d4a63a"],["Červená","#8c2f3b","#5f1d27","#d9b25a"],["Fialová","#5b3f8c","#3c2762","#d6a43f"],["Grafitová","#454b49","#1f2321","#c9962f"]];
const DICE=[["Slonovina","#fbf8ee","#1d2622","#b83a2a"],["Bílá","#ffffff","#222222","#d11f1f"],["Černá","#1d1d1f","#f4f0e2","#ff6b57"],["Červená","#b83a2a","#ffffff","#ffd23f"],["Modrá","#2f5d8c","#ffffff","#ffd23f"]];
let sel={t:0,d:0};
try{const sv=JSON.parse(localStorage.getItem('kostka-barvy'));if(sv&&TABLES[sv.t]&&DICE[sv.d])sel=sv}catch(e){}
function applyColors(){
  const r=document.documentElement.style,t=TABLES[sel.t],d=DICE[sel.d];
  r.setProperty('--felt',t[1]);r.setProperty('--felt-dark',t[2]);r.setProperty('--brass',t[3]);
  r.setProperty('--die',d[1]);r.setProperty('--pip',d[2]);r.setProperty('--pip1',d[3]);
  const mk=(el,list,key,sq)=>{el.innerHTML='';list.forEach((c,i)=>{const b=document.createElement('button');
    b.className='sw'+(sq?' sq':'');b.style.background=c[1];b.title=c[0];b.setAttribute('aria-label',c[0]);
    b.setAttribute('aria-pressed',sel[key]===i);b.onclick=()=>{sel[key]=i;applyColors()};el.appendChild(b)})};
  mk($('pt'),TABLES,'t',false);mk($('pd'),DICE,'d',true);
  try{localStorage.setItem('kostka-barvy',JSON.stringify(sel))}catch(e){}
}
applyColors();build();render();
</script>
</body>
</html>
