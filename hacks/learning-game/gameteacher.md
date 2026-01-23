<style>
    /* MISSION OVERLAY - START OF STOP */
    #teacher-overlay {
        display: none; 
        position: fixed; 
        top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(2, 6, 23, 0.98);
        z-index: 9998; /* High, but under the robot icon */
        justify-content: center; align-items: center;
        backdrop-filter: blur(15px);
    }

    .teacher-card {
        background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
        border: 2px solid #06b6d4;
        border-radius: 24px; padding: 40px; max-width: 550px; 
        text-align: center; box-shadow: 0 0 50px rgba(6, 182, 212, 0.3);
    }

    /* THE PERSISTENT ROBOT - Fixed to stay on top of EVERYTHING */
    #help-bot-icon {
        position: fixed; 
        bottom: 30px; right: 30px;
        width: 70px; height: 70px;
        background: #06b6d4;
        border-radius: 50%;
        display: flex; justify-content: center; align-items: center;
        cursor: pointer;
        z-index: 10001; /* Highest layer in the whole game */
        box-shadow: 0 0 30px rgba(6, 182, 212, 0.6);
        font-size: 35px;
        transition: transform 0.3s;
    }

    #help-bot-icon:hover { transform: scale(1.1); }

    #help-bubble {
        display: none;
        position: fixed;
        bottom: 110px; right: 30px;
        background: white; color: #020617;
        padding: 15px; border-radius: 12px;
        max-width: 250px;
        z-index: 10001; 
        border: 3px solid #06b6d4;
        box-shadow: 0 5px 15px rgba(0,0,0,0.5);
        font-family: sans-serif;
    }
</style>

<!-- Teacher Overlay UI -->
<div id="teacher-overlay">
    <div class="teacher-card">
        <div style="font-size: 50px; margin-bottom: 10px;">🤖</div>
        <h2 id="teacher-title" style="color: #06b6d4; margin-bottom: 15px; font-family: sans-serif;"></h2>
        <p id="teacher-msg" style="color: #e2e8f0; line-height: 1.6; margin-bottom: 25px; font-family: sans-serif;"></p>
        <button onclick="dismissTeacher()" style="padding: 12px 30px; background: #06b6d4; color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: bold;">BEGIN TRAINING</button>
    </div>
</div>

<!-- Persistent Global Robot Icon -->
<div id="help-bubble"></div>
<div id="help-bot-icon" onclick="toggleHint()">🤖</div>

<script>
    const teacherData = {
        1: { title: "Stop 1: Navigation", msg: "Welcome Cadet! Use Robot Code to reach the flowers. Q3 is a logic check on patterns.", hints: ["robot.moveRight() or robot.moveDown()", "Count the steps carefully!", "Q3: 4 sides and 4 turns = Square."] },
        2: { title: "Stop 2: Logic Core", msg: "Master the IF/ELSE logic. Your robot needs a brain to make decisions.", hints: ["Sequence is everything!", "Check your logic flow.", "Q3: If the IF is false, do the ELSE!"] },
        3: { title: "Stop 3: Simulation", msg: "Loops make your code shorter. Work smarter, not harder.", hints: ["Pattern = Loop!", "Check the sum.", "Q3: 1+2+3+4 = 10."] },
        4: { title: "Stop 4: Alpha Deck", msg: "Abstraction hides complexity. Use functions to organize your code.", hints: ["Avoid the walls!", "Functions are reusable.", "Q3: Functions make code cleaner."] },
        5: { title: "Stop 5: Beta Deck", msg: "Hacker Level: Efficiency. Compare Binary vs Linear search speed.", hints: ["Expert navigation required!", "Merge sort logic!", "Q3: Binary Search is way faster."] }
    };

    let currentStopId = 1;

    function initTeacher(stopId, qIdx = 0) {
        currentStopId = stopId;
        const data = teacherData[stopId];
        document.getElementById('teacher-title').innerText = data.title;
        document.getElementById('teacher-msg').innerText = data.msg;
        document.getElementById('help-bubble').innerText = data.hints[qIdx];
        document.getElementById('teacher-overlay').style.display = 'flex';
    }

    function updateHint(qIdx) {
        const data = teacherData[currentStopId];
        if (data) document.getElementById('help-bubble').innerText = data.hints[qIdx];
    }

    function dismissTeacher() {
        document.getElementById('teacher-overlay').style.display = 'none';
        document.getElementById('questionModal').classList.add('active');
        // This function lives in homescreen.md
        if (typeof showQuestion === "function") showQuestion(); 
    }

    function toggleHint() {
        const bubble = document.getElementById('help-bubble');
        bubble.style.display = (bubble.style.display === 'none' || bubble.style.display === '') ? 'block' : 'none';
    }
</script>