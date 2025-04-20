<!DOCTYPE html>
<html>
<head>
    <style>
        body {
            background: #1a1a1a;
            display: flex;
            flex-direction: column;
            align-items: center;
            color: white;
            font-family: wiguru;
        }
        
        .title {
            font-weight: bold;
            color: white;
            font-size: 35px;
            margin: 25px;
            color: white
        }
        
        .grid {
            display: grid;
            grid-template-columns: repeat(5, 50px);
            gap: 10px;
            background: #2d2d2d;
            padding: 20px;
            border-radius: 20px;
        }
        
        .cell {
            width: 50px;
            height: 50px;
            background: #3a3a3a;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            position: relative;
            transition: 0.5s;
        }
        
        .mine::after {
            content: "⭐";
            color: gold;
            font-size: 29px;
            animation: appear 0.5s ease;
        }
        
        .mode-selector {
            margin: 25px;
            display: flex;
            gap: 30px;
        }
        
        .mode-btn {
            padding: 8px 15px;
            background: #2d2d2d;
            border: none;
            color: white;
            border-radius: 8px;
            cursor: pointer;
        }
        
        .mode-btn.active {
            background:#a028b0;
            box-shadow: 0 0 10px gold;
        }
        
        .action-btns {
            display: flex;
            gap: 40px;
            margin: 30px;
        }
        
        .action-btn {
            padding: 21px 26px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        
        #getSignal {
            background: #007bff;
            color: white;
        }
        
        #exit {
            background:#000000;
            color: white;
        }
        
        @keyframes appear {
            from { opacity: 0; transform: scale(0.5); }
            to { opacity: 1; transform: scale(1); }
        }
        
        .red-bg {
            background: #ff4444 !important;
        }
    </style>
</head>
<body>
    <div class="title">Mines</div>
    
    <div class="mode-selector">
        <button class="mode-btn active" data-mines="3">3 МИН</button>
       <button class="mode-btn" data-mines="4">4 МИН</button>
        <button class="mode-btn" data-mines="5">5 МИН</button>
    </div>
    
    <div class="grid" id="grid"></div>
    
    <div class="action-btns">
        <button class="action-btn" id="getSignal">ПОЛУЧИТЬ СИГНАЛ</button>
        <button class="action-btn" id="exit">ВЕРНУТЬСЯ</button>
    </div>

    <script>
        const grid = document.getElementById('grid');
        let minesCount = 3;
        let minePositions = [];
        
        // Создание сетки
        for(let i = 0; i < 25; i++) {
            const cell = document.createElement('div');
            cell.className = 'cell';
            grid.appendChild(cell);
        }
        
        // Выбор режима
        document.querySelectorAll('.mode-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                minesCount = parseInt(btn.dataset.mines);
            });
        });
        
        // Генерация мин
        function generateMines() {
            minePositions = [];
            const cells = document.querySelectorAll('.cell');
            cells.forEach(cell => {
                cell.classList.remove('mine', 'red-bg');
            });
            
            while(minePositions.length < minesCount) {
                const randomPos = Math.floor(Math.random() * 25);
                if(!minePositions.includes(randomPos)) {
                    minePositions.push(randomPos);
                    cells[randomPos].classList.add('mine', 'red-bg');
                }
            }
        }
        
        // Кнопки действий
        document.getElementById('getSignal').addEventListener('click', generateMines);
        document.getElementById('exit').addEventListener('click', () => {
            Telegram.WebApp.close();
        });
    </script>
</body>
</html>
