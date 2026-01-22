---
layout: base
title: Pseudocode Question
authors: Rebecca, Meryl
permalink: /learninggame/pseudocodequestion
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Runner: Average of a List</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        :root {
            --bg-primary: #0d1117;
            --bg-secondary: #161b22;
            --bg-tertiary: #21262d;
            --border-color: #30363d;
            --text-primary: #f0f6fc;
            --text-secondary: #8b949e;
            --accent-blue: #58a6ff;
            --accent-blue-dark: #1f6feb;
            --success-green: #238636;
            --error-red: #f85149;
            --warning-yellow: #d29922;
            --code-bg: #010409;
            --button-hover: #1a5fb4;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background-color: var(--bg-primary);
            color: var(--text-primary);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 24px;
        }

        @media (max-width: 900px) {
            .container {
                grid-template-columns: 1fr;
            }
        }

        .card {
            background-color: var(--bg-secondary);
            border-radius: 8px;
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        .card-header {
            background-color: var(--bg-tertiary);
            padding: 16px 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .card-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--accent-blue);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .card-title i {
            font-size: 16px;
        }

        .card-body {
            padding: 20px;
        }

        h1 {
            font-size: 28px;
            margin-bottom: 8px;
            color: var(--accent-blue);
        }

        .subtitle {
            color: var(--text-secondary);
            font-size: 16px;
            margin-bottom: 24px;
        }

        h2 {
            font-size: 20px;
            margin: 20px 0 12px 0;
            color: var(--text-primary);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 6px;
        }

        h3 {
            font-size: 16px;
            margin: 16px 0 8px 0;
            color: var(--accent-blue);
        }

        ul, ol {
            padding-left: 20px;
            margin-bottom: 16px;
        }

        li {
            margin-bottom: 8px;
        }

        .function-signature {
            background-color: var(--code-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 16px;
            font-family: 'Courier New', monospace;
            font-size: 15px;
            margin: 16px 0;
            color: var(--accent-blue);
        }

        .keyword {
            color: #ff7b72;
        }

        .function-name {
            color: #d2a8ff;
        }

        .parameter {
            color: #79c0ff;
        }

        .comment {
            color: var(--text-secondary);
        }

        .code-editor-container {
            height: 300px;
            position: relative;
            border-radius: 6px;
            overflow: hidden;
            border: 1px solid var(--border-color);
            background-color: var(--code-bg);
        }

        #codeEditor {
            width: 100%;
            height: 100%;
            background-color: var(--code-bg);
            color: var(--text-primary);
            font-family: 'Courier New', monospace;
            font-size: 15px;
            line-height: 1.5;
            padding: 16px;
            border: none;
            resize: none;
            outline: none;
            tab-size: 4;
        }

        .editor-footer {
            background-color: var(--bg-tertiary);
            padding: 12px 16px;
            border-top: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: var(--text-secondary);
            font-size: 14px;
        }

        .button-group {
            display: flex;
            gap: 12px;
            margin-top: 20px;
        }

        .btn {
            padding: 10px 20px;
            border-radius: 6px;
            border: none;
            font-weight: 600;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary {
            background-color: var(--accent-blue-dark);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--button-hover);
            transform: translateY(-1px);
        }

        .btn-secondary {
            background-color: var(--bg-tertiary);
            color: var(--text-primary);
            border: 1px solid var(--border-color);
        }

        .btn-secondary:hover {
            background-color: var(--border-color);
        }

        .test-cases {
            margin-top: 24px;
        }

        .test-case {
            background-color: var(--bg-tertiary);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 16px;
            margin-bottom: 16px;
        }

        .test-case:last-child {
            margin-bottom: 0;
        }

        .test-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .test-title {
            font-weight: 600;
            color: var(--text-primary);
        }

        .test-status {
            font-size: 12px;
            padding: 4px 10px;
            border-radius: 12px;
            background-color: var(--bg-secondary);
            color: var(--text-secondary);
        }

        .test-status.passed {
            background-color: rgba(35, 134, 54, 0.2);
            color: #3fb950;
        }

        .test-status.failed {
            background-color: rgba(248, 81, 73, 0.2);
            color: var(--error-red);
        }

        .test-input, .test-expected, .test-actual {
            margin-bottom: 8px;
            font-size: 14px;
        }

        .test-input span, .test-expected span, .test-actual span {
            color: var(--text-secondary);
        }

        .test-result {
            margin-top: 20px;
            padding: 16px;
            border-radius: 6px;
            display: none;
        }

        .test-result.success {
            background-color: rgba(35, 134, 54, 0.1);
            border: 1px solid rgba(35, 134, 54, 0.3);
            color: #3fb950;
            display: block;
        }

        .test-result.error {
            background-color: rgba(248, 81, 73, 0.1);
            border: 1px solid rgba(248, 81, 73, 0.3);
            color: var(--error-red);
            display: block;
        }

        .hint-box {
            background-color: rgba(88, 166, 255, 0.1);
            border: 1px solid rgba(88, 166, 255, 0.3);
            border-radius: 6px;
            padding: 16px;
            margin-top: 20px;
        }

        .hint-title {
            color: var(--accent-blue);
            font-weight: 600;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .code-inline {
            font-family: 'Courier New', monospace;
            background-color: var(--code-bg);
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 14px;
            color: var(--accent-blue);
        }

        .console-output {
            background-color: var(--code-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 16px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            min-height: 80px;
            margin-top: 20px;
            white-space: pre-wrap;
        }

        .console-header {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 12px;
            color: var(--text-secondary);
            font-size: 14px;
        }

        .console-header i {
            color: var(--accent-blue);
        }
    </style>
</head>
<body>
    <div class="container">
        <div>
            <div class="card">
                <div class="card-header">
                    <div class="card-title">
                        <i class="fas fa-tasks"></i>
                        Task Description
                    </div>
                </div>
                <div class="card-body">
                    <h1>Q1: Average of a List</h1>
                    <p class="subtitle">Write a function that returns the average of numbers in a list</p>
                    
                    <h2>Task</h2>
                    <p>Write a function <span class="code-inline">Average(nums)</span> that returns the average (mean) of the numbers in list <span class="code-inline">nums</span>.</p>
                    
                    <h2>Input</h2>
                    <ul>
                        <li><span class="code-inline">nums</span> - a list of numbers (length ≥ 1)</li>
                    </ul>
                    
                    <h2>Output</h2>
                    <ul>
                        <li>A number representing the average</li>
                    </ul>
                    
                    <h2>Requirements</h2>
                    <ol>
                        <li>Use a loop to add all elements</li>
                        <li>Return <span class="code-inline">sum / nums.length</span></li>
                        <li>Do not use built-in functions like <span class="code-inline">reduce()</span> or <span class="code-inline">Math.sum()</span></li>
                    </ol>
                    
                    <div class="function-signature">
                        <span class="keyword">function</span> <span class="function-name">Average</span>(<span class="parameter">nums</span>) {<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;<span class="comment">// Your code here</span><br>
                        }
                    </div>
                    
                    <div class="hint-box">
                        <div class="hint-title">
                            <i class="fas fa-lightbulb"></i>
                            Hint
                        </div>
                        <p>You can solve this by:</p>
                        <ol>
                            <li>Initialize a variable to store the sum (start at 0)</li>
                            <li>Use a for loop to iterate through all elements in the array</li>
                            <li>Add each element to the sum variable</li>
                            <li>Divide the sum by the length of the array</li>
                            <li>Return the result</li>
                        </ol>
                    </div>
                </div>
            </div>
        </div>
        
        <div>
            <div class="card">
                <div class="card-header">
                    <div class="card-title">
                        <i class="fas fa-code"></i>
                        Code Editor
                    </div>
                    <div style="font-size: 14px; color: var(--text-secondary);">
                        JavaScript
                    </div>
                </div>
                <div class="card-body">
                    <div class="code-editor-container">
                        <textarea id="codeEditor" spellcheck="false">function Average(nums) {
    // Write your solution here
    // Use a loop to calculate the sum of all numbers
    let sum = 0;
    
    // Iterate through each element in the array
    for (let i = 0; i < nums.length; i++) {
        sum = sum + nums[i];
    }
    
    // Calculate and return the average
    return sum / nums.length;
}

// Test cases (will be executed when you click "Run Code")
console.log("Test 1: Average of [1, 2, 3, 4, 5] = " + Average([1, 2, 3, 4, 5]));
console.log("Test 2: Average of [10, 20, 30, 40] = " + Average([10, 20, 30, 40]));
console.log("Test 3: Average of [5] = " + Average([5]));
console.log("Test 4: Average of [2.5, 3.5, 4.5] = " + Average([2.5, 3.5, 4.5]));</textarea>
                    </div>
                    
                    <div class="editor-footer">
                        <div>Line 1, Column 1</div>
                        <div>UTF-8</div>
                    </div>
                    
                    <div class="button-group">
                        <button id="runBtn" class="btn btn-primary">
                            <i class="fas fa-play"></i>
                            Run Code
                        </button>
                        <button id="resetBtn" class="btn btn-secondary">
                            <i class="fas fa-redo"></i>
                            Reset Code
                        </button>
                    </div>
                    
                    <div class="console-output">
                        <div class="console-header">
                            <i class="fas fa-terminal"></i>
                            Console Output
                        </div>
                        <div id="consoleOutput">Click "Run Code" to see the output here...</div>
                    </div>
                </div>
            </div>
            
            <div class="card test-cases">
                <div class="card-header">
                    <div class="card-title">
                        <i class="fas fa-vial"></i>
                        Test Cases
                    </div>
                    <div id="testSummary">0/4 tests passed</div>
                </div>
                <div class="card-body">
                    <div id="testResult" class="test-result"></div>
                    
                    <div class="test-case">
                        <div class="test-header">
                            <div class="test-title">Test Case 1: Simple integers</div>
                            <div id="testStatus1" class="test-status">Not run</div>
                        </div>
                        <div class="test-input">
                            <span>Input:</span> nums = [1, 2, 3, 4, 5]
                        </div>
                        <div class="test-expected">
                            <span>Expected Output:</span> 3
                        </div>
                        <div class="test-actual">
                            <span>Actual Output:</span> <span id="testOutput1">Not executed</span>
                        </div>
                    </div>
                    
                    <div class="test-case">
                        <div class="test-header">
                            <div class="test-title">Test Case 2: Multiples of 10</div>
                            <div id="testStatus2" class="test-status">Not run</div>
                        </div>
                        <div class="test-input">
                            <span>Input:</span> nums = [10, 20, 30, 40]
                        </div>
                        <div class="test-expected">
                            <span>Expected Output:</span> 25
                        </div>
                        <div class="test-actual">
                            <span>Actual Output:</span> <span id="testOutput2">Not executed</span>
                        </div>
                    </div>
                    
                    <div class="test-case">
                        <div class="test-header">
                            <div class="test-title">Test Case 3: Single element</div>
                            <div id="testStatus3" class="test-status">Not run</div>
                        </div>
                        <div class="test-input">
                            <span>Input:</span> nums = [5]
                        </div>
                        <div class="test-expected">
                            <span>Expected Output:</span> 5
                        </div>
                        <div class="test-actual">
                            <span>Actual Output:</span> <span id="testOutput3">Not executed</span>
                        </div>
                    </div>
                    
                    <div class="test-case">
                        <div class="test-header">
                            <div class="test-title">Test Case 4: Decimal numbers</div>
                            <div id="testStatus4" class="test-status">Not run</div>
                        </div>
                        <div class="test-input">
                            <span>Input:</span> nums = [2.5, 3.5, 4.5]
                        </div>
                        <div class="test-expected">
                            <span>Expected Output:</span> 3.5
                        </div>
                        <div class="test-actual">
                            <span>Actual Output:</span> <span id="testOutput4">Not executed</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const codeEditor = document.getElementById('codeEditor');
            const runBtn = document.getElementById('runBtn');
            const resetBtn = document.getElementById('resetBtn');
            const consoleOutput = document.getElementById('consoleOutput');
            const testSummary = document.getElementById('testSummary');
            const testResult = document.getElementById('testResult');
            
            // Test case elements
            const testStatusElements = [
                document.getElementById('testStatus1'),
                document.getElementById('testStatus2'),
                document.getElementById('testStatus3'),
                document.getElementById('testStatus4')
            ];
            
            const testOutputElements = [
                document.getElementById('testOutput1'),
                document.getElementById('testOutput2'),
                document.getElementById('testOutput3'),
                document.getElementById('testOutput4')
            ];
            
            // Default code
            const defaultCode = `function Average(nums) {
    // Write your solution here
    // Use a loop to calculate the sum of all numbers
    let sum = 0;
    
    // Iterate through each element in the array
    for (let i = 0; i < nums.length; i++) {
        sum = sum + nums[i];
    }
    
    // Calculate and return the average
    return sum / nums.length;
}

// Test cases (will be executed when you click "Run Code")
console.log("Test 1: Average of [1, 2, 3, 4, 5] = " + Average([1, 2, 3, 4, 5]));
console.log("Test 2: Average of [10, 20, 30, 40] = " + Average([10, 20, 30, 40]));
console.log("Test 3: Average of [5] = " + Average([5]));
console.log("Test 4: Average of [2.5, 3.5, 4.5] = " + Average([2.5, 3.5, 4.5]));`;
            
            // Initialize code editor
            codeEditor.value = defaultCode;
            
            // Track cursor position for editor footer
            codeEditor.addEventListener('input', updateCursorPosition);
            codeEditor.addEventListener('click', updateCursorPosition);
            codeEditor.addEventListener('keyup', updateCursorPosition);
            
            function updateCursorPosition() {
                const position = getCursorPosition(codeEditor);
                const editorFooter = document.querySelector('.editor-footer div:first-child');
                editorFooter.textContent = `Line ${position.line}, Column ${position.column}`;
            }
            
            function getCursorPosition(textarea) {
                const startPos = textarea.selectionStart;
                const textLines = textarea.value.substr(0, startPos).split("\n");
                const line = textLines.length;
                const column = textLines[textLines.length - 1].length + 1;
                return { line, column };
            }
            
            // Run button event listener
            runBtn.addEventListener('click', function() {
                // Clear previous results
                consoleOutput.textContent = '';
                testResult.className = 'test-result';
                testResult.textContent = '';
                
                // Reset test statuses
                testStatusElements.forEach(el => {
                    el.textContent = 'Running...';
                    el.className = 'test-status';
                });
                
                testOutputElements.forEach(el => {
                    el.textContent = 'Calculating...';
                });
                
                // Capture console.log output
                const originalConsoleLog = console.log;
                let consoleOutputText = '';
                
                console.log = function(...args) {
                    consoleOutputText += args.map(arg => 
                        typeof arg === 'object' ? JSON.stringify(arg) : String(arg)
                    ).join(' ') + '\n';
                    originalConsoleLog.apply(console, args);
                };
                
                try {
                    // Get user code
                    const userCode = codeEditor.value;
                    
                    // Evaluate the code
                    eval(userCode);
                    
                    // Check if Average function exists
                    if (typeof Average !== 'function') {
                        throw new Error('Average function is not defined. Please make sure your solution includes a function named Average.');
                    }
                    
                    // Define test cases
                    const testCases = [
                        { 
                            input: [1, 2, 3, 4, 5], 
                            expected: 3,
                            description: 'Simple integers'
                        },
                        { 
                            input: [10, 20, 30, 40], 
                            expected: 25,
                            description: 'Multiples of 10'
                        },
                        { 
                            input: [5], 
                            expected: 5,
                            description: 'Single element'
                        },
                        { 
                            input: [2.5, 3.5, 4.5], 
                            expected: 3.5,
                            description: 'Decimal numbers'
                        }
                    ];
                    
                    // Run test cases
                    let passedTests = 0;
                    let failedTests = [];
                    
                    testCases.forEach((testCase, index) => {
                        try {
                            const result = Average(testCase.input);
                            const roundedResult = Math.round(result * 100) / 100;
                            const expected = testCase.expected;
                            
                            // Update test output
                            testOutputElements[index].textContent = roundedResult;
                            
                            // Check if test passed
                            const tolerance = 0.001; // Allow for floating point precision issues
                            const passed = Math.abs(roundedResult - expected) < tolerance;
                            
                            if (passed) {
                                testStatusElements[index].textContent = 'Passed';
                                testStatusElements[index].className = 'test-status passed';
                                passedTests++;
                            } else {
                                testStatusElements[index].textContent = 'Failed';
                                testStatusElements[index].className = 'test-status failed';
                                failedTests.push({
                                    testNumber: index + 1,
                                    expected: expected,
                                    actual: roundedResult
                                });
                            }
                        } catch (error) {
                            testStatusElements[index].textContent = 'Error';
                            testStatusElements[index].className = 'test-status failed';
                            testOutputElements[index].textContent = `Error: ${error.message}`;
                            failedTests.push({
                                testNumber: index + 1,
                                error: error.message
                            });
                        }
                    });
                    
                    // Update test summary
                    testSummary.textContent = `${passedTests}/${testCases.length} tests passed`;
                    
                    // Show overall result
                    if (passedTests === testCases.length) {
                        testResult.className = 'test-result success';
                        testResult.innerHTML = `<i class="fas fa-check-circle"></i> All tests passed! Your solution is correct.`;
                    } else {
                        testResult.className = 'test-result error';
                        let errorMsg = `<i class="fas fa-times-circle"></i> ${failedTests.length} test(s) failed.`;
                        
                        if (failedTests.length > 0) {
                            errorMsg += '<ul style="margin-top: 8px; margin-left: 20px;">';
                            failedTests.forEach(failed => {
                                if (failed.error) {
                                    errorMsg += `<li>Test ${failed.testNumber}: ${failed.error}</li>`;
                                } else {
                                    errorMsg += `<li>Test ${failed.testNumber}: Expected ${failed.expected}, got ${failed.actual}</li>`;
                                }
                            });
                            errorMsg += '</ul>';
                        }
                        
                        testResult.innerHTML = errorMsg;
                    }
                    
                    // Show console output
                    consoleOutput.textContent = consoleOutputText || 'No console output generated.';
                    
                } catch (error) {
                    consoleOutput.textContent = `Error: ${error.message}`;
                    testResult.className = 'test-result error';
                    testResult.innerHTML = `<i class="fas fa-exclamation-triangle"></i> Code execution error: ${error.message}`;
                    
                    // Mark all tests as failed
                    testStatusElements.forEach((el, index) => {
                        el.textContent = 'Error';
                        el.className = 'test-status failed';
                        testOutputElements[index].textContent = 'Code error';
                    });
                    
                    testSummary.textContent = `0/4 tests passed`;
                } finally {
                    // Restore original console.log
                    console.log = originalConsoleLog;
                }
            });
            
            // Reset button event listener
            resetBtn.addEventListener('click', function() {
                codeEditor.value = defaultCode;
                consoleOutput.textContent = 'Click "Run Code" to see the output here...';
                testResult.className = 'test-result';
                testResult.textContent = '';
                testSummary.textContent = '0/4 tests passed';
                
                // Reset test statuses
                testStatusElements.forEach(el => {
                    el.textContent = 'Not run';
                    el.className = 'test-status';
                });
                
                testOutputElements.forEach(el => {
                    el.textContent = 'Not executed';
                });
                
                updateCursorPosition();
            });
            
            // Initialize cursor position display
            updateCursorPosition();
            
            // Add tab support to textarea
            codeEditor.addEventListener('keydown', function(e) {
                if (e.key === 'Tab') {
                    e.preventDefault();
                    const start = this.selectionStart;
                    const end = this.selectionEnd;
                    
                    // Insert 4 spaces
                    this.value = this.value.substring(0, start) + '    ' + this.value.substring(end);
                    
                    // Move cursor
                    this.selectionStart = this.selectionEnd = start + 4;
                    
                    updateCursorPosition();
                }
            });
        });
    </script>
</body>
</html>