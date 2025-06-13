<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>음주 영향 퀴즈 게임</title>
    <style>
        /* 기본 스타일 */
        html, body {
            margin: 0;
            padding: 0;
            font-family: 'Inter', sans-serif;
            background: #eef;
            overflow-x: hidden; /* Prevent horizontal overflow */
            height: 100%;
        }

        /* 게임 영역 스타일 */
        #gameArea {
            position: relative;
            width: 100%; /* Use full width */
            max-width: 100vw; /* Prevent exceeding viewport */
            height: 100vh;
            background: linear-gradient(to bottom, #cceeff, #99ccff);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            padding: 5vh 2vw; /* Reduced side padding */
            box-sizing: border-box;
            overflow-x: hidden;
        }

        /* 문제 상자 스타일 */
        #questionBox {
            background: white;
            padding: 2vh 3vw;
            border: 2px solid #555;
            border-radius: 10px;
            max-width: 90vw;
            width: auto;
            font-size: 1.2rem; /* Use rem for better scaling */
            text-align: center;
            white-space: pre-wrap;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
            line-height: 1.4;
            margin-bottom: 3vh;
            box-sizing: border-box;
        }

        /* 보기 컨테이너 스타일 */
        #optionsContainer {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2vh;
            margin-bottom: 5vh;
            max-width: 90vw;
            box-sizing: border-box;
        }

        /* 보기 상자 스타일 */
        .optionBox {
            background: #f0f0f0;
            border: 1px solid #ccc;
            border-radius: 8px;
            padding: 3vh 2vw;
            font-size: 1rem;
            font-weight: bold;
            text-align: center;
            cursor: pointer;
            box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.1);
            transition: background 0.2s ease, border-color 0.2s ease, color 0.2s ease;
            min-width: 80vw; /* Wider for mobile */
            max-width: 90vw;
            box-sizing: border-box;
            white-space: normal;
        }

        .optionBox:hover {
            background: #e0e0e0;
            border-color: #999;
        }

        .optionBox.correct {
            background-color: #d4edda;
            border-color: #28a745;
            color: #155724;
        }

        .optionBox.incorrect {
            background-color: #f8d7da;
            border-color: #dc3545;
            color: #721c24;
        }

        /* 주관식 입력창 스타일 */
        #answerInput {
            width: 70vw;
            max-width: 500px;
            height: 15vh;
            padding: 2vw;
            font-size: 1rem;
            border: 2px solid #ccc;
            border-radius: 8px;
            box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.1);
            resize: vertical;
            box-sizing: border-box;
            margin-top: 2vh;
        }

        /* 자동차 스타일 */
        #car {
            position: absolute;
            width: 18vw;
            max-width: 150px;
            height: 10vh;
            max-height: 80px;
            background: linear-gradient(to right, #3498db, #2980b9);
            border-radius: 10px;
            bottom: 8vh;
            left: 5vw;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.3);
            transition: left 0.03s linear;
            white-space: nowrap;
            padding: 0 1vw;
            box-sizing: border-box;
        }

        #car span {
            font-size: 0.9rem;
            padding: 0.5vh 1vw;
            border: 1px solid rgba(255, 255, 255, 0.7);
            border-radius: 5px;
            background-color: rgba(0, 0, 0, 0.2);
        }

        /* 점수 표시 스타일 */
        #score {
            position: absolute;
            top: 2vh;
            left: 3vw;
            font-size: 1.2rem;
            font-weight: bold;
            color: #333;
        }

        /* 다음 버튼 스타일 */
        #nextBtn {
            /* position: fixed; */ /* Removed fixed positioning */
            /* bottom: 3vh; */ /* Removed bottom positioning */
            /* right: 3vw; */ /* Removed right positioning */
            padding: 1.5vh 3vw;
            font-size: 1rem;
            background: #27ae60;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
            transition: background 0.3s ease;
            z-index: 30;
            min-width: 100px;
            margin-top: 2vh; /* Add margin-top to position below feedback */
        }

        #nextBtn:hover {
            background: #2ecc71;
        }

        /* 다시 시작 버튼 스타일 */
        #restartBtn {
            margin-top: 3vh;
            padding: 1.5vh 3vw;
            font-size: 1rem;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
            transition: background 0.3s ease;
        }

        #restartBtn:hover {
            background: #0056b3;
        }

        /* 피드백 메시지 스타일 */
        #feedback {
            /* position: absolute; */ /* Removed absolute positioning */
            /* bottom: 20vh; */ /* Removed bottom positioning */
            /* left: 50%; */ /* Removed left positioning */
            /* transform: translateX(-50%); */ /* Removed transform */
            font-size: 1rem;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
            min-width: 200px;
            text-align: center;
            z-index: 10;
            margin-top: 2vh; /* Add margin-top for spacing */
        }

        /* 최종 결과 화면 스타일 */
        #finalResult {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 5vh 6vw;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
            text-align: center;
            font-size: 1rem;
            font-weight: bold;
            line-height: 1.6;
            display: none;
            z-index: 20;
            overflow-y: auto;
            max-height: 90vh;
        }

        #finalResult p {
            margin: 1vh 0;
        }

        #descriptiveAnswers p {
            text-align: left;
            margin-top: 2vh;
            border-top: 1px dashed #ccc;
            padding-top: 1vh;
            font-size: 0.9rem;
            font-weight: normal;
        }

        #descriptiveAnswers strong {
            color: #0056b3;
        }
        #descriptiveAnswers span {
            color: #28a745;
        }
        #descriptiveAnswers .user-answer {
            color: #6a0dad;
        }
        #descriptiveAnswers .result-feedback {
            font-weight: bold;
        }

        /* 미디어 쿼리 - 태블릿 (768px 이상) */
        @media (min-width: 768px) {
            #questionBox {
                font-size: 1.5rem;
                max-width: 70vw;
            }
            .optionBox {
                font-size: 1.2rem;
                padding: 2vh 2vw;
                min-width: 50vw;
                max-width: 70vw;
            }
            #answerInput {
                width: 60vw;
                font-size: 1.2rem;
            }
            #score, #nextBtn, #restartBtn {
                font-size: 1.2rem;
            }
            #feedback {
                font-size: 1.2rem;
            }
            #car {
                font-size: 1.2rem;
            }
            #car span {
                font-size: 1rem;
            }
            #finalResult {
                font-size: 1.2rem;
                padding: 4vh 5vw;
            }
            #descriptiveAnswers p {
                font-size: 1rem;
            }
        }

        /* 미디어 쿼리 - 데스크톱 (1024px 이상) */
        @media (min-width: 1024px) {
            #questionBox {
                font-size: 1.8rem;
                max-width: 60vw;
            }
            .optionBox {
                font-size: 1.4rem;
                padding: 1.5vh 1.5vw;
                min-width: 40vw;
                max-width: 50vw;
            }
            #answerInput {
                width: 50vw;
                font-size: 1.4rem;
            }
            #score, #nextBtn, #restartBtn {
                font-size: 1.4rem;
            }
            #feedback {
                font-size: 1.4rem;
            }
            #car {
                width: 12vw;
                max-width: 100px;
                font-size: 1.4rem;
            }
            #car span {
                font-size: 1.2rem;
            }
            #finalResult {
                font-size: 1.4rem;
                max-width: 50vw;
            }
            #descriptiveAnswers p {
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>

