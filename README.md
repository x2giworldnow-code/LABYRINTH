# LABYRINTH
EXPLORE THE MAZE OF THE VOID
index.html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Door Game</title>

<style>
  body {
    margin: 0;
    height: 100%;
    background: radial-gradient(circle at center, #1a1a25, #060609 80%);
    font-family: Arial, sans-serif;
    color: #dfe6ff;
  }

  .container {
    display: flex;
    justify-content: center;
    padding: 40px;
    gap: 30px;
  }

  /* LEFT PANEL */
  .game {
    width: 900px;
    background: rgba(10,10,15,0.6);
    border: 1px solid rgba(255,255,255,0.05);
    border-radius: 10px;
    padding: 20px;
    position: relative;
    overflow: hidden;
  }

  /* Fog overlay */
  .fog {
    position: absolute;
    inset: 0;
    background: url('https://i.imgur.com/k4WVyKM.png');
    opacity: 0.07;
    mix-blend-mode: screen;
    animation: fogMove 25s linear infinite;
    pointer-events: none;
  }
  @keyframes fogMove {
    from { transform: translateX(-10%); }
    to { transform: translateX(10%); }
  }

  h1 {
    margin: 0;
    font-size: 22px;
    color: #9fb6ff;
  }
  h2 {
    margin-top: 25px;
    font-size: 16px;
    color: #9fb6ff;
  }

  .subtitle {
    color: #9aa0a6;
    margin: 5px 0 20px;
  }

  /* Door layout */
  .door-row {
    display: flex;
    gap: 25px;
    margin-top: 20px;
  }

  .door {
    width: 220px;
    height: 330px;
    background: rgba(255,255,255,0.05);
    border-radius: 10px;
    overflow: hidden;
    cursor: pointer;
    position: relative;
    box-shadow: 0 0 40px rgba(0,0,0,0.6);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .door:hover {
    transform: scale(1.06);
    box-shadow: 0 0 50px rgba(150,150,255,0.4);
  }

  .door img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: brightness(0.8) contrast(1.1);
    transition: filter 0.3s;
  }

  .door:hover img {
    filter: brightness(1.1) contrast(1.2);
  }

  .door-label {
    position: absolute;
    bottom: 8px;
    left: 0;
    right: 0;
    text-align: center;
    color: #dfe6ff;
    text-shadow: 0 0 5px black;
    font-size: 14px;
    padding: 3px;
  }

  /* Right journal panel */
  .journal {
    width: 330px;
    background: rgba(10,10,15,0.7);
    padding: 15px;
    border-radius: 10px;
    border: 1px solid rgba(255,255,255,0.05);
  }
</style>
</head>

<body>

<div class="container">

  <!-- LEFT GAME PANEL -->
  <div class="game">

    <div class="fog"></div>

    <h1 id="title">Concrete Sleeper</h1>
    <div class="subtitle" id="subtitle">You don't know how to wake up.</div>

    <div class="door-row" id="doorRow"></div>

    <h2>Sanity: <span id="sanity">100</span></h2>

  </div>

  <!-- RIGHT JOURNAL PANEL -->
  <div class="journal">
    <h2>Journal</h2>
    <div id="journalText">You awaken inside concrete walls…</div>
  </div>

</div>

<script>
/* ---------------- SCENE DATA ---------------- */
const SCENES = {
  start: {
    title: "Concrete Sleeper",
    subtitle: "Cold halls. Something hums. Choose a door.",
    sanity: 100,
    journal: "You awaken inside concrete walls. Something clicks behind you.",
    doors: [
      { label: "Low Groove", img: "door1.png", outcome: "a1" },
      { label: "Glass Hiss", img: "door2.png", outcome: "b1" },
      { label: "Rust Crack", img: "door3.png", outcome: "c1" }
    ]
  },

  /* Example outcomes */
  a1: {
    title: "Low Groove",
    subtitle: "A vibration crawls under the floor.",
    sanityDelta: -5,
    journal: "You feel something pacing beneath the concrete.",
    doors: []
  },
  b1: {
    title: "Glass Hiss",
    subtitle: "Your reflection doesn't match your face.",
    sanityDelta: -10,
    journal: "You blink. The reflection blinks later.",
    doors: []
  },
  c1: {
    title: "Rust Crack",
    subtitle: "Something metallic breathes behind the door.",
    sanityDelta: -8,
    journal: "Metal groans. Something is alive in the wall.",
    doors: []
  }
};

/* ------------- STATE ------------- */
let state = {
  node: "start",
  sanity: 100,
  journal: []
};

/* ---------------- RENDER ---------------- */
function render() {
  const data = SCENES[state.node];

  document.getElementById("title").textContent = data.title;
  document.getElementById("subtitle").textContent = data.subtitle;

  document.getElementById("sanity").textContent = state.sanity;
  document.getElementById("journalText").textContent =
    state.journal.join("\n\n") || data.journal;

  const row = document.getElementById("doorRow");
  row.innerHTML = "";

  data.doors.forEach(d => {
    const el = document.createElement("div");
    el.className = "door";
    el.innerHTML = `
      <img src="${d.img}" alt="${d.label}">
      <div class="door-label">${d.label}</div>
    `;
    el.onclick = () => chooseDoor(d.outcome);
    row.appendChild(el);
  });
}

/* ---------------- ACTION ---------------- */
function chooseDoor(target) {
  const next = SCENES[target];

  if (!next) return;

  state.node = target;

  if (next.sanityDelta !== undefined) {
    state.sanity = Math.max(0, Math.min(100, state.sanity + next.sanityDelta));
  }
  if (next.journal) state.journal.unshift(next.journal);

  render();
}

/* ---------------- INIT ---------------- */
render();
</script>

</body>
</html>
<img width="900" height="1920" alt="door 3" src="https://github.com/user-attachments/assets/8b695b68-b7c5-47e4-bf58-4ff9b38d1b3d" />
<img width="900" height="1920" alt="door 2" src="https://github.com/user-attachments/assets/93c1aebc-459d-49a4-bc7e-c03a8473a9d7" />
<img width="900" height="1920" alt="DOOR 1111" src="https://github.com/user-attachments/assets/737b1797-eb57-4cb2-a056-d6ad290c0358" />
