---
layout: base
title: Maze - Homescreen
authors: Anika, Cyrus, Rishabh, Jaynee, Lillian
permalink: /learninggame/home
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Station Navigation</title>
    <style>
        /* ADDING MCQ STYLES */
        .mc-container { display: none; flex-direction: column; gap: 12px; margin-top: 15px; }
        .mc-option {
            background: rgba(15, 23, 42, 0.8); border: 2px solid #06b6d4;
            color: white; padding: 14px; border-radius: 10px; cursor: pointer;
            transition: 0.2s; font-size: 15px; text-align: left;
        }
        .mc-option:hover { background: rgba(6, 182, 212, 0.2); }
        .mc-option.correct { border-color: #10b981; background: rgba(16, 185, 129, 0.2); }
        .mc-option.wrong { border-color: #ef4444; background: rgba(239, 68, 68, 0.2); }

        /* YOUR ORIGINAL GRID AND SPACE STYLES */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', sans-serif; background: #020617; min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 20px; color: white; overflow-x: hidden; }
        .stars { position: fixed; inset: 0; z-index: 0; }
        .star { position: absolute; width: 2px; height: 2px; background: white; border-radius: 50%; }
        .title-section { position: relative; width: 100%; max-width: 900px; background: rgba(15,23,42,0.9); padding: 24px; border-radius: 20px; border: 2px solid #06b6d4; margin-bottom: 24px; z-index: 10; text-align: center;}
        .container { position: relative; width: 100%; max-width: 900px; background: rgba(15,23,42,0.85); border-radius: 24px; border: 2px solid #06b6d4; padding: 24px; z-index: 10; }
        .maze { width: 100%; max-width: 750px; aspect-ratio: 15/11; background: rgba(2,6,23,0.5); border-radius: 20px; border: 2px solid #10b981; display: grid; grid-template-columns: repeat(15, 1fr); grid-template-rows: repeat(11, 1fr); gap: 2px; padding: 8px; margin: 0 auto; }
        .cell { border: 1px solid rgba(6,182,212,0.1); }
        .wall { background: #1e293b; }
        .path { background: rgba(30,41,59,0.3); }
        .player { background: radial-gradient(circle, #06b6d4, #3b82f6); border-radius: 50%; box-shadow: 0 0 15px #06b6d4; z-index: 20; position: relative; }
        .sector { background: rgba(251,191,36,0.4); border-radius: 50%; display: flex; justify-content: center; align-items: center; color: #fbbf24; font-weight: 900; }
        .sector.completed { background: rgba(16,185,129,0.4); color: #10b981; }
        .question-modal { display: none; position: fixed; inset: 0; background: rgba(2,6,23,0.95); z-index: 1000; justify-content: center; align-items: center; padding: 20px; }
        .question-modal.active { display: flex; }
        .question-card { background: #1e293b; border: 2px solid #06b6d4; border-radius: 24px; padding: 32px; max-width: 700px; width: 100%; position: relative; }
        .run-btn { width: 100%; padding: 12px; background: #06b6d4; color: white; border: none; border-radius: 8px; cursor: pointer; margin-top: 10px; font-weight: bold; }
        .badge { display: inline-block; padding: 5px 10px; opacity: 0.3; }
        .badge.earned { opacity: 1; }
    </style>
</head>
<body>
    <div class="stars" id="stars"></div>

    <div class="title-section">
        <h1 style="color: #06b6d4; letter-spacing: 3px;">STATION NAVIGATION</h1>
        <div style="margin-top: 10px;">
            <div id="badge-1" class="badge">🥉 Easy</div>
            <div id="badge-2" class="badge">🥈 Medium</div>
            <div id="badge-3" class="badge">🥇 Hard</div>
        </div>
    </div>

    <div class="container">
        <div class="maze" id="maze"></div>
    </div>

    <div class="question-modal" id="questionModal">
        <div class="question-card">
            <h2 id="sectorTitle" style="color: #06b6d4; margin-bottom: 10px;"></h2>
            <div class="question-content">
                <p id="questionTypeText" style="color: #fbbf24; font-weight: bold; text-transform: uppercase; font-size: 14px;"></p>
                <p id="questionText" style="margin: 15px 0; font-size: 18px; color: white;"></p>
                
                <div id="mcContainer" class="mc-container"></div>

                <div id="codeRunner" style="display:none;">
                    <textarea id="codeInput" style="width: 100%; height: 80px; background: #0f172a; color: white; padding: 10px; border: 1px solid #06b6d4;"></textarea>
                    <button class="run-btn" id="runRobotBtn">Run Robot Code</button>
                    <pre id="robotOutput" style="margin-top:10px; background:#020617; padding:10px; border-radius:8px; white-space:pre-wrap;"></pre>
                </div>

                <div id="pseudoRunner" style="display:none;">
                    <textarea id="pseudoInput" style="width: 100%; height: 80px; background: #0f172a; color: white; padding: 10px; border: 1px solid #06b6d4;"></textarea>
                    <button class="run-btn" id="runPseudoBtn">Run Pseudocode</button>
                    <pre id="pseudoOutput" style="margin-top:10px; background:#020617; padding:10px; border-radius:8px; white-space:pre-wrap;"></pre>
                </div>
            </div>
            <button class="run-btn" id="nextBtn" style="display:none;">Next Module →</button>
            <button class="run-btn" id="backBtn" style="display:none; background: #10b981;">Complete Sector ✓</button>
        </div>
    </div>

    {% include_relative gameteacher.md %}

    <script>
        // STARS
        const starsContainer = document.getElementById('stars');
        for (let i = 0; i < 150; i++) {
            const star = document.createElement('div'); star.className = 'star';
            star.style.left = Math.random() * 100 + '%'; star.style.top = Math.random() * 100 + '%';
            starsContainer.appendChild(star);
        }

        const sectorQuestions = {
            1: [
                { type: "Robot Code", text: "Objective: Move robot to (3,3).", mode: "robot" },
                { type: "Pseudocode", text: "Logic: Print numbers 1-5.", mode: "pseudo" },
                { type: "Computational Thinking", text: "Logic: A robot repeats [Forward, Right 90] four times. What shape is made?", mode: "mc", options: ["Triangle", "Square", "Circle"], correct: 1 }
            ],
            2: [
                { type: "Robot Code", text: "Avoid the asteroid at (2,2).", mode: "robot" },
                { type: "Pseudocode", text: "Write the logic to find the largest number in a list.", mode: "pseudo" },
                { type: "Computational Thinking", text: "Boolean Check: What is the result of 'NOT (True AND False)'?", mode: "mc", options: ["True", "False", "Error"], correct: 0 }
            ],
            3: [
                { type: "Robot Code", text: "Navigate the Simulation Bay.", mode: "robot" },
                { type: "Pseudocode", text: "Logic for a Binary Search.", mode: "pseudo" },
                { type: "Computational Thinking", text: "A loop runs 4 times, adding the loop index (1+2+3+4). Total?", mode: "mc", options: ["4", "10", "15"], correct: 1 }
            ]
        };

        const mazeLayout = [
            [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
            [2,1,0,1,4,1,0,1,5,1,0,1,6,1,1],
            [0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],
            [0,1,1,1,1,1,1,1,1,1,1,1,1,1,0],
            [0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],
            [0,1,1,1,7,1,0,1,1,1,1,1,1,1,0],
            [0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],
            [0,1,1,1,1,1,1,1,8,1,0,1,1,1,0],
            [0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],
            [0,1,1,1,1,1,1,1,1,1,1,1,1,1,3],
            [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
        ];

        let playerPos = { x: 0, y: 1 };
        let currentSectorNum = 0;
        let currentQuestion = 0;
        const completedSectors = new Set();

        function createMaze() {
            const m = document.getElementById('maze'); m.innerHTML = '';
            for (let y = 0; y < mazeLayout.length; y++) {
                for (let x = 0; x < mazeLayout[y].length; x++) {
                    const cell = document.createElement('div'); cell.className = 'cell';
                    if (mazeLayout[y][x] === 0) cell.classList.add('wall');
                    else if (mazeLayout[y][x] === 1) cell.classList.add('path');
                    else if (mazeLayout[y][x] === 2) { cell.classList.add('path', 'start'); cell.textContent = 'S'; }
                    else if (mazeLayout[y][x] === 3) { cell.classList.add('path', 'end'); cell.textContent = 'E'; }
                    else if (mazeLayout[y][x] >= 4) {
                        const sn = mazeLayout[y][x] - 3; cell.classList.add('path', 'sector'); cell.textContent = sn;
                        if (completedSectors.has(sn)) cell.classList.add('completed');
                    }
                    if (x === playerPos.x && y === playerPos.y) cell.classList.add('player');
                    m.appendChild(cell);
                }
            }
        }

        function movePlayer(dx, dy) {
            const nx = playerPos.x + dx, ny = playerPos.y + dy;
            if (ny >= 0 && ny < 11 && nx >= 0 && nx < 15 && mazeLayout[ny][nx] !== 0) {
                playerPos.x = nx; playerPos.y = ny;
                const val = mazeLayout[ny][nx];
                if (val >= 4) {
                    const sn = val - 3;
                    if (!completedSectors.has(sn)) {
                        currentSectorNum = sn; currentQuestion = 0;
                        initTeacher(sn, 0); // Open Robot Overlay
                    }
                }
                createMaze();
            }
        }

        function showQuestion() {
            const currentQ = sectorQuestions[currentSectorNum][currentQuestion];
            const mcC = document.getElementById('mcContainer');
            
            document.getElementById('codeRunner').style.display = 'none';
            document.getElementById('pseudoRunner').style.display = 'none';
            mcC.style.display = 'none';
            document.getElementById('nextBtn').style.display = 'none';
            document.getElementById('backBtn').style.display = 'none';
            
            document.getElementById('sectorTitle').innerText = `SECTOR ${currentSectorNum}`;
            document.getElementById('questionTypeText').innerText = currentQ.type;
            document.getElementById('questionText').innerText = currentQ.text;
            document.getElementById('robotOutput').textContent = '';
        document.getElementById('pseudoOutput').textContent = '';

            
            if (typeof updateHint === "function") updateHint(currentQuestion);

            if (currentQ.mode === 'robot') {
                document.getElementById('codeRunner').style.display = 'block';
                document.getElementById('nextBtn').style.display = 'block';
            } else if (currentQ.mode === 'pseudo') {
                document.getElementById('pseudoRunner').style.display = 'block';
                document.getElementById('nextBtn').style.display = 'block';
            } else if (currentQ.mode === 'mc') {
                mcC.style.display = 'flex'; mcC.innerHTML = '';
                currentQ.options.forEach((opt, idx) => {
                    const b = document.createElement('div'); b.className = 'mc-option'; b.innerText = opt;
                    b.onclick = () => {
                        if (idx === currentQ.correct) {
                            b.classList.add('correct');
                            if (currentQuestion === 2) document.getElementById('backBtn').style.display = 'block';
                            else document.getElementById('nextBtn').style.display = 'block';
                        } else b.classList.add('wrong');
                    };
                    mcC.appendChild(b);
                });
            }
        }

        document.getElementById('nextBtn').onclick = () => { currentQuestion++; showQuestion(); };
        document.getElementById('backBtn').onclick = () => {
            document.getElementById('questionModal').classList.remove('active');
            completedSectors.add(currentSectorNum);
            document.getElementById(`badge-${currentSectorNum}`).classList.add('earned');
            createMaze();
        };

        document.addEventListener('keydown', (e) => {
            if (document.getElementById('questionModal').classList.contains('active') || document.getElementById('teacher-overlay').style.display === 'flex') return;
            if(e.key === 'ArrowUp') movePlayer(0, -1);
            if(e.key === 'ArrowDown') movePlayer(0, 1);
            if(e.key === 'ArrowLeft') movePlayer(-1, 0);
            if(e.key === 'ArrowRight') movePlayer(1, 0);
        });

        // --- CODE RUNNER LOGIC (Robot + Pseudocode) ---
        const runRobotBtn = document.getElementById('runRobotBtn');
        const runPseudoBtn = document.getElementById('runPseudoBtn');

        runRobotBtn.onclick = () => {
            const code = document.getElementById('codeInput').value;
            const out = document.getElementById('robotOutput');

            try {
                // Very simple JS runner (placeholder)
                const result = eval(code);
                out.textContent = result !== undefined ? String(result) : "Robot code ran.";
            } catch (err) {
                out.textContent = "Error: " + err.message;
            }
        };

        runPseudoBtn.onclick = () => {
            const pseudo = document.getElementById('pseudoInput').value;
            const out = document.getElementById('pseudoOutput');

            // Placeholder runner: just echoes what they typed
            out.textContent = "Pseudocode submitted:\n" + pseudo;
        };

        createMaze();
    </script>
</body>
</html>