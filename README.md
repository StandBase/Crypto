<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Баланс</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
            background: linear-gradient(to right, #ff7e5f, #feb47b); /* Градиент фона */
            color: white; /* Цвет текста */
        }
        #balance {
            font-size: 2em;
            margin-bottom: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 1em;
            margin: 10px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            transition: background 0.3s;
        }
        #upgrade {
            background: linear-gradient(to right, #4caf50, #81c784); /* Градиент кнопки апгрейда */
            color: white;
        }
        #add-money {
            background: linear-gradient(to right, #007bff, #5cb85c); /* Градиент кнопки добавления */
            color: white;
        }
        #daily-bonus {
            background: linear-gradient(to right, #ff9800, #ffc107); /* Градиент кнопки бонуса */
            color: white;
        }
        #withdraw {
            background: linear-gradient(to right, #f44336, #e57373); /* Градиент кнопки вывода */
            color: white;
        }
        button:hover {
            opacity: 0.9; /* Эффект наведения */
        }
        #click-value {
            font-size: 1.5em;
            margin-top: 20px;
        }
        #notification {
            display: none;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 5px;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <div id="balance">Баланс: $0</div>
    <button id="add-money">Клик</button>
    <button id="upgrade">Апгрейд ($5)</button>
    <button id="daily-bonus">Получить ежедневный бонус</button>
    <button id="withdraw">Вывод средств</button>
    <div id="click-value">Каждый клик дает: $1</div>
    <div id="notification"></div>

    <script>
        let balance = parseInt(localStorage.getItem('balance')) || 0; // Загрузка баланса из локального хранилища
        let clickValue = parseInt(localStorage.getItem('clickValue')) || 1; // Загрузка значения клика из локального хранилища
        let upgradeCost = parseInt(localStorage.getItem('upgradeCost')) || 5; // Загрузка стоимости апгрейда
        let lastBonusDate = localStorage.getItem('lastBonusDate') || null; // Дата последнего получения бонуса

        // Обновляем текстовые значения на странице
        updateBalance();
        updateClickValue();
        updateUpgradeButton();

        document.getElementById('add-money').addEventListener('click', function() {
            balance += clickValue; // Увеличиваем баланс на значение клика
            updateBalance();
        });

        document.getElementById('upgrade').addEventListener('click', function() {
            if (balance >= upgradeCost) { // Проверяем, достаточно ли средств
                balance -= upgradeCost; // Уменьшаем баланс на стоимость апгрейда
                clickValue++; // Увеличиваем значение клика
                upgradeCost = Math.round(upgradeCost * 1.5); // Увеличиваем стоимость апгрейда на 50%
                updateBalance();
                updateUpgradeButton(); // Обновляем текст кнопки апгрейда
                updateClickValue(); // Обновляем текст о значении клика
                showNotification('Апгрейд успешен! Теперь каждый клик дает $' + clickValue);
            } else {
                showNotification('Недостаточно средств для апгрейда!', true);
            }
        });

        document.getElementById('daily-bonus').addEventListener('click', function() {
            claimDailyBonus();
        });

        document.getElementById('withdraw').addEventListener('click', function() {
            showNotification('Функция временно недоступна', true);
        });

        // Функция для получения ежедневного бонуса
        function claimDailyBonus() {
            const today = new Date().toDateString();
            if (lastBonusDate !== today) {
                let bonusChance = Math.random();
                if (bonusChance < 0.10) { // 10% шанс на 90к
                    balance += 90000;
                    showNotification('Поздравляем! Вы получили $90,000 в качестве бонуса!');
                } else if (bonusChance < 0.21) { // 11% шанс на 80к
                    balance += 80000;
                    showNotification('Поздравляем! Вы получили $80,000 в качестве бонуса!');
                } else if (bonusChance < 0.34) { // 13% шанс на 70к
                    balance += 70000;
                    showNotification('Поздравляем! Вы получили $70,000 в качестве бонуса!');
                } else if (bonusChance < 0.50) { // 16% шанс на 60к
                    balance += 60000;
                    showNotification('Поздравляем! Вы получили $60,000 в качестве бонуса!');
                } else if (bonusChance < 0.70) { // 20% шанс на 50к
                    balance += 50000;
                    showNotification('Поздравляем! Вы получили $50,000 в качестве бонуса!');
                } else if (bonusChance < 0.85) { // 15% шанс на 30к
                    balance += 30000;
                    showNotification('Поздравляем! Вы получили $30,000 в качестве бонуса!');
                } else { // 15% шанс на 10к
                    balance += 10000;
                    showNotification('Поздравляем! Вы получили $10,000 в качестве бонуса!');
                }
                lastBonusDate = today; // Обновляем дату последнего бонуса
                updateBalance();
                localStorage.setItem('lastBonusDate', lastBonusDate); // Сохраняем дату в локальном хранилище
            } else {
                showNotification('Вы уже получили свой ежедневный бонус!', true);
            }
        }

        // Функция для обновления отображения баланса
        function updateBalance() {
            document.getElementById('balance').innerText = 'Баланс: $' + balance;
            localStorage.setItem('balance', balance); // Сохранение баланса в локальном хранилище
            localStorage.setItem('clickValue', clickValue); // Сохранение значения клика в локальном хранилище
            localStorage.setItem('upgradeCost', upgradeCost); // Сохранение стоимости апгрейда в локальном хранилище
        }

        // Функция для обновления кнопки апгрейда
        function updateUpgradeButton() {
            document.getElementById('upgrade').innerText = 'Апгрейд ($' + Math.round(upgradeCost) + ')';
        }

        // Функция для обновления значения клика
        function updateClickValue() {
            document.getElementById('click-value').innerText = 'Каждый клик дает: $' + clickValue;
        }

        // Функция для отображения уведомлений
        function showNotification(message, isError = false) {
            const notification = document.getElementById('notification');
            notification.innerText = message;
            notification.style.display = 'block';
            notification.style.color = isError ? 'red' : 'white';
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3000);
        }
    </script>

</body>
</html>
