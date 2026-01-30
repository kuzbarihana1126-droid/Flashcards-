<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Finnish Verb Flashcards</title>

<style>
:root {
  --bg: #f5f5dc;
  --card: #ffffff;
  --text: #000000;
  --muted: #555555;
  --accent: #2563eb;
  --success: #15803d;
  --danger: #b91c1c;
  --border: rgba(0,0,0,0.15);
}

body {
  margin: 0;
  font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.app {
  width: 100%;
  max-width: 520px;
  background: #ffffff;
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 22px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.15);
}

.header {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 14px;
}

h1 {
  font-size: 18px;
  margin: 0;
}

.subtitle {
  font-size: 13px;
  color: var(--muted);
  margin-top: 4px;
}

.progress {
  font-size: 13px;
  color: var(--muted);
}

.card {
  margin-top: 16px;
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 18px;
  min-height: 170px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10px;
}

.label {
  font-size: 12px;
  color: var(--muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.word {
  font-size: 28px;
  font-weight: 700;
}

.answer {
  font-size: 18px;
  font-weight: 600;
  color: var(--success);
  display: none;
}

.hint {
  font-size: 13px;
  color: var(--muted);
}

.controls,
.controls2 {
  display: grid;
  gap: 10px;
  margin-top: 14px;
}

.controls {
  grid-template-columns: 1fr 1fr;
}

.controls2 {
  grid-template-columns: 1fr 1fr 1fr;
}

button {
  cursor: pointer;
  border: 1px solid var(--border);
  background: #f9f9f9;
  color: var(--text);
  padding: 10px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 600;
}

button:hover {
  background: #eeeeee;
}

.primary {
  background: var(--accent);
  color: white;
  border: none;
}

.primary:hover {
  background: #1d4ed8;
}

.danger {
  background: var(--danger);
  color: white;
  border: none;
}

.footer {
  margin-top: 14px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}

.toggle {
  display: flex;
  gap: 8px;
  align-items: center;
  font-size: 13px;
  color: var(--muted);
}

.small {
  font-size: 13px;
  color: var(--muted);
  margin-top: 14px;
}
</style>
</head>

<body>
<div class="app">
  <div class="header">
    <div>
      <h1>Finnish Verb Flashcards 🇫🇮</h1>
      <div class="subtitle">Basic verbs for everyday communication</div>
    </div>
    <!-- FIX: no hard-coded number -->
    <div class="progress" id="progressText"></div>
  </div>

  <div class="card">
    <div class="label">Finnish verb</div>
    <div class="word" id="questionText"></div>

    <div class="label" style="margin-top:10px;">English meaning</div>
    <div class="answer" id="answerText"></div>

    <div class="hint" id="hintText"></div>
  </div>

  <div class="controls">
    <button class="primary" id="revealBtn">Reveal</button>
    <button id="hideBtn">Hide</button>
  </div>

  <div class="controls2">
    <button id="prevBtn">⬅ Previous</button>
    <button id="shuffleBtn">🔀 Shuffle</button>
    <button id="nextBtn">Next ➡</button>
  </div>

  <div class="footer">
    <div class="toggle">
      <input type="checkbox" id="autoRevealToggle" />
      <label for="autoRevealToggle">Auto-reveal</label>
    </div>
    <button class="danger" id="resetBtn">Reset</button>
  </div>

  <div class="small">
    💡 Beginner-friendly Finnish verb flashcards
  </div>
</div>

<script>
const originalDeck = [
  { fi: "olla", en: "to be" },
  { fi: "tehdä", en: "to do / to make" },
  { fi: "mennä", en: "to go" },
  { fi: "tulla", en: "to come" },
  { fi: "nähdä", en: "to see" },
  { fi: "kuulla", en: "to hear" },
  { fi: "puhua", en: "to speak" },
  { fi: "sanoa", en: "to say" },
  { fi: "kysyä", en: "to ask" },
  { fi: "vastata", en: "to answer" },
  { fi: "syödä", en: "to eat" },
  { fi: "juoda", en: "to drink" },
  { fi: "nukkua", en: "to sleep" },
  { fi: "herätä", en: "to wake up" },
  { fi: "kirjoittaa", en: "to write" },
  { fi: "lukea", en: "to read" },
  { fi: "opiskella", en: "to study" },
  { fi: "oppia", en: "to learn" },
  { fi: "työskennellä", en: "to work" },
  { fi: "asua", en: "to live" },
  { fi: "käydä", en: "to visit" },
  { fi: "ajaa", en: "to drive" },
  { fi: "matkustaa", en: "to travel" },
  { fi: "ottaa", en: "to take" },
  { fi: "antaa", en: "to give" },
  { fi: "saada", en: "to get" },
  { fi: "tuoda", en: "to bring" },
  { fi: "viedä", en: "to take away" },
  { fi: "ostaa", en: "to buy" },
  { fi: "myydä", en: "to sell" },
  { fi: "maksaa", en: "to pay" },
  { fi: "löytää", en: "to find" },
  { fi: "etsiä", en: "to look for" },
  { fi: "auttaa", en: "to help" },
  { fi: "tarvita", en: "to need" },
  { fi: "haluta", en: "to want" },
  { fi: "pitää", en: "to like / must" },
  { fi: "rakastaa", en: "to love" },
  { fi: "muistaa", en: "to remember" },
  { fi: "unohtaa", en: "to forget" },
  { fi: "ymmärtää", en: "to understand" },
  { fi: "aloittaa", en: "to start" },
  { fi: "lopettaa", en: "to finish" },
  { fi: "jatkaa", en: "to continue" },
  { fi: "odottaa", en: "to wait" },
  { fi: "soittaa", en: "to call / play music" },
  { fi: "avata", en: "to open" },
  { fi: "sulkea", en: "to close" }
];

let deck = [...originalDeck];
let index = 0;
let revealed = false;

const q = document.getElementById("questionText");
const a = document.getElementById("answerText");
const hint = document.getElementById("hintText");
const progress = document.getElementById("progressText");
const autoReveal = document.getElementById("autoRevealToggle");

function updateCard() {
  q.textContent = deck[index].fi;
  a.textContent = deck[index].en;
  progress.textContent = `${index + 1} / ${deck.length}`;
  revealed = autoReveal.checked;
  updateReveal();
}

function updateReveal() {
  a.style.display = revealed ? "block" : "none";
  hint.textContent = revealed
    ? "Click Hide to test yourself."
    : "Click Reveal to see the meaning.";
}

document.getElementById("revealBtn").onclick = () => { revealed = true; updateReveal(); };
document.getElementById("hideBtn").onclick = () => { revealed = false; updateReveal(); };
document.getElementById("nextBtn").onclick = () => { index = (index + 1) % deck.length; updateCard(); };
document.getElementById("prevBtn").onclick = () => { index = (index - 1 + deck.length) % deck.length; updateCard(); };
document.getElementById("shuffleBtn").onclick = () => {
  deck.sort(() => Math.random() - 0.5);
  index = 0;
  updateCard();
};
document.getElementById("resetBtn").onclick = () => {
  deck = [...originalDeck];
  index = 0;
  autoReveal.checked = false;
  updateCard();
};

updateCard();
</script>
</body>
</html>
