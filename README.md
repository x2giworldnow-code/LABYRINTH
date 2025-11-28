# LABYRINTH
EXPLORE THE MAZE OF THE VOID
index.html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Door Game</title>

<style>
  body {
    margin: 0;
    background: #0c0d12;
    font-family: Arial, sans-serif;
    color: #dfe6ff;
  }

  .container {
    display: flex;
    justify-content: center;
    padding: 40px;
    gap: 30px;
  }

  .game {
    width: 900px;
    background: rgba(20, 22, 30, 0.8);
    border: 1px solid rgba(255,255,255,0.05);
    border-radius: 10px;
    padding: 20px;
  }

  h1 {
    margin: 0;
    font-size: 22px;
    color: #9fb6ff;
  }

  .subtitle {
    color: #9aa0a6;
    margin: 5px 0 20px;
  }

  .door-row {
    display: flex;
    justify-content: center;
    gap: 25px;
    margin-top: 20px;
  }

  .door {
    width: 220px;
    height: 330px;
    background: rgba(255,255,255,0.05);
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 0 20px rgba(0,0,0,0.6);
    cursor: pointer;
    transition: transform 0.3s ease;
  }

  .door:hover {
    transform: scale(1.05);
  }

  .door img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .door-label {
    margin-top: 8px;
    color: #dfe6ff;
    text-align: center;
  }

  .journal {
    width: 330px;
    background: rgba(20,22,30,0.8);
    padding: 15px;
    border-radius: 10px;
    border: 1px solid rgba(255,255,255,0.05);
  }

  .journal h2 {
    margin-top: 0;
    font-size: 18px;
    color: #9fb6ff;
  }
</style>
</head>

<body>

<div class="container">

  <div class="game">
    <h1 id="title">Concrete Sleeper</h1>
    <div class="subtitle" id="subtitle">You don't know how to wake up.</div>

    <div class="door-row" id="doorRow"></div>

    <h2>Sanity: <span id="sanity">100</span></h2>
  </div>

  <div class="journal">
    <h2>Journal</h2>
    <div id="journalText">You awaken inside concrete walls…</div>
  </div>

</div>

<script>
/* --- SCENE DATA (simple version) --- */
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

  a1: {
    title: "Low Groove",
    subtitle: "The hum gets closer.",
    sanityDelta: -5,
    journal: "A vibration crawls under your feet.",
    doors: []
  },
  b1: {
    title: "Glass Hiss",
    subtitle: "Your reflection moves wrong.",
    sanityDelta: -10,
    journal:

<img width="900" height="1920" alt="door 3" src="https://github.com/user-attachments/assets/8b695b68-b7c5-47e4-bf58-4ff9b38d1b3d" />
<img width="900" height="1920" alt="door 2" src="https://github.com/user-attachments/assets/93c1aebc-459d-49a4-bc7e-c03a8473a9d7" />
<img width="900" height="1920" alt="DOOR 1111" src="https://github.com/user-attachments/assets/737b1797-eb57-4cb2-a056-d6ad290c0358" />
