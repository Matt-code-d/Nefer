# Nefer App

Questa applicazione offre un'esperienza immersiva nella lingua e nella scrittura egiziana, sia antica che moderna, permettendo agli utenti di esplorare geroglifici e frasi in arabo egiziano. L'app include un'interfaccia utente interattiva con simboli dinamici di geroglifici e caratteri arabi che si muovono verticalmente sullo sfondo.

## Caratteristiche principali

- **Modalità Egiziano Antico**: Esplora i geroglifici egiziani con simboli che si alternano nella scrittura antica.
- **Modalità Arabo Egiziano Moderno**: Visualizza frasi in arabo egiziano moderno, il dialetto parlato in Egitto.
- **Animazioni fluide**: Simboli e frasi appaiono gradualmente sullo sfondo con animazioni di movimento verticale, per un effetto visivo affascinante.
- **Interazione semplice**: Naviga facilmente tra i simboli e le frasi tramite i pulsanti "Avanti" e "Indietro".

## Dipendenze principali

- **Vue 3**: Framework JavaScript reattivo per costruire interfacce utente moderne.
- **Bulma**: Framework CSS per una rapida prototipazione con design responsivi.
- **FontAwesome (opzionale)**: Per icone personalizzate nell'interfaccia utente.

## Installazione

Per avviare il progetto in locale, segui questi passaggi:

1. **Clona il repository**:
   ```bash
   git clone https://github.com/tuo-utente/nefer.git

2. **Naviga nella cartella del progetto:**:
   ```bash
   cd nefer

3. **Installa le dipendenze: Assicurati di avere Node.js e npm (o yarn) installati. Esegui:**:
   ```bash
   npm install

4. **Avvia il server di sviluppo:**:
   ```bash
   npm run dev

**Avvia il server di sviluppo:**:
   ```bash
   nefer/
├── src/
│   ├── assets/
│   │   ├── hieroglyphs.json        # Dati dei geroglifici
│   │   └── modernArabic.json       # Dati delle frasi in arabo egiziano moderno
│   ├── components/
│   │   └── EgyptianApp.vue         # Componente principale dell'app
│   └── styles/
│       └── global.css              # Stili globali per l'app
├── public/
│   └── index.html                  # Pagina principale
└── package.json