<template>
    <div class="section is-medium egyptian-background">
      <div class="floating-symbols-container"></div>
  
      <div class="container">
        <h1 class="title has-text-centered is-size-3 egyptian-title">Lingua Egiziana</h1>
        <div class="field">
          <label class="label has-text-weight-bold egyptian-subtitle">Modalità:</label>
          <div class="control">
            <div class="select is-rounded egyptian-select">
              <select v-model="mode">
                <option value="antico">Egiziano Antico</option>
                <option value="moderno">Egiziano Moderno</option>
              </select>
            </div>
          </div>
        </div>
  
        <!-- Modalità contenuto -->
        <div v-if="mode === 'antico'" class="box egyptian-box">
          <h2 class="subtitle has-text-weight-bold egyptian-subtitle">Egiziano Antico (Geroglifici)</h2>
          <p class="mb-4 egyptian-text">L'egiziano antico era scritto con geroglifici...</p>
          <div class="text-center">
            <div class="is-size-2 mb-4 egyptian-hieroglyph">{{ currentHieroglyph.char }}</div>
            <div class="is-size-4 has-text-weight-semibold egyptian-font">{{ currentHieroglyph.sound }}</div>
            <div class="is-size-5 has-text-grey-light egyptian-text">{{ currentHieroglyph.meaning }}</div>
          </div>
          <div class="buttons is-centered mt-5">
            <button @click="prevSymbol" class="button is-warning is-rounded egyptian-button">Indietro</button>
            <button @click="nextSymbol" class="button is-warning is-rounded egyptian-button">Avanti</button>
          </div>
        </div>
  
        <div v-else class="box egyptian-box">
          <h2 class="subtitle has-text-weight-bold egyptian-subtitle">Arabo Egiziano Moderno</h2>
          <p class="mb-4 egyptian-text">L'arabo egiziano è la lingua parlata oggi in Egitto...</p>
          <div class="text-center">
            <div class="is-size-2 mb-4 egyptian-hieroglyph">{{ currentModernPhrase.arabic }}</div>
            <div class="is-size-4 has-text-weight-semibold egyptian-font">{{ currentModernPhrase.romanization }}</div>
            <div class="is-size-5 has-text-grey-light egyptian-text">{{ currentModernPhrase.meaning }}</div>
          </div>
          <div class="buttons is-centered mt-5">
            <button @click="prevPhrase" class="button is-warning is-rounded egyptian-button">Indietro</button>
            <button @click="nextPhrase" class="button is-warning is-rounded egyptian-button">Avanti</button>
          </div>
        </div>
        
        <!-- Footer -->
        <footer class="footer-area">Made by
          <a href="https://matteosantoro.dev" target="_blank" rel="noopener noreferrer" class="footer-link">
            matteosantoro.dev
          </a>
        </footer>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, computed, onMounted } from 'vue';
  import hieroglyphsData from '../assets/hieroglyphs.json';
  import modernArabicData from '../assets/modernArabic.json';
  
  const mode = ref('antico');
  const currentIndex = ref(0);
  const currentModernIndex = ref(0);
  const hieroglyphs = hieroglyphsData;
  const modernArabic = modernArabicData;
  
  const currentHieroglyph = computed(() => hieroglyphs[currentIndex.value]);
  const currentModernPhrase = computed(() => modernArabic[currentModernIndex.value]);
  
  const nextSymbol = () => {
    if (currentIndex.value < hieroglyphs.length - 1) currentIndex.value++;
  };
  const prevSymbol = () => {
    if (currentIndex.value > 0) currentIndex.value--;
  };
  const nextPhrase = () => {
    if (currentModernIndex.value < modernArabic.length - 1) currentModernIndex.value++;
  };
  const prevPhrase = () => {
    if (currentModernIndex.value > 0) currentModernIndex.value--;
  };
  
  // Function to create a floating symbol
  const createFloatingSymbol = () => {
    // Get container dimensions
    const width = window.innerWidth;
    const height = window.innerHeight;
    
    // Create symbol element
    const symbol = document.createElement('div');
    symbol.classList.add('floating-symbol');
    
    // Random content selection
    const type = Math.random() > 0.5 ? 'hieroglyph' : 'modernArabic';
    symbol.textContent = type === 'hieroglyph'
      ? hieroglyphs[Math.floor(Math.random() * hieroglyphs.length)].char
      : modernArabic[Math.floor(Math.random() * modernArabic.length)].arabic;
    
    // Random positioning across the entire viewport
    const posX = Math.floor(Math.random() * width);
    const posY = Math.floor(Math.random() * height);
    
    // Visual styling
    symbol.style.left = `${posX}px`;
    symbol.style.top = `${posY}px`;
    symbol.style.fontSize = `${Math.random() * 2 + 1}rem`;
    
    // Lower opacity for better readability of main content
    symbol.style.opacity = `${Math.random() * 0.15 + 0.05}`;
    
    // Animation customization
    const animDuration = Math.random() * 15 + 10; // 10-25 seconds
    symbol.style.setProperty('--float-duration', `${animDuration}s`);
    symbol.style.animationDelay = `-${Math.random() * animDuration}s`;
    
    return symbol;
  };
  
  onMounted(() => {
    const container = document.querySelector('.floating-symbols-container');
    if (!container) return;
    
    // Number of symbols based on screen size
    const symbolCount = Math.max(30, Math.floor((window.innerWidth * window.innerHeight) / 25000));
    
    // Create and add symbols
    for (let i = 0; i < symbolCount; i++) {
      const symbol = createFloatingSymbol();
      container.appendChild(symbol);
    }
    
    // Add window resize handler to adjust symbols when screen size changes
    window.addEventListener('resize', () => {
      // Clear existing symbols
      container.innerHTML = '';
      
      // Recalculate number of symbols
      const newSymbolCount = Math.max(30, Math.floor((window.innerWidth * window.innerHeight) / 25000));
      
      // Create new symbols for new dimensions
      for (let i = 0; i < newSymbolCount; i++) {
        const symbol = createFloatingSymbol();
        container.appendChild(symbol);
      }
    });
  });
  </script>
  
  <style>
  .egyptian-background {
    position: relative;
    background: linear-gradient(to bottom, #1a1c1e, #262830);
    min-height: 100vh;
    overflow: hidden;
    padding: 2rem 1rem;
  }
  
  .floating-symbols-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 0;
    overflow: hidden;
    pointer-events: none;
  }
  
  .floating-symbol {
    position: absolute;
    color: rgba(255, 230, 180, 0.25);
    font-family: 'Papyrus', fantasy;
    font-weight: bold;
    white-space: nowrap;
    animation: floatSymbol var(--float-duration, 15s) ease-in-out infinite alternate;
  }
  
  @keyframes floatSymbol {
    0% {
      transform: translate(0, 0) rotate(0deg);
    }
    33% {
      transform: translate(15px, -15px) rotate(2deg);
    }
    66% {
      transform: translate(-15px, -25px) rotate(-1deg);
    }
    100% {
      transform: translate(0, -30px) rotate(1deg);
    }
  }
  
  .container {
    position: relative;
    z-index: 1;
    max-width: 700px;
    display: flex;
    flex-direction: column;
    min-height: calc(100vh - 4rem);
  }
  
  .egyptian-title {
    color: #ffd700;
    font-family: 'Papyrus', fantasy;
    font-size: 2.5rem;
    margin-bottom: 2.5rem;
    text-shadow: 0 0 10px rgba(255, 215, 0, 0.4), 0 0 20px rgba(0, 0, 0, 0.8);
    letter-spacing: 1px;
  }
  
  .egyptian-box {
    background-color: rgba(30, 32, 36, 0.92);
    border-radius: 12px;
    padding: 2.5rem;
    box-shadow: 0 12px 25px rgba(0, 0, 0, 0.3), 0 0 1px rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.05);
    margin-bottom: 2rem;
  }
  
  .egyptian-subtitle {
    color: #ffd166;
    font-size: 1.4rem;
    letter-spacing: 0.5px;
    margin-bottom: 1.2rem;
    text-shadow: 0 0 8px rgba(0, 0, 0, 0.5);
  }
  
  .egyptian-text {
    color: #e6e6e6;
    font-size: 1.1rem;
    line-height: 1.6;
  }
  
  .egyptian-font {
    color: #fff9e6;
    letter-spacing: 0.5px;
    margin: 0.8rem 0;
    text-shadow: 0 0 5px rgba(0, 0, 0, 0.7);
  }
  
  .egyptian-hieroglyph {
    color: #ffd700;
    text-shadow: 0 0 12px rgba(255, 215, 0, 0.5), 0 0 20px rgba(0, 0, 0, 0.7);
    font-size: 3rem;
    margin: 1.5rem 0;
    letter-spacing: 2px;
  }
  
  .egyptian-select {
    display: block;
    margin-bottom: 1.5rem;
  }
  
  .egyptian-select select {
    background-color: #2a2c32;
    color: #fff9e6;
    border: 1px solid rgba(255, 215, 0, 0.3);
    padding: 0.6rem 2rem 0.6rem 1rem;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
  }
  
  .egyptian-select select:focus,
  .egyptian-select select:hover {
    border-color: rgba(255, 215, 0, 0.6);
    box-shadow: 0 0 0 2px rgba(255, 215, 0, 0.2);
  }
  
  .egyptian-button {
    background-color: #ffd700;
    color: #1a1c1e;
    border: none;
    font-weight: 600;
    padding: 0.7rem 1.5rem;
    font-size: 1rem;
    transition: all 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    margin: 0 0.5rem;
  }
  
  .egyptian-button:hover {
    background-color: #ffcc00;
    box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
    transform: translateY(-2px);
  }
  
  .egyptian-button:active {
    transform: translateY(1px);
  }
  
  /* Footer styles */
  .footer-area {
    margin-top: auto;
    padding: 1rem 0;
    text-align: center;
  }
  
  .footer-link {
    color: rgba(255, 215, 0, 0.7);
    font-size: 1.1rem;
    text-decoration: none;
    transition: all 0.3s ease;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    letter-spacing: 0.5px;
    position: relative;
    padding: 0.3rem 0.8rem;
    border-radius: 4px;
  }
  
  .footer-link:hover {
    color: #ffd700;
    text-shadow: 0 0 8px rgba(255, 215, 0, 0.4);
  }
  
  .footer-link::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    width: 0;
    height: 1px;
    background: #ffd700;
    transition: all 0.3s ease;
    transform: translateX(-50%);
  }
  
  .footer-link:hover::after {
    width: calc(100% - 1.6rem);
  }
  
  /* Media queries for responsiveness */
  @media screen and (max-width: 768px) {
    .egyptian-box {
      padding: 1.5rem;
    }
    
    .egyptian-title {
      font-size: 2rem;
    }
    
    .egyptian-hieroglyph {
      font-size: 2.5rem;
    }
  }
  
  @media screen and (max-width: 480px) {
    .egyptian-box {
      padding: 1.2rem;
    }
    
    .egyptian-title {
      font-size: 1.8rem;
    }
    
    .egyptian-hieroglyph {
      font-size: 2.2rem;
    }
    
    .egyptian-button {
      padding: 0.6rem 1.2rem;
    }
    
    .footer-link {
      font-size: 1rem;
    }
  }
  </style>