<div id="gameArea">
    <div id="score">점수: 0</div>
    <div id="questionBox">문제 로딩 중...</div>
    <div id="optionsContainer">
        </div>
    <div id="car"></div>
    <div id="feedback"></div>
    <button id="nextBtn">다음</button> 
    <div id="finalResult">
        <p>🎉 퀴즈 완료!</p>
        <p>총 객관식 문제: <span id="totalQuestions"></span>개</p>
        <p style="color: green;">✅ 정답: <span id="correctAnswers"></span>개</p>
        <p style="color: red;">❌ 오답: <span id="incorrectAnswers"></span>개</p>
        <p>최종 점수: <span id="finalScore"></span>점</p>
        <div id="descriptiveAnswers">
            <h3>주관식 문제 & 정답 예시</h3>
        </div>
        <button id="restartBtn" onclick="restartGame()">다시 시작</button>
    </div>
</div>

<script>
    // 퀴즈 문제 데이터
    const questions = [
        {
            type: "multiple-choice",
            q: "과도한 음주가 인체에 미치는 영향으로 옳지 않은 것은?",
            choices: [
                { letter: "A", text: "간 기능 저하" },
                { letter: "B", text: "심혈관 질환 발생 위험 증가" },
                { letter: "C", text: "면역력 향상" },
                { letter: "D", text: "뇌 기능 저하" }
            ],
            a: "C"
        },
        {
            type: "multiple-choice",
            q: "다음 중 알코올 중독의 가정 내 부작용으로 가장 일반적인 것은?",
            choices: [
                { letter: "A", text: "가정 경제의 향상" },
                { letter: "B", text: "가족 간 소통 증가" },
                { letter: "C", text: "가정 폭력 증가" },
                { letter: "D", text: "가족 구성원 건강 개선" }
            ],
            a: "C"
        },
        {
            type: "multiple-choice",
            q: "술이 사회에 미치는 부정적인 영향으로 옳은 것은?",
            choices: [
                { letter: "A", text: "범죄 발생률 감소" },
                { letter: "B", text: "생산성 증가" },
                { letter: "C", text: "음주운전 사고 증가" },
                { letter: "D", text: "사회적 신뢰도 향상" }
            ],
            a: "C"
        },
        {
            type: "multiple-choice",
            q: "다음 중 알코올 의존증의 증상으로 보기 어려운 것은?",
            choices: [
                { letter: "A", text: "금단 증상" },
                { letter: "B", text: "조절 능력 상실" },
                { letter: "C", text: "반복적인 음주로 인한 문제 발생" },
                { letter: "D", text: "음주 후 신체 건강 회복" }
            ],
            a: "D"
        },
        {
            type: "multiple-choice",
            q: "음주운전으로 인한 사회적 피해에 해당하지 않는 것은?",
            choices: [
                { letter: "A", text: "타인의 생명 위협" },
                { letter: "B", text: "운전 능력 향상" },
                { letter: "C", text: "벌금이나 처벌" },
                { letter: "D", text: "교통사고로 인한 사망"}
            ],
            a: "B"
        },
        {
            type: "multiple-choice",
            q: "청소년의 음주가 사회에 미치는 영향으로 옳은 것은?",
            choices: [
                { letter: "A", text: "학업 능력 향상" },
                { letter: "B", text: "또래 관계의 긍정적 강화" },
                { letter: "C", text: "범죄 연루 가능성 증가" },
                { letter: "D", text: "건강한 성장 촉진" }
            ],
            a: "C"
        },
        {
            type: "descriptive-scored",
            q: "술이 간에 미치는 대표적인 질병 두 가지를 서술하세요.",
            a: "간경변, 지방간"
        },
        {
            type: "descriptive-unscored",
            q: "가정에서 과도한 음주가 가정에 미칠 수 있는 영향 1가지 이상 서술하세요.",
            a: "가정 폭력, 불화, 경제적 어려움, 아동 방치"
        },
        {
            type: "descriptive-unscored",
            q: "음주운전이 사회적으로 왜 큰 문제가 되는지 이유를 2가지 이상 서술하세요.",
            a: "교통사고 및 사망자 증가, 사회적 비용 발생, 타인의 생명 위협, 법적 처벌"
        },
        {
            type: "descriptive-unscored",
            q: "건강하고 책임 있는 음주 문화를 형성하기 위해 개인이나 사회가 할 수 있는 노력을 서술하세요.",
            a: "절주 교육, 캠페인, 대중교통 이용 권장, 음주 자제 분위기 조성"
        }
    ];

    let currentQuestionIndex = 0;
    let score = 0;
    let mcCorrectCount = 0;
    let mcIncorrectCount = 0;
    let carMoveInterval;
    let isCarMovingRight = true;
    let optionsDisabled = false;
    let userDescriptiveAnswers = [];

    const car = document.getElementById("car");
    const questionBox = document.getElementById("questionBox");
    const optionsContainer = document.getElementById("optionsContainer");
    const scoreDisplay = document.getElementById("score");
    const feedback = document.getElementById("feedback");
    const nextBtn = document.getElementById("nextBtn");
    const finalResultDiv = document.getElementById("finalResult");
    const descriptiveAnswersDiv = document.getElementById("descriptiveAnswers");
    const totalQuestionsSpan = document.getElementById("totalQuestions");
    const correctAnswersSpan = document.getElementById("correctAnswers");
    const incorrectAnswersSpan = document.getElementById("incorrectAnswers");
    const finalScoreSpan = document.getElementById("finalScore");

    nextBtn.addEventListener("click", () => {
        const currentQType = questions[currentQuestionIndex].type;
        if (currentQType.startsWith("descriptive")) {
            const answerInput = document.getElementById('answerInput');
            if (answerInput) {
                checkDescriptiveAnswer(answerInput.value, currentQType);
            }
        } else {
            nextQuestion();
        }
    });

    function loadQuestion() {
        if (currentQuestionIndex >= questions.length) {
            showResult();
            return;
        }

        const qData = questions[currentQuestionIndex];
        questionBox.innerText = qData.q;
        optionsContainer.innerHTML = "";
        car.innerHTML = "";
        car.style.backgroundColor = '#3498db';
        feedback.innerText = "";
        feedback.style.color = "";
        nextBtn.style.display = "none";
        optionsDisabled = false;

        if (qData.type === "multiple-choice") {
            optionsContainer.style.display = "flex";
            qData.choices.forEach((choice) => {
                const optionBox = document.createElement("div");
                optionBox.classList.add("optionBox");
                optionBox.innerText = `${choice.letter}. ${choice.text}`;
                optionBox.dataset.letter = choice.letter;
                optionBox.addEventListener("click", () => checkAnswer(optionBox, choice.letter));
                optionsContainer.appendChild(optionBox);
            });
            car.innerHTML = `<span>정답 대기중...</span>`;
            car.style.backgroundColor = '#6c757d';
        } else if (qData.type.startsWith("descriptive")) {
            optionsContainer.style.display = "flex";
            const answerInput = document.createElement("textarea");
            answerInput.id = "answerInput";
            answerInput.placeholder = "여기에 답변을 입력하세요.";
            optionsContainer.appendChild(answerInput);
            car.innerHTML = `<span>답변 입력 대기중...</span>`;
            car.style.backgroundColor = '#6c757d';
            feedback.innerText = "답변을 입력하고 다음 버튼을 눌러주세요.";
            feedback.style.color = "#3498db";
            nextBtn.style.display = "block";
        }

        startCarMovement();
    }

    function startCarMovement() {
        clearInterval(carMoveInterval);
        let carLeft = parseFloat(car.style.left) || 5 * window.innerWidth / 100; // Initialize carLeft if not set
        const gameAreaWidth = window.innerWidth;
        const carWidth = car.offsetWidth;
        const padding = 50;

        carMoveInterval = setInterval(() => {
            if (isCarMovingRight) {
                carLeft += 3;
                if (carLeft + carWidth >= gameAreaWidth - padding) {
                    isCarMovingRight = false;
                }
            } else {
                carLeft -= 3;
                if (carLeft <= padding) {
                    isCarMovingRight = true;
                }
            }
            car.style.left = carLeft + "px";
            car.dataset.isMovingRight = isCarMovingRight;
        }, 30);
    }

    function checkAnswer(selectedOptionBox, selectedLetter) {
        if (optionsDisabled) return;
        optionsDisabled = true;
        clearInterval(carMoveInterval);

        const qData = questions[currentQuestionIndex];
        const correctAnswerLetter = qData.a;

        document.querySelectorAll('.optionBox').forEach(box => {
            box.style.pointerEvents = 'none';
        });

        let isCorrect = false;
        if (selectedLetter === correctAnswerLetter) {
            feedback.innerText = "✅ 정답입니다!";
            feedback.style.color = "green";
            score += 10;
            mcCorrectCount++;
            selectedOptionBox.classList.add("correct");
            isCorrect = true;
        } else {
            feedback.innerText = "❌ 오답입니다!";
            feedback.style.color = "red";
            mcIncorrectCount++;
            selectedOptionBox.classList.add("incorrect");
            document.querySelectorAll('.optionBox').forEach(option => {
                if (option.dataset.letter === correctAnswerLetter) {
                    option.classList.add("correct");
                }
            });
        }
        scoreDisplay.innerText = "점수: " + score;
        car.innerHTML = `<span>정답: ${correctAnswerLetter}</span>`;
        car.style.backgroundColor = '#27ae60';
        nextBtn.style.display = "block";
    }

    function checkDescriptiveAnswer(userAnswer, questionType) {
        if (optionsDisabled) return;
        optionsDisabled = true;
        clearInterval(carMoveInterval);

        const qData = questions[currentQuestionIndex];
        const trimmedUserAnswer = userAnswer.trim();
        const isCorrect = trimmedUserAnswer.length > 0;

        userDescriptiveAnswers[currentQuestionIndex] = {
            question: qData.q,
            userAnswer: userAnswer,
            correctAnswer: qData.a,
            isCorrect: isCorrect,
            type: questionType
        };

        if (questionType === "descriptive-scored") {
            if (isCorrect) {
                feedback.innerText = "✅ 정답입니다!";
                feedback.style.color = "green";
                score += 10;
            } else {
                feedback.innerText = "❌ 오답입니다!";
                feedback.style.color = "red";
            }
            scoreDisplay.innerText = "점수: " + score;
            car.innerHTML = `<span>${isCorrect ? '정답!' : '오답!'}</span>`;
            car.style.backgroundColor = isCorrect ? '#27ae60' : '#dc3545';
        } else {
            feedback.innerText = "답변이 기록되었습니다.";
            feedback.style.color = "#3498db";
            car.innerHTML = `<span>답변 완료!</span>`;
            car.style.backgroundColor = '#6c757d';
        }

        setTimeout(() => {
            nextQuestion();
        }, 1500);
    }

    function nextQuestion() {
        clearInterval(carMoveInterval);
        currentQuestionIndex++;
        loadQuestion();
    }

    function restartGame() {
        currentQuestionIndex = 0;
        score = 0;
        mcCorrectCount = 0;
        mcIncorrectCount = 0;
        userDescriptiveAnswers = [];
        optionsDisabled = false;
        finalResultDiv.style.display = "none";
        questionBox.style.display = "block";
        optionsContainer.style.display = "flex";
        car.style.display = "flex";
        scoreDisplay.style.display = "block";
        feedback.style.display = "block";
        scoreDisplay.innerText = "점수: 0";
        loadQuestion();
    }

    function showResult() {
        document.getElementById("gameArea").style.background = "#eef";
        questionBox.style.display = "none";
        optionsContainer.style.display = "none";
        car.style.display = "none";
        nextBtn.style.display = "none";
        feedback.style.display = "none";
        scoreDisplay.style.display = "none";

        finalResultDiv.style.display = "block";
        totalQuestionsSpan.innerText = questions.filter(q => q.type === "multiple-choice").length;
        correctAnswersSpan.innerText = mcCorrectCount;
        incorrectAnswersSpan.innerText = mcIncorrectCount;
        finalScoreSpan.innerText = score;

        descriptiveAnswersDiv.innerHTML = '<h3>주관식 문제 & 정답 예시</h3>';
        let hasDescriptive = false;
        questions.forEach((qData, index) => {
            if (qData.type.startsWith("descriptive")) {
                hasDescriptive = true;
                const p = document.createElement("p");
                const storedAnswer = userDescriptiveAnswers[index];
                const userAnswerText = storedAnswer ? storedAnswer.userAnswer : "답변 없음";
                const correctAnswerText = qData.a;
                const isCorrectForDisplay = (qData.type === "descriptive-unscored") ? (userAnswerText !== "답변 없음") : (storedAnswer ? storedAnswer.isCorrect : false);

                let feedbackHtml = '';
                if (qData.type === "descriptive-scored") {
                    feedbackHtml = `<br><span class="result-feedback" style="color: ${isCorrectForDisplay ? 'green' : 'red'};">${isCorrectForDisplay ? '✅ 정답' : '❌ 오답'}</span>`;
                }

                p.innerHTML = `<strong>문제 ${index + 1}:</strong> ${qData.q.split('\n')[0].trim()}<br>` +
                                `<span class="user-answer"><strong>내 답변:</strong> ${userAnswerText}</span><br>` +
                                `<span><strong>정답 예시:</strong> ${correctAnswerText}</span>` +
                                feedbackHtml;
                descriptiveAnswersDiv.appendChild(p);
            }
        });

        if (!hasDescriptive) {
            descriptiveAnswersDiv.innerHTML += "<p>주관식 문제가 없습니다.</p>";
        }
    }

    window.onload = loadQuestion;
</script>

</body>
</html>
