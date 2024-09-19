<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Баланс с регистрацией и входом</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        button, input {
            padding: 10px 20px;
            font-size: 16px;
            margin-top: 10px;
        }
        #bonusMessage {
            color: red;
            margin-top: 20px;
        }
        #authSection, #gameSection {
            margin-top: 20px;
        }
        #gameSection {
            display: none;
        }
    </style>
</head>
<body>
    <div id="authSection">
        <h2>Регистрация</h2>
        <input type="text" id="regLogin" placeholder="Логин" required>
        <input type="email" id="regEmail" placeholder="Почта" required>
        <input type="password" id="regPassword" placeholder="Пароль" required>
        <button id="registerButton">Зарегистрироваться</button>

        <h2>Вход</h2>
        <input type="text" id="loginEmail" placeholder="Логин/Почта" required>
        <input type="password" id="loginPassword" placeholder="Пароль" required>
        <button id="loginButton">Войти</button>
        
        <p id="authMessage" style="color: red;"></p>
    </div>

    <div id="gameSection">
        <h1>Ваш баланс: <span id="balance">0</span></h1>
        <h2>Клики за раз: <span id="clickValue">1</span></h2>
        <button id="increaseButton">+ к балансу</button>
        <button id="upgradeButton">Купить апгрейд (<span id="upgradeCost">5</span> монет, +2 клика)</button>
        <br><br>
        <button id="bonusButton">Получить ежедневный бонус</button>
        <p id="bonusMessage"></p>
        <button id="logoutButton">Выйти</button>
    </div>

    <script>
        // Проверка текущего пользователя
        let currentUser = localStorage.getItem('currentUser');

        if (currentUser) {
            showGameSection(); // Если пользователь залогинен, показываем игровую секцию
        }

        // Функция показа игрового раздела
        function showGameSection() {
            document.getElementById('authSection').style.display = 'none';
            document.getElementById('gameSection').style.display = 'block';
            loadGameData();
        }

        // Функция загрузки данных игры
        function loadGameData() {
            currentUser = JSON.parse(localStorage.getItem(localStorage.getItem('currentUser')));
            document.getElementById('balance').textContent = currentUser.balance;
            document.getElementById('clickValue').textContent = currentUser.clickValue;
            document.getElementById('upgradeCost').textContent = currentUser.upgradeCost;
        }

        // Функция сохранения данных игры
        function saveGameData() {
            localStorage.setItem(currentUser.email, JSON.stringify(currentUser));
        }

        // Регистрация пользователя
        document.getElementById('registerButton').addEventListener('click', function() {
            let login = document.getElementById('regLogin').value;
            let email = document.getElementById('regEmail').value;
            let password = document.getElementById('regPassword').value;

            if (login && email && password) {
                // Проверка наличия пользователя
                if (localStorage.getItem(email)) {
                    document.getElementById('authMessage').textContent = 'Пользователь с такой почтой уже существует!';
                } else {
                    let user = {
                        login: login,
                        email: email,
                        password: password,
                        balance: 0,
                        clickValue: 1,
                        upgradeCost: 5,
                        lastBonusTime: 0
                    };
                    localStorage.setItem(email, JSON.stringify(user));
                    document.getElementById('authMessage').textContent = 'Регистрация успешна! Войдите в систему.';
                }
            } else {
                document.getElementById('authMessage').textContent = 'Заполните все поля!';
            }
        });

        // Вход пользователя
        document.getElementById('loginButton').addEventListener('click', function() {
            let loginEmail = document.getElementById('loginEmail').value;
            let password = document.getElementById('loginPassword').value;

            if (loginEmail && password) {
                // Проверяем пользователя по email или логину
                let user = JSON.parse(localStorage.getItem(loginEmail)) || null;
                if (!user) {
                    // Поиск по логину
                    for (let key in localStorage) {
                        if (localStorage.hasOwnProperty(key)) {
                            let u = JSON.parse(localStorage.getItem(key));
                            if (u && (u.login === loginEmail || u.email === loginEmail)) {
                                user = u;
                                break;
                            }
                        }
                    }
                }

                if (user && user.password === password) {
                    localStorage.setItem('currentUser', user.email);
                    currentUser = user;
                    showGameSection(); // Показать игровой раздел
                } else {
                    document.getElementById('authMessage').textContent = 'Неверный логин или пароль!';
                }
            } else {
                document.getElementById('authMessage').textContent = 'Заполните все поля!';
            }
        });

        // Увеличение баланса
        document.getElementById('increaseButton').addEventListener('click', function() {
            currentUser.balance += currentUser.clickValue;
            document.getElementById('balance').textContent = currentUser.balance;
            saveGameData();
        });

        // Покупка апгрейда
        document.getElementById('upgradeButton').addEventListener('click', function() {
            if (currentUser.balance >= currentUser.upgradeCost) {
                currentUser.balance -= currentUser.upgradeCost;
                currentUser.clickValue += 2;
                currentUser.upgradeCost = Math.ceil(currentUser.upgradeCost * 1.5);

                document.getElementById('balance').textContent = currentUser.balance;
                document.getElementById('clickValue').textContent = currentUser.clickValue;
                document.getElementById('upgradeCost').textContent = currentUser.upgradeCost;

                saveGameData();
            } else {
                alert('Недостаточно средств для апгрейда!');
            }
        });

        // Получение ежедневного бонуса
        document.getElementById('bonusButton').addEventListener('click', function() {
            let currentTime = new Date().getTime();
            if (currentTime - currentUser.lastBonusTime >= 24 * 60 * 60 * 1000) {
                let bonus = Math.floor(Math.random() * 50000) + 1; // Случайный бонус от 1 до 50 000
                currentUser.balance += bonus;
                currentUser.lastBonusTime = currentTime;
                document.getElementById('balance').textContent = currentUser.balance;
                document.getElementById('bonusMessage').textContent = `Вы получили бонус: ${bonus} монет!`;
                saveGameData();
            } else {
                let timeLeft = 24 * 60 * 60 * 1000 - (currentTime - currentUser.lastBonusTime);
                let hoursLeft = Math.floor(timeLeft / (60 * 60 * 1000));
                document.getElementById('bonusMessage').textContent = `Бонус доступен через ${hoursLeft} часов.`;
            }
        });

        // Выход из системы
        document.getElementById('logoutButton').addEventListener('click', function() {
            localStorage.removeItem('currentUser');
            document.getElementById('authSection').style.display = 'block';
            document.getElementById('gameSection').style.display = 'none';
        });
    </script>
</body>
</html>
