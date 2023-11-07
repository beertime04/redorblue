<!DOCTYPE html>
<html lang="en">
<head>
  <title>Blue or red?</title>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Foldit:wght@500">
  <style>
    /* Stili CSS rimangono invariati */
  </style>
</head>
<body>
  <h1>Blue or red?</h1>

  <div class="buttons">
    <button type="button" class="button blue">
      <span>Blue</span>
    </button>
    <button type="button" class="button red">
      <span>Red</span>
    </button>
  </div>

  <div class="counter">
    <p>Blue: <span id="counter-blue">0</span></p>
    <p>Red: <span id="counter-red">0</span></p>
  </div>

  <div class="letters"></div>

  <script>
    const buttons = document.querySelectorAll(".button");
    const counterBlue = document.getElementById("counter-blue");
    const counterRed = document.getElementById("counter-red");
    const lettersDiv = document.querySelector(".letters");
    let isAnimationStarted = false;

    // Imposta un limite massimo di lettere
    const maxLetters = 1000;
    let letterCount = 0;

    function createRandomLetter() {
      const alphabet = "abcdefghijklmnopqrstuvwxyz가나다라마바사아자차카타파하абвгдеёжзийклмнопрстуфхцчшщъыьэюя";
      const randomIndex = Math.floor(Math.random() * alphabet.length);
      return alphabet[randomIndex];
    }

    function checkOverlap(letterElement, existingLetters) {
      const letterRect = letterElement.getBoundingClientRect();

      for (const existingLetter of existingLetters) {
        const existingRect = existingLetter.getBoundingClientRect();

        if (
          letterRect.left < existingRect.right &&
          letterRect.right > existingRect.left &&
          letterRect.top < existingRect.bottom &&
          letterRect.bottom > existingRect.top
        ) {
          // Sovrapposizione rilevata, sposta la lettera
          const randomX = Math.random() * 280;
          const randomY = Math.random() * 280;
          letterElement.style.left = `${randomX}%`;
          letterElement.style.top = `${randomY}%`;
          return checkOverlap(letterElement, existingLetters);
        }
      }
    }

    function addLetter() {
      if (isScreenFull()) {
        return;
      }
      const letter = createRandomLetter();
      const letterElement = document.createElement("div");
      letterElement.classList.add("letter");
      letterElement.textContent = letter;

      const randomX = Math.random() * 270;
      const randomY = Math.random() * 270;

      letterElement.style.left = `${randomX}%`;
      letterElement.style.top = `${randomY}%`;

      checkOverlap(letterElement, lettersDiv.querySelectorAll('.letter'));

      lettersDiv.appendChild(letterElement);
      letterCount++; // Incrementa il conteggio delle lettere
    }

    function startAnimation() {
      isAnimationStarted = true;
      addLetter();
      setInterval(addLetter, 0.20); // Cambia da 100 a 20 millisecondi per rendere l'animazione più veloce
    }

    function isScreenFull() {
      return letterCount >= 3000;
    }

    function makeSiteGreen() {
      document.body.style.backgroundColor = "green";
    }

    function handleTouchStart(event) {
      if (!isAnimationStarted) {
        startAnimation();
      }

      const button = event.target;
      const color = button.classList.contains("blue") ? "blue" : "red";

      if (color === "blue") {
        counterBlue.textContent = parseInt(counterBlue.textContent) + 1;
      } else {
        counterRed.textContent = parseInt(counterRed.textContent) + 1;
      }

      button.disabled = true;

      if (button.classList.contains("blue")) {
        buttons[1].disabled = true;
      } else {
        buttons[0].disabled = true;
      }
    }

    // Aggiungi gli eventi "touchstart" ai bottoni
    buttons[0].addEventListener("touchstart", handleTouchStart);
    buttons[1].addEventListener("touchstart", handleTouchStart);
  </script>
</body>
</html>
