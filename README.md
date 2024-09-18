<html lang="ru">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Счетчик баланса</title>  
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

<h1>Баланс: <span id="balance">0</span></h1>  
<button onclick="increaseBalance()">+1 к балансу</button>  

<script>  
    let balance = 0;  

    function increaseBalance() {  
        balance++;  
        document.getElementById('balance').innerText = balance;  
    }  
</script>  

</body>  
</html>
