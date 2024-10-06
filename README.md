<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 빤짝</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            position: relative;
            overflow: hidden;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>

    <script>
        const colors = [
            'red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'
        ];

        const button = document.getElementById('rainbowButton');

        button.addEventListener('click', () => {
            let duration = 3000; // 3초
            let interval = 200; // 0.2초마다 색 변경
            let currentIndex = 0;
            let steps = duration / interval; // 총 단계 수

            const changeColor = () => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;

                if (currentIndex < steps) {
                    setTimeout(changeColor, interval);
                } else {
                    // 무지개 효과가 끝난 후 원래 색으로 복구
                    document.body.style.backgroundColor = '';
                }
            };

            changeColor(); // 색 변경 시작
        });
    </script>
</body>
</html>
