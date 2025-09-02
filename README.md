# sss
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>러시안 룰렛 게임</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
            background-color: #f4f4f4;
        }

        .game-container {
            margin: 20px;
        }

        .button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
        }

        .button:disabled {
            background-color: #ddd;
        }

        .status {
            margin-top: 20px;
            font-size: 18px;
        }

        .player-list {
            margin-top: 10px;
            list-style-type: none;
            padding: 0;
        }

        .player-list li {
            font-size: 20px;
        }
    </style>
</head>
<body>

    <h1>러시안 룰렛 게임</h1>
    <div class="game-container">
        <button class="button" onclick="startGame()">게임 시작</button>
        <div class="status"></div>
        <ul class="player-list"></ul>
        <button class="button" id="shootButton" onclick="shoot()" disabled>방아쇠 당기기</button>
    </div>

    <script>
        let players = [];
        let currentPlayerIndex = 0;
        let isGameActive = false;
        let chamber = [];

        function startGame() {
            // 플레이어 이름을 입력받고, 2명 이상의 플레이어가 있어야 시작
            const playerCount = prompt("몇 명이 게임에 참여할까요? (최소 2명 이상)");
            if (playerCount < 2) {
                alert("최소 2명 이상이어야 합니다.");
                return;
            }

            players = [];
            for (let i = 0; i < playerCount; i++) {
                players.push("플레이어 " + (i + 1));
            }
            chamber = [0, 0, 0, 0, 0, 1];  // 6발 중 하나에 총알이 있음 (1 = 총알)
            chamber = chamber.sort(() => Math.random() - 0.5); // 총알 위치 랜덤화

            currentPlayerIndex = 0;
            isGameActive = true;

            document.querySelector(".player-list").innerHTML = players.map(player => `<li>${player} : 살아있음</li>`).join('');
            document.querySelector(".status").innerText = `${players[currentPlayerIndex]}의 차례입니다!`;
            document.getElementById("shootButton").disabled = false;
        }

        function shoot() {
            if (!isGameActive) return;

            // 현재 플레이어의 차례에 방아쇠를 당김
            const currentPlayer = players[currentPlayerIndex];
            const shotResult = chamber.pop();  // 총알이 있는지 확인 (가장 뒤에서 하나씩 빼기)

            if (shotResult === 1) {
                document.querySelector(".status").innerText = `${currentPlayer}가 총알에 맞았습니다! 게임 종료!`;
                isGameActive = false;
                document.getElementById("shootButton").disabled = true;
            } else {
                players[currentPlayerIndex] = `${currentPlayer} : 살아있음`;
                document.querySelector(".player-list").children[currentPlayerIndex].innerText = `${currentPlayer} : 살아있음`;
                document.querySelector(".status").innerText = `${currentPlayer}가 살아남았습니다! 다음 차례!`;
                
                // 다음 플레이어로 차례 이동
                currentPlayerIndex = (currentPlayerIndex + 1) % players.length;
            }

            if (chamber.length === 0) {
                document.querySelector(".status").innerText = "모든 총알이 사용되었습니다. 게임 종료!";
                document.getElementById("shootButton").disabled = true;
            }
        }
    </script>

</body>
</html>
