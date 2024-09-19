<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Баланс</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <h1>Ваш баланс: <span id="balance">0</span></h1>
    <h2>Клики за раз: <span id="clickValue">1</span></h2>
    <button id="increaseButton">+ к балансу</button>
    <button id="upgradeButton">Купить апгрейд (<span id="upgradeCost">5</span> монет, +2 клика)</button>

    <script>
        // Загрузка данных из localStorage
        let balance = localStorage.getItem('balance') ? parseInt(localStorage.getItem('balance')) : 0;
        let clickValue = 1; // Значение клика
        let upgradeCost = localStorage.getItem('upgradeCost') ? parseInt(localStorage.getItem('upgradeCost')) : 5; // Стоимость апгрейда

        // Отображение начальных значений
        document.getElementById('balance').textContent = balance;
        document.getElementById('clickValue').textContent = clickValue;
        document.getElementById('upgradeCost').textContent = upgradeCost;

        // Обработчик для кнопки увеличения баланса
        document.getElementById('increaseButton').addEventListener('click', function() {
            balance += clickValue; // Увеличиваем баланс
            document.getElementById('balance').textContent = balance; // Обновляем отображаемый баланс
            localStorage.setItem('balance', balance); // Сохраняем новый баланс в localStorage
        });

        // Обработчик для кнопки апгрейда
        document.getElementById('upgradeButton').addEventListener('click', function() {
            if (balance >= upgradeCost) { // Проверка, достаточно ли баланса для апгрейда
                balance -= upgradeCost; // Снимаем стоимость апгрейда с баланса
                clickValue += 2; // Увеличиваем значение клика
                upgradeCost = Math.ceil(upgradeCost * 1.5); // Увеличиваем стоимость апгрейда на 50%
                
                // Обновляем отображаемые значения
                document.getElementById('balance').textContent = balance;
                document.getElementById('clickValue').textContent = clickValue;
                document.getElementById('upgradeCost').textContent = upgradeCost;

                // Сохраняем данные в localStorage
                localStorage.setItem('balance', balance);
                localStorage.setItem('upgradeCost', upgradeCost);
            } else {
                alert('Недостаточно средств для апгрейда!');
            }
        });
    </script>
</body>
</html>
