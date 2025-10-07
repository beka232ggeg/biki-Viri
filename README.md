<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Немецкий квиз | Улучшенный дизайн</title>
    <style>
        /* Общий фон и шрифт */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #1f4068 0%, #162447 100%); /* Градиентный фон */
            color: #f0f0f0;
            margin: 0;
            padding: 20px;
        }

        /* Контейнер игры */
        .game-container {
            background-color: #2a3a5a; /* Более мягкий темный фон */
            padding: 40px;
            border-radius: 15px; /* Скругленные углы */
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5); /* Глубокая тень */
            text-align: center;
            width: 90%;
            max-width: 550px;
            border: 1px solid #3c5478; /* Легкая рамка */
        }

        /* Заголовок */
        h1 {
            color: #ffc947; /* Яркий акцентный цвет */
            margin-bottom: 25px;
            font-weight: 700;
            text-transform: uppercase;
        }

        /* Подсказка */
        #word-prompt {
            font-size: 1.1em;
            font-weight: 500;
            color: #90b8f8; /* Цвет для текста-подсказки */
            margin-bottom: 5px;
        }

        /* Отображение русского слова */
        #german-word-display {
            font-size: 2.2em;
            font-weight: 900;
            margin-bottom: 30px;
            padding: 15px 20px;
            background-color: #1e283d; /* Фон для слова */
            border-radius: 10px;
            color: #ffffff;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.3);
        }

        /* Поле ввода */
        #user-input {
            width: 80%;
            padding: 12px;
            font-size: 1.1em;
            border: 2px solid #5a7092;
            border-radius: 8px;
            margin-bottom: 20px;
            background-color: #fdfdfd;
            color: #1f4068;
            text-align: center;
            transition: border-color 0.3s, box-shadow 0.3s;
        }
        #user-input:focus {
            border-color: #ffc947;
            box-shadow: 0 0 8px rgba(255, 201, 71, 0.5);
            outline: none;
        }

        /* Кнопки */
        button {
            padding: 12px 25px;
            font-size: 1.05em;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.1s;
            display: block;
            width: 85%;
            margin: 15px auto;
            font-weight: bold;
        }
        button:active {
            transform: scale(0.98);
        }

        #check-button {
            background-color: #34d075; /* Ярко-зеленый */
            color: #162447;
        }
        #check-button:hover {
            background-color: #27ae60;
        }

        #next-button {
            background-color: #4da6ff; /* Голубой */
            color: white;
        }
        #next-button:hover {
            background-color: #3a8ee6;
        }

        /* Сообщение о результате */
        #result-message {
            font-weight: bold;
            margin-top: 20px;
            min-height: 25px;
            font-size: 1.1em;
        }
    </style>
