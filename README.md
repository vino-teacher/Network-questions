<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .quiz-container {
            max-width: 700px;
            margin: auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
        }
        .question {
            margin-bottom: 15px;
            font-size: 20px;
            font-weight: bold;
        }
        .answers {
            margin-bottom: 25px;
        }
        .answers button {
            display: block;
            width: 100%;
            margin-bottom: 10px;
            padding: 12px;
            font-size: 16px;
            cursor: pointer;
            border: 2px solid #007bff;
            border-radius: 5px;
            background-color: #fff;
            transition: background-color 0.3s, color 0.3s;
        }
        .answers button:hover:not(:disabled) {
            background-color: #e7f3ff;
        }
        .correct {
            background-color: #28a745 !important;
            color: white;
            border-color: #28a745;
        }
        .wrong {
            background-color: #dc3545 !important;
            color: white;
            border-color: #dc3545;
        }
        #result {
            font-size: 22px;
            font-weight: bold;
            text-align: center;
            margin-top: 20px;
            display: none;
        }
        #result.congrats {
            color: green;
        }
        #result.score {
            color: red;
        }
        #try-again, #feedback-button {
            display: none;
            width: 100%;
            padding: 12px;
            font-size: 16px;
            margin-top: 15px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
        }
        #try-again {
            background-color: #007bff;
        }
        #feedback-button {
            background-color: #17a2b8;
        }
        .feedback-message {
            text-align: center;
            margin-top: 10px;
            font-size: 16px;
            color: #555;
        }
    </style>
</head>
<body>

<div class="quiz-container">
    <div id="quiz"></div>

    <div id="result"></div>
    <button id="try-again" onclick="resetQuiz()">Try Again</button>
    <button id="feedback-button" onclick="window.open('https://docs.google.com/forms/d/e/1FAIpQLSdcu9mgFQ_6RZtevJONKDX6ovsRfBUsx3EtS2jjQFKLdAzblg/viewform?usp=sf_link', '_blank')">Give Feedback</button>
    <div class="feedback-message" id="feedback-message" style="display: none;">
        Click the "Give Feedback" button to provide your feedback. Thank you!
    </div>
</div>

<script>
    const questions = [
        {
            question: "1. What is network?",
            correct: "Group or system of connected things",
            incorrect: ["A set of unrelated parts", "An individual machine"]
        },
        {
            question: "2. What is networking?",
            correct: "Making connections and exchanging information",
            incorrect: ["Running multiple machines", "A system shutdown process"]
        },
        {
            question: "3. What is infrastructure?",
            correct: "The physical things that you can see in a network that allow it to work properly",
            incorrect: ["The wireless connection", "Software code"]
        },
        {
            question: "4. Who has introduced the first postage stamp?",
            correct: "United Kingdom",
            incorrect: ["United States", "France"]
        },
        {
            question: "5. When was the email invented?",
            correct: "1971",
            incorrect: ["1969", "1980"]
        },
        {
            question: "6. When was the Morse code invented?",
            correct: "1844",
            incorrect: ["1850", "1870"]
        },
        {
            question: "7. When was the Radio Message invented?",
            correct: "1901",
            incorrect: ["1910", "1895"]
        },
        {
            question: "8. When was the stamped letter invented?",
            correct: "1840",
            incorrect: ["1830", "1850"]
        },
        {
            question: "9. When were text messages (SMS) invented?",
            correct: "1992",
            incorrect: ["1995", "1989"]
        },
        {
            question: "10. When was the Telephone call invented?",
            correct: "1876",
            incorrect: ["1880", "1890"]
        }
    ];

    let currentScore = 0;
    const totalQuestions = questions.length;

    const quizContainer = document.getElementById('quiz');
    const resultContainer = document.getElementById('result');
    const tryAgainButton = document.getElementById('try-again');
    const feedbackButton = document.getElementById('feedback-button');
    const feedbackMessage = document.getElementById('feedback-message');

    function shuffle(array) {
        for (let i = array.length -1; i >0; i--){
            const j = Math.floor(Math.random() * (i +1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }

    function loadQuiz() {
        quizContainer.innerHTML = '';
        questions.forEach((q, index) => {
            const questionDiv = document.createElement('div');
            questionDiv.classList.add('question');
            questionDiv.textContent = q.question;

            const answersDiv = document.createElement('div');
            answersDiv.classList.add('answers');

            const allAnswers = shuffle([q.correct, ...q.incorrect]);

            allAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.textContent = answer;
                button.onclick = () => selectAnswer(button, q.correct);
                answersDiv.appendChild(button);
            });

            quizContainer.appendChild(questionDiv);
            quizContainer.appendChild(answersDiv);
        });
    }

    function selectAnswer(selectedButton, correctAnswer) {
        const buttons = selectedButton.parentElement.querySelectorAll('button');
        buttons.forEach(button => button.disabled = true);

        if (selectedButton.textContent === correctAnswer) {
            selectedButton.classList.add('correct');
            currentScore++;
        } else {
            selectedButton.classList.add('wrong');
        }

        // Check if all questions have been answered
        const totalAnswered = document.querySelectorAll('.answers button:disabled').length;
        if (totalAnswered === totalQuestions * 3) { // Each question has 3 buttons
            showResult();
        }
    }

    function showResult() {
        if (currentScore === totalQuestions) {
            resultContainer.textContent = "Congratulations!!! You're the best!! Wishes from your teacher Ms. Vino.";
            resultContainer.classList.add('congrats');
        } else {
            resultContainer.textContent = `Your score is ${currentScore}/${totalQuestions}. Try again!`;
            resultContainer.classList.add('score');
        }
        resultContainer.style.display = 'block';
        tryAgainButton.style.display = 'block';
        feedbackButton.style.display = 'block';
        feedbackMessage.style.display = 'block';
    }

    function resetQuiz() {
        currentScore = 0;
        resultContainer.style.display = 'none';
        resultContainer.classList.remove('congrats', 'score');
        tryAgainButton.style.display = 'none';
        feedbackButton.style.display = 'none';
        feedbackMessage.style.display = 'none';
        loadQuiz();
    }

    // Initialize the quiz on page load
    window.onload = loadQuiz;
</script>

</body>
</html>
