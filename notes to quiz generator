<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Mock Quiz Generator</title>

    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <!-- Libraries -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/tesseract.js@2/dist/tesseract.min.js"></script>

    <!-- Icons (Lucide) -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <style>
        :root {
            --primary: #4F46E5;
            --primary-hover: #4338CA;
            --secondary: #10B981;
            --bg: #F3F4F6;
            --card-bg: rgba(255, 255, 255, 0.85);
            --text-main: #111827;
            --text-muted: #6B7280;
            --border: #E5E7EB;
            --danger: #EF4444;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Outfit', sans-serif;
            background: radial-gradient(circle at top left, #e0c3fc 0%, #8ec5fc 100%);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 40px 20px;
            background-attachment: fixed;
        }

        header {
            text-align: center;
            margin-bottom: 40px;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 700;
            color: #1a1a2e;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        h1 i {
            color: var(--primary);
        }

        p.subtitle {
            color: #4b5563;
            font-size: 1.1rem;
            margin-top: 8px;
        }

        .container {
            width: 100%;
            max-width: 800px;
            background: var(--card-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.5);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            transition: all 0.4s ease;
        }

        .step {
            display: none;
            animation: fadeIn 0.4s ease forwards;
        }

        .step.active {
            display: block;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .form-group {
            margin-bottom: 24px;
        }

        label {
            display: block;
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--text-main);
        }

        textarea {
            width: 100%;
            height: 160px;
            padding: 16px;
            border: 2px solid var(--border);
            border-radius: 12px;
            font-family: inherit;
            font-size: 1rem;
            resize: vertical;
            transition: border-color 0.3s;
            background: rgba(255, 255, 255, 0.9);
        }

        textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(79, 70, 229, 0.1);
        }

        .file-drop-area {
            position: relative;
            display: flex;
            align-items: center;
            width: 100%;
            padding: 25px;
            border: 2px dashed #a78bfa;
            border-radius: 12px;
            transition: 0.3s;
            background: rgba(139, 92, 246, 0.05);
            cursor: pointer;
            justify-content: center;
            flex-direction: column;
            gap: 10px;
            text-align: center;
        }

        .file-drop-area:hover,
        .file-drop-area.dragover {
            background: rgba(139, 92, 246, 0.1);
            border-color: var(--primary);
        }

        .file-drop-area i {
            color: var(--primary);
            width: 32px;
            height: 32px;
        }

        .file-drop-area input {
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 100%;
            cursor: pointer;
            opacity: 0;
        }

        .file-name {
            font-size: 0.9rem;
            color: var(--primary);
            font-weight: 500;
            margin-top: 5px;
        }

        .button-group {
            display: flex;
            gap: 16px;
            margin-top: 30px;
        }

        button {
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 14px 24px;
            font-size: 1rem;
            font-weight: 600;
            color: #fff;
            background: var(--primary);
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-family: inherit;
        }

        button:hover {
            background: var(--primary-hover);
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(79, 70, 229, 0.2);
        }

        button.secondary {
            background: #fff;
            color: var(--primary);
            border: 2px solid var(--primary);
        }

        button.secondary:hover {
            background: #f5f3ff;
        }

        button:disabled {
            opacity: 0.7;
            cursor: not-allowed;
            transform: none;
        }

        .loader-container {
            display: none;
            text-align: center;
            padding: 40px 0;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(79, 70, 229, 0.2);
            border-top-color: var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 16px;
        }

        @keyframes spin {
            100% {
                transform: rotate(360deg);
            }
        }

        /* Quiz Styles */
        .quiz-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
            padding-bottom: 16px;
            border-bottom: 2px solid var(--border);
        }

        .question-card {
            background: #fff;
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 24px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.02);
            transition: transform 0.2s;
        }

        .question-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.05);
        }

        .question-card p {
            font-size: 1.1rem;
            font-weight: 500;
            margin-bottom: 16px;
            line-height: 1.5;
        }

        .option-label {
            display: flex;
            align-items: center;
            padding: 12px 16px;
            border: 2px solid var(--border);
            border-radius: 8px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .option-label:hover {
            background: #f9fafb;
            border-color: #d1d5db;
        }

        .option-label input[type="radio"] {
            margin-right: 12px;
            width: 18px;
            height: 18px;
            accent-color: var(--primary);
        }

        .option-label.selected {
            border-color: var(--primary);
            background: rgba(79, 70, 229, 0.05);
        }

        input[type="text"] {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid var(--border);
            border-radius: 8px;
            font-family: inherit;
            font-size: 1rem;
            outline: none;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus {
            border-color: var(--primary);
        }

        .result-view {
            text-align: center;
            padding: 40px 20px;
        }

        .score-circle {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            background: conic-gradient(var(--primary) calc(var(--score) * 1%), #e5e7eb 0);
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 24px;
            position: relative;
        }

        .score-circle::before {
            content: "";
            position: absolute;
            inset: 10px;
            background: var(--card-bg);
            border-radius: 50%;
        }

        .score-text {
            position: relative;
            font-size: 2.5rem;
            font-weight: 700;
            color: var(--primary);
        }

        .summary-list {
            list-style: none;
        }

        .summary-list li {
            background: #fff;
            margin-bottom: 12px;
            padding: 16px 20px;
            border-radius: 8px;
            border-left: 4px solid var(--primary);
            display: flex;
            align-items: flex-start;
            gap: 12px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
        }

        .summary-list li i {
            color: var(--primary);
            flex-shrink: 0;
            margin-top: 2px;
        }

        .nav-btn {
            background: transparent;
            color: var(--text-muted);
            border: none;
            box-shadow: none;
            padding: 8px 16px;
            font-size: 0.95rem;
            width: auto;
            flex: none;
        }

        .nav-btn:hover {
            background: rgba(0, 0, 0, 0.05);
            color: var(--text-main);
            transform: none;
            box-shadow: none;
        }
    </style>
</head>

<body>

    <header>
        <h1><i data-lucide="brain-circuit"></i> MockIt</h1>
        <p class="subtitle">AI-Powered Mock Tests & Summaries</p>
    </header>

    <div class="container">

        <!-- STEP 1: INPUT -->
        <div id="step-input" class="step active">
            <div class="form-group">
                <label for="notes">Paste your notes</label>
                <textarea id="notes" placeholder="Paste your study materials, articles, or notes here..."></textarea>
            </div>

            <div class="form-group">
                <label>Or upload a document</label>
                <div class="file-drop-area" id="drop-area">
                    <i data-lucide="upload-cloud"></i>
                    <span class="file-msg">Choose a file or drag it here</span>
                    <span class="file-desc" style="color:var(--text-muted); font-size:0.85rem;">Supports PDF and Images
                        (JPEG/PNG)</span>
                    <input type="file" id="fileInput" accept=".pdf,image/jpeg,image/png">
                </div>
                <div id="fileName" class="file-name"></div>
            </div>

            <div class="loader-container" id="processingLoader">
                <div class="spinner"></div>
                <p id="loaderText">Extracting text from document...</p>
            </div>

            <div class="button-group" id="actionButtons">
                <button onclick="startGeneration('quiz')"><i data-lucide="list-checks"></i> Generate Quiz</button>
                <button class="secondary" onclick="startGeneration('summary')"><i data-lucide="file-text"></i>
                    Summarize</button>
            </div>
        </div>

        <!-- STEP 2: QUIZ -->
        <div id="step-quiz" class="step">
            <div class="quiz-header">
                <h2>Your Mock Test</h2>
                <button class="nav-btn" onclick="goBack()"><i data-lucide="arrow-left"></i> Back</button>
            </div>
            <div id="quizContainer"></div>
            <div class="button-group" style="margin-top:20px;">
                <button onclick="submitQuiz()"><i data-lucide="check-circle"></i> Submit Test</button>
            </div>
        </div>

        <!-- STEP 3: RESULT -->
        <div id="step-result" class="step">
            <div class="quiz-header">
                <h2>Test Results</h2>
                <button class="nav-btn" onclick="goBack()"><i data-lucide="home"></i> Home</button>
            </div>
            <div class="result-view" id="resultContainer"></div>
        </div>

        <!-- STEP 4: SUMMARY -->
        <div id="step-summary" class="step">
            <div class="quiz-header">
                <h2>Key Takeaways</h2>
                <button class="nav-btn" onclick="goBack()"><i data-lucide="arrow-left"></i> Back</button>
            </div>
            <div id="summaryContainer"></div>
        </div>

    </div>

    <script>
        lucide.createIcons();

        let extractedText = "";
        let answers = [];
        const fileInput = document.getElementById("fileInput");
        const dropArea = document.getElementById("drop-area");

        // File UI updates
        fileInput.addEventListener("change", handleFile);

        ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
            dropArea.addEventListener(eventName, preventDefaults, false);
        });

        function preventDefaults(e) {
            e.preventDefault();
            e.stopPropagation();
        }

        ['dragenter', 'dragover'].forEach(eventName => {
            dropArea.addEventListener(eventName, () => dropArea.classList.add('dragover'), false);
        });

        ['dragleave', 'drop'].forEach(eventName => {
            dropArea.addEventListener(eventName, () => dropArea.classList.remove('dragover'), false);
        });

        dropArea.addEventListener('drop', (e) => {
            let dt = e.dataTransfer;
            let files = dt.files;
            fileInput.files = files;
            handleFile({ target: fileInput });
        });

        function handleFile(e) {
            const file = e.target.files[0];
            if (!file) return;

            document.getElementById("fileName").innerText = "Selected: " + file.name;
            document.getElementById("actionButtons").style.display = "none";
            document.getElementById("processingLoader").style.display = "block";

            let loaderText = document.getElementById("loaderText");

            if (file.type === "application/pdf") {
                loaderText.innerText = "Parsing PDF document...";
                const reader = new FileReader();
                reader.onload = async function () {
                    try {
                        const typedarray = new Uint8Array(this.result);
                        const pdf = await pdfjsLib.getDocument(typedarray).promise;
                        let text = "";
                        for (let i = 1; i <= pdf.numPages; i++) {
                            const page = await pdf.getPage(i);
                            const content = await page.getTextContent();
                            content.items.forEach(item => text += item.str + " ");
                        }
                        extractedText = text;
                        finishProcessing();
                    } catch (err) {
                        alert("Error reading PDF");
                        finishProcessing();
                    }
                };
                reader.readAsArrayBuffer(file);
            } else if (file.type === "image/jpeg" || file.type === "image/png") {
                loaderText.innerText = "Running OCR on image... (This might take a moment)";

                // Use async/await for Tesseract to ensure it completes before finishing
                async function processImage() {
                    try {
                        const result = await Tesseract.recognize(file, 'eng');
                        extractedText = result.data.text;
                        finishProcessing();
                    } catch (err) {
                        alert("Error processing image: " + err.message);
                        finishProcessing();
                    }
                }
                processImage();
            } else {
                alert("Unsupported file format");
                finishProcessing();
            }
        }

        function finishProcessing() {
            document.getElementById("processingLoader").style.display = "none";
            document.getElementById("actionButtons").style.display = "flex";
            // Optionally show success toast
        }

        function getText() {
            return (document.getElementById("notes").value + " " + extractedText).trim();
        }

        function showStep(stepId) {
            document.querySelectorAll('.step').forEach(el => el.classList.remove('active'));
            document.getElementById(stepId).classList.add('active');
            window.scrollTo(0, 0);
            lucide.createIcons();
        }

        function goBack() {
            showStep('step-input');
        }

        function startGeneration(type) {
            let text = getText();

            // Allow processing even if there's no text input if a file was uploaded
            if (text.length < 30 && extractedText.length < 30) {
                alert("Please provide more content (at least a few sentences) or upload a document with text to generate meaningful results.");
                return;
            }

            if (type === 'quiz') {
                generateQuiz(text);
            } else {
                generateSummary(text);
            }
        }

        function generateQuiz(text) {
            let container = document.getElementById("quizContainer");
            container.innerHTML = "";

            // Simple keyword extraction (words > 5 chars, letters only)
            let words = text.match(/\b[A-Za-z]{6,}\b/g) || [];
            // Deduplicate
            let uniqueWords = [...new Set(words)];

            if (uniqueWords.length < 5) {
                alert("Not enough distinct keywords found to create a quiz.");
                return;
            }

            let keywords = shuffle(uniqueWords).slice(0, 20); // 20 questions max
            answers = [];
            let questionsData = [];

            let html = "";
            keywords.forEach((word, i) => {
                let type = i % 3; // Simplified to 3 types to keep layout cleaner

                html += `<div class="question-card" data-index="${i}">`;
                let questionText = "";

                if (type === 0) {
                    // Multiple choice
                    let distractors = ["Concept", "Theory", "Process", "Mechanism", "System", "Framework"];
                    let options = shuffle([word, ...shuffle(distractors).slice(0, 3)]);

                    questionText = `What best explains the term <strong>"${word}"</strong> in this context?`;
                    html += `<p><b>${i + 1}.</b> ${questionText}</p>`;
                    options.forEach(o => {
                        html += `
                        <label class="option-label" onclick="selectOption(this, 'q${i}')">
                            <input type="radio" name="q${i}" value="${o}">
                            <span>${o}</span>
                        </label>
                    `;
                    });
                    answers.push(word);
                }
                else if (type === 1) {
                    // Fill in the blank
                    questionText = `Identify one of the key concepts discussed that ends with '${word.slice(-2)}':`;
                    html += `<p><b>${i + 1}.</b> ${questionText}</p>`;
                    html += `<input type="text" name="q${i}" placeholder="Type your answer here...">`;
                    answers.push(word);
                }
                else if (type === 2) {
                    // True/False
                    questionText = `The term <strong>"${word}"</strong> is a significant concept discussed in the text.`;
                    html += `<p><b>${i + 1}.</b> ${questionText}</p>`;
                    ["True", "False"].forEach(o => {
                        html += `
                        <label class="option-label" onclick="selectOption(this, 'q${i}')">
                            <input type="radio" name="q${i}" value="${o}">
                            <span>${o}</span>
                        </label>
                    `;
                    });
                    answers.push("True");
                }

                questionsData.push(questionText);
                html += `</div>`;
            });

            // Store questionsData globally so we can access it on submit
            window.currentQuestionsData = questionsData;

            container.innerHTML = html;
            showStep('step-quiz');
        }

        function selectOption(label, name) {
            // Highlight selected option visually
            let group = document.getElementsByName(name);
            group.forEach(input => {
                input.closest('.option-label').classList.remove('selected');
            });
            label.classList.add('selected');
            label.querySelector('input').checked = true;
        }

        function submitQuiz() {
            let score = 0;
            let total = answers.length;
            let missedQuestionsHTML = "";

            for (let i = 0; i < total; i++) {
                let selected = document.querySelector(`[name="q${i}"]:checked`) ||
                    document.querySelector(`input[type="text"][name="q${i}"]`);

                let card = document.querySelector(`.question-card[data-index="${i}"]`);

                let isCorrect = false;
                let userValue = "";

                if (selected && selected.value.trim()) {
                    userValue = selected.value.trim();
                    if (userValue.toLowerCase() === answers[i].toLowerCase()) {
                        isCorrect = true;
                    }
                }

                if (isCorrect) {
                    score += 1;
                    card.style.border = "2px solid var(--secondary)";
                } else {
                    card.style.border = "2px solid var(--danger)";

                    // Build the review div for incorrect answers
                    missedQuestionsHTML += `
                        <div class="question-card" style="text-align: left; border: 1px solid var(--danger); background: rgba(239, 68, 68, 0.05); margin-top: 10px;">
                            <p style="margin-bottom: 8px;"><b>Q${i + 1}.</b> ${window.currentQuestionsData[i]}</p>
                            <div style="font-size: 0.95rem; color: var(--danger); margin-bottom: 6px;">
                                <i data-lucide="x-circle" style="width: 16px; height: 16px; vertical-align: text-bottom;"></i> Your Answer: <strong>${userValue || "(No Answer)"}</strong>
                            </div>
                            <div style="font-size: 0.95rem; color: var(--secondary);">
                                <i data-lucide="check-circle" style="width: 16px; height: 16px; vertical-align: text-bottom;"></i> Correct Answer: <strong>${answers[i]}</strong>
                            </div>
                        </div>
                     `;
                }
            }

            let percentage = Math.round((score / total) * 100);

            let resultHTML = `
            <div class="score-circle" style="--score: ${percentage}">
                <div class="score-text">${percentage}%</div>
            </div>
            <h3>Final Score: ${score} / ${total}</h3>
            <p style="color:var(--text-muted); margin-top:10px;">
                ${percentage >= 80 ? 'Excellent work! You mastered this topic.' :
                    percentage >= 50 ? 'Good job! Review the concepts you missed.' :
                        'Keep studying! You will get it next time.'}
            </p>
            `;

            if (missedQuestionsHTML !== "") {
                resultHTML += `
                    <div style="margin-top: 30px; border-top: 2px solid var(--border); padding-top: 20px;">
                        <h3 style="margin-bottom: 16px;">Questions to Review</h3>
                        ${missedQuestionsHTML}
                    </div>
                `;
            }

            document.getElementById("resultContainer").innerHTML = resultHTML;
            showStep('step-result');
            window.scrollTo(0, 0);

            // Re-initialize icons for the newly injected HTML
            setTimeout(() => lucide.createIcons(), 0);
        }

        function generateSummary(text) {
            let container = document.getElementById("summaryContainer");

            let words = text.toLowerCase().match(/\b[a-z]{6,}\b/g) || [];
            let freq = {};

            // Exclude common stop words (basic list)
            let stops = ['should', 'would', 'could', 'because', 'however', 'therefore', 'between', 'through'];

            words.forEach(w => {
                if (!stops.includes(w)) {
                    freq[w] = (freq[w] || 0) + 1;
                }
            });

            let top = Object.entries(freq)
                .sort((a, b) => b[1] - a[1])
                .slice(0, 8);

            let html = "<ul class='summary-list'>";

            if (top.length === 0) {
                html += "<li>No significant keywords found to summarize.</li>";
            } else {
                top.forEach(item => {
                    let countStr = item[1] > 2 ? ` (Appears ${item[1]} times)` : '';
                    html += `
                    <li>
                        <i data-lucide="zap"></i>
                        <div>
                            <strong>${item[0].charAt(0).toUpperCase() + item[0].slice(1)}</strong>
                            <div style="font-size:0.9rem; color:var(--text-muted); margin-top:4px;">
                                A key highly emphasized concept in this material.${countStr}
                            </div>
                        </div>
                    </li>
                `;
                });
            }

            html += "</ul>";
            container.innerHTML = html;
            showStep('step-summary');
        }

        function shuffle(arr) {
            let array = [...arr];
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

    </script>

</body>

</html>