</head>
<body>

    <div class="game-container">
        <h1>Немецкий квиз</h1>
        <p id="word-prompt">Напишите немецкое слово для:</p>
        <p id="german-word-display">Загрузка...</p>
        <input type="text" id="user-input" placeholder="Введите немецкое слово">
        
        <button id="check-button">Проверить</button>
        
        <p id="result-message"></p>
        
        <button id="next-button" style="display:none;">Следующее слово</button>
    </div>

    <script>
        const words = [
            { german: "Achtung", russian: "внимание" },
            { german: "Bewerbung", russian: "заявление" },
            { german: "Herbst", russian: "осень" },
            { german: "Sommer", russian: "лето" },
            { german: "Frühling", russian: "весна" },
            { german: "Winter", russian: "зима" },
            { german: "Heizung", russian: "отопление" },
            { german: "Junge", russian: "мальчик" },
            { german: "Kündigung", russian: "увольнение" },
            { german: "Banane", russian: "банан" },
            { german: "Drucker", russian: "принтер" },
            { german: "Tante", russian: "тётя" },
            { german: "Aufgabe", russian: "задание" },
            { german: "Grün", russian: "зелёный" },
            { german: "antworten", russian: "отвечать" },
            { german: "Antwort", russian: "ответ" },
            { german: "fragen", russian: "спрашивать" },
            { german: "Frage", russian: "вопрос" },
            { german: "Flasche", russian: "бутылка, банка" },
            { german: "Briefpapier", russian: "конверт, бумага для письма" },
            { german: "Bleistift", russian: "карандаш" },
            { german: "Kugelschreiber", russian: "ручка" },
            { german: "Zahn", russian: "зуб" },
            { german: "Zahnpasta", russian: "зубная паста" },
            { german: "Seife", russian: "мыло" },
            { german: "Streichhölzer", russian: "спички" },
            { german: "wünschen", russian: "желать" },
            { german: "verkaufen", russian: "продавать" },
            { german: "kosten", russian: "стоить" },
            { german: "bekommen", russian: "получать" },
            { german: "kriegen", russian: "получать" },
            { german: "alle", russian: "все" },
            { german: "alles", russian: "всё" },
            { german: "wirklich", russian: "действительно" },
            { german: "preiswert", russian: "дёшево" },
            { german: "dann", russian: "потом" },
            { german: "kostenlos", russian: "бесплатно" },
            { german: "Entschuldigung", russian: "извините" },
            { german: "Freiheit", russian: "свобода" },
            { german: "Bäckerei", russian: "пекарня" },
            { german: "Büchlein", russian: "библиотека / книжечка" },
            { german: "einladen", russian: "приглашать" },
            { german: "langweilig", russian: "скучный" },
            { german: "Drogerie", russian: "аптечный магазин / косметика" },
            { german: "Apotheke", russian: "аптека" },
            { german: "Ganz", russian: "совсем / полностью" },
            { german: "Verstehen", russian: "понимать" },
            { german: "Rückfahrt", russian: "обратная поездка" },
            { german: "anmelden", russian: "регистрироваться" }
        ];

        let currentWordIndex = 0;
        let score = 0;

        const wordDisplay = document.getElementById("german-word-display");
        const userInput = document.getElementById("user-input");
        const checkButton = document.getElementById("check-button");
        const nextButton = document.getElementById("next-button");
        const resultMessage = document.getElementById("result-message");

        function displayWord() {
            // Выбираем случайное слово
            currentWordIndex = Math.floor(Math.random() * words.length);
            const word = words[currentWordIndex];
            
            // Показываем только русский перевод
            wordDisplay.textContent = word.russian;
            
            // Сбрасываем состояние
            nextButton.style.display = "none";
            checkButton.style.display = "block";
            userInput.value = "";
            resultMessage.textContent = "";
            userInput.disabled = false;
            userInput.focus();
        }

        function checkAnswer() {
            // Если кнопка "Проверить" не активна, ничего не делаем
            if (checkButton.style.display === "none") return;
            
            const userAnswer = userInput.value.trim().toLowerCase();
            const correctAnswer = words[currentWordIndex].german.toLowerCase();

            if (userAnswer === correctAnswer) {
                resultMessage.innerHTML = `<span style="color:#34d075;">✅ Верно! Слово: <b>${words[currentWordIndex].german}</b></span>`;
                score++;
            } else {
                resultMessage.innerHTML = `<span style="color:#ff6b6b;">❌ Неверно. Правильное слово: <b>${words[currentWordIndex].german}</b></span>`;
            }
            
            // Блокируем ввод и показываем кнопку "Следующее слово"
            userInput.disabled = true;
            checkButton.style.display = "none";
            nextButton.style.display = "block";
        }

        checkButton.addEventListener("click", checkAnswer);
        nextButton.addEventListener("click", displayWord);

        // Улучшаем обработку Enter: Enter проверяет, Enter переходит к следующему
        userInput.addEventListener("keyup", function(event) {
            if (event.key === "Enter") {
                if (checkButton.style.display === "block") {
                    checkAnswer();
                } else if (nextButton.style.display === "block") {
                    displayWord();
                }
            }
        });

        // Запускаем игру
        displayWord();
    </script>
</body>
</html>****
