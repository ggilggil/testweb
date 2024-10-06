# testweb
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 색 변경</title>
    <style>
        body {
            transition: background-color 1s;
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;

        document.getElementById('rainbowButton').addEventListener('click', () => {
            document.body.style.backgroundColor = colors[currentIndex];
            currentIndex = (currentIndex + 1) % colors.length;
        });
    </script>
</body>
</html>
