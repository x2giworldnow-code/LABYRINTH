# LABYRINTH
EXPLORE THE MAZE OF THE VOID
index.html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Doors — Prototype</title>
<style>
  :root{
    --bg:#0b0b0e; --card:#121216; --accent:#9fb6ff;
    --muted:#9aa0a6; --glass: rgba(255,255,255,0.03);
  }
  html,body{height:100%;margin:0;background:linear-gradient(180deg,#06060a 0%, #0f0f12 100%);font-family:Inter,system-ui,Segoe UI,Arial;}
  .stage{height:100%;display:flex;align-items:center;justify-content:center;padding:3rem;box-sizing:border-box;gap:2rem;}
  .panel{width:980px;max-width:100%;display:flex;gap:2rem;align-items:stretch;}
  .scene{flex:1;background:linear-gradient(180deg,var(--card),#0b0b0e);border-radius:12px;padding:20px;box-shadow:0 8px 30px rgba(2,6,23,0.7);display:flex;flex-direction:column;gap:12px;}
  header{display:flex;align-items:center;gap:12px;}
  h1{color:var(--accent);margin:0;font-size:18px;letter-spacing:1px}
  p.desc{color:var(--muted);margin:0;font-size:14px}
  .room{flex:1;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.05));border-radius:8px;padding:18px;display:flex;gap:16px;align-items:center;justify-content:center;position:relative;overflow:hidden;}
  .doors{display:flex;gap:28px;align-items:flex-end;}
  .door{width:160px;height:260px;border-radius:8px;background:var(--glass);backdrop-filter: blur(2px);display:flex;flex-direction:column;align-items:center;justify-content:flex-end;padding:14px;box-shadow:inset 0 -20px 40px rgba(0,0,0,0.6);cursor:pointer;transition:transform .28s cubic-bezier(.2,.9,.2,1), box-shadow .2s;}
  .door:hover{transform:translateY(-8px) scale(1.02); box-shadow:0 18px 40px rgba(0,0,0,0.6);}
  .door .label{color:var(--muted);font-size:13px;margin-bottom:6px;text-align:center}
  .door .glyph{width:86px;height:86px;border-radius:6px;background: linear-gradient(180deg,#111214, #171719);display:flex;align-items:center;justify-content:center;color:var(--accent);font-weight:700;box-shadow:0 6px 18px rgba(0,0,0,0.6)}
  .log{width:360px;background:linear-gradient(180deg,#0b0b0e, #0a0a0d);border-radius:12px;padding:16px;color:var(--muted);font-size:14px;box-shadow:0 8px 30px rgba(2,6,23,.6);}
  .log h2{color:var(--accent);margin:0 0 8px 0;font-size:15px}
  .btns{display:flex;gap:8px;margin-top:12px}
  button.small{background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted);padding:8px 10px;border-radius:8px;cursor:pointer}
  .overlay{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;pointer-events:none}
  .flash{position:absolute;inset:0;background:radial-gradient(circle at 30% 20%, rgba(159,182,255,0.06), transparent 20%);pointer-events:none;mix-blend-mode:screen;}
  .scene-text{color:#dfe6ff;font-size:15px;line-height:1.5}
  .meta{font-size:13px;color:var(--muted)}
  /* simple transition screens */
  .scene-card{position:absolute;inset:0;background:linear-gradient(180deg,#09090b, #050507);display:flex;align-items:center;justify-content:center;color:var(--accent);font-size:20px;transform:scale(.98);opacity:0;transition:all .45s ease;pointer-events:none}
  .scene-card.show{opacity:1;transform:scale(1);pointer-events:auto}
</style>
</head>
<body>
<div class="stage">
  <div class="panel">
    <div class="scene" id="mainScene" aria-live="polite">
      <header>
        <h1 id="title">Concrete Sleeper</h1>
        <p class="desc" id="subtitle">You don't know how to wake up. Doors hum in the walls.</p>
      </header>
      <div class="room" id="room">
        <div class="doors" id="doors"></div>
        <div class="overlay flash" aria-hidden="true"></div>
        <div class="scene-card" id="sceneCard"></div>
      </div>
      <div class="meta" style="display:flex;justify-content:space-between;align-items:center">
        <div>Sanity: <span id="sanity">100</span></div>
        <div class="meta">Save: <span id="autosave">—</span></div>
      </div>
    </div>

    <aside class="log" aria-live="polite">
      <h2>Journal</h2>
      <div id="journal">You awake in concrete. The clicking returns.</div>
      <div class="btns">
        <button class="small" id="saveBtn">Save</button>
        <button class="small" id="resetBtn">Reset</button>
      </div>
    </aside>
  </div>
</div>

<script>
/* ---- Scene data (authorable JSON) ---- */
const SCENES = {
  start: {
    title: "Concrete Sleeper",
    subtitle: "Cold halls. Something hums. Choose a door.",
    sanity: 100,
    journal: "You awake in concrete. The clicking returns.",
    doors: [
      { id: "door_a", label: "Low Groove", glyph: "I", outcome: "a1", hint: "A soft rhythm behind the wall." },
      { id: "door_b", label: "Glass Hiss", glyph: "II", outcome: "b1", hint: "A reflection that isn't yours." },
      { id: "door_c", label: "Rust Crack", glyph: "III", outcome: "c1", hint: "Metal teeth turning in the dark." }
    ]
  },
  a1: {
    title: "Low Groove",
    subtitle: "The floor vibrates with an internal drum.",
    sanityDelta: -6,
    journal: "You feel the drum in your chest. The rabbit listens.",
    doors: [
      { id: "door_d", label: "Warm Leak", glyph: "∴", outcome: "d1", hint: "A soft warmth beyond the slab." },
      { id: "door_e", label: "Narrow Light", glyph: "●", outcome: "e1", hint: "A slit of light like an eye." }
    ]
  },
  b1: {
    title: "Glass Hiss",
    subtitle: "Mirrors line the corridor and they don't match.",
    sanityDelta: -12,
    journal: "You watch your reflection blink first.",
    doors: [
      { id: "door_f", label: "Shimmer", glyph: "~", outcome: "f1", hint: "A shimmer that lures your thumb." },
      { id: "door_g", label: "Offline", glyph: "⊗", outcome: "g1", hint: "A dark frame with static." }
    ]
  },
  c1: {
    title: "Rust Crack",
    subtitle: "The air tastes like old batteries and rain.",
    sanityDelta: -4,
    journal: "A metallic groan suggests something sharpening in the walls.",
    doors: [
      { id: "door_h", label: "Throat", glyph: "Y", outcome: "h1", hint: "A narrow throat with breath." },
      { id: "door_i", label: "Hollow", glyph: "0", outcome: "i1", hint: "An echo that returns faster than you speak." }
    ]
  },
  // sample endings / nodes
  d1: { title:"Warm Leak", subtitle:"You find a soft cloth and a faint heartbeat.", sanityDelta:+8, journal:"A heartbeat. Not yours.", doors:[] },
  e1: { title:"Narrow Light", subtitle:"The slit reveals an eye. It blinks.", sanityDelta:-20, journal:"The eye knows you.", doors:[] },
  f1: { title:"Shimmer", subtitle:"You touch the shimmer and your fingers ache with distant static.", sanityDelta:-8, journal:"Static in the blood.", doors:[] },
  g1: { title:"Offline", subtitle:"A room full of sleeping machines.", sanityDelta:+12, journal:"Machines hum like lullabies.", doors:[] },
  h1: { title:"Throat", subtitle:"You crawl. The walls move with you.", sanityDelta:-18, journal:"The walls are patient.", doors:[] },
  i1: { title:"Hollow", subtitle:"An empty chamber with a single light.", sanityDelta:+10, journal:"Silence like an offering.", doors:[] }
};

/* ---- State management ---- */
const STORAGE_KEY = "doors_prototype_v1";
let state = loadState() || { node:"start", sanity:100, journal:[] };

/* ---- Utils ---- */
function nowISO(){ return new Date().toISOString().slice(0,19).replace('T',' '); }
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); document.getElementById('autosave').textContent = nowISO(); }
function loadState(){ try{ return JSON.parse(localStorage.getItem(STORAGE_KEY)); }catch(e){return null;} }
function resetState(){ localStorage.removeItem(STORAGE_KEY); state = { node:"start", sanity:100, journal:[] }; render(); }

/* ---- Rendering ---- */
const doorsEl = document.getElementById('doors');
const titleEl = document.getElementById('title');
const subtitleEl = document.getElementById('subtitle');
const journalEl = document.getElementById('journal');
const sanityEl = document.getElementById('sanity');
const sceneCard = document.getElementById('sceneCard');

function render(){
  const node = state.node;
  const data = SCENES[node];
  titleEl.textContent = data.title || "…";
  subtitleEl.textContent = data.subtitle || "";
  sanityEl.textContent = state.sanity;
  // journal
  const jstr = (state.journal.length? state.journal.join('\n\n') : data.journal || "");
  journalEl.textContent = jstr;
  // doors
  doorsEl.innerHTML = "";
  (data.doors || []).forEach(d => {
    const el = document.createElement('div');
    el.className = "door";
    el.tabIndex = 0;
    el.setAttribute('role','button');
    el.setAttribute('aria-label', d.label + " — " + d.hint);
    el.innerHTML = `<div class="glyph">${d.glyph}</div><div class="label">${d.label}</div>`;
    el.addEventListener('click', ()=> chooseDoor(d));
    el.addEventListener('keydown', (e)=>{ if(e.key === "Enter" || e.key === " ") chooseDoor(d); });
    doorsEl.appendChild(el);
  });

  // if no doors: show ending card
  if(!(data.doors && data.doors.length)){
    sceneCard.textContent = data.subtitle || data.title || "End";
    sceneCard.classList.add('show');
    // end state — add journal entry
    if(!state.journal.includes(data.journal || "")){
      if(data.journal) state.journal.unshift( (new Date()).toLocaleString() + " — " + data.journal );
      saveState();
    }
  } else {
    sceneCard.classList.remove('show');
  }
}

/* ---- Game actions ---- */
function chooseDoor(door){
  // small visual feedback
  flashRoom();
  // get next node
  const current = SCENES[state.node];
  const next = SCENES[door.outcome];
  // animate card with incoming title
  sceneCard.textContent = "…";
  sceneCard.classList.add('show');
  setTimeout(()=> {
    // apply outcome
    if(next){
      state.node = door.outcome;
      // apply sanity change if present
      if(typeof next.sanityDelta === "number"){
        state.sanity = Math.max(0, Math.min(100, state.sanity + next.sanityDelta));
      }
      // add journal
      if(next.journal) state.journal.unshift( (new Date()).toLocaleString() + " — " + next.journal );
      saveState();
      render();
      // small delay then hide card
      sceneCard.textContent = next.title || "";
      setTimeout(()=> sceneCard.classList.remove('show'), 700);
    } else {
      // fallback: dead end
      state.journal.unshift( (new Date()).toLocaleString() + " — " + "The door leads to nothing." );
      saveState();
      render();
      setTimeout(()=> sceneCard.classList.remove('show'), 600);
    }
  }, 300);
}

function flashRoom(){
  const overlay = document.querySelector('.flash');
  overlay.style.transition = 'none';
  overlay.style.opacity = '1';
  overlay.style.transform = 'scale(1.03)';
  setTimeout(()=> {
    overlay.style.transition = 'opacity .8s ease, transform .8s ease';
    overlay.style.opacity = '0';
    overlay.style.transform = 'scale(1)';
  }, 60);
}

/* ---- Bind UI ---- */
document.getElementById('saveBtn').addEventListener('click', ()=>{
  saveState();
  alert('Saved.');
});
document.getElementById('resetBtn').addEventListener('click', ()=>{
  if(confirm('Reset progress?')) resetState();
});

/* ---- Init ---- */
if(!loadState()) saveState();
render();
</script>
</body>
</html>
