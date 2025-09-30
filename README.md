# QuizwebApplication

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScriptQuiz</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #9df9ef 1%, #a28089 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background-color: white;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            width: 100%;
            max-width: 600px;
            overflow: hidden;
        }
        
        .header {
            background: linear-gradient(90deg,#ffa8B6,#ffa8B6);
            color: white;
            text-align: center;
            padding: 20px;
            position: relative;
        }
        
        .header h1 {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .header p {
            font-size: 16px;
            opacity: 0.9;
        }
        
        .progress-container {
            height: 8px;
            width: 100%;
            background-color:  #e0e0e0;;
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #29021A, #29021A);
            width: 0%;
            transition: width 0.3s ease;
        }
        
        .quiz-container {
            padding: 25px;
        }
        
        .question-number {
            font-size: 18px;
            font-weight: 600;
            color: #99586a;
            margin-bottom: 5px;
        }
        
        .question {
            font-size: 22px;
            font-weight: 600;
            margin-bottom: 20px;
            color: #333;
        }
        
        .options-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 25px;
        }
        
        .option {
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 16px;
        }
        
        .option:hover {
            background-color: #f5f5f5;
            border-color: #c0c0c0;
        }
        
        .option.selected {
            background-color: #E8F5E9;
            border-color: #29021A;
        }
        
        .option.correct {
            background-color: #C8E6C9;
            border-color: #2E7D32;
        }
        
        .option.incorrect {
            background-color: #FFCDD2;
            border-color: #D32F2F;
        }
        
        .navigation {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        button {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        #prev-btn {
            background-color: #f5f5f5;
            color: #333;
        }
        
        #prev-btn:hover:not(:disabled) {
            background-color: #e0e0e0;
        }
        
        #next-btn {
            background: linear-gradient(90deg, #ffa8B6 , #ffa8B6);
            color: white;
        }
        
        #next-btn:hover:not(:disabled) {
            opacity: 0.9;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .score-container {
            text-align: center;
            padding: 30px;
        }
        
        .score-container h2 {
            font-size: 32px;
            color: #4CAF50;
            margin-bottom: 15px;
        }
        
        .score-container p {
            font-size: 18px;
            margin-bottom: 25px;
            color: #555;
        }
        
        #restart-btn {
            background: linear-gradient(90deg,  #ffa8B6 , #ffa8B6);
            color: white;
            padding: 12px 30px;
        }
        
        #restart-btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .hidden {
            display: none;
        }
        
        /* Responsive design */
        @media (max-width: 600px) {
            .header h1 {
                font-size: 24px;
            }
            
            .question {
                font-size: 20px;
            }
            
            .option {
                padding: 12px;
            }
            
            button {
                padding: 10px 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>QuizMaster</h1>
            <p>Test your knowledge with our engaging quiz</p>
        </div>
        
        <div class="progress-container">
            <div class="progress-bar" id="progress-bar"></div>
        </div>
        
        <div class="quiz-container" id="quiz-container">
            <!-- Question will be inserted here -->
        </div>
        
        <div class="score-container hidden" id="score-container">
            <h2>Quiz Completed!</h2>
            <p id="score-text">Your score: 0/0</p>
            <button id="restart-btn">Restart Quiz</button>
        </div>
    </div>

    <script>
        // Quiz questions data
        const quizData = [
            {
                question: "Which type of language is JavaScript?",
                options: ["Object-Oriented", "Object-Based", "Assembly-language", "High-level"],
                correct: 1
            },
            {
                question: "Which of the following is used to define a variable in JavaScript?",
                options:  ["Var", "Let", "Const", "All of the above"],
                correct: 3
            },
            {
                question: "What is the output of typeof null in JavaScript?",
                options:  ["null", "object", "undefiend", "number"],
                correct: 1
            },
            {
                question: "What year was JavaScript launched?",
                options:   ["1996", "1995", "1994", "None of the above"],
                correct: 1
            },
            {
                question: "Which function is used to parse a string to an integer in JavaScript?",
                options:["parseInt()", "parseFloat()", "Number()", "Both A & C"],
                correct: 3
            }
        ];
        
        // DOM elements
        const quizContainer = document.getElementById('quiz-container');
        const progressBar = document.getElementById('progress-bar');
        const scoreContainer = document.getElementById('score-container');
        const scoreText = document.getElementById('score-text');
        const restartBtn = document.getElementById('restart-btn');
        
        // Quiz state
        let currentQuestion = 0;
        let score = 0;
        let userAnswers = new Array(quizData.length).fill(null);
        
        // Initialize the quiz
        function initQuiz() {
            showQuestion();
            
            // Add event listener for restart button
            restartBtn.addEventListener('click', () => {
                resetQuiz();
                showQuestion();
            });
        }
        
        // Display the current question
        function showQuestion() {
            // Update progress bar
            progressBar.style.width = `${((currentQuestion) / quizData.length) * 100}%`;
            
            const question = quizData[currentQuestion];
            
            // Create question HTML
            const questionHTML = `
                <div class="question-number">Question ${currentQuestion + 1} of ${quizData.length}</div>
                <div class="question">${question.question}</div>
                <div class="options-container">
                    ${question.options.map((option, index) => `
                        <div class="option ${userAnswers[currentQuestion] === index ? 'selected' : ''}" data-index="${index}">
                            ${option}
                        </div>
                    `).join('')}
                </div>
                <div class="navigation">
                    <button id="prev-btn" ${currentQuestion === 0 ? 'disabled' : ''}>Previous</button>
                    <button id="next-btn">${currentQuestion === quizData.length - 1 ? 'Finish' : 'Next'}</button>
                </div>
            `;
            
            quizContainer.innerHTML = questionHTML;
            
            // Add event listeners to options
            document.querySelectorAll('.option').forEach(option => {
                option.addEventListener('click', selectOption);
            });
            
            // Add event listeners to navigation buttons
            document.getElementById('prev-btn').addEventListener('click', goToPrevQuestion);
            document.getElementById('next-btn').addEventListener('click', goToNextQuestion);
        }
        
        // Handle option selection
        function selectOption(e) {
            const selectedOption = e.currentTarget;
            const optionIndex = parseInt(selectedOption.dataset.index);
            
            // Remove selected class from all options
            document.querySelectorAll('.option').forEach(option => {
                option.classList.remove('selected');
            });
            
            // Add selected class to clicked option
            selectedOption.classList.add('selected');
            
            // Store user's answer
            userAnswers[currentQuestion] = optionIndex;
        }
        
        // Go to previous question
        function goToPrevQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion();
            }
        }
        
        // Go to next question or finish quiz
        function goToNextQuestion() {
            // If user hasn't selected an option, don't proceed
            if (userAnswers[currentQuestion] === null) {
                alert('Please select an option before proceeding.');
                return;
            }
            
            // Check if answer is correct
            if (userAnswers[currentQuestion] === quizData[currentQuestion].correct) {
                score++;
            }
            
            // Move to next question or show results
            if (currentQuestion < quizData.length - 1) {
                currentQuestion++;
                showQuestion();
            } else {
                showResults();
            }
        }
        
        // Show quiz results
        function showResults() {
            // Update progress bar to 100%
            progressBar.style.width = '100%';
            
            // Hide quiz container and show score container
            quizContainer.classList.add('hidden');
            scoreContainer.classList.remove('hidden');
            
            // Update score text
            scoreText.textContent = `Your score: ${score}/${quizData.length}`;
        }
        
        // Reset quiz to initial state
        function resetQuiz() {
            currentQuestion = 0;
            score = 0;
            userAnswers = new Array(quizData.length).fill(null);
            
            // Hide score container and show quiz container
            scoreContainer.classList.add('hidden');
            quizContainer.classList.remove('hidden');
        }
        
        // Initialize the quiz when the page loads
        document.addEventListener('DOMContentLoaded', initQuiz);
    </script>
</body>
</html>

