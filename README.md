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
            transition: background-color 0.2s; /* 색 변경 시 애니메이션 효과 */
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
    <audio id="backgroundMusic" src="sound.mp3" preload="auto"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;
        let intervalId;

        document.getElementById('rainbowButton').addEventListener('click', () => {
            // 초기화
            clearInterval(intervalId);
            currentIndex = 0;

            // 음악 재생
            const music = document.getElementById('backgroundMusic');
            music.currentTime = 0; // 음악 시작 부분으로 이동
            music.play(); // 음악 재생

            // 15초 동안 색 변경
            intervalId = setInterval(() => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            }, 200); // 0.2초마다 색 변경

            // 15초 후에 색 변경 중지 및 음악 정지
            setTimeout(() => {
                clearInterval(intervalId);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                music.pause(); // 음악 정지
                music.currentTime = 0; // 음악 시작 부분으로 이동
            }, 15000); // 15초
        });
    </script>
</body>
</html>
