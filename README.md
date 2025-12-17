<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Exercise: Transition & Reflection</title>
    <style>
        :root {
            --primary-color: #008080;
            --secondary-color: #20b2aa;
            --accent-color: #3cb371;
            --light-bg: #f0fff0;
            --dark-text: #333;
            --success-color: #28a745;
            --error-color: #dc3545;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0; padding: 0;
            background-color: var(--light-bg);
            color: var(--dark-text);
            line-height: 1.6;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 20px;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }

        .score-display {
            position: absolute;
            top: 15px; right: 20px;
            font-weight: bold;
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px; border-radius: 5px;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .nav-buttons {
            display: flex; justify-content: center;
            gap: 10px; margin-bottom: 20px; flex-wrap: wrap; 
        }

        .nav-button {
            padding: 10px 15px; border: none; border-radius: 8px;
            cursor: pointer; background-color: var(--secondary-color);
            color: white; font-weight: bold; flex-grow: 1; min-width: 150px;
        }

        .nav-button.active { background-color: var(--primary-color); }

        .exercise-section {
            background-color: white; padding: 25px;
            border-radius: 12px; box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        h2 { color: var(--primary-color); border-bottom: 3px solid var(--secondary-color); padding-bottom: 10px; }

        .vocab-item { display: flex; padding: 15px 0; border-bottom: 1px dashed #ccc; }
        .word-col { flex: 0 0 180px; font-size: 1.1em; font-weight: bold; color: var(--primary-color); }
        .details-col { flex: 1; }

        .word-bank { display: flex; flex-wrap: wrap; gap: 10px; padding: 15px; margin-bottom: 20px; border-radius: 8px; background: linear-gradient(45deg, var(--secondary-color), var(--accent-color)); }
        .word-bank-item { background: white; padding: 5px 10px; border-radius: 5px; font-weight: bold; }

        .gap-sentence { margin-bottom: 15px; padding: 10px; border-left: 5px solid var(--secondary-color); background: #f9f9f9; }
        .gap-input { width: 140px; padding: 5px; border: 2px solid #ccc; border-radius: 4px; text-align: center; }

        .matching-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .match-item { padding: 12px; border: 2px solid #ddd; border-radius: 8px; cursor: pointer; }
        .matched { background-color: var(--success-color) !important; color: white !important; }

        .mic-btn { background: var(--primary-color); color: white; border: none; padding: 8px; border-radius: 50%; cursor: pointer; }
    </style>
</head>
<body>

    <header>
        <h1>English Vocabulary Interactive Practice</h1>
        <div class="score-display">Score: <span id="current-score">0</span> / 24</div>
    </header>

    <div class="container">
        <div class="nav-buttons">
            <button class="nav-button active" onclick="showSection('vocab-list-section', this)">1. 📚 Vocabulary List</button>
            <button class="nav-button" onclick="showSection('writing-gaps-section', this)">2. ✍️ Fill in Gaps</button>
            <button class="nav-button" onclick="showSection('matching-section', this)">3. 🔗 Match Definitions</button>
            <button class="nav-button" onclick="showSection('pronunciation-section', this)">4. 🗣️ Pronunciation</button>
        </div>

        <div id="vocab-list-section" class="exercise-section">
            <h2>📚 Vocabulary List</h2>
            <div id="vocab-list-content"></div>
        </div>

        <div id="writing-gaps-section" class="exercise-section" style="display: none;">
            <h2>✍️ Fill in the Gaps (Writing)</h2>
            <div class="word-bank" id="writing-word-bank"></div>
            <div id="writing-gaps-content"></div>
            <button onclick="checkWritingGaps()" style="background: var(--success-color); color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer;">Check Answers</button>
        </div>

        <div id="matching-section" class="exercise-section" style="display: none;">
            <h2>🔗 Match Definitions</h2>
            <div class="matching-grid" id="matching-grid"></div>
        </div>

        <div id="pronunciation-section" class="exercise-section" style="display: none;">
            <h2>🗣️ Pronunciation Practice</h2>
            <div id="pronunciation-content"></div>
        </div>
    </div>

    <script>
        const vocabData = [
            {
                word: "in this moment",
                context: "current time",
                meaning: "At this exact time; currently.",
                example: "**In this moment**, I am too busy to discuss the new project.",
                writingSentence: "**___**, we are focusing all our energy on customer support.",
                definition: "Right now; at the present time.",
                pronSentence: "I feel very happy **___**."
            },
            {
                word: "scarf",
                context: "clothing accessory",
                meaning: "A piece of fabric worn around the neck for warmth or fashion.",
                example: "It was so cold outside that I had to wrap a wool **scarf** around my face.",
                writingSentence: "She wore a silk **___** that matched her blue dress perfectly.",
                definition: "A length of fabric worn around the neck or head.",
                pronSentence: "Don't forget your **___**; it's freezing!"
            },
            {
                word: "furthermore",
                context: "adding information",
                meaning: "In addition; besides (used to introduce a fresh consideration).",
                example: "The house is too expensive; **furthermore**, it is too far from the city.",
                writingSentence: "The laptop is fast; **___**, it has a very long battery life.",
                definition: "In addition to what has just been said.",
                pronSentence: "**___**, we must consider the environmental impact."
            },
            {
                word: "pop-up",
                context: "technology/appearance",
                meaning: "To appear suddenly or unexpectedly, especially on a computer screen.",
                example: "A small window will **pop-up** asking for your email address.",
                writingSentence: "I hate it when advertisements **___** while I'm reading an article.",
                definition: "To appear suddenly; a small window that opens automatically on a screen.",
                pronSentence: "Did a notification **___** on your phone?"
            },
            {
                word: "reasonable",
                context: "fair or sensible",
                meaning: "Fair and sensible; as much as is appropriate or fair.",
                example: "The price for the repair seemed very **reasonable** to me.",
                writingSentence: "It is perfectly **___** to ask for a refund if the product is broken.",
                definition: "Having sound judgment; fair and sensible; not expensive.",
                pronSentence: "That sounds like a **___** request."
            },
            {
                word: "in hindsight",
                context: "looking back",
                meaning: "Understanding of a situation only after it has happened (Col sennò di poi).",
                example: "**In hindsight**, I should have taken the job offer when I had the chance.",
                writingSentence: "**___**, we realize that our marketing strategy was too aggressive.",
                definition: "The ability to understand an event only after it has happened.",
                pronSentence: "It's easy to be right **___**."
            },
            {
                word: "driver",
                context: "motivation/factor",
                meaning: "A factor that causes a particular phenomenon to happen or develop.",
                example: "Innovation is the main **driver** of economic growth in this decade.",
                writingSentence: "A desire for success is a powerful **___** for many entrepreneurs.",
                definition: "A person who drives a vehicle; or a force that makes something happen.",
                pronSentence: "Passion is a key **___** for her."
            },
            {
                word: "strength",
                context: "power or quality",
                meaning: "The quality or state of being physically or mentally strong.",
                example: "She found the **strength** to finish the race despite her injury.",
                writingSentence: "One of the major **___** of this team is its diversity.",
                definition: "The state or quality of being strong; a good characteristic.",
                pronSentence: "Knowledge is a source of **___**."
            }
        ];

        let score = 0;
        let writingResults = new Array(vocabData.length).fill(false);
        let matchResults = new Array(vocabData.length).fill(false);
        let pronResults = new Array(vocabData.length).fill(false);

        function showSection(id, btn) {
            document.querySelectorAll('.exercise-section').forEach(s => s.style.display = 'none');
            document.getElementById(id).style.display = 'block';
            document.querySelectorAll('.nav-button').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        function setup() {
            const list = document.getElementById('vocab-list-content');
            list.innerHTML = vocabData.map(i => `
                <div class="vocab-item">
                    <div class="word-col">${i.word}</div>
                    <div class="details-col">
                        <span style="font-style:italic; color: #3cb371;">(${i.context})</span>
                        <p>${i.meaning}<br><small>Example: ${i.example}</small></p>
                    </div>
                </div>
            `).join('');

            const bank = document.getElementById('writing-word-bank');
            bank.innerHTML = vocabData.map(i => `<div class="word-bank-item">${i.word}</div>`).join('');
            const gaps = document.getElementById('writing-gaps-content');
            gaps.innerHTML = vocabData.map((i, idx) => {
                const parts = i.writingSentence.split('___');
                return `<div class="gap-sentence">${parts[0]}<input type="text" class="gap-input" data-idx="${idx}" data-ans="${i.word.toLowerCase()}">${parts[1]}<span id="fb-${idx}"></span></div>`;
            }).join('');

            const grid = document.getElementById('matching-grid');
            const words = vocabData.map((i, idx) => ({t: i.word, id: idx, type: 'w'}));
            const defs = vocabData.map((i, idx) => ({t: i.definition, id: idx, type: 'd'}));
            const items = [...words, ...defs].sort(() => Math.random() - 0.5);
            grid.innerHTML = items.map(i => `<div class="match-item" data-id="${i.id}" data-type="${i.type}" onclick="handleMatch(this)">${i.t}</div>`).join('');

            const pron = document.getElementById('pronunciation-content');
            pron.innerHTML = vocabData.map((i, idx) => `
                <div class="gap-sentence">
                    ${i.pronSentence.replace(i.word, '_____')} 
                    <button class="mic-btn" onclick="testPron('${i.word}', ${idx})">🎤</button>
                    <span id="pron-fb-${idx}">Click to say "${i.word}"</span>
                </div>
            `).join('');
        }

        function checkWritingGaps() {
            document.querySelectorAll('.gap-input').forEach(input => {
                const idx = input.dataset.idx;
                const ans = input.dataset.ans.replace('-', '');
                const val = input.value.trim().toLowerCase().replace('-', '');
                if (val === ans) {
                    writingResults[idx] = true;
                    document.getElementById(`fb-${idx}`).textContent = " ✅";
                } else {
                    document.getElementById(`fb-${idx}`).textContent = " ❌";
                }
            });
            updateScore();
        }

        let selected = null;
        function handleMatch(el) {
            if (el.classList.contains('matched')) return;
            if (!selected) {
                selected = el; el.style.background = '#c8e6c9';
            } else {
                if (selected.dataset.id === el.dataset.id && selected.dataset.type !== el.dataset.type) {
                    selected.classList.add('matched'); el.classList.add('matched');
                    matchResults[el.dataset.id] = true; updateScore();
                }
                selected.style.background = ''; selected = null;
            }
        }

        function testPron(word, idx) {
            const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            rec.lang = 'en-US';
            rec.start();
            rec.onresult = (e) => {
                const say = e.results[0][0].transcript.toLowerCase();
                if (say.includes(word.toLowerCase().replace('-', ' '))) {
                    document.getElementById(`pron-fb-${idx}`).textContent = "✅ Correct!";
                    pronResults[idx] = true; updateScore();
                } else {
                    document.getElementById(`pron-fb-${idx}`).textContent = `❌ Heard: "${say}"`;
                }
            };
        }

        function updateScore() {
            const s = writingResults.filter(x => x).length + matchResults.filter(x => x).length + pronResults.filter(x => x).length;
            document.getElementById('current-score').textContent = s;
        }

        window.onload = setup;
    </script>
</body>
</html>
