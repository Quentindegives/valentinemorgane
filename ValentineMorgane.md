<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Proposition Valentine</title>

<style>
  body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    min-height: 100vh;
    margin: 0;
    padding: 20px;
    font-family: sans-serif;
    background: #fff0f5;
    text-align: center;
  }

  h1, h2 {
    margin: 20px 0;
  }

  .button-container {
    position: relative;
    width: 100%;
    max-width: 400px;
    height: 200px;
    margin: 20px 0;
  }

  button {
    padding: 15px 30px;
    font-size: 1rem;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    position: absolute;
    transition: all 0.3s ease;
  }

  #yes {
    background-color: #4CAF50;
    color: white;
  }

  #no {
    background-color: #f44336;
    color: white;
  }

  #counter {
    margin-top: 10px;
    font-size: 1.1rem;
  }

  .hidden {
    display: none;
  }

  #congrats {
    color: #e91e63;
    font-size: 1.6rem;
    margin-top: 40px;
    animation: fadeIn 0.6s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: scale(0.9); }
    to   { opacity: 1; transform: scale(1); }
  }
</style>
</head>

<body>

<h1 id="mainTitle">Morgane, veux-tu être ma valentine ?</h1>

<div class="button-container" id="buttons">
  <button id="yes">Oui</button>
  <button id="no">Non</button>
</div>

<div id="counter">Nombre de "Non" : 0</div>

<h2 id="question2" class="hidden">Même si je t'apporte du café ?</h2>
<h2 id="question3" class="hidden">Et si je rajoute du chocolat ?</h2>
<h2 id="finalMessage" class="hidden">Tu es déterminée je vois !</h2>
<h2 id="extraMessage" class="hidden">Tu crois qu'il y a encore quelque chose plus loin ?</h2>

<div id="congrats" class="hidden">
  Félicitation Morgane,<br>
  tu es ma valentine ❤️
</div>

<!-- Formulaire invisible -->
<form id="valentineForm"
      action="https://formspree.io/f/xlgwdwya"
      method="POST"
      class="hidden">
  <input type="hidden" name="reponse" value="Oui">
  <input type="hidden" name="nombreNon" id="formNoCount" value="0">
</form>

<script>
const yesBtn = document.getElementById('yes');
const noBtn = document.getElementById('no');
const counterDisplay = document.getElementById('counter');
const question2 = document.getElementById('question2');
const question3 = document.getElementById('question3');
const finalMessage = document.getElementById('finalMessage');
const extraMessage = document.getElementById('extraMessage');
const congrats = document.getElementById('congrats');
const valentineForm = document.getElementById('valentineForm');
const formNoCount = document.getElementById('formNoCount');
const buttons = document.getElementById('buttons');
const mainTitle = document.getElementById('mainTitle');

let noCount = 0;
let yesScale = 1;

function moveNo() {
  const container = document.querySelector('.button-container');
  const maxX = container.clientWidth - noBtn.offsetWidth;
  const maxY = container.clientHeight - noBtn.offsetHeight;

  let x, y, collision;
  do {
    x = Math.random() * maxX;
    y = Math.random() * maxY;
    collision =
      x < yesBtn.offsetLeft + yesBtn.offsetWidth &&
      x + noBtn.offsetWidth > yesBtn.offsetLeft &&
      y < yesBtn.offsetTop + yesBtn.offsetHeight &&
      y + noBtn.offsetHeight > yesBtn.offsetTop;
  } while (collision);

  noBtn.style.left = x + 'px';
  noBtn.style.top = y + 'px';
}

noBtn.addEventListener('click', () => {
  noCount++;
  counterDisplay.textContent = `Nombre de "Non" : ${noCount}`;
  moveNo();

  yesScale += 0.05;
  yesBtn.style.transform = `translate(-50%, -50%) scale(${yesScale})`;

  if (noCount >= 3) question2.classList.remove('hidden');
  if (noCount >= 7) question3.classList.remove('hidden');
  if (noCount >= 50) finalMessage.classList.remove('hidden');
  if (noCount >= 100) extraMessage.classList.remove('hidden');
});

// Centrage initial du Oui
yesBtn.style.left = '50%';
yesBtn.style.top = '50%';
yesBtn.style.transform = 'translate(-50%, -50%) scale(1)';

// CLIC SUR OUI
yesBtn.addEventListener('click', () => {

  // Masquer tout le reste
  buttons.classList.add('hidden');
  counterDisplay.classList.add('hidden');
  question2.classList.add('hidden');
  question3.classList.add('hidden');
  finalMessage.classList.add('hidden');
  extraMessage.classList.add('hidden');
  mainTitle.classList.add('hidden');

  // Afficher félicitations
  congrats.classList.remove('hidden');

  // Délai avant envoi du formulaire (3 secondes)
  formNoCount.value = noCount;
  setTimeout(() => {
    valentineForm.submit();
  }, 5000);
});
</script>

</body>
</html>
