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
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #020617 0%, #0f172a 50%, #1e1b4b 100%);
            height: 100vh;
            display: flex; 
            justify-content: center; 
            align-items: center;
            overflow: hidden;
            position: relative;
        }

        /* Animated stars background */
        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .stars {
            position: fixed;
            inset: 0;
            overflow: hidden;
            z-index: 0;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite;
        }

        /* Ambient glow effects */
        body::before {
            content: '';
            position: fixed;
            top: 10%;
            left: 10%;
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, rgba(6,182,212,0.15), transparent 70%);
            filter: blur(80px);
            z-index: 0;
        }

        body::after {
            content: '';
            position: fixed;
            bottom: 10%;
            right: 10%;
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, rgba(168,85,247,0.15), transparent 70%);
            filter: blur(80px);
            z-index: 0;
        }

        .container {
            position: relative; 
            width: 95vw; 
            max-width: 1000px; 
            height: 95vh; 
            max-height: 700px;
            background: rgba(15, 23, 42, 0.85); 
            backdrop-filter: blur(20px);
            border-radius: 24px; 
            border: 2px solid rgba(6,182,212,0.4);
            box-shadow: 0 0 60px rgba(6,182,212,0.25); 
            overflow: hidden;
            z-index: 1;
        }

        .title-section {
            position: absolute; 
            top: 0; 
            left: 0;
            right: 0;
            background: linear-gradient(135deg, rgba(15,23,42,0.95), rgba(30,41,59,0.95));
            backdrop-filter: blur(10px);
            padding: 20px;
            border-bottom: 2px solid rgba(6,182,212,0.3);
            z-index: 50;
        }

        .title-header {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin-bottom: 8px;
        }

        .title-icon {
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); filter: drop-shadow(0 0 10px rgba(6,182,212,0.5)); }
            50% { transform: scale(1.05); filter: drop-shadow(0 0 20px rgba(6,182,212,0.8)); }
        }

        .title {
            color: #06b6d4;
            font-size: 28px; 
            font-weight: 900; 
            text-transform: uppercase;
            letter-spacing: 4px;
            text-shadow: 0 0 20px rgba(6,182,212,0.5);
        }

        .subtitle {
            text-align: center;
            color: rgba(103,232,249,0.7);
            font-size: 12px;
            letter-spacing: 0.8px;
            font-family: 'Courier New', monospace;
        }

        .maze-container {
            width: 100%; 
            height: 100%; 
            padding: 100px 20px 20px 20px;
            display: flex; 
            justify-content: center; 
            align-items: center;
        }

        .maze {
            position: relative; 
            width: 100%; 
            max-width: 800px; 
            height: 100%; 
            max-height: 600px;
            background: rgba(2, 6, 23, 0.5);
            backdrop-filter: blur(10px);
            border-radius: 20px; 
            border: 2px solid rgba(16,185,129,0.4);
            box-shadow: inset 0 0 30px rgba(16,185,129,0.1);
            display: grid; 
            grid-template-columns: repeat(15, 1fr); 
            grid-template-rows: repeat(11, 1fr);
            padding: 4px;
            gap: 1px;
        }

        .cell { 
            border: 1px solid rgba(6,182,212,0.08);
            border-radius: 2px;
        }

        .wall { 
            background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
        }

        .path { 
            background: rgba(30, 41, 59, 0.3); 
        }

        .player {
            background: radial-gradient(circle, #06b6d4 0%, #3b82f6 100%);
            border-radius: 50%; 
            box-shadow: 0 0 20px rgba(6,182,212,0.8);
            position: relative; 
            z-index: 20;
            animation: playerPulse 1.5s infinite;
        }

        @keyframes playerPulse {
            0%, 100% { box-shadow: 0 0 20px rgba(6,182,212,0.8); }
            50% { box-shadow: 0 0 30px rgba(6,182,212,1); }
        }

        .sector {
            background: linear-gradient(135deg, rgba(251,191,36,0.3) 0%, rgba(217,119,6,0.3) 100%);
            border-radius: 50%; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            color: #fbbf24; 
            font-weight: 900; 
            font-size: 11px; 
            z-index: 10;
            position: relative;
            overflow: hidden;
        }

        .sector::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .sector.completed { 
            background: linear-gradient(135deg, rgba(16,185,129,0.3) 0%, rgba(5,150,105,0.3) 100%);
            color: #10b981;
        }

        .start { 
            background: rgba(16,185,129,0.3);
            border-radius: 50%; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            color: #10b981;
            font-size: 10px;
            font-weight: 900;
            box-shadow: 0 0 15px rgba(16,185,129,0.5);
        }

        .end { 
            background: rgba(168,85,247,0.3);
            border-radius: 50%; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            color: #a855f7;
            font-size: 10px;
            font-weight: 900;
            box-shadow: 0 0 15px rgba(168,85,247,0.5);
        }

        .controls-hint {
            margin-top: 12px;
            text-align: center;
            color: rgba(6,182,212,0.6);
            font-size: 12px;
            font-family: 'Courier New', monospace;
        }

        /* Modal Styles */
        .question-modal {
            display: none; 
            position: absolute; 
            top: 0; 
            left: 0; 
            width: 100%; 
            height: 100%;
            background: rgba(2,6,23,0.95); 
            backdrop-filter: blur(10px);
            z-index: 100; 
            justify-content: center; 
            align-items: center;
        }

        .question-modal.active { display: flex; }

        .question-card {
            background: linear-gradient(135deg, rgba(30,41,59,0.95), rgba(51,65,85,0.95));
            backdrop-filter: blur(20px);
            border-radius: 24px; 
            border: 2px solid rgba(6,182,212,0.5); 
            padding: 40px; 
            max-width: 600px; 
            width: 90%;
            box-shadow: 0 0 60px rgba(6,182,212,0.3);
            position: relative;
            overflow: hidden;
        }

        .question-card::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(90deg, transparent, rgba(6,182,212,0.05), transparent);
            animation: hologramScan 3s infinite;
        }

        @keyframes hologramScan {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .question-header {
            display: flex;
            align-items: center;
            gap: 16px;
            margin-bottom: 24px;
            position: relative;
            z-index: 1;
        }

        .sector-badge {
            width: 56px;
            height: 56px;
            background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
            font-weight: 900;
            box-shadow: 0 0 30px rgba(251,191,36,0.6);
        }

        .sector-info h2 {
            color: #06b6d4;
            font-size: 24px;
            font-weight: 900;
            margin-bottom: 4px;
        }

        .sector-info p {
            color: rgba(103,232,249,0.7);
            font-size: 13px;
            font-family: 'Courier New', monospace;
        }

        .question-content {
            margin: 24px 0;
            padding: 24px;
            background: rgba(2,6,23,0.5);
            border-radius: 16px;
            border: 1px solid rgba(6,182,212,0.3);
            position: relative;
            z-index: 1;
        }

        .question-type {
            color: #fbbf24;
            font-weight: 900;
            text-transform: uppercase;
            font-size: 13px;
            letter-spacing: 1px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .question-text {
            color: #e2e8f0;
            font-size: 16px;
            line-height: 1.6;
        }

        .progress-indicators {
            display: flex;
            gap: 8px;
            margin-bottom: 24px;
            position: relative;
            z-index: 1;
        }

        .progress-bar {
            height: 6px;
            flex: 1;
            border-radius: 3px;
            background: rgba(51,65,85,0.5);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .progress-bar.active {
            background: #06b6d4;
            box-shadow: 0 0 15px rgba(6,182,212,0.6);
        }

        .progress-bar.active::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            animation: progressShine 1.5s infinite;
        }

        @keyframes progressShine {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .progress-bar.completed {
            background: rgba(16,185,129,0.6);
        }

        .question-nav {
            display: flex;
            gap: 12px;
            position: relative;
            z-index: 1;
        }

        .btn { 
            flex: 1;
            padding: 16px 24px; 
            border: none; 
            border-radius: 12px; 
            cursor: pointer; 
            text-transform: uppercase;
            font-weight: 900;
            letter-spacing: 1.5px;
            transition: all 0.2s ease;
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transform: translateX(-100%);
            transition: transform 0.5s ease;
        }

        .btn:hover::before {
            transform: translateX(100%);
        }

        .btn-next { 
            background: linear-gradient(135deg, #06b6d4 0%, #3b82f6 100%);
            color: white;
            box-shadow: 0 0 25px rgba(6,182,212,0.4);
        }

        .btn-next:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 35px rgba(6,182,212,0.6);
        }

        .btn-complete {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            color: white;
            box-shadow: 0 0 25px rgba(16,185,129,0.4);
        }

        .btn-complete:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 35px rgba(16,185,129,0.6);
        }

        @media (max-width: 768px) {
            .title { font-size: 20px; }
            .question-card { padding: 28px 24px; }
        }
    </style>
</head>
<body>
    <!-- Stars background -->
    <div class="stars" id="stars"></div>

    <div class="container">
        <div class="title-section">
            <div class="title-header">
                <div class="title-icon">🚀</div>
                <div class="title">Station Navigation</div>
            </div>
            <div class="subtitle">Cadet Training Protocol // Sector Clearance Required</div>
        </div>

        <div class="maze-container">
            <div>
                <div class="maze" id="maze"></div>
                <div class="controls-hint">
                    Use arrow keys to navigate • Reach sector checkpoints to begin training
                </div>
            </div>
        </div>

        <!-- Question Modal -->
        <div class="question-modal" id="questionModal">
            <div class="question-card">
                <div class="question-header">
                    <div class="sector-badge">
                        <span id="sectorNumber">1</span>
                    </div>
                    <div class="sector-info">
                        <h2 id="sectorTitle">SECTOR 1</h2>
                        <p id="sectorName">Navigation Deck</p>
                    </div>
                </div>

                <div class="progress-indicators" id="progressIndicators">
                    <div class="progress-bar"></div>
                    <div class="progress-bar"></div>
                    <div class="progress-bar"></div>
                </div>

                <div class="question-content">
                    <div class="question-type" id="questionType">
                        <span>📡</span>
                        <span id="questionTypeText">Navigation Systems</span>
                    </div>
                    <div class="question-text" id="questionText">Configure robot navigation protocols</div>
                </div>

                <div class="question-nav">
                    <button class="btn btn-next" id="nextBtn">Next Module →</button>
                    <button class="btn btn-complete" id="backBtn" style="display: none;">Complete Sector ✓</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Integrate Game Teacher Assets -->
   {% capture teacher_raw %}
    {% include_relative gameteacher.md %}
    {% endcapture %}

    {% assign parts = teacher_raw | split: '---' %}
    {{ parts | slice: 2, parts.size | join: '---' }}

    <script>
        // Generate stars
        const starsContainer = document.getElementById('stars');
        for (let i = 0; i < 150; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.animationDelay = Math.random() * 3 + 's';
            star.style.opacity = Math.random() * 0.7 + 0.3;
            starsContainer.appendChild(star);
        }

        const maze = document.getElementById('maze');
        const modal = document.getElementById('questionModal');
        const sectorNumber = document.getElementById('sectorNumber');
        const sectorTitle = document.getElementById('sectorTitle');
        const sectorName = document.getElementById('sectorName');
        const questionType = document.getElementById('questionTypeText');
        const questionText = document.getElementById('questionText');
        const nextBtn = document.getElementById('nextBtn');
        const backBtn = document.getElementById('backBtn');

        let currentQuestion = 0;
        let currentSectorNum = 0;
        const completedSectors = new Set();

        const sectorNames = [
            "Navigation Deck",
            "Logic Core", 
            "Simulation Bay",
            "Dock Alpha",
            "Dock Beta"
        ];

        const questions = [
            { type: "Navigation Systems", text: "Configure robot navigation protocols", icon: "📡" },
            { type: "Logic Core", text: "Program decision algorithms", icon: "⚡" },
            { type: "Simulation Bay", text: "Interactive training module", icon: "🎮" }
        ];

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

        function createMaze() {
            maze.innerHTML = '';
            for (let y = 0; y < mazeLayout.length; y++) {
                for (let x = 0; x < mazeLayout[y].length; x++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    
                    if (mazeLayout[y][x] === 0) {
                        cell.classList.add('wall');
                    } else if (mazeLayout[y][x] === 1) {
                        cell.classList.add('path');
                    } else if (mazeLayout[y][x] === 2) {
                        cell.classList.add('path', 'start');
                        cell.textContent = 'S';
                    } else if (mazeLayout[y][x] === 3) {
                        cell.classList.add('path', 'end');
                        cell.textContent = 'E';
                    } else if (mazeLayout[y][x] >= 4 && mazeLayout[y][x] <= 8) {
                        const sectorNum = mazeLayout[y][x] - 3;
                        cell.classList.add('path', 'sector');
                        if (completedSectors.has(sectorNum)) {
                            cell.classList.add('completed');
                        }
                        cell.textContent = sectorNum;
                    }
                    
                    if (x === playerPos.x && y === playerPos.y) {
                        cell.classList.add('player');
                    }
                    
                    maze.appendChild(cell);
                }
            }
        }

        function dismissTeacher() {
            document.getElementById('teacher-overlay').style.display = 'none';
            modal.classList.add('active');
            showQuestion();
        }

        function movePlayer(dx, dy) {
            const newX = playerPos.x + dx;
            const newY = playerPos.y + dy;
            
            if (newY >= 0 && newY < mazeLayout.length && 
                newX >= 0 && newX < mazeLayout[0].length && 
                mazeLayout[newY][newX] !== 0) {
                
                playerPos.x = newX;
                playerPos.y = newY;
                
                const cellValue = mazeLayout[newY][newX];
                if (cellValue >= 4 && cellValue <= 8) {
                    const sectorNum = cellValue - 3;
                    if (!completedSectors.has(sectorNum)) {
                        currentSectorNum = sectorNum;
                        currentQuestion = 0;
                        initTeacher(sectorNum, 0);
                    }
                }
                
                createMaze();
            }
        }

        function showQuestion() {
            sectorNumber.textContent = currentSectorNum;
            sectorTitle.textContent = `SECTOR ${currentSectorNum}`;
            sectorName.textContent = sectorNames[currentSectorNum - 1] || "Training Module";
            
            const currentQ = questions[currentQuestion];
            questionType.textContent = currentQ.type;
            questionText.textContent = currentQ.text;
            
            document.querySelector('.question-type span:first-child').textContent = currentQ.icon;
            
            // Update progress indicators
            const progressBars = document.querySelectorAll('.progress-bar');
            progressBars.forEach((bar, idx) => {
                bar.classList.remove('active', 'completed');
                if (idx === currentQuestion) {
                    bar.classList.add('active');
                } else if (idx < currentQuestion) {
                    bar.classList.add('completed');
                }
            });
            
            if (typeof updateHint === "function") updateHint(currentQuestion);

            if (currentQuestion === questions.length - 1) {
                nextBtn.style.display = 'none';
                backBtn.style.display = 'block';
            } else {
                nextBtn.style.display = 'block';
                backBtn.style.display = 'none';
            }
        }

        nextBtn.addEventListener('click', () => {
            currentQuestion++;
            showQuestion();
        });

        backBtn.addEventListener('click', () => {
            modal.classList.remove('active');
            completedSectors.add(currentSectorNum);
            createMaze();
        });

        document.addEventListener('keydown', (e) => {
            if (modal.classList.contains('active') || 
                document.getElementById('teacher-overlay')?.style.display === 'flex') return;
            
            if(e.key === 'ArrowUp') movePlayer(0, -1);
            if(e.key === 'ArrowDown') movePlayer(0, 1);
            if(e.key === 'ArrowLeft') movePlayer(-1, 0);
            if(e.key === 'ArrowRight') movePlayer(1, 0);
        });

        createMaze();
    </script>
</body>
</html>