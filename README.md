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
    <button id="increaseButton">+1 к балансу</button>  

    <script>  
        // Загрузка баланса из localStorage  
        let balance = localStorage.getItem('balance') ? parseInt(localStorage.getItem('balance')) : 0;  
        document.getElementById('balance').textContent = balance;  

        // Обработчик для кнопки  
        document.getElementById('increaseButton').addEventListener('click', function() {  
            balance += 1; // Увеличиваем баланс  
            document.getElementById('balance').textContent = balance; // Обновляем отображаемый баланс  
            localStorage.setItem('balance', balance); // Сохраняем новый баланс в localStorage  
        });  
    </script>  
</body>  
</html>
