<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Поиск Киблы з Компасом</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
        }
        h1 {
            margin-top: 20px;
        }
        #compass {
            margin-top: 50px;
            width: 300px;
            height: 300px;
            border-radius: 50%;
            background-color: #e6f2f7;
            position: relative;
            border: 2px solid #007bff;
        }
        #needle {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 2px;
            height: 100px;
            background-color: red;
            transform-origin: bottom center;
            transform: translate(-50%, -100%);
            transition: transform 0.5s ease;
        }
        #north, #south, #east, #west {
            position: absolute;
            font-weight: bold;
            color: #007bff;
        }
        #north {
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
        }
        #south {
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
        }
        #east {
            top: 50%;
            right: 10px;
            transform: translateY(-50%);
        }
        #west {
            top: 50%;
            left: 10px;
            transform: translateY(-50%);
        }
        #status {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>

    <h1>Поиск Киблы (Направление на Мекку)</h1>
    <p>Нажми на кнопку для нахаждения направления на Киблу.</p>

    <button onclick="findQibla()">Найди Киблу</button>

    <div id="compass">
        <div id="needle"></div>
        <div id="north">Севир</div>
        <div id="south">Юг</div>
        <div id="east">Васток</div>
        <div id="west">Запод</div>
    </div>

    <p id="status">Ожидание местоположения...</p>

    <script>
        // Координаты Каабы в Мекке
        const kaabaLat = 21.4225;   // Широта Каабы
        const kaabaLon = 39.8262;   // Долгота Каабы

        function findQibla() {
            if (navigator.geolocation) {
                // Получаем текущие координаты пользователя
                navigator.geolocation.getCurrentPosition(function(position) {
                    const userLat = position.coords.latitude;
                    const userLon = position.coords.longitude;
                    calculateQiblaDirection(userLat, userLon);
                }, function() {
                    document.getElementById('status').innerText = 'Ошибка получения местоположения.';
                });
            } else {
                document.getElementById('status').innerText = 'Ваш браузер не поддерживает геолокацию.';
            }
        }

        function calculateQiblaDirection(userLat, userLon) {
            // Рассчитываем азимут (угол) на Каабу
            const deltaLon = kaabaLon - userLon;
            const x = Math.cos(kaabaLat * Math.PI / 180) * Math.sin(deltaLon * Math.PI / 180);
            const y = Math.cos(userLat * Math.PI / 180) * Math.sin(kaabaLat * Math.PI / 180) - Math.sin(userLat * Math.PI / 180) * Math.cos(kaabaLat * Math.PI / 180) * Math.cos(deltaLon * Math.PI / 180);

            // Азимут в градусах
            const qiblaAngle = Math.atan2(x, y) * 180 / Math.PI;

            // Преобразуем угол так, чтобы компас указывал в нужное направление
            const needle = document.getElementById('needle');
            needle.style.transform = `translate(-50%, -100%) rotate(${qiblaAngle}deg)`;

            // Обновляем статус
            document.getElementById('status').innerText = `Кибла: ${qiblaAngle.toFixed(2)}° от севера`;
        }
    </script>

</body>
</html>
