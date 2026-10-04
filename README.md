# lesna-akademia
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Leśna Akademia Przedszkolaka</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Quicksand', sans-serif;
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            min-height: 100vh;
            touch-action: manipulation;
        }
        .title-font {
            font-family: 'Fredoka One', cursive;
        }
        .bounce-custom {
            animation: bounce 2s infinite;
        }
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        .card-shadow {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body class="flex flex-col items-center justify-center p-4">

    <div class="w-full max-w-xl bg-white/90 backdrop-blur-md rounded-3xl card-shadow p-6 sm:p-8 flex flex-col items-center text-center relative overflow-hidden border-4 border-white">
        
        <!-- HEADER / SCORE -->
        <div id="header-section" class="w-full flex justify-between items-center mb-6 hidden">
            <button onclick="returnToMenu()" class="bg-amber-100 hover:bg-amber-200 text-amber-800 font-bold px-4 py-2 rounded-2xl text-sm transition shadow">
                ⬅️ Menu
            </button>
            <div class="flex items-center gap-2 bg-yellow-100 text-yellow-800 px-4 py-2 rounded-2xl font-bold shadow-sm">
                <span>⭐ Gwiazdki:</span>
                <span id="score-val" class="text-xl">0</span>
            </div>
            <div id="level-indicator" class="bg-emerald-100 text-emerald-800 px-3 py-1.5 rounded-2xl text-xs font-bold shadow-sm">
                Poziom
            </div>
        </div>

        <!-- SCREEN 1: MENU -->
        <div id="screen-menu" class="w-full flex flex-col items-center">
            <div class="text-6xl mb-2 bounce-custom">🦊🐰🦉</div>
            <h1 class="title-font text-3xl sm:text-4xl text-emerald-600 mb-2">Leśna Akademia</h1>
            <p class="text-gray-600 mb-6 font-medium">Wybierz poziom trudności dla swojego dziecka:</p>

            <div class="w-full flex flex-col gap-4">
                <button onclick="startGame(1)" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-bold p-4 rounded-2xl card-shadow transition transform hover:scale-105 flex items-center justify-between text-left">
                    <div>
                        <div class="title-font text-xl">🌱 Poziom 1: 4 Latka</div>
                        <div class="text-xs text-emerald-100 mt-1">Liczenie do 5 oraz proste dopasowywanie słów</div>
                    </div>
                    <span class="text-3xl">🐞</span>
                </button>

                <button onclick="startGame(2)" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold p-4 rounded-2xl card-shadow transition transform hover:scale-105 flex items-center justify-between text-left">
                    <div>
                        <div class="title-font text-xl">🚀 Poziom 2: 5 Lat</div>
                        <div class="text-xs text-blue-100 mt-1">Dodawanie do 10 oraz rozpoznawanie słów</div>
                    </div>
                    <span class="text-3xl">🦊</span>
                </button>

                <button onclick="startGame(3)" class="w-full bg-purple-500 hover:bg-purple-600 text-white font-bold p-4 rounded-2xl card-shadow transition transform hover:scale-105 flex items-center justify-between text-left">
                    <div>
                        <div class="title-font text-xl">⭐ Poziom 3: Vorschule</div>
                        <div class="text-xs text-purple-100 mt-1">Trudniejsze równania i słowa dwujęzyczne (PL/DE)</div>
                    </div>
                    <span class="text-3xl">🦉</span>
                </button>
            </div>
        </div>

        <!-- SCREEN 2: GAME -->
        <div id="screen-game" class="w-full flex flex-col items-center hidden">
            <!-- Question prompt container -->
            <div id="question-box" class="w-full bg-amber-50 border-2 border-amber-200 rounded-2xl p-4 sm:p-6 mb-6">
                <div id="question-text" class="title-font text-xl sm:text-2xl text-gray-800 mb-2">Treść zadania</div>
                <div id="question-visual" class="text-4xl sm:text-5xl my-4 tracking-wider">🍎🍎🍎</div>
            </div>

            <!-- Options grid -->
            <div id="options-grid" class="w-full grid grid-cols-2 gap-4 mb-4">
                <!-- Dynamically populated buttons -->
            </div>

            <!-- Feedback message -->
            <div id="feedback-msg" class="h-8 font-bold text-lg mt-2 transition-all"></div>
        </div>

        <!-- SCREEN 3: VICTORY REWARD -->
        <div id="screen-victory" class="w-full flex flex-col items-center hidden py-4">
            <div class="text-7xl mb-4 bounce-custom">🏆🎉</div>
            <h2 class="title-font text-3xl text-emerald-600 mb-2">Brawo! Gratulacje!</h2>
            <p class="text-gray-600 mb-6 font-medium">Udało Ci się ukończyć rundę zadań i nakarmić leśnego przyjaciela!</p>
            
            <div class="bg-yellow-50 border-2 border-yellow-200 rounded-2xl p-4 w-full mb-6">
                <p class="text-yellow-800 font-bold text-lg">Zdobyte gwiazdki w tej sesji: <span id="final-score" class="text-2xl">5</span> ⭐</p>
            </div>

            <button onclick="returnToMenu()" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-bold p-4 rounded-2xl card-shadow transition transform hover:scale-105 title-font text-lg">
                Zagraj Ponownie 🔄
            </button>
        </div>

    </div>

    <script>
        // Sound utility using Web Audio API
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        
        function playSound(type) {
            if (!audioCtx) return;
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            const osc = audioCtx.createOscillator();
            const gainNode = audioCtx.createGain();
            osc.connect(gainNode);
            gainNode.connect(audioCtx.destination);

            if (type === 'correct') {
                osc.type = 'sine';
                osc.frequency.setValueAtTime(440, audioCtx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(880, audioCtx.currentTime + 0.15);
                gainNode.gain.setValueAtTime(0.2, audioCtx.currentTime);
                gainNode.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.3);
                osc.start();
                osc.stop(audioCtx.currentTime + 0.3);
            } else if (type === 'wrong') {
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(200, audioCtx.currentTime);
                osc.frequency.setValueAtTime(150, audioCtx.currentTime + 0.1);
                gainNode.gain.setValueAtTime(0.2, audioCtx.currentTime);
                gainNode.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.25);
                osc.start();
                osc.stop(audioCtx.currentTime + 0.25);
            }
        }

        // Game State Variables
        let currentLevel = 1;
        let score = 0;
        let questionsAnswered = 0;
        const totalQuestionsPerSession = 5;
        let currentQuestion = null;

        // Question banks for different levels
        const level1Questions = [
            {
                type: 'count',
                prompt: 'Ile widzisz jabłek?',
                visual: '🍎🍎🍎',
                answer: 3,
                options: [2, 3, 4, 5]
            },
            {
                type: 'count',
                prompt: 'Ile widzisz gwiazdek?',
                visual: '⭐⭐⭐⭐',
                answer: 4,
                options: [2, 4, 3, 5]
            },
            {
                type: 'word',
                prompt: 'Który obrazek przedstawia kota? 🐱',
                visual: '🐱',
                answer: 'Kot',
                options: ['Kot', 'Pies', 'Auto', 'Dom']
            },
            {
                type: 'count',
                prompt: 'Ile widzisz baloników?',
                visual: '🎈🎈',
                answer: 2,
                options: [1, 2, 3, 4]
            },
            {
                type: 'word',
                prompt: 'Co to za zwierzę? 🐶',
                visual: '🐶',
                answer: 'Pies',
                options: ['Kot', 'Pies', 'Krowa', 'Ryba']
            }
        ];

        const level2Questions = [
            {
                type: 'math',
                prompt: 'Rozwiąż równanie: 3 + 2 = ?',
                visual: '🍎🍎🍎 + 🍎🍎',
                answer: 5,
                options: [4, 5, 6, 7]
            },
            {
                type: 'math',
                prompt: 'Rozwiąż równanie: 4 + 1 = ?',
                visual: '⭐ ️⭐ ️⭐ ️⭐ + ⭐',
                answer: 5,
                options: [3, 5, 6, 4]
            },
            {
                type: 'math',
                prompt: 'Rozwiąż równanie: 6 - 2 = ?',
                visual: '🎈🎈🎈🎈🎈🎈 (odejmij 2)',
                answer: 4,
                options: [3, 4, 5, 2]
            },
            {
                type: 'word',
                prompt: 'Wybierz prawidłowe słowo dla: 🚗',
                visual: '🚗',
                answer: 'Samochód',
                options: ['Rowerek', 'Samochód', 'Samolot', 'Dom']
            },
            {
                type: 'math',
                prompt: 'Rozwiąż równanie: 5 + 3 = ?',
                visual: '🔵🔵🔵🔵🔵 + 🔵🔵🔵',
                answer: 8,
                options: [7, 8, 9, 6]
            }
        ];

        const level3Questions = [
            {
                type: 'math',
                prompt: 'Zadanie Vorschule: 7 + 3 = ?',
                visual: '🔢 Liczymy w pamięci',
                answer: 10,
                options: [9, 10, 11, 8]
            },
            {
                type: 'math',
                prompt: 'Zadanie Vorschule: 9 - 4 = ?',
                visual: '🔢 Odejmowanie',
                answer: 5,
                options: [4, 5, 6, 3]
            },
            {
                type: 'word',
                prompt: 'Po niemiecku (Vorschule): Jak jest po niemiecku "Kot"?',
                visual: '🐱 = ?',
                answer: 'Katze',
                options: ['Hund', 'Katze', 'Haus', 'Apfel']
            },
            {
                type: 'math',
                prompt: 'Zadanie Vorschule: 6 + 4 = ?',
                visual: '🔢 Dodawanie do 10',
                answer: 10,
                options: [8, 9, 10, 12]
            },
            {
                type: 'word',
                prompt: 'Po niemiecku (Vorschule): Jak jest po niemiecku "Jabłko"?',
                visual: '🍎 = ?',
                answer: 'Apfel',
                options: ['Apfel', 'Banane', 'Katze', 'Auto']
            }
        ];

        let activeQuestionList = [];

        function startGame(level) {
            currentLevel = level;
            score = 0;
            questionsAnswered = 0;
            
            // Select questions pool
            if (level === 1) activeQuestionList = [...level1Questions];
            else if (level === 2) activeQuestionList = [...level2Questions];
            else activeQuestionList = [...level3Questions];

            // Shuffle questions
            activeQuestionList.sort(() => Math.random() - 0.5);

            document.getElementById('screen-menu').classList.add('hidden');
            document.getElementById('screen-victory').classList.add('hidden');
            document.getElementById('screen-game').classList.remove('hidden');
            document.getElementById('header-section').classList.remove('hidden');
            
            document.getElementById('level-indicator').innerText = `Poziom ${level}`;
            document.getElementById('score-val').innerText = score;

            loadNextQuestion();
        }

        function loadNextQuestion() {
            if (questionsAnswered >= totalQuestionsPerSession || activeQuestionList.length === 0) {
                showVictoryScreen();
                return;
            }

            currentQuestion = activeQuestionList.pop();
            document.getElementById('question-text').innerText = currentQuestion.prompt;
            document.getElementById('question-visual').innerText = currentQuestion.visual;
            document.getElementById('feedback-msg').innerText = '';

            // Shuffle options
            let shuffledOptions = [...currentQuestion.options].sort(() => Math.random() - 0.5);

            const optionsGrid = document.getElementById('options-grid');
            optionsGrid.innerHTML = '';

            shuffledOptions.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'bg-amber-100 hover:bg-amber-200 text-amber-900 title-font text-xl sm:text-2xl py-4 rounded-2xl card-shadow transition transform hover:scale-105 active:scale-95 border-2 border-amber-300';
                btn.innerText = opt;
                btn.onclick = () => checkAnswer(opt, btn);
                optionsGrid.appendChild(btn);
            });
        }

        function checkAnswer(selectedOpt, btnElement) {
            const feedback = document.getElementById('feedback-msg');
            
            // Disable all options temporarily to prevent double clicking
            const buttons = document.getElementById('options-grid').querySelectorAll('button');
            buttons.forEach(b => b.disabled = true);

            if (selectedOpt === currentQuestion.answer) {
                playSound('correct');
                score += 1;
                document.getElementById('score-val').innerText = score;
                btnElement.classList.remove('bg-amber-100', 'border-amber-300');
                btnElement.classList.add('bg-emerald-500', 'text-white', 'border-emerald-600');
                feedback.innerText = '🌟 Super! Dobra odpowiedź!';
                feedback.className = 'h-8 font-bold text-lg mt-2 text-emerald-600';
                
                questionsAnswered++;
                setTimeout(loadNextQuestion, 1200);
            } else {
                playSound('wrong');
                btnElement.classList.remove('bg-amber-100', 'border-amber-300');
                btnElement.classList.add('bg-rose-400', 'text-white', 'border-rose-500');
                feedback.innerText = '💡 Spróbuj jeszcze raz!';
                feedback.className = 'h-8 font-bold text-lg mt-2 text-rose-500';

                // Re-enable buttons after short delay for retry
                setTimeout(() => {
                    buttons.forEach(b => b.disabled = false);
                    btnElement.classList.remove('bg-rose-400', 'text-white', 'border-rose-500');
                    btnElement.classList.add('bg-amber-100', 'text-amber-900', 'border-amber-300');
                    feedback.innerText = '';
                }, 1000);
            }
        }

        function showVictoryScreen() {
            document.getElementById('screen-game').classList.add('hidden');
            document.getElementById('header-section').classList.add('hidden');
            document.getElementById('screen-victory').classList.remove('hidden');
            document.getElementById('final-score').innerText = score;
        }

        function returnToMenu() {
            document.getElementById('screen-game').classList.add('hidden');
            document.getElementById('screen-victory').classList.add('hidden');
            document.getElementById('header-section').classList.add('hidden');
            document.getElementById('screen-menu').classList.remove('hidden');
        }
    </script>
</body>
</html>
