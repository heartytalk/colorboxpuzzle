<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>컬러 박스 퍼즐</title>
    <style>
        .container { text-align: center; padding: 20px; display: flex; flex-direction: column; align-items: center; }
        .box-container { display: flex; justify-content: center; gap: 2vw; margin-bottom: 2vw; }
        .box { width: 10vw; height: 10vw; max-width: 100px; max-height: 100px; border: 1px solid #000; }
        .color-buttons { display: flex; justify-content: center; gap: 2vw; margin-bottom: 2vw; flex-wrap: wrap; }
        .color-button { width: 10vw; height: 10vw; max-width: 100px; max-height: 100px; border: none; cursor: pointer; }
        .action-buttons { margin-top: 2vw; display: flex; flex-direction: column; gap: 1vw; align-items: center; }
        .footer { margin-top: 4vw; font-size: 1rem; text-align: center; }
        h1 { text-align: center; width: 100%; }
        @media (max-width: 600px) {
            .box { width: 15vw; height: 15vw; }
            .color-button { width: 15vw; height: 15vw; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>컬러 박스 퍼즐</h1>
        <div id="targetPattern" class="box-container"></div>
        <div id="userPattern" class="box-container"></div>
        <div id="colorButtons" class="color-buttons"></div>
        <div class="action-buttons">
            <button onclick="undoLastMove()">취소하기</button>
            <button onclick="checkSolution()">확인하기</button>
        </div>
        <div class="footer">
            하트선생님이 언어치료 수업을 위해 만들었어요.<br>
            @heartytalk_slp <br>
            <a href="mailto:tjdah0420@naver.com">tjdah0420@naver.com</a><br>
            <a href="https://blog.naver.com/mindcarelog" target="_blank">https://blog.naver.com/mindcarelog</a>
        </div>
    </div>
    <script>
        const COLORS = ["red", "orange", "yellow", "blue", "purple", "pink", "black"];
        const LEVELS = [2, 3, 4, 5, 6, 7];
        let level = 0;
        let targetPattern = [];
        let userPattern = [];
        
        function getRandomPattern(size) {
            return Array.from({ length: size }, () => COLORS[Math.floor(Math.random() * COLORS.length)]);
        }
        
        function loadLevel() {
            targetPattern = getRandomPattern(LEVELS[level]);
            userPattern = [];
            renderPattern(targetPattern, "targetPattern");
            renderPattern(userPattern, "userPattern");
        }
        
        function renderPattern(pattern, elementId) {
            const container = document.getElementById(elementId);
            container.innerHTML = "";
            pattern.forEach(color => {
                const div = document.createElement("div");
                div.className = "box";
                div.style.backgroundColor = color;
                container.appendChild(div);
            });
        }
        
        function handleColorClick(color) {
            if (userPattern.length < targetPattern.length) {
                userPattern.push(color);
                renderPattern(userPattern, "userPattern");
            }
        }
        
        function undoLastMove() {
            userPattern.pop();
            renderPattern(userPattern, "userPattern");
        }
        
        function checkSolution() {
            if (JSON.stringify(userPattern) === JSON.stringify(targetPattern)) {
                alert("성공! 다음 레벨로 갑니다.");
                if (level < LEVELS.length - 1) {
                    level++;
                    loadLevel();
                } else {
                    alert("모든 레벨을 클리어했습니다!");
                }
            } else {
                alert("틀렸습니다. 다시 시도하세요!");
            }
        }
        
        function createColorButtons() {
            const container = document.getElementById("colorButtons");
            COLORS.forEach(color => {
                const button = document.createElement("button");
                button.className = "color-button";
                button.style.backgroundColor = color;
                button.onclick = () => handleColorClick(color);
                container.appendChild(button);
            });
        }
        
        createColorButtons();
        loadLevel();
    </script>
</body>
</html>
