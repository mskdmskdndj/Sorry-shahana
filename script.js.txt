document.addEventListener('DOMContentLoaded', () => {
  const loadingScreen = document.getElementById('loading-screen');
  const countdownScreen = document.getElementById('countdown-screen');
  const countdownNumber = document.getElementById('countdown-number');
  const matrixCanvas = document.getElementById('matrix-canvas');
  const catAnimation = document.getElementById('cat-animation');
  const cardContainer = document.getElementById('card-container');
  const card = document.getElementById('card');
  const music = document.getElementById('background-music');
  const quoteScreen = document.getElementById('quote-screen');

  setTimeout(() => {
    loadingScreen.classList.add('hidden');
    countdownScreen.classList.remove('hidden');
    startCountdown();
  }, 3000);

  function startCountdown() {
    let count = 3;
    const interval = setInterval(() => {
      countdownNumber.textContent = count;
      if (count === 1) {
        clearInterval(interval);
        setTimeout(() => {
          countdownScreen.classList.add('hidden');
          matrixCanvas.classList.remove('hidden');
          runMatrixAnimation();
        }, 1000);
      }
      count--;
    }, 1000);
  }

  function runMatrixAnimation() {
    const canvas = matrixCanvas;
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const letters = 'SORRY';
    const fontSize = 16;
    const columns = canvas.width / fontSize;
    const drops = Array(Math.floor(columns)).fill(1);

    const targetText = "💗SORRY SHAHANA💗";
    let revealedText = "";
    let textIndex = 0;

    function draw() {
      ctx.fillStyle = 'rgba(255, 255, 255, 0.05)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = '#ff69b4';
      ctx.font = `${fontSize}px 'Segoe UI', cursive`;

      for (let i = 0; i < drops.length; i++) {
        const text = letters[Math.floor(Math.random() * letters.length)];
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);

        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
          drops[i] = 0;
        }
        drops[i]++;
      }

      if (textIndex < targetText.length) {
        revealedText += targetText[textIndex];
        textIndex++;
      }

      ctx.fillStyle = '#ff1493';
      ctx.font = 'bold 4vw "Dancing Script", cursive';
      const textWidth = ctx.measureText(revealedText).width;
      ctx.fillText(revealedText, (canvas.width - textWidth) / 2, canvas.height / 2);
    }

    const matrixInterval = setInterval(draw, 50);

    setTimeout(() => {
      clearInterval(matrixInterval);
      matrixCanvas.classList.add('hidden');
      catAnimation.classList.remove('hidden');

      setTimeout(() => {
        catAnimation.classList.add('hidden');
        quoteScreen.classList.remove('hidden');

        setTimeout(() => {
          quoteScreen.classList.add('hidden');
          cardContainer.classList.remove('hidden');
          fadeInMusic();
        }, 4000);
      }, 5000);
    }, 10000);
  }

  function fadeInMusic() {
    music.volume = 0;
    music.play();
    let volume = 0;
    const interval = setInterval(() => {
      if (volume < 1) {
        volume += 0.05;
        music.volume = volume;
      } else {
        clearInterval(interval);
      }
    }, 200);
  }

  if (card) {
    let rotation = 0;
    card.addEventListener('click', () => {
      rotation += 120;
      card.style.transform = `rotateY(${rotation}deg)`;
      if (rotation % 360 === 0) {
        setTimeout(() => {
          location.reload();
        }, 2000);
      }
    });
  }

  function createHearts() {
    const heartsContainer = document.querySelector('.hearts');
    for (let i = 0; i < 30; i++) {
      const heart = document.createElement('div');
      heart.classList.add('heart');
      heart.textContent = '💖';
      heart.style.left = `${Math.random() * 100}%`;
      heart.style.animationDuration = `${3 + Math.random() * 5}s`;
      heart.style.fontSize = `${1 + Math.random() * 2}em`;
      heart.style.top = `${Math.random() * 100}vh`;
      heartsContainer.appendChild(heart);
    }
  }

  createHearts();
});