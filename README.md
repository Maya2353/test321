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
        // QUIZ DATEN - 30 LEVEL MIT ALLGEMEINEN FRAGEN
        const quizData = {
            1: [
                { question: "Was ist die Hauptstadt von Frankreich?", answers: ["Berlin", "Paris", "London", "Rom"], correct: 1 },
                { question: "Wie viele Kontinente gibt es?", answers: ["5", "6", "7", "8"], correct: 2 },
                { question: "Welcher Planet ist der größte im Sonnensystem?", answers: ["Saturn", "Jupiter", "Neptun"], correct: 1 },
                { question: "In welchem Jahr fiel die Berliner Mauer?", answers: ["1987", "1989", "1991", "1993"], correct: 1 },
                { question: "Wasser kocht bei welcher Temperatur?", answers: ["90°C", "100°C", "110°C"], correct: 1 }
            ],
            2: [
                { question: "Welcher Ozean ist der größte?", answers: ["Atlantik", "Pazifik", "Indischer Ozean"], correct: 1 },
                { question: "Wie viele Saiten hat eine Gitarre normalerweise?", answers: ["5", "6", "7"], correct: 1 },
                { question: "Wer schrieb 'Romeo und Julia'?", answers: ["Goethe", "Shakespeare", "Cervantes"], correct: 1 },
                { question: "Welches ist das kleinste Land der Welt?", answers: ["Monaco", "Vatikanstadt", "Liechtenstein"], correct: 1 },
                { question: "Wie viele Zähne hat ein erwachsener Mensch?", answers: ["28", "32", "36"], correct: 1 }
            ],
            3: [
                { question: "Welche Farbe hat ein Smaragd?", answers: ["Rot", "Grün", "Blau"], correct: 1 },
                { question: "Wie viele Augen hat eine Spinne?", answers: ["6", "8", "4"], correct: 1 },
                { question: "Wer war der erste Präsident der USA?", answers: ["Jefferson", "Washington", "Lincoln"], correct: 1 },
                { question: "Welcher Berg ist der höchste der Welt?", answers: ["K2", "Mount Everest", "Kangchenjunga"], correct: 1 },
                { question: "Wie viele Saiten hat eine Violine?", answers: ["3", "4", "5"], correct: 1 }
            ],
            4: [
                { question: "In welchem Land liegt die Statue of Liberty?", answers: ["Kanada", "USA", "Mexiko"], correct: 1 },
                { question: "Welcher Planet ist bekannt als der 'Rote Planet'?", answers: ["Venus", "Mars", "Merkur"], correct: 1 },
                { question: "Wie viele Beine hat eine Krabbe?", answers: ["6", "8", "10"], correct: 2 },
                { question: "Wer malte die Mona Lisa?", answers: ["Michelangelo", "Leonardo da Vinci", "Raphael"], correct: 1 },
                { question: "Was ist die Hauptstadt von Japan?", answers: ["Osaka", "Tokio", "Kyoto"], correct: 1 }
            ],
            5: [
                { question: "Wie viele Seiten hat ein Würfel?", answers: ["4", "6", "8"], correct: 1 },
                { question: "Welches Tier ist das schnellste an Land?", answers: ["Antilope", "Gepard", "Gazelle"], correct: 1 },
                { question: "Wie viele Tasten hat ein Standard-Klavier?", answers: ["76", "88", "100"], correct: 1 },
                { question: "In welchem Land wurde Kaffee zuerst angebaut?", answers: ["Brasilien", "Äthiopien", "Kolumbien"], correct: 1 },
                { question: "Welcher ist der längste Fluss der Welt?", answers: ["Amazonas", "Nil", "Jangtse"], correct: 1 }
            ],
            6: [
                { question: "Wie viele Kontinente grenzen an den Pazifischen Ozean?", answers: ["3", "4", "5"], correct: 2 },
                { question: "Welcher Künstler schnitt sich sein eigenes Ohr ab?", answers: ["Monet", "Van Gogh", "Dalí"], correct: 1 },
                { question: "Was ist das größte Organ des menschlichen Körpers?", answers: ["Gehirn", "Herz", "Haut"], correct: 2 },
                { question: "In welchem Jahr landete Apollo 11 auf dem Mond?", answers: ["1967", "1969", "1971"], correct: 1 },
                { question: "Welcher ist der längste Berg der Welt?", answers: ["Everest", "Anden", "Alpen"], correct: 1 }
            ],
            7: [
                { question: "Wie viele Finger haben Pandas an einer Hand?", answers: ["4", "5", "6"], correct: 2 },
                { question: "Welches ist das größte Säugetier der Welt?", answers: ["Elefant", "Blauwal", "Giraffe"], correct: 1 },
                { question: "In welchem Jahr begann der Zweite Weltkrieg?", answers: ["1938", "1939", "1940"], correct: 1 },
                { question: "Welche Farbe hat das Blut eines Hummers?", answers: ["Rot", "Blau", "Grün"], correct: 1 },
                { question: "Wie viele Saiten hat ein Bass-Instrument?", answers: ["3", "4", "5"], correct: 1 }
            ],
            8: [
                { question: "Welcher Planet ist der nächste zur Sonne?", answers: ["Venus", "Merkur", "Mars"], correct: 1 },
                { question: "Wie lange leben Schildkröten durchschnittlich?", answers: ["20-30 Jahre", "50-100 Jahre", "100-150 Jahre"], correct: 2 },
                { question: "Wer erfand die Glühbirne?", answers: ["Tesla", "Edison", "Franklin"], correct: 1 },
                { question: "Welche Hauptstadt ist die höchste der Welt?", answers: ["Mexico City", "La Paz", "Quito"], correct: 1 },
                { question: "Wie viele Kammern hat das Herz eines Fisches?", answers: ["2", "3", "4"], correct: 0 }
            ],
            9: [
                { question: "Welcher Ozean ist am flachsten?", answers: ["Arktischer Ozean", "Pazifik", "Atlantik"], correct: 0 },
                { question: "Wie viele Finger hat ein Koala normalerweise?", answers: ["4", "5", "6"], correct: 1 },
                { question: "Wer schrieb '1984'?", answers: ["Ray Bradbury", "George Orwell", "Aldous Huxley"], correct: 1 },
                { question: "Welcher Staat hat die meisten Inseln?", answers: ["Japan", "Philippinen", "Schweden"], correct: 2 },
                { question: "Wie viele Beine hat eine Krabbe insgesamt?", answers: ["6", "8", "10"], correct: 2 }
            ],
            10: [
                { question: "Was ist das schnellste Tier im Wasser?", answers: ["Delfin", "Schwertfisch", "Hai"], correct: 1 },
                { question: "Wie viele Zähne hat ein Hai?", answers: ["200", "500", "3000"], correct: 2 },
                { question: "Welches ist das älteste Bauwerk der Welt?", answers: ["Kolosseum", "Pyramide von Gizeh", "Taj Mahal"], correct: 1 },
                { question: "Wie viele Farben hat der Regenbogen?", answers: ["5", "7", "9"], correct: 1 },
                { question: "Welcher Künstler malte Sonnenblumen?", answers: ["Monet", "Van Gogh", "Cézanne"], correct: 1 }
            ],
            11: [
                { question: "Was ist das kleinste Knochengerüst im menschlichen Körper?", answers: ["Speiche", "Steigbügel", "Wadenbein"], correct: 1 },
                { question: "Wie lange dauert eine Minute auf der Venus?", answers: ["1 Stunde", "243 Tage", "1 Monat"], correct: 1 },
                { question: "Welcher Fluss ist der längste in Europa?", answers: ["Donau", "Wolga", "Rhein"], correct: 1 },
                { question: "Wie viele Seiten hat ein Oktaeder?", answers: ["6", "8", "12"], correct: 1 },
                { question: "Welches Tier hat die längste Lebenserwartung?", answers: ["Papagei", "Elefant", "Meeresschildkröte"], correct: 2 }
            ],
            12: [
                { question: "Wie viele Nervenbahnen hat das menschliche Rückenmark?", answers: ["31", "33", "35"], correct: 0 },
                { question: "Welcher Berg ist der zweithöchste der Welt?", answers: ["Kangchenjunga", "Lhotse", "K2"], correct: 2 },
                { question: "Wie viele Kameras hat das Auge eines Mantis-Garnelen?", answers: ["6", "12", "16"], correct: 2 },
                { question: "Welches ist das einzige Säugetier, das nicht schwimmen kann?", answers: ["Nilpferd", "Biber", "Affe"], correct: 2 },
                { question: "Wie viele Zellen hat das menschliche Gehirn?", answers: ["10 Milliarden", "86 Milliarden", "100 Milliarden"], correct: 1 }
            ],
            13: [
                { question: "Welches Element ist das häufigste im Universum?", answers: ["Helium", "Wasserstoff", "Sauerstoff"], correct: 1 },
                { question: "Wie viele Herzen hat eine Krakenart (Dumbo)?", answers: ["3", "4", "5"], correct: 1 },
                { question: "Welcher Kontinent ist der größte?", answers: ["Afrika", "Asien", "Nordamerika"], correct: 1 },
                { question: "Wie viele Atmungsöffnungen hat ein Delfin?", answers: ["1", "2", "3"], correct: 0 },
                { question: "Welches Tier hat den längsten Hals?", answers: ["Straußvogel", "Giraffe", "Schwan"], correct: 1 }
            ],
            14: [
                { question: "Wie viele Zähen hat ein Krokodil?", answers: ["30", "60", "80"], correct: 2 },
                { question: "Welches ist das größte Korallenriff der Welt?", answers: ["Belize Reef", "Great Barrier Reef", "Red Sea Reef"], correct: 1 },
                { question: "Wie viele Augen hat eine Garnele?", answers: ["1", "2", "3"], correct: 0 },
                { question: "Welcher Planet rotiert am schnellsten?", answers: ["Saturn", "Jupiter", "Uranus"], correct: 1 },
                { question: "Wie viele Flüsse gibt es in Deutschland?", answers: ["850", "1200", "1500"], correct: 1 }
            ],
            15: [
                { question: "Welches ist das seltenste Blut-Bluttyp?", answers: ["AB-Negativ", "RH-Null", "O-Positiv"], correct: 1 },
                { question: "Wie viele Meter ist der Mount Everest hoch?", answers: ["8848", "8896", "9000"], correct: 1 },
                { question: "Welcher Fisch hat das größte Gehirn?", answers: ["Barsch", "Hecht", "Forellle"], correct: 0 },
                { question: "Wie viele Arten von Dinosauriern gab es?", answers: ["500", "700", "1000"], correct: 2 },
                { question: "Welches Tier schläft mit offenen Augen?", answers: ["Fisch", "Delfin", "Vogel"], correct: 1 }
            ],
            16: [
                { question: "Wie viele Kameras kann ein Insekt sehen?", answers: ["1", "3", "6"], correct: 1 },
                { question: "Welcher Vogel ist der schnellste im Flug?", answers: ["Falke", "Adler", "Schwalbe"], correct: 0 },
                { question: "Wie viele Typen von Zahnwalen gibt es?", answers: ["20", "35", "70"], correct: 2 },
                { question: "Welches ist das meistgesprochene Sprach der Welt?", answers: ["Spanisch", "Mandarin", "Englisch"], correct: 1 },
                { question: "Wie viele Knochen hat ein Vogel?", answers: ["156", "206", "256"], correct: 1 }
            ],
            17: [
                { question: "Welcher Staat hat die meisten Flüsse?", answers: ["Norwegen", "Russland", "Kanada"], correct: 2 },
                { question: "Wie viele Augen hat ein Bienenauge?", answers: ["2", "5", "8"], correct: 1 },
                { question: "Welches ist das höchste Tier der Welt?", answers: ["Elefant", "Giraffe", "Kamele"], correct: 1 },
                { question: "Wie viele Farbtöne kann ein Mensch sehen?", answers: ["1 Million", "10 Millionen", "100 Millionen"], correct: 1 },
                { question: "Welcher Planet hat die meisten Monde?", answers: ["Saturn", "Jupiter", "Uranus"], correct: 1 }
            ],
            18: [
                { question: "Wie viele Stunden schlafen Katzen pro Tag?", answers: ["8-12", "12-16", "16-20"], correct: 1 },
                { question: "Welches ist das am schnellsten wachsende Tier?", answers: ["Blauwal", "Giraffe", "Elefant"], correct: 0 },
                { question: "Wie viele Geschlechter hat Schnecken?", answers: ["1", "2", "3"], correct: 0 },
                { question: "Welcher Ozean ist am kältesten?", answers: ["Arktis", "Antarktis", "Pazifik"], correct: 1 },
                { question: "Wie viele Tage hat das Jahr auf Merkur?", answers: ["88", "225", "365"], correct: 0 }
            ],
            19: [
                { question: "Welches ist das giftigste Tier auf Erden?", answers: ["Schlange", "Qualle", "Insekt"], correct: 1 },
                { question: "Wie viele Menschen sprechen Chinesisch?", answers: ["0.5 Milliarden", "1 Milliarde", "1.5 Milliarden"], correct: 2 },
                { question: "Welcher Vogel ist der größte der Welt?", answers: ["Strauß", "Emu", "Kasuar"], correct: 0 },
                { question: "Wie viele Nervenzellen hat das Gehirn eines Oktopus?", answers: ["50 Millionen", "500 Millionen", "1 Milliarde"], correct: 1 },
                { question: "Welches Tier hat die größten Augen?", answers: ["Strauß", "Riesenkalamar", "Elefantenlaus"], correct: 1 }
            ],
            20: [
                { question: "Wie viele Arten von Bären gibt es?", answers: ["6", "8", "10"], correct: 1 },
                { question: "Welcher Fisch ist das schnellste Tier des Meeres?", answers: ["Hai", "Marlin", "Schwertfisch"], correct: 2 },
                { question: "Wie viele Zellen hat ein menschliches Auge?", answers: ["127 Millionen", "250 Millionen", "1 Milliarde"], correct: 0 },
                { question: "Welches ist das am längsten lebende Landtier?", answers: ["Elefant", "Schildkröte", "Lurch"], correct: 1 },
                { question: "Wie viele Ringe hat ein Baum pro Jahr?", answers: ["1", "2", "10"], correct: 0 }
            ],
            21: [
                { question: "Welches Element ist giftig für Haustiere?", answers: ["Wasserstoff", "Schokolade", "Salz"], correct: 1 },
                { question: "Wie viele Sprechströme hat die englische Sprache?", answers: ["20", "44", "60"], correct: 1 },
                { question: "Welcher Vogel legt die größten Eier?", answers: ["Strauß", "Emu", "Adler"], correct: 0 },
                { question: "Wie viele verschiedene Farbtypen können Mantis-Shrimps sehen?", answers: ["3", "12", "16"], correct: 2 },
                { question: "Welches ist das größte Insekt auf Erden?", answers: ["Hirschkäfer", "Riesen-Stabheuschrecke", "Riesenameise"], correct: 1 }
            ],
            22: [
                { question: "Wie viele Finger hat ein Tukan?", answers: ["2", "3", "4"], correct: 2 },
                { question: "Welches ist das giftigste Insekt der Welt?", answers: ["Biene", "Wespe", "Hornisse"], correct: 0 },
                { question: "Wie viele Gene haben Menschen?", answers: ["20000", "30000", "40000"], correct: 0 },
                { question: "Welcher Fisch hat Zähne wie ein Mensch?", answers: ["Piranha", "Barrakuda", "Scheepshaai"], correct: 2 },
                { question: "Wie viele verschiedene Arten von Käfern gibt es?", answers: ["1 Million", "2 Millionen", "4 Millionen"], correct: 2 }
            ],
            23: [
                { question: "Welches ist das längste Insekt auf Erden?", answers: ["Stabheuschrecke", "Schmetterling", "Libelle"], correct: 0 },
                { question: "Wie viele Geschmacksknospen hat eine menschliche Zunge?", answers: ["5000", "8000", "10000"], correct: 1 },
                { question: "Welches Tier hat das stärkste Biss?", answers: ["Löwe", "Krokodil", "Hippopotamus"], correct: 1 },
                { question: "Wie viele Knochen hat ein menschliches Skelett?", answers: ["186", "206", "226"], correct: 1 },
                { question: "Welches ist das schnellste Landtier?", answers: ["Löwe", "Gepard", "Gazelle"], correct: 1 }
            ],
            24: [
                { question: "Wie viele Arten von Blutegeln gibt es?", answers: ["100", "300", "700"], correct: 2 },
                { question: "Welches ist das intelligenteste Tier unter Wasser?", answers: ["Delphin", "Tintenfisch", "Wal"], correct: 1 },
                { question: "Wie viele Wirbel hat die menschliche Wirbelsäule?", answers: ["24", "33", "40"], correct: 1 },
                { question: "Welcher Vogel kann nicht fliegen?", answers: ["Pinguin", "Strauß", "Emu"], correct: 0 },
                { question: "Wie viele Atemzüge macht ein Mensch pro Minute?", answers: ["12-20", "20-30", "30-40"], correct: 0 }
            ],
            25: [
                { question: "Welches ist das höchste Gebäude der Welt?", answers: ["Petronas", "Burj Khalifa", "Empire State Building"], correct: 1 },
                { question: "Wie viele Arten von Mücken gibt es?", answers: ["2500", "3500", "5000"], correct: 1 },
                { question: "Welches ist das älteste Land der Welt?", answers: ["China", "Äthiopien", "Ägypten"], correct: 1 },
                { question: "Wie viele Muskeln haben Schmetterlinge in ihren Flügeln?", answers: ["200", "500", "1000"], correct: 2 },
                { question: "Welcher Berg ist der aktive Vulkan?", answers: ["Mount Vesuvius", "Mount Kilimanjaro", "Mount Denali"], correct: 0 }
            ],
            26: [
                { question: "Wie viele Bienenvölker gibt es auf der Welt?", answers: ["1 Million", "5 Millionen", "10 Millionen"], correct: 1 },
                { question: "Welches ist das grüßte Raubtier auf Erden?", answers: ["Eisbär", "Löwe", "Tiger"], correct: 0 },
                { question: "Wie viele Arten von Pinguinen gibt es?", answers: ["11", "17", "23"], correct: 1 },
                { question: "Welches ist das größte Loch auf der Welt?", answers: ["Grand Canyon", "Kimberley", "Kupfermine"], correct: 2 },
                { question: "Wie viele Flügel hat eine Fliege?", answers: ["2", "4", "6"], correct: 0 }
            ],
            27: [
                { question: "Welches ist das tiefste Loch im Ozean?", answers: ["Marianengraben", "Mindanaograben", "Kurilen-Kamchatka Graben"], correct: 0 },
                { question: "Wie viele Arten von Vögeln gibt es?", answers: ["5000", "10000", "15000"], correct: 1 },
                { question: "Welches ist das kleinste Säugetier?", answers: ["Spitzmaus", "Fledermaus", "Igel"], correct: 1 },
                { question: "Wie viele verschiedene Arten von Fröschen gibt es?", answers: ["4000", "6000", "8000"], correct: 2 },
                { question: "Welcher Kontinent hat die meisten Länder?", answers: ["Afrika", "Asien", "Europa"], correct: 0 }
            ],
            28: [
                { question: "Wie viele verschiedene Arten von Fischen gibt es?", answers: ["30000", "33000", "35000"], correct: 1 },
                { question: "Welches ist das seltenste Säugetier?", answers: ["Panda", "Javan-Nashorn", "Vaquita"], correct: 2 },
                { question: "Wie viele verschiedene Arten von Amphibien gibt es?", answers: ["5000", "7000", "9000"], correct: 1 },
                { question: "Welcher Staat ist der größte nach Fläche?", answers: ["Kanada", "Russland", "USA"], correct: 1 },
                { question: "Wie viele verschiedene Arten von Reptilien gibt es?", answers: ["6000", "8000", "11000"], correct: 2 }
            ],
            29: [
                { question: "Welches ist das höchste Tor der Welt?", answers: ["Taj Mahal", "Freiheitsstatue", "Christus-Statue"], correct: 1 },
                { question: "Wie viele verschiedene Arten von Säugetieren gibt es?", answers: ["3000", "4000", "6500"], correct: 2 },
                { question: "Welches ist das längste Meeresriff der Welt?", answers: ["Belize Barrier Reef", "Great Barrier Reef", "Belizean Reef"], correct: 1 },
                { question: "Wie viele verschiedene Arten von Käfern wurden entdeckt?", answers: ["100000", "300000", "1000000"], correct: 2 },
                { question: "Welcher Staat hat die meisten Einwohner?", answers: ["Indien", "China", "Bangladesch"], correct: 0 }
            ],
            30: [
                { question: "Welches ist das schnellste Flugzeug der Welt?", answers: ["SR-71", "MiG-31", "F-15"], correct: 0 },
                { question: "Wie viele verschiedene Arten von Schmetterlingen gibt es?", answers: ["10000", "20000", "40000"], correct: 2 },
                { question: "Welches ist das größte Denkmal der Welt?", answers: ["Great Pyramid", "Taj Mahal", "Christ the Redeemer"], correct: 0 },
                { question: "Wie viele verschiedene Arten von Ameisen gibt es?", answers: ["10000", "20000", "30000"], correct: 1 },
                { question: "Welcher Kontinent hat die meisten Arten von Wildtieren?", answers: ["Afrika", "Asien", "Südamerika"], correct: 1 }
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

            for (let i = 1; i <= 30; i++) {
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
