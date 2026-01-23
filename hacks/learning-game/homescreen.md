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
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            overflow-x: hidden;
            position: relative;
            padding: 20px;
        }

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

        .title-section {
            position: relative;
            width: 100%;
            max-width: 900px;
            background: linear-gradient(135deg, rgba(15,23,42,0.95), rgba(30,41,59,0.95));
            backdrop-filter: blur(20px);
            padding: 24px;
            border-radius: 20px;
            border: 2px solid rgba(6,182,212,0.4);
            box-shadow: 0 0 40px rgba(6,182,212,0.25);
            margin-bottom: 24px;
            z-index: 10;
        }

        .title-header {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 16px;
            margin-bottom: 8px;
        }

        .title-icon {
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); filter: drop-shadow(0 0 10px rgba(6,182,212,0.5)); }
            50% { transform: scale(1.05); filter: drop-shadow(0 0 20px rgba(6,182,212,0.8)); }
        }

        .title {
            color: #06b6d4;
            font-size: 36px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 4px;
            text-shadow: 0 0 20px rgba(6,182,212,0.5);
        }

        .subtitle {
            text-align: center;
            color: rgba(103,232,249,0.7);
            font-size: 14px;
            letter-spacing: 1px;
            font-family: 'Courier New', monospace;
        }

        .badge-container {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin-top: 16px;
            flex-wrap: wrap;
        }

        .badge {
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 1px;
            opacity: 0.3;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .badge.earned {
            opacity: 1;
            box-shadow: 0 0 20px currentColor;
        }

        .badge.easy { background: linear-gradient(135deg, #10b981, #059669); color: white; border-color: #10b981; }
        .badge.medium { background: linear-gradient(135deg, #3b82f6, #2563eb); color: white; border-color: #3b82f6; }
        .badge.hard { background: linear-gradient(135deg, #f59e0b, #d97706); color: white; border-color: #f59e0b; }

        .container {
            position: relative;
            width: 100%;
            max-width: 900px;
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            border: 2px solid rgba(6,182,212,0.4);
            box-shadow: 0 0 60px rgba(6,182,212,0.25);
            padding: 24px;
            z-index: 10;
        }

        .maze-container {
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 16px;
        }

        .maze {
            width: 100%;
            max-width: 750px;
            aspect-ratio: 15/11;
            background: rgba(2, 6, 23, 0.5);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            border: 2px solid rgba(16,185,129,0.4);
            box-shadow: inset 0 0 30px rgba(16,185,129,0.1);
            display: grid;
            grid-template-columns: repeat(15, 1fr);
            grid-template-rows: repeat(11, 1fr);
            padding: 8px;
            gap: 2px;
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
            animation: playerPulse 1.5s infinite;
        }

        @keyframes playerPulse {
            0%, 100% { box-shadow: 0 0 20px rgba(6,182,212,0.8); }
            50% { box-shadow: 0 0 30px rgba(6,182,212,1); }
        }

        .sector {
            background: linear-gradient(135deg, rgba(251,191,36,0.4) 0%, rgba(217,119,6,0.4) 100%);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #fbbf24;
            font-weight: 900;
            font-size: 14px;
            box-shadow: 0 0 15px rgba(251,191,36,0.4);
        }

        .sector.completed {
            background: linear-gradient(135deg, rgba(16,185,129,0.4) 0%, rgba(5,150,105,0.4) 100%);
            color: #10b981;
            box-shadow: 0 0 15px rgba(16,185,129,0.5);
        }

        .start, .end {
            background: linear-gradient(135deg, rgba(168,85,247,0.4) 0%, rgba(147,51,234,0.4) 100%);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #a855f7;
            font-size: 14px;
            font-weight: 900;
            box-shadow: 0 0 20px rgba(168,85,247,0.6);
        }

        .controls-hint {
            text-align: center;
            color: rgba(6,182,212,0.7);
            font-size: 14px;
            font-family: 'Courier New', monospace;
            padding: 12px;
            background: rgba(2,6,23,0.5);
            border-radius: 12px;
            border: 1px solid rgba(6,182,212,0.2);
        }

        .question-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(2,6,23,0.95);
            backdrop-filter: blur(10px);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-y: auto;
        }

        .question-modal.active { display: flex; }

        .question-card {
            background: linear-gradient(135deg, rgba(30,41,59,0.98), rgba(51,65,85,0.98));
            backdrop-filter: blur(20px);
            border-radius: 24px;
            border: 2px solid rgba(6,182,212,0.5);
            padding: 32px;
            max-width: 700px;
            width: 100%;
            box-shadow: 0 0 60px rgba(6,182,212,0.3);
            position: relative;
            max-height: 90vh;
            overflow-y: auto;
        }

        .question-header {
            display: flex;
            align-items: center;
            gap: 16px;
            margin-bottom: 20px;
        }

        .sector-badge {
            width: 64px;
            height: 64px;
            background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 28px;
            font-weight: 900;
            box-shadow: 0 0 30px rgba(251,191,36,0.6);
            flex-shrink: 0;
        }

        .sector-info h2 {
            color: #06b6d4;
            font-size: 24px;
            font-weight: 900;
            margin-bottom: 4px;
        }

        .sector-info p {
            color: rgba(103,232,249,0.7);
            font-size: 14px;
            font-family: 'Courier New', monospace;
        }

        .progress-indicators {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }

        .progress-bar {
            height: 8px;
            flex: 1;
            border-radius: 4px;
            background: rgba(51,65,85,0.5);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .progress-bar.active {
            background: #06b6d4;
            box-shadow: 0 0 15px rgba(6,182,212,0.6);
        }

        .progress-bar.completed {
            background: rgba(16,185,129,0.8);
        }

        .question-content {
            margin: 20px 0;
            padding: 24px;
            background: rgba(2,6,23,0.6);
            border-radius: 16px;
            border: 1px solid rgba(6,182,212,0.3);
        }

        .question-type {
            color: #fbbf24;
            font-weight: 900;
            text-transform: uppercase;
            font-size: 14px;
            letter-spacing: 1px;
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .question-text {
            color: #e2e8f0;
            font-size: 18px;
            line-height: 1.6;
            margin-bottom: 16px;
        }

        /* Multiple Choice Styles */
        .mc-container {
            display: none;
            flex-direction: column;
            gap: 12px;
            margin-top: 15px;
        }

        .mc-option {
            background: rgba(15, 23, 42, 0.8);
            border: 2px solid #06b6d4;
            color: white;
            padding: 14px;
            border-radius: 10px;
            cursor: pointer;
            transition: 0.2s;
            font-size: 15px;
            text-align: left;
        }

        .mc-option:hover {
            background: rgba(6, 182, 212, 0.2);
        }

        .mc-option.correct {
            border-color: #10b981;
            background: rgba(16, 185, 129, 0.2);
        }

        .mc-option.wrong {
            border-color: #ef4444;
            background: rgba(239, 68, 68, 0.2);
        }

        #robotMazeContainer {
            margin: 20px 0;
            display: flex;
            justify-content: center;
        }

        #robotMaze {
            display: grid;
            grid-template-columns: repeat(5, 60px);
            grid-template-rows: repeat(5, 60px);
            gap: 3px;
            background: #020617;
            padding: 12px;
            border-radius: 12px;
            border: 2px solid rgba(6,182,212,0.5);
            box-shadow: 0 0 30px rgba(6,182,212,0.3);
        }

        #codeInput, #pseudoInput {
            width: 100%;
            min-height: 120px;
            border-radius: 12px;
            padding: 16px;
            font-family: 'Courier New', monospace;
            background: #0f172a;
            color: #e2e8f0;
            border: 2px solid rgba(6,182,212,0.4);
            font-size: 14px;
            resize: vertical;
        }

        .run-btn {
            margin-top: 12px;
            padding: 12px 24px;
            border-radius: 12px;
            cursor: pointer;
            background: linear-gradient(135deg, #06b6d4 0%, #3b82f6 100%);
            color: white;
            border: none;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            box-shadow: 0 0 20px rgba(6,182,212,0.4);
            transition: all 0.2s ease;
            width: 100%;
        }

        .run-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 30px rgba(6,182,212,0.6);
        }

        #codeOutput, #pseudoOutput {
            margin-top: 12px;
            background: #020617;
            padding: 16px;
            border-radius: 12px;
            color: #10b981;
            font-family: 'Courier New', monospace;
            border: 2px solid rgba(6,182,212,0.3);
            min-height: 50px;
        }

        .question-nav {
            display: flex;
            gap: 12px;
            margin-top: 20px;
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
            font-size: 14px;
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
            .title { font-size: 24px; }
            .question-card { padding: 24px; }
            #robotMaze {
                grid-template-columns: repeat(5, 50px);
                grid-template-rows: repeat(5, 50px);
            }
        }
    </style>
</head>
<body>
    <div class="stars" id="stars"></div>

    <div class="title-section">
        <div class="title-header">
            <div class="title-icon">🚀</div>
            <div class="title">Station Navigation</div>
        </div>
        <div class="subtitle">Cadet Training Protocol // Sector Clearance Required</div>
        <div class="badge-container">
            <div class="badge easy" id="badge-1">🥉 Easy</div>
            <div class="badge medium" id="badge-2">🥈 Medium</div>
            <div class="badge hard" id="badge-3">🥇 Hard</div>
        </div>
    </div>

    <div class="container">
        <div class="maze-container">
            <div class="maze" id="maze"></div>
            <div class="controls-hint">
                ⌨️ Use arrow keys to navigate • 🎯 Reach sector checkpoints to begin training
            </div>
        </div>
    </div>

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

            <div class="progress-indicators">
                <div class="progress-bar"></div>
                <div class="progress-bar"></div>
                <div class="progress-bar"></div>
            </div>

            <div class="question-content">
                <div class="question-type">
                    <span id="questionIcon">🤖</span>
                    <span id="questionTypeText">Robot Code</span>
                </div>

                <div class="question-text" id="questionText">Program the robot to reach the flowers</div>

                <!-- Multiple Choice Container -->
                <div id="mcContainer" class="mc-container"></div>

                <!-- Robot Code Runner -->
                <div id="codeRunner" style="display:none;">
                    <div id="robotMazeContainer">
                        <div id="robotMaze"></div>
                    </div>

                    <textarea id="codeInput" placeholder="// Write your code here...
// Example: robot.moveRight(2); robot.moveDown(2);"></textarea>

                    <button id="runCodeBtn" class="run-btn">▶ Run Code</button>

                    <pre id="codeOutput"></pre>
                </div>

                <!-- Pseudocode Runner -->
                <div id="pseudoRunner" style="display:none;">
                    <textarea id="pseudoInput" placeholder="// Write your pseudocode here..."></textarea>

                    <button id="runPseudoBtn" class="run-btn">▶ Run Pseudocode</button>

                    <pre id="pseudoOutput"></pre>
                </div>
            </div>

            <div class="question-nav">
                <button class="btn btn-next" id="nextBtn" style="display:none;">Next Module →</button>
                <button class="btn btn-complete" id="backBtn" style="display: none;">Complete Sector ✓</button>
            </div>
        </div>
    </div>

    <script>
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
        const questionIcon = document.getElementById('questionIcon');
        const nextBtn = document.getElementById('nextBtn');
        const backBtn = document.getElementById('backBtn');
        const codeRunner = document.getElementById('codeRunner');
        const pseudoRunner = document.getElementById('pseudoRunner');
        const mcContainer = document.getElementById('mcContainer');
        const codeInput = document.getElementById('codeInput');
        const codeOutput = document.getElementById('codeOutput');
        const runCodeBtn = document.getElementById('runCodeBtn');
        const pseudoInput = document.getElementById('pseudoInput');
        const pseudoOutput = document.getElementById('pseudoOutput');
        const runPseudoBtn = document.getElementById('runPseudoBtn');

        let currentQuestion = 0;
        let currentSectorNum = 0;
        const completedSectors = new Set();

        const sectorNames = [
            "Navigation Deck",
            "Logic Core",
            "Simulation Bay"
        ];

        // Questions from the document
        const sectorQuestions = {
            1: [
                { 
                    type: "Robot Code", 
                    text: "Objective: Move robot to (3,3).", 
                    mode: "robot",
                    robotStart: { x: 1, y: 1 },
                    flowerPos: { x: 3, y: 3 }
                },
                { 
                    type: "Pseudocode", 
                    text: "Logic: Print numbers 1-5.", 
                    mode: "pseudo"
                },
                { 
                    type: "Computational Thinking", 
                    text: "Logic: A robot repeats [Forward, Right 90] four times. What shape is made?", 
                    mode: "mc", 
                    options: ["Triangle", "Square", "Circle"], 
                    correct: 1 
                }
            ],
            2: [
                { 
                    type: "Robot Code", 
                    text: "Avoid the asteroid at (2,2).", 
                    mode: "robot",
                    robotStart: { x: 1, y: 1 },
                    flowerPos: { x: 3, y: 3 },
                    obstacles: [[2, 2]]
                },
                { 
                    type: "Pseudocode", 
                    text: "Write the logic to find the largest number in a list.", 
                    mode: "pseudo"
                },
                { 
                    type: "Computational Thinking", 
                    text: "Boolean Check: What is the result of 'NOT (True AND False)'?", 
                    mode: "mc", 
                    options: ["True", "False", "Error"], 
                    correct: 0 
                }
            ],
            3: [
                { 
                    type: "Robot Code", 
                    text: "Navigate the Simulation Bay.", 
                    mode: "robot",
                    robotStart: { x: 1, y: 1 },
                    flowerPos: { x: 3, y: 3 },
                    obstacles: [[2, 1], [1, 2]]
                },
                { 
                    type: "Pseudocode", 
                    text: "Logic for a Binary Search.", 
                    mode: "pseudo"
                },
                { 
                    type: "Computational Thinking", 
                    text: "A loop runs 4 times, adding the loop index (1+2+3+4). Total?", 
                    mode: "mc", 
                    options: ["4", "10", "15"], 
                    correct: 1 
                }
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

        let robotMazeLayout = [];
        let robotPos = { x: 1, y: 1 };
        let flowerPos = { x: 3, y: 3 };

        function createRobotMaze() {
            const robotMaze = document.getElementById('robotMaze');
            robotMaze.innerHTML = '';
            
            for (let y = 0; y < 5; y++) {
                for (let x = 0; x < 5; x++) {
                    const cell = document.createElement('div');
                    cell.style.cssText = 'width:60px; height:60px; display:flex; align-items:center; justify-content:center; font-size:28px; border-radius:6px;';
                    
                    if (robotMazeLayout[y][x] === 0) {
                        cell.style.background = 'rgba(51,65,85,0.6)';
                        cell.style.border = '2px solid rgba(30,41,59,0.8)';
                    } else {
                        cell.style.background = 'rgba(30,41,59,0.9)';
                        cell.style.border = '2px solid rgba(6,182,212,0.2)';
                    }
                    
                    if (x === robotPos.x && y === robotPos.y) {
                        cell.textContent = '🤖';
                    }
                    
                    if (x === flowerPos.x && y === flowerPos.y) {
                        cell.textContent = '🌸';
                    }
                    
                    robotMaze.appendChild(cell);
                }
            }
        }

        function setupRobotMaze(question) {
            robotMazeLayout = [
                [1, 1, 1, 1, 1],
                [1, 0, 0, 0, 1],
                [1, 0, 0, 0, 1],
                [1, 0, 0, 0, 1],
                [1, 1, 1, 1, 1]
            ];

            if (question.obstacles) {
                question.obstacles.forEach(([x, y]) => {
                    robotMazeLayout[y][x] = 0;
                });
            }

            robotPos = { ...question.robotStart };
            flowerPos = { ...question.flowerPos };
        }

        const robot = {
            moveRight: function(steps = 1) {
                for (let i = 0; i < steps; i++) {
                    if (robotPos.x < 4 && robotMazeLayout[robotPos.y][robotPos.x + 1] !== 0) {
                        robotPos.x++;
                        createRobotMaze();
                    } else {
                        return false;
                    }
                }
                return true;
            },
            moveLeft: function(steps = 1) {
                for (let i = 0; i < steps; i++) {
                    if (robotPos.x > 0 && robotMazeLayout[robotPos.y][robotPos.x - 1] !== 0) {
                        robotPos.x--;
                        createRobotMaze();
                    } else {
                        return false;
                    }
                }
                return true;
            },
            moveDown: function(steps = 1) {
                for (let i = 0; i < steps; i++) {
                    if (robotPos.y < 4 && robotMazeLayout[robotPos.y + 1][robotPos.x] !== 0) {
                        robotPos.y++;
                        createRobotMaze();
                    } else {
                        return false;
                    }
                }
                return true;
            },
            moveUp: function(steps = 1) {
                for (let i = 0; i < steps; i++) {
                    if (robotPos.y > 0 && robotMazeLayout[robotPos.y - 1][robotPos.x] !== 0) {
                        robotPos.y--;
                        createRobotMaze();
                    } else {
                        return false;
                    }
                }
                return true;
            }
        };

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

        function movePlayer(dx, dy) {
            const newX = playerPos.x + dx;
            const newY = playerPos.y + dy;

            if (newY >= 0 && newY < mazeLayout.length && 
                newX >= 0 && newX < mazeLayout[0].length && 
                mazeLayout[newY][newX] !== 0) {

                const cellValue = mazeLayout[newY][newX];

                if (cellValue >= 4 && cellValue <= 8) {
                    const sectorNum = cellValue - 3;
                    if (!completedSectors.has(sectorNum - 1) && sectorNum > 1) {
                        alert("Complete the previous sector first!");
                        return;
                    }
                }

                playerPos.x = newX;
                playerPos.y = newY;

                if (cellValue >= 4 && cellValue <= 8) {
                    const sectorNum = cellValue - 3;
                    if (!completedSectors.has(sectorNum)) {
                        currentSectorNum = sectorNum;
                        currentQuestion = 0;
                        modal.classList.add('active');
                        showQuestion();
                    }
                }

                createMaze();
            }
        }

        function showQuestion() {
            sectorNumber.textContent = currentSectorNum;
            sectorTitle.textContent = `SECTOR ${currentSectorNum}`;
            sectorName.textContent = sectorNames[currentSectorNum - 1] || "Training Module";
            
            const currentQ = sectorQuestions[currentSectorNum][currentQuestion];
            questionType.textContent = currentQ.type;
            questionText.textContent = currentQ.text;
            questionIcon.textContent = currentQ.mode === 'robot' ? '🤖' : currentQ.mode === 'pseudo' ? '📝' : '🧠';
            
            // Hide all runners
            codeRunner.style.display = 'none';
            pseudoRunner.style.display = 'none';
            mcContainer.style.display = 'none';
            nextBtn.style.display = 'none';
            backBtn.style.display = 'none';
            
            // Clear outputs
            codeOutput.textContent = '';
            pseudoOutput.textContent = '';
            
            // Show appropriate runner
            if (currentQ.mode === 'robot') {
                codeRunner.style.display = 'block';
                questionText.style.display = 'block';
                setupRobotMaze(currentQ);
                createRobotMaze();
                nextBtn.style.display = 'block';
            } else if (currentQ.mode === 'pseudo') {
                pseudoRunner.style.display = 'block';
                questionText.style.display = 'block';
                nextBtn.style.display = 'block';
            } else if (currentQ.mode === 'mc') {
                questionText.style.display = 'block';
                mcContainer.style.display = 'flex';
                mcContainer.innerHTML = '';
                
                currentQ.options.forEach((opt, idx) => {
                    const button = document.createElement('div');
                    button.className = 'mc-option';
                    button.textContent = opt;
                    button.onclick = () => {
                        if (idx === currentQ.correct) {
                            button.classList.add('correct');
                            if (currentQuestion === 2) {
                                backBtn.style.display = 'block';
                            } else {
                                nextBtn.style.display = 'block';
                            }
                        } else {
                            button.classList.add('wrong');
                        }
                    };
                    mcContainer.appendChild(button);
                });
            }
            
            const progressBars = document.querySelectorAll('.progress-bar');
            progressBars.forEach((bar, idx) => {
                bar.classList.remove('active', 'completed');
                if (idx === currentQuestion) {
                    bar.classList.add('active');
                } else if (idx < currentQuestion) {
                    bar.classList.add('completed');
                }
            });
        }

        nextBtn.addEventListener('click', () => {
            currentQuestion++;
            showQuestion();
        });

        backBtn.addEventListener('click', () => {
            modal.classList.remove('active');
            completedSectors.add(currentSectorNum);
            
            const badgeId = `badge-${currentSectorNum}`;
            const badge = document.getElementById(badgeId);
            if (badge) {
                badge.classList.add('earned');
            }
            
            createMaze();
        });

        document.addEventListener('keydown', (e) => {
            if (modal.classList.contains('active')) return;
            
            if(e.key === 'ArrowUp') movePlayer(0, -1);
            if(e.key === 'ArrowDown') movePlayer(0, 1);
            if(e.key === 'ArrowLeft') movePlayer(-1, 0);
            if(e.key === 'ArrowRight') movePlayer(1, 0);
        });

        runCodeBtn.addEventListener('click', () => {
            const currentQ = sectorQuestions[currentSectorNum][currentQuestion];
            setupRobotMaze(currentQ);
            createRobotMaze();
            codeOutput.textContent = '';
            
            try {
                const userCode = codeInput.value;
                const executeCode = new Function('robot', userCode);
                executeCode(robot);
                
                if (robotPos.x === flowerPos.x && robotPos.y === flowerPos.y) {
                    codeOutput.textContent = "✓ Success! Robot reached the flowers!";
                    codeOutput.style.color = '#10b981';
                } else {
                    codeOutput.textContent = "❌ Robot didn't reach the flowers. Try again!";
                    codeOutput.style.color = '#fbbf24';
                }
            } catch (err) {
                codeOutput.textContent = "❌ Error: " + err.message;
                codeOutput.style.color = '#ef4444';
            }
        });

        runPseudoBtn.addEventListener('click', () => {
            const pseudo = pseudoInput.value;
            pseudoOutput.textContent = "Pseudocode submitted:\n" + pseudo;
            pseudoOutput.style.color = '#10b981';
        });

        createMaze();
    </script>
</body>
</html>