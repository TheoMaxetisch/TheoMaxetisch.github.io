
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website Berufswahl</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #f5f5f5;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 600px;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        header {
            margin-bottom: 30px;
        }
        
        h1 {
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .card-container {
            position: relative;
            width: 100%;
            height: 300px;
            perspective: 1000px;
            margin-bottom: 30px;
        }
        
        .card {
            position: absolute;
            width: 100%;
            height: 100%;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 30px;
            transform-origin: center;
            transition: transform 0.3s ease;
            z-index: 1;
        }
        
        .card.swipe-left {
            transform: translateX(-150%) rotate(-10deg);
        }
        
        .card.swipe-right {
            transform: translateX(150%) rotate(10deg);
        }
        
        .requirement-text {
            font-size: 1.5rem;
            text-align: center;
            margin-bottom: 20px;
        }
        
        .instruction {
            color: #7f8c8d;
            margin-bottom: 20px;
            font-size: 1rem;
        }
        
        .controls {
            display: flex;
            justify-content: space-around;
            width: 100%;
            /*max-width: 400px;*/
            margin-top: 20px;
        }
        
        .key {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0 10px;
        }
        
        .key-icon {
            width: 50px;
            height: 50px;
            background-color: #ecf0f1;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5rem;
            margin-bottom: 5px;
            box-shadow: 0 3px 0 #bdc3c7;
        }
        
        .key-label {
            font-size: 0.8rem;
            color: #7f8c8d;
        }
        
        .progress-bar {
            width: 100%;
            height: 5px;
            background-color: #ecf0f1;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
        }
        
        .progress {
            height: 100%;
            background-color: #3498db;
            width: 0%;
            transition: width 0.3s ease;
        }
        
        .results-container {
            display: none;
            width: 100%;
        }
        
        .result-item {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
            text-align: left;
        }
        
        .result-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .result-title {
            font-size: 1.3rem;
            color: #2c3e50;
            font-weight: bold;
        }
        
        .result-duration {
            color: #7f8c8d;
        }
        
        .result-bar-container {
            width: 100%;
            height: 10px;
            background-color: #ecf0f1;
            border-radius: 5px;
            overflow: hidden;
        }
        
        .result-bar {
            height: 100%;
            border-radius: 5px;
        }
        
        .result-percentage {
            text-align: right;
            margin-top: 5px;
            font-size: 0.9rem;
            color: #7f8c8d;
            margin-bottom: 1vw;
        }
        
        .start-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        
        .start-button {
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 15px 30px;
            font-size: 1.2rem;
            cursor: pointer;
            box-shadow: 0 5px 0 #2980b9;
            transition: all 0.2s ease;
            margin-top: 30px;
        }
        
        .start-button:hover {
            background-color: #2980b9;
            transform: translateY(2px);
            box-shadow: 0 3px 0 #2980b9;
        }
        
        .restart {
            margin-top: 20px;
            color: #3498db;
            cursor: pointer;
            text-decoration: underline;
        }

        .results-header {
            text-align: center;
            margin-bottom: 20px;
        }

        @media (max-width: 768px) {
            .result-header {
                flex-direction: column;  /* Stellt sicher, dass es auf kleinen Geräten untereinander ist */
                align-items: flex-start; /* Optional: sorgt dafür, dass die Items links ausgerichtet sind */
            }

            .result-title, .result-duration {
                margin-bottom: 5px; /* Optional: sorgt für Abstand zwischen den Elementen */
            }
        
            .result-percentage {
                font-size: 0.7rem;
            }
        
            .result-title {
                font-size: 1rem;
            }
        
            .result-duration {
                font-size: 0.8rem;
            }
        
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Berufswahlhelfer</h1>
            <p class="instruction" id="main-instruction">Finde heraus, welche Ausbildung zu dir passt!</p>
        </header>
        
        <div class="start-screen" id="start-screen">
            <p>
                Beantworte einige Fragen zu deinen Fähigkeiten und Interessen, 
                um herauszufinden, welche Ausbildungsberufe am besten zu dir passen.
            </p>
            <p style="margin-top: 15px;">
                Benutze die Pfeiltasten zur Steuerung:
                <br>→ für "Ja" / ← für "Nein"
                <br>↑ um deine letzte Antwort zu ändern
                <br>↓ um neu zu starten
            </p>
            <button class="start-button" id="start-button">Starten</button>
        </div>
        
        <div id="question-screen" style="display: none; width: 100%;">
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            
            <div class="card-container">
                <div class="card" id="current-card">
                    <p class="requirement-text" id="requirement-text"></p>
                </div>
            </div>
            
            <div class="controls">
                <div class="key" id="key-no">
                    <div class="key-icon">←</div>
                    <div class="key-label" id="nein-btn">Nein</div>
                </div>
                <div class="key" id="key-back">
                    <div class="key-icon">↑</div>
                    <div class="key-label">Zurück</div>
                </div>
                <div class="key" id="key-yes">
                    <div class="key-icon">→</div>
                    <div class="key-label" id="ja-btn">Ja</div>
                </div>
            </div>
        </div>
        
        <div class="results-container" id="results-container">
            <div class="results-header">
                <h2>Deine passenden Ausbildungsberufe</h2>
                <p>Basierend auf deinen Antworten passen diese Berufe am besten zu dir:</p>
            </div>
            <div id="results-list"></div>
            <div class="restart" id="restart">Neu starten</div>
        </div>
    </div>
    <!----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------->
    <!----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------->
    <!----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------->
    <!----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------->
    <!----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------->
    <script>
        // Anforderungen
        const anforderungen = [
            "Ich bin handwerklich begabt",
            "Ich interessiere mich für Technik",
            "Ich bin kreativ",
            "Ich kann gut mit Menschen umgehen",
            "Ich bin gut im Organisieren",
            "Ich bin in Mathe keine Niete",
            "Ich arbeite gerne im Team",
            "Ich bin körperlich belastbar",
            "Ich arbeite gerne draußen",
            "Ich interessiere mich für Naturwissenschaften",
            "Ich bin geduldig",
            "Ich kann mich gut konzentrieren",
            "Ich interessiere mich für den Arbeitsschutz",
            "Umweltschutz ist mir wichtig",
            "Ich kann gut mit Zahlen umgehen",
            "Ich habe Interesse an einer Verbeamtung",
            "Ich möchte studieren"
        ];

        const bildungsstand = [
            "Bachelor of Science",
            "Bachelor",
            "Abitur / allgemeine Hochschulreife",
            "Fachhochschulreife",
            "Realschulabschluss",
            "Meisterbrief im Handwerk",
            "Hauptschulabschluss",
        ];

        const alter = [
            "39 Jahre",
            "40 Jahre",
            "99 Jahre"
        ];

        // Berufe im JSON-Format
        const berufeJSON = `[
            {
                "titel": "Regierungssekretäranwärter/in",
                "ausbildungsdauer": "2 Jahre",
                "bildungsstand": "Realschulabschluss",
                "maxalter": "",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren": false
                }
            },
            {
                "titel": "Verwaltungsfachangestellte/r",
                "ausbildungsdauer": "3 Jahre",
                "bildungsstand": "Realschulabschluss",
                "maxalter": "",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":false,
                    "Ich möchte studieren":false
                }
            },
            {
                "titel": "Regierungsinspektoranwärter/in",
                "ausbildungsdauer": "3 Jahre",
                "bildungsstand": "Abitur / allgemeine Hochschulreife",
                "maxalter": "39 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Verwaltungsinformatiker/in",
                "ausbildungsdauer": "3 Jahre",
                "bildungsstand": "Abitur / allgemeine Hochschulreife",
                "maxalter": "39 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":true,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Fachinformatiker/in für Systemintegration",
                "ausbildungsdauer": "3 Jahre",
                "bildungsstand": "Realschulabschluss",
                "maxalter": "",
                "anforderungen": {
                    "Ich bin handwerklich begabt":true,
                    "Ich interessiere mich für Technik":true,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":false,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":true,
                    "Ich habe Interesse an einer Verbeamtung":false,
                    "Ich möchte studieren":false
                }
            },
            {
                "titel": "Vermessungstechniker/in",
                "ausbildungsdauer": "3 Jahre",
                "bildungsstand": "Hauptschulabschluss",
                "maxalter": "",
                "anforderungen": {
                    "Ich bin handwerklich begabt":true,
                    "Ich interessiere mich für Technik":true,
                    "Ich bin kreativ":false,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":false,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":true,
                    "Ich arbeite gerne draußen":true,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":false,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":true,
                    "Ich habe Interesse an einer Verbeamtung":false,
                    "Ich möchte studieren":false
                }
            },
            {
                "titel": "Gewerbeoberinspektoranwärter/in",
                "ausbildungsdauer": "15 Monate",
                "bildungsstand": "Bachelor of Science",
                "maxalter": "40 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Gewerbeobersekretäranwärter/in",
                "ausbildungsdauer": "15 Monate",
                "bildungsstand": "Meisterbrief im Handwerk",
                "maxalter": "40 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":true,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":false,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Umweltoberinspektoranwärter/in",
                "ausbildungsdauer": "15 Monate",
                "bildungsstand": "Bachelor",
                "maxalter": "40 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":false,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":true,
                    "Ich arbeite gerne draußen":true,
                    "Ich interessiere mich für Naturwissenschaften":true,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":true,
                    "Ich kann gut mit Zahlen umgehen":false,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Regierungsvermessungsober- inspektoranwärter/in - Duales Studium",
                "ausbildungsdauer": "43 Monate",
                "bildungsstand": "Fachhochschulreife",
                "maxalter": "40 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":true,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":true,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":true,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":true
                }
            },
            {
                "titel": "Regierungsvermessungsober- inspektoranwärter/in - Ausbildung",
                "ausbildungsdauer": "12 Monate",
                "bildungsstand": "Bachelor",
                "maxalter": "40 Jahre",
                "anforderungen": {
                    "Ich bin handwerklich begabt":true,
                    "Ich interessiere mich für Technik":false,
                    "Ich bin kreativ":true,
                    "Ich kann gut mit Menschen umgehen":true,
                    "Ich bin gut im Organisieren ":true,
                    "Ich bin in Mathe keine Niete":true,
                    "Ich arbeite gerne im Team":true,
                    "Ich bin körperlich belastbar":false,
                    "Ich arbeite gerne draußen":true,
                    "Ich interessiere mich für Naturwissenschaften":false,
                    "Ich bin geduldig":true,
                    "Ich kann mich gut konzentrieren":true,
                    "Ich interessiere mich für den Arbeitsschutz":false,
                    "Umweltschutz ist mir wichtig":false,
                    "Ich kann gut mit Zahlen umgehen":true,
                    "Ich habe Interesse an einer Verbeamtung":true,
                    "Ich möchte studieren":false
                }
            }
        ]`;

        // Hauptvariablen
        let berufe = JSON.parse(berufeJSON);
        let shuffledAnforderungen = [];
        let modus = 0; // 0: Bildungsstand, 1: Alter, 2: Anforderungen, 
        let currentIndex = 0;
        let nutzerProfil = {};
        let antwortHistorie = [];

        // DOM-Elemente
        const startScreen = document.getElementById('start-screen');
        const questionScreen = document.getElementById('question-screen');
        const resultsContainer = document.getElementById('results-container');
        const requirementText = document.getElementById('requirement-text');
        const progressBar = document.getElementById('progress');
        const currentCard = document.getElementById('current-card');
        const resultsListElement = document.getElementById('results-list');
        const startButton = document.getElementById('start-button');
        const restartButton = document.getElementById('restart');
        const keyNo = document.getElementById('key-no');
        const keyBack = document.getElementById('key-back');
        const keyYes = document.getElementById('key-yes');
        const jaButton = document.getElementById('ja-btn');
        const neinButton = document.getElementById('nein-btn');

        // Event-Listener für Tasten
        document.addEventListener('keydown', handleKeyPress);
        startButton.addEventListener('click', startApplication);
        restartButton.addEventListener('click', restartApplication);
        keyNo.addEventListener('click', () => handleAnswer(false));
        keyYes.addEventListener('click', () => handleAnswer(true));
        keyBack.addEventListener('click', goBack);

        // Anwendung initialisieren
        function startApplication() {
            startScreen.style.display = 'none';
            questionScreen.style.display = 'block';
            
            // Anforderungen mischen
            shuffledAnforderungen = [...anforderungen];
            shuffleArray(shuffledAnforderungen);
            
            currentIndex = 0;
            nutzerProfil = {};
            antwortHistorie = [];
            
            showCurrentRequirement();
        }

        // Zeigt die aktuelle Anforderung an
        function showCurrentRequirement() {
            if (modus === 0) {
                if (currentIndex < bildungsstand.length) {
                    console.log (modus + " : " + currentIndex)
                    requirementText.textContent = `Bildungsstand: ${bildungsstand[currentIndex]}`;
                    neinButton.textContent = "Hab ich nicht";
                    jaButton.textContent = "Hab ich";
                } 
                if (currentIndex >= bildungsstand.length - 1) {
                    modus++;
                    currentIndex = -1;
                }
            } else if (modus === 1) {
                if (currentIndex < alter.length) {
                    console.log (modus + " : " + currentIndex)
                    requirementText.textContent = `Alter: ${alter[currentIndex]}`;
                    neinButton.textContent = "Jünger";
                    jaButton.textContent = "Älter";
                } 
                if (currentIndex >= alter.length - 1) {
                    modus++;
                    currentIndex = -1;
                }
            } else {
                if (currentIndex < shuffledAnforderungen.length) {
                    console.log (modus + " : " + currentIndex)
                    requirementText.textContent = shuffledAnforderungen[currentIndex];
                    neinButton.textContent = "Nein";
                    jaButton.textContent = "Ja";
                } else {
                    showResults();
                }
            }

            updateProgressBar();
            
            // Animation zurücksetzen
            currentCard.classList.remove('swipe-left', 'swipe-right');
            void currentCard.offsetWidth; // Reflow erzwingen
        }

        // Fortschrittsbalken aktualisieren
        function updateProgressBar() {
            let progress = (1 / (shuffledAnforderungen.length + bildungsstand.length + alter.length)) * 100;
            const gesamtAnforderungen = shuffledAnforderungen.length + bildungsstand.length + alter.length;
            if (modus === 0) {
                progress = (currentIndex / gesamtAnforderungen) * 100;
            } else if (modus === 1) {
                progress = ((currentIndex + bildungsstand.length) / gesamtAnforderungen) * 100;
            } else {
                progress = ((currentIndex + bildungsstand.length + alter.length) / gesamtAnforderungen) * 100;
            }
            progressBar.style.width = `${progress}%`;
        }

        // Array mischen (Fisher-Yates-Algorithmus)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]]; // Swap
            }
        }

        // Tastendrücke verarbeiten
        function handleKeyPress(event) {
            console.log('Pressed key:', event.key);
            if (questionScreen.style.display === 'none') {
                console.log('In Startscreen');
                switch (event.key) {
                    case 'ArrowDown': // Neustart
                        restartApplication();
                        break;
                    case ' ': // Starten des Spiels
                        startApplication();
                        break;
                    case 'Enter': // Starten des Spiels
                        startApplication();
                        break;
                    case '1': // Starten des Spiels
                        window.location.href='ZitatAnzeige.html';
                        return;
                    case '2': // Starten des Spiels
                        window.location.href='VideoAnzeige.html';
                        return;
                    case '3': // Starten des Spiels
                        window.location.href='KartenAnzeige.html';
                        return;
                    case '4': // Starten des Spiels
                        window.location.href='Berufswahlhelfer.html';
                        return;
                    case '5': // Starten des Spiels
                        window.location.href='index.html';
                        return;
                }
            } else {
                switch (event.key) {
                    case 'ArrowRight': // Ja
                        handleAnswer(true);
                        break;
                    case 'ArrowLeft': // Nein
                        handleAnswer(false);
                        break;
                    case 'ArrowUp': // Zurück
                        goBack();
                        break;
                    case 'ArrowDown': // Neustart
                        restartApplication();
                        break;
                }
            }
        }

        // Antwort verarbeiten
        function handleAnswer(answer) {
            if (currentIndex >= shuffledAnforderungen.length) return;
            let currentRequirement = '';
            // Aktuelle Anforderung speichern
            if (modus === 0) {
                currentRequirement = bildungsstand[currentIndex];
            } else if (modus === 1) {
                currentRequirement = alter[currentIndex];
                console.log("Alter: " + currentRequirement);
            } else {
                currentRequirement = shuffledAnforderungen[currentIndex];
            }
            nutzerProfil[currentRequirement] = answer;
            
            antwortHistorie.push({
                requirement: currentRequirement,
                answer: answer
            });
            
            //Ausschlusskriterien auswerten im Bezug auf Überspringen der Überfällig gewordenen Anforderungen
            if (modus === 0 && currentIndex < bildungsstand.length && answer) {
                for (let i = currentIndex + 1; i < bildungsstand.length; i++) {
                    nutzerProfil[bildungsstand[i]] = true;
                }
                modus++;
                currentIndex = -1;
            } else if (modus === 1 && currentIndex < alter.length && !answer) {
                for (let i = currentIndex + 1; i < alter.length; i++) {
                    nutzerProfil[alter[i]] = false;
                }
                modus++;
                currentIndex = -1;
            }

            // Swipe-Animation starten
            if (answer) {
                currentCard.classList.add('swipe-right');
            } else {
                currentCard.classList.add('swipe-left');
            }
            
            // Warten, bis die Animation abgeschlossen ist
            setTimeout(() => {
                currentIndex++;
                showCurrentRequirement();
            }, 300);
        }

        // Einen Schritt zurückgehen
        function goBack() {
            if (currentIndex > 0 && antwortHistorie.length > 0) {
                currentIndex--;
                
                const lastAnswer = antwortHistorie.pop();
                delete nutzerProfil[lastAnswer.requirement];
                
                showCurrentRequirement();
            } else if (currentIndex === 0 && antwortHistorie.length > 0 && modus > 0) {
                modus--;
                if (modus == 0) {
                    currentIndex = bildungsstand.length - 1;
                } else if (modus == 1) {
                    currentIndex = alter.length - 1;
                } else {
                    currentIndex = shuffledAnforderungen.length - 1;
                }
            }
        }

        // Ergebnisse anzeigen
        function showResults() {
            console.log('Nutzerprofil:', nutzerProfil);

            questionScreen.style.display = 'none';
            resultsContainer.style.display = 'block';
            
            // Übereinstimmungen berechnen und sortieren
            const berufeWithScore = calculateMatches();
            
            // Ergebnisliste leeren
            resultsListElement.innerHTML = '';
            
            // Ergebnisse anzeigen
            berufeWithScore.forEach(beruf => {
                const resultItem = document.createElement('div');
                resultItem.className = 'result-item';
                
                const resultHeader = document.createElement('div');
                resultHeader.className = 'result-header';
                
                const resultTitle = document.createElement('div');
                resultTitle.className = 'result-title';
                resultTitle.textContent = beruf.titel;
                
                const resultDuration = document.createElement('div');
                resultDuration.className = 'result-duration';
                resultDuration.textContent = `Ausbildungsdauer: ${beruf.ausbildungsdauer}`;
                
                resultHeader.appendChild(resultTitle);
                resultHeader.appendChild(resultDuration);
                
                const resultBarContainer = document.createElement('div');
                resultBarContainer.className = 'result-bar-container';
                
                const resultBar = document.createElement('div');
                resultBar.className = 'result-bar';
                resultBar.style.width = `${beruf.score}%`;
                
                // Farbe nach Übereinstimmung
                if (beruf.score >= 80) {
                    resultBar.style.backgroundColor = '#2ecc71'; // Grün
                } else if (beruf.score >= 50) {
                    resultBar.style.backgroundColor = '#f1c40f'; // Gelb
                } else {
                    resultBar.style.backgroundColor = '#e74c3c'; // Rot
                }
                
                resultBarContainer.appendChild(resultBar);
                
                const resultPercentage = document.createElement('div');
                resultPercentage.className = 'result-percentage';
                resultPercentage.textContent = `${beruf.score}% Übereinstimmung`;


                //"bildungsstand": "Abitur / allgemeine Hochschulreif",
                //"maxalter": "39 Jahre",
                console.log(nutzerProfil);

                const resultRequirementsOne = document.createElement('div');
                const resultRequirementsTwo = document.createElement('div');
                if (!nutzerProfil[beruf.bildungsstand]) {
                    resultRequirementsOne.textContent += `! Bildungsstand: ${beruf.bildungsstand} erforderlich !`;
                    resultRequirementsOne.style.color = '#e74c3c'; // Rot
                }
                if (nutzerProfil[beruf.maxalter]) {
                    resultRequirementsTwo.textContent += `! Die Altersgrenze liegt bei ${beruf.maxalter}n, aber sprechen Sie uns sehr gerne an ! (Ausnahmen sind möglich)`;
                    resultRequirementsTwo.style.color = '#e74c3c'; // Rot
                }
                
                resultItem.appendChild(resultHeader);
                resultItem.appendChild(resultBarContainer);
                resultItem.appendChild(resultPercentage);
                resultItem.appendChild(resultRequirementsOne);
                resultItem.appendChild(resultRequirementsTwo);
                
                resultsListElement.appendChild(resultItem);
            });
        }

        // Übereinstimmungen berechnen
        function calculateMatches() {
            const results = berufe.map(beruf => {
                const berufAnforderungen = beruf.anforderungen;
                let matchCount = 0;
                let totalRequirements = 0;
                
                // Zähle erfüllte Anforderungen
                for (const req in berufAnforderungen) {
                    if (berufAnforderungen[req]) {
                        totalRequirements++;
                        if (nutzerProfil[req] === true) {
                            matchCount++;
                        }
                    }
                }
                
                // Berechne Übereinstimmungsprozentsatz
                const score = totalRequirements > 0 
                    ? Math.round((matchCount / totalRequirements) * 100) 
                    : 0;
                
                return {
                    ...beruf,
                    score
                };
            });
            
            // Nach Übereinstimmung absteigend sortieren
            return results.sort((a, b) => b.score - a.score);
        }

        // Anwendung neu starten
        function restartApplication() {
            resultsContainer.style.display = 'none';
            //Neustart der Anwendung
            startScreen.style.display = 'block';
            questionScreen.style.display = 'none';
            modus = 0;
        }
    </script>
</body>
</html>
