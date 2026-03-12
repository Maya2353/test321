<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Village Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .hidden {
            display: none !important;
        }

        /* LEVEL AUSWAHL */
        .levels-section {
            background: white;
            border-radius: 15px;
            padding: 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        .levels-section h2 {
            font-size: 32px;
            color: #333;
            margin-bottom: 30px;
            text-align: center;
        }

        .levels-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .level-btn {
            aspect-ratio: 1;
            border: none;
            border-radius: 10px;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .level-btn.unlocked {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
            box-shadow: 0 5px 15px rgba(76, 175, 80, 0.3);
        }

        .level-btn.unlocked:hover {
            transform: scale(1.1);
            box-shadow: 0 8px 25px rgba(76, 175, 80, 0.5);
        }

        .level-btn.unlocked:active {
            transform: scale(0.98);
        }

        .level-btn.locked {
            background: #ccc;
            color: #999;
            cursor: not-allowed;
            opacity: 0.7;
        }

        .level-btn.completed {
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: white;
            box-shadow: 0 5px 15px rgba(255, 215, 0, 0.3);
        }

        .level-number {
            font-size: 32px;
            font-weight: bold;
        }

        .level-reward {
            font-size: 14px;
            opacity: 0.9;
        }

        /* QUIZ */
        .quiz-section {
            background: white;
            border-radius: 15px;
            padding: 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            max-width: 700px;
            margin: 0 auto;
        }

        .top-nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .back-btn {
            background: #FF6B6B;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 14px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
        }

        .back-btn:hover {
            background: #FF5252;
            transform: scale(1.05);
        }

        .back-btn:active {
            transform: scale(0.98);
        }

        .quiz-header {
            text-align: center;
            margin-bottom: 30px;
            color: #333;
        }

        .quiz-header h3 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        .quiz-progress {
            font-size: 14px;
            color: #667eea;
            font-weight: bold;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: #ddd;
            border-radius: 5px;
            margin: 15px 0;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            transition: width 0.3s;
        }

        .quiz-question {
            font-size: 20px;
            color: #333;
            margin-bottom: 30px;
            font-weight: bold;
            line-height: 1.6;
        }

        .quiz-answers {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 25px;
        }

        .answer-btn {
            padding: 15px 20px;
            font-size: 16px;
            border: 2px solid #667eea;
            background: white;
            color: #667eea;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }

        .answer-btn:hover:not(:disabled) {
            background: #667eea;
            color: white;
            transform: translateX(5px);
            box-shadow: 0 3px 10px rgba(102, 126, 234, 0.3);
        }

        .answer-btn:active:not(:disabled) {
            transform: scale(0.98);
        }

        .answer-btn:disabled {
            cursor: not-allowed;
            opacity: 1;
        }

        .answer-btn.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .answer-btn.incorrect {
            background: #FF6B6B;
            color: white;
            border-color: #FF6B6B;
        }

        .feedback {
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            margin-top: 20px;
            min-height: 30px;
        }

        .feedback.correct {
            color: #4CAF50;
        }

        .feedback.incorrect {
            color: #FF6B6B;
        }

        .results {
            text-align: center;
            background: #f9f9f9;
            padding: 30px;
            border-radius: 10px;
        }

        .results h3 {
            font-size: 32px;
            color: #333;
            margin-bottom: 20px;
        }

        .results-info {
            font-size: 18px;
            color: #667eea;
            margin-bottom: 30px;
            font-weight: bold;
        }

        .results-info div {
            margin: 10px 0;
        }

        .results-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .result-btn {
            padding: 12px 30px;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.3s;
        }

        .result-btn.retry {
            background: #667eea;
            color: white;
        }

        .result-btn.retry:hover {
            background: #5568d3;
            transform: scale(1.05);
        }

        .result-btn.back {
            background: #FF6B6B;
            color: white;
        }

        .result-btn.back:hover {
            background: #FF5252;
            transform: scale(1.05);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- LEVEL AUSWAHL -->
        <div id="levelsView">
            <div class="levels-section">
                <h2>📚 Quiz Level</h2>
                <div class="levels-grid" id="levelsGrid"></div>
            </div>
        </div>

        <!-- QUIZ -->
        <div id="quizView" class="hidden">
            <div class="top-nav">
                <button class="back-btn" onclick="goToLevels()">← Zurück</button>
                <h3 style="flex: 1; text-align: center; margin: 0;" id="quizLevelTitle">Level 1</h3>
                <div style="width: 80px;"></div>
            </div>
            <div class="quiz-section">
                <div class="quiz-header">
                    <div class="quiz-progress">Frage <span id="questionNumber">1</span> von 5</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="progressBar" style="width: 0%"></div>
                    </div>
                </div>
                <div id="quizContent"></div>
            </div>
        </div>
    </div>

    <script>
        // QUIZ DATEN - 9 LEVEL MIT JE 5 FRAGEN
        const quizData = {
            1: [
                { question: "Paris ist die Hauptstadt von Frankreich.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Wie viele Kontinente gibt es?", answers: ["5", "6", "7", "8"], correct: 2 },
                { question: "Der Mond ist größer als die Erde.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Welcher Planet ist der größte?", answers: ["Saturn", "Jupiter", "Neptun"], correct: 1 },
                { question: "Wasser kocht bei 100°C.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            2: [
                { question: "Berlin ist die Hauptstadt von Deutschland.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Die Erde ist flach.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Wie viele Saiten hat eine Gitarre?", answers: ["5", "6", "7"], correct: 1 },
                { question: "Gold ist flüssig.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Der höchste Berg ist der Everest.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            3: [
                { question: "Rom ist die Hauptstadt von Italien.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Die Sonne ist ein Planet.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Wie viele Zähne hat ein Erwachsener?", answers: ["28", "32", "36"], correct: 1 },
                { question: "Bananen wachsen auf Bäumen.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Das Licht ist schneller als der Schall.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            4: [
                { question: "Madrid ist die Hauptstadt von Spanien.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Alle Schlangen sind giftig.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Wie viele Saiten hat eine Violine?", answers: ["3", "4", "5"], correct: 1 },
                { question: "Koalas sind Bären.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Diamanten sind aus Kohlenstoff.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            5: [
                { question: "Tokio ist die Hauptstadt von Japan.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Die Antarktis ist ein Land.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Wie viele Kontinente gibt es?", answers: ["6", "7", "8"], correct: 1 },
                { question: "Honig verdirbt nie.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Das menschliche Herz hat 4 Kammern.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            6: [
                { question: "London ist die Hauptstadt von England.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Fledermäuse sind blind.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Wie lange lebt eine Eintagsfliege?", answers: ["1 Tag", "1 Woche", "1 Monat"], correct: 0 },
                { question: "Der Ozean macht 71% der Erde aus.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Pinguine leben auf dem Nordpol.", answers: ["Wahr", "Falsch"], correct: 1 }
            ],
            7: [
                { question: "Canberra ist die Hauptstadt von Australien.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Haie sind Fische.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Wie viele Augen hat eine Spinne?", answers: ["6", "8", "4"], correct: 1 },
                { question: "Giraffen können nicht schwimmen.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Das Blut von Oktopussen ist rot.", answers: ["Wahr", "Falsch"], correct: 1 }
            ],
            8: [
                { question: "Kairo ist die Hauptstadt von Ägypten.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Emus können rückwärts laufen.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Wie viele Herzen hat ein Tintenfisch?", answers: ["1", "2", "3"], correct: 2 },
                { question: "Elefanten fürchten sich vor Mäusen.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Gorillas essen kein Fleisch.", answers: ["Wahr", "Falsch"], correct: 0 }
            ],
            9: [
                { question: "Istanbul ist die Hauptstadt der Türkei.", answers: ["Wahr", "Falsch"], correct: 1 },
                { question: "Schmetterlinge können schmecken mit den Füßen.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Wie lange kann ein Elefant unter Wasser atmen?", answers: ["5 Sekunden", "30 Sekunden", "5 Minuten"], correct: 1 },
                { question: "Kühe haben beste Freunde.", answers: ["Wahr", "Falsch"], correct: 0 },
                { question: "Zebras sind weiß mit schwarzen Streifen.", answers: ["Wahr", "Falsch"], correct: 1 }
            ]
        };

        // GAME STATE
        let gameState = {
            money: 0,
            completedLevels: [],
            currentLevel: null,
            currentQuestion: 0,
            correctAnswers: 0,
            answered: false
        };

        // INIT
        function init() {
            loadGame();
            renderLevels();
        }

        // SPEICHERN/LADEN
        function loadGame() {
            const saved = localStorage.getItem('quizGameState');
            if (saved) {
                gameState = JSON.parse(saved);
            }
        }

        function saveGame() {
            localStorage.setItem('quizGameState', JSON.stringify(gameState));
        }

        // LEVELS ANZEIGEN
        function renderLevels() {
            const grid = document.getElementById('levelsGrid');
            grid.innerHTML = '';

            for (let i = 1; i <= 9; i++) {
                const btn = document.createElement('button');
                btn.className = 'level-btn';

                const isCompleted = gameState.completedLevels.includes(i);
                const isUnlocked = i === 1 || gameState.completedLevels.includes(i - 1);

                if (isCompleted) {
                    btn.classList.add('completed');
                } else if (isUnlocked) {
                    btn.classList.add('unlocked');
                } else {
                    btn.classList.add('locked');
                }

                btn.innerHTML = `
                    <div class="level-number">Level ${i}</div>
                    <div class="level-reward">💰 ${50 + (i * 10)}€</div>
                    ${isCompleted ? '<div style="font-size: 20px;">✅</div>' : ''}
                    ${!isUnlocked ? '<div style="font-size: 20px;">🔒</div>' : ''}
                `;

                if (isUnlocked) {
                    btn.onclick = () => startQuiz(i);
                }

                grid.appendChild(btn);
            }
        }

        // QUIZ STARTEN
        function startQuiz(levelId) {
            gameState.currentLevel = levelId;
            gameState.currentQuestion = 0;
            gameState.correctAnswers = 0;
            gameState.answered = false;

            document.getElementById('levelsView').classList.add('hidden');
            document.getElementById('quizView').classList.remove('hidden');
            document.getElementById('quizLevelTitle').textContent = `Level ${levelId}`;

            showQuestion();
        }

        // FRAGE ANZEIGEN
        function showQuestion() {
            const levelId = gameState.currentLevel;
            const questions = quizData[levelId];
            const questionIndex = gameState.currentQuestion;
            const question = questions[questionIndex];

            const progress = ((questionIndex + 1) / 5) * 100;
            document.getElementById('progressBar').style.width = progress + '%';

            let answersHTML = question.answers.map((answer, i) => {
                return `<button class="answer-btn" onclick="answerQuestion(${i})" data-index="${i}">${answer}</button>`;
            }).join('');

            document.getElementById('quizContent').innerHTML = `
                <div class="quiz-question">${question.question}</div>
                <div class="quiz-answers" id="answersContainer">
                    ${answersHTML}
                </div>
                <div class="feedback" id="feedback"></div>
            `;

            document.getElementById('questionNumber').textContent = questionIndex + 1;
            gameState.answered = false;
        }

        // ANTWORT ÜBERPRÜFEN
        function answerQuestion(selectedIndex) {
            // Verhindere mehrfaches Antworten
            if (gameState.answered) return;
            gameState.answered = true;

            const levelId = gameState.currentLevel;
            const questions = quizData[levelId];
            const question = questions[gameState.currentQuestion];
            const isCorrect = selectedIndex === question.correct;

            const buttons = document.querySelectorAll('.answer-btn');
            
            buttons.forEach(button => {
                button.disabled = true;
                
                const index = parseInt(button.getAttribute('data-index'));
                
                if (index === question.correct) {
                    button.classList.add('correct');
                }
                
                if (!isCorrect && index === selectedIndex) {
                    button.classList.add('incorrect');
                }
            });

            const feedback = document.getElementById('feedback');
            if (isCorrect) {
                feedback.textContent = '✅ Richtig!';
                feedback.className = 'feedback correct';
                gameState.correctAnswers++;
            } else {
                feedback.textContent = '❌ Falsch! Die richtige Antwort ist grün markiert.';
                feedback.className = 'feedback incorrect';
            }

            // NÄCHSTE FRAGE NACH 1.5 SEKUNDEN
            setTimeout(() => {
                gameState.currentQuestion++;
                
                if (gameState.currentQuestion < 5) {
                    showQuestion();
                } else {
                    endQuiz();
                }
            }, 1500);
        }

        // QUIZ BEENDEN
        function endQuiz() {
            const levelId = gameState.currentLevel;
            const reward = 50 + (levelId * 10);
            const passed = gameState.correctAnswers >= 3;

            let resultHTML = `
                <div class="results">
                    <h3>${passed ? '🎉 Bestanden!' : '❌ Nicht bestanden'}</h3>
                    <div class="results-info">
                        <div>Richtige Antworten: <strong>${gameState.correctAnswers}/5</strong></div>
                        <div>Verdienst: <strong>${passed ? reward : 0}€</strong></div>
                    </div>
                    <div class="results-buttons">
                        <button class="result-btn retry" onclick="startQuiz(${levelId})">🔄 Wiederholen</button>
                        <button class="result-btn back" onclick="goToLevels()">← Zurück zu Leveln</button>
                    </div>
                </div>
            `;

            document.getElementById('quizContent').innerHTML = resultHTML;

            if (passed && !gameState.completedLevels.includes(levelId)) {
                gameState.completedLevels.push(levelId);
                gameState.money += reward;
                saveGame();
            }
        }

        // ZU LEVELS ZURÜCK
        function goToLevels() {
            document.getElementById('quizView').classList.add('hidden');
            document.getElementById('levelsView').classList.remove('hidden');
            renderLevels();
        }

        // START
        init();
    </script>
</body>
</html>
