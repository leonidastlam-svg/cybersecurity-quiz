<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cybersecurity Quiz</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2em;
            margin-bottom: 10px;
        }

        .progress-container {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            height: 10px;
            margin-top: 20px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: #4CAF50;
            width: 0%;
            transition: width 0.3s ease;
        }

        .quiz-content {
            padding: 40px;
        }

        .question-card {
            display: none;
            animation: fadeIn 0.5s;
        }

        .question-card.active {
            display: block;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .question-number {
            color: #667eea;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .question-text {
            font-size: 1.2em;
            margin-bottom: 25px;
            color: #333;
            line-height: 1.6;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .option-btn {
            background: #f5f5f5;
            border: 2px solid #e0e0e0;
            padding: 15px 20px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            text-align: left;
            font-size: 1em;
        }

        .option-btn:hover {
            background: #e8e8e8;
            border-color: #667eea;
            transform: translateX(5px);
        }

        .option-btn.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .option-btn.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option-btn.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .option-btn:disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }

        textarea {
            width: 100%;
            min-height: 120px;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-family: inherit;
            font-size: 1em;
            resize: vertical;
            transition: border-color 0.3s;
        }

        textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .feedback {
            margin-top: 20px;
            padding: 15px;
            border-radius: 10px;
            display: none;
        }

        .feedback.show {
            display: block;
            animation: fadeIn 0.5s;
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .feedback.info {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 30px;
        }

        .nav-btn {
            padding: 12px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s;
        }

        .nav-btn.primary {
            background: #667eea;
            color: white;
        }

        .nav-btn.primary:hover {
            background: #5568d3;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .nav-btn.secondary {
            background: #e0e0e0;
            color: #333;
        }

        .nav-btn.secondary:hover {
            background: #d0d0d0;
        }

        .nav-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .results {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .results.show {
            display: block;
            animation: fadeIn 0.5s;
        }

        .score-circle {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin: 30px auto;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .score-circle .score {
            font-size: 3em;
            font-weight: bold;
        }

        .score-circle .total {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .result-message {
            font-size: 1.5em;
            margin: 20px 0;
            color: #333;
        }

        .restart-btn {
            background: #667eea;
            color: white;
            padding: 15px 40px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s;
        }

        .restart-btn:hover {
            background: #5568d3;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .answer-key {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin-top: 30px;
            text-align: left;
        }

        .answer-key h3 {
            color: #667eea;
            margin-bottom: 15px;
        }

        .answer-item {
            margin-bottom: 15px;
            padding: 10px;
            background: white;
            border-radius: 5px;
        }

        .answer-item strong {
            color: #333;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🔒 Cybersecurity Quiz</h1>
            <p>Test your knowledge on malware, security threats, and best practices</p>
            <div class="progress-container">
                <div class="progress-bar" id="progressBar"></div>
            </div>
        </div>

        <div class="quiz-content" id="quizContent">
            <!-- Questions will be generated here -->
        </div>

        <div class="results" id="results">
            <h2>Quiz Complete! 🎉</h2>
            <div class="score-circle">
                <div class="score" id="finalScore">0</div>
                <div class="total">out of 15</div>
            </div>
            <div class="result-message" id="resultMessage"></div>
            <button class="restart-btn" onclick="restartQuiz()">Take Quiz Again</button>
            <div class="answer-key" id="answerKey"></div>
        </div>
    </div>

    <script>
        const questions = [
            {
                type: 'truefalse',
                question: 'A computer virus can spread automatically without any user action',
                options: ['True', 'False'],
                correct: 1,
                explanation: 'FALSE - That describes a WORM, viruses need user action to spread.'
            },
            {
                type: 'truefalse',
                question: 'Ransomware encrypts a victim\'s files and demands payment to restore access',
                options: ['True', 'False'],
                correct: 0,
                explanation: 'TRUE - Ransomware encrypts files and demands payment for restoration.'
            },
            {
                type: 'truefalse',
                question: 'Phishing scams often use fake emails or websites to trick users into revealing personal information',
                options: ['True', 'False'],
                correct: 0,
                explanation: 'TRUE - Phishing uses deceptive methods to steal personal information.'
            },
            {
                type: 'multiple',
                question: 'Which of the following best describes a TROJAN?',
                options: [
                    'A program that replicates itself',
                    'A malicious program disguised as legitimate software',
                    'Software that displays unwanted ads',
                    'A worm that spreads through networks'
                ],
                correct: 1,
                explanation: 'A Trojan is malicious software disguised as legitimate software.'
            },
            {
                type: 'multiple',
                question: 'Spyware is primarily used to:',
                options: [
                    'Encrypt files',
                    'Display advertisements',
                    'Secretly gather information',
                    'Destroy files'
                ],
                correct: 2,
                explanation: 'Spyware secretly gathers information without the user\'s knowledge.'
            },
            {
                type: 'short',
                question: 'Explain the main difference between malware and adware.',
                answer: 'Malware is any malicious software designed to harm or exploit a system, while adware focuses on displaying unwanted advertisements and sometimes tracking user behaviour.'
            },
            {
                type: 'short',
                question: 'Describe how a computer worm spreads and one method to prevent infection.',
                answer: 'Worms spread automatically through networks by exploiting vulnerabilities. Keeping systems updated and using firewalls helps prevent infection.'
            },
            {
                type: 'short',
                question: 'What are some warning signs of a phishing email?',
                answer: 'Generic greetings, spelling errors, urgent language, mismatched links, or any suspicious attachments.'
            },
            {
                type: 'short',
                question: 'How does ransomware typically impact an organisation?',
                answer: 'It encrypts important data, halting operations until a ransom is paid or backups are restored, often causing financial and reputational damage.'
            },
            {
                type: 'short',
                question: 'What is social engineering, and why is it considered a major security threat?',
                answer: 'It manipulates people into revealing confidential information or performing unsafe actions. It exploits human trust rather than technical flaws.'
            },
            {
                type: 'short',
                question: 'Briefly describe the Optus data breach and its impact on customers.',
                answer: 'In 2022, Optus suffered a major data breach exposing personal information like names, birth dates, and ID numbers. It affected millions of Australians and led to stronger data protection calls.'
            },
            {
                type: 'short',
                question: 'What are two strong password creation tips for improved security?',
                answer: 'Use at least 12 characters with a mix of upper and lowercase letters, numbers and symbols. Avoid common words or reused passwords.'
            },
            {
                type: 'short',
                question: 'How can users verify if a website is legitimate before entering personal data?',
                answer: 'Check for HTTPS, look for spelling errors in the domain, and avoid clicking on suspicious links in emails.'
            },
            {
                type: 'short',
                question: 'Discuss how spyware and adware differ in their objectives.',
                answer: 'Spyware secretly collects data without consent, while adware aims to show ads, sometimes legitimately but often intrusively.'
            },
            {
                type: 'short',
                question: 'Explain why Australian cybersecurity incidents like Optus or Medibank are valuable case studies for learning.',
                answer: 'They show real-world consequences of breaches, highlight the importance of data protection, and help organisations learn how to improve their cybersecurity practices.'
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let userAnswers = [];

        function initQuiz() {
            renderQuestion();
        }

        function renderQuestion() {
            const q = questions[currentQuestion];
            const content = document.getElementById('quizContent');
            
            let html = `
                <div class="question-card active">
                    <div class="question-number">Question ${currentQuestion + 1} of ${questions.length}</div>
                    <div class="question-text">${q.question}</div>
                    <div class="options">
            `;

            if (q.type === 'truefalse' || q.type === 'multiple') {
                q.options.forEach((option, index) => {
                    html += `<button class="option-btn" onclick="selectOption(${index})">${option}</button>`;
                });
            } else if (q.type === 'short') {
                html += `<textarea id="shortAnswer" placeholder="Type your answer here..."></textarea>`;
            }

            html += `
                    </div>
                    <div class="feedback" id="feedback"></div>
                    <div class="navigation">
                        <button class="nav-btn secondary" onclick="previousQuestion()" ${currentQuestion === 0 ? 'disabled' : ''}>Previous</button>
                        <button class="nav-btn primary" id="nextBtn" onclick="nextQuestion()">${currentQuestion === questions.length - 1 ? 'Finish Quiz' : 'Next'}</button>
                    </div>
                </div>
            `;

            content.innerHTML = html;
            updateProgress();
        }

        function selectOption(index) {
            const buttons = document.querySelectorAll('.option-btn');
            buttons.forEach((btn, i) => {
                btn.classList.remove('selected');
                if (i === index) {
                    btn.classList.add('selected');
                }
            });
            userAnswers[currentQuestion] = index;
        }

        function nextQuestion() {
            const q = questions[currentQuestion];
            const feedback = document.getElementById('feedback');
            
            if (q.type === 'short') {
                const answer = document.getElementById('shortAnswer').value.trim();
                if (!answer) {
                    feedback.className = 'feedback incorrect show';
                    feedback.textContent = 'Please provide an answer before continuing.';
                    return;
                }
                userAnswers[currentQuestion] = answer;
                feedback.className = 'feedback info show';
                feedback.innerHTML = `<strong>Sample Answer:</strong> ${q.answer}`;
                
                setTimeout(() => {
                    proceedToNext();
                }, 3000);
            } else {
                if (userAnswers[currentQuestion] === undefined) {
                    feedback.className = 'feedback incorrect show';
                    feedback.textContent = 'Please select an answer before continuing.';
                    return;
                }
                
                const isCorrect = userAnswers[currentQuestion] === q.correct;
                if (isCorrect) score++;
                
                const buttons = document.querySelectorAll('.option-btn');
                buttons.forEach((btn, i) => {
                    btn.disabled = true;
                    if (i === q.correct) {
                        btn.classList.add('correct');
                    } else if (i === userAnswers[currentQuestion] && !isCorrect) {
                        btn.classList.add('incorrect');
                    }
                });
                
                feedback.className = `feedback ${isCorrect ? 'correct' : 'incorrect'} show`;
                feedback.innerHTML = `<strong>${isCorrect ? 'Correct!' : 'Incorrect.'}</strong> ${q.explanation}`;
                
                setTimeout(() => {
                    proceedToNext();
                }, 3000);
            }
        }

        function proceedToNext() {
            currentQuestion++;
            if (currentQuestion < questions.length) {
                renderQuestion();
            } else {
                showResults();
            }
        }

        function previousQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                renderQuestion();
            }
        }

        function updateProgress() {
            const progress = ((currentQuestion) / questions.length) * 100;
            document.getElementById('progressBar').style.width = progress + '%';
        }

        function showResults() {
            document.getElementById('quizContent').style.display = 'none';
            const results = document.getElementById('results');
            results.classList.add('show');
            
            document.getElementById('finalScore').textContent = score;
            
            const percentage = (score / 15) * 100;
            let message = '';
            if (percentage >= 90) {
                message = 'Outstanding! You have excellent cybersecurity knowledge! 🌟';
            } else if (percentage >= 75) {
                message = 'Great job! You have a strong understanding of cybersecurity. 👏';
            } else if (percentage >= 60) {
                message = 'Good effort! Keep learning to improve your security awareness. 📚';
            } else {
                message = 'Keep studying! Cybersecurity is crucial in today\'s digital world. 💪';
            }
            document.getElementById('resultMessage').textContent = message;
            
            generateAnswerKey();
        }

        function generateAnswerKey() {
            const answerKey = document.getElementById('answerKey');
            let html = '<h3>Answer Key</h3>';
            
            questions.forEach((q, i) => {
                if (q.type !== 'short') {
                    const userAnswer = q.options[userAnswers[i]] || 'No answer';
                    const correctAnswer = q.options[q.correct];
                    const isCorrect = userAnswers[i] === q.correct;
                    
                    html += `
                        <div class="answer-item">
                            <strong>Q${i + 1}:</strong> ${q.question}<br>
                            <span style="color: ${isCorrect ? '#4CAF50' : '#f44336'}">Your answer: ${userAnswer}</span><br>
                            <span style="color: #4CAF50">Correct answer: ${correctAnswer}</span>
                        </div>
                    `;
                }
            });
            
            answerKey.innerHTML = html;
        }

        function restartQuiz() {
            currentQuestion = 0;
            score = 0;
            userAnswers = [];
            document.getElementById('results').classList.remove('show');
            document.getElementById('quizContent').style.display = 'block';
            renderQuestion();
        }

        initQuiz();
    </script>
</body>
</html>
