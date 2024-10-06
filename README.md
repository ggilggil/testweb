# testweb
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 빤짝</title>
    <style>
        body {
            transition: background-color 0.5s;
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        const button = document.getElementById('rainbowButton');

        button.addEventListener('click', () => {
            let duration = 3000; // 3초
            let interval = 200; // 0.2초마다 색 변경
            let currentIndex = 0;
            let totalIterations = duration / interval;

            const changeColor = () => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            };

            const colorInterval = setInterval(changeColor, interval);

            setTimeout(() => {
                clearInterval(colorInterval);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
            }, duration);
        });
    </script>
</body>
</html>
