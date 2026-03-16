# game-monopoli-PAK
game
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Monopoli PAK SMP - Fase D</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .game-container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            padding: 20px;
            max-width: 1400px;
            width: 100%;
        }

        .game-header {
            text-align: center;
            margin-bottom: 20px;
        }

        .game-header h1 {
            color: #4a5568;
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .game-header p {
            color: #718096;
            font-size: 1.1em;
        }

        .main-board {
            display: grid;
            grid-template-columns: 120px 1fr 120px;
            grid-template-rows: 120px 1fr 120px;
            gap: 10px;
            background: #2d3748;
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 20px;
        }

        .board-section {
            display: grid;
            gap: 3px;
        }

        .top-row {
            grid-template-columns: repeat(9, 1fr);
            grid-column: 2;
        }

        .left-col {
            grid-template-rows: repeat(9, 1fr);
            grid-column: 1;
            grid-row: 2;
        }

        .right-col {
            grid-template-rows: repeat(9, 1fr);
            grid-column: 3;
            grid-row: 2;
        }

        .bottom-row {
            grid-template-columns: repeat(9, 1fr);
            grid-column: 2;
            grid-row: 3;
        }

        .corner {
            background: linear-gradient(135deg, #f6ad55 0%, #ed8936 100%);
            border: 3px solid #c05621;
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            font-size: 0.9em;
            text-align: center;
            padding: 10px;
            color: white;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
            position: relative;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .corner:hover {
            transform: scale(1.05);
        }

        .corner.top-left { grid-column: 1; grid-row: 1; }
        .corner.top-right { grid-column: 3; grid-row: 1; }
        .corner.bottom-left { grid-column: 1; grid-row: 3; }
        .corner.bottom-right { grid-column: 3; grid-row: 3; }

        .space {
            background: white;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            padding: 5px;
            font-size: 0.7em;
            text-align: center;
            position: relative;
            cursor: pointer;
            transition: all 0.3s;
            min-height: 80px;
        }

        .space:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }

        .space.owned {
            border-color: #48bb78;
            background: #f0fff4;
        }

        .space-header {
            width: 100%;
            height: 20px;
            border-radius: 4px 4px 0 0;
            font-size: 0.6em;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
        }

        .space-title {
            font-weight: 600;
            color: #2d3748;
            padding: 2px;
            line-height: 1.2;
        }

        .space-price {
            color: #e53e3e;
            font-weight: bold;
            font-size: 0.9em;
        }

        .player-token {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            position: absolute;
            border: 2px solid white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.3);
            z-index: 10;
            transition: all 0.5s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
        }

        .token-1 { background: #e53e3e; }
        .token-2 { background: #3182ce; }
        .token-3 { background: #38a169; }
        .token-4 { background: #d69e2e; }

        .center-area {
            grid-column: 2;
            grid-row: 2;
            background: linear-gradient(135deg, #faf5ff 0%, #e9d8fd 100%);
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow: hidden;
        }

        .center-area::before {
            content: '✝';
            position: absolute;
            font-size: 200px;
            opacity: 0.05;
            color: #6b46c1;
        }

        .dice-container {
            display: flex;
            gap: 20px;
            margin-bottom: 20px;
        }

        .dice {
            width: 60px;
            height: 60px;
            background: white;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border: 2px solid #cbd5e0;
        }

        .btn {
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            font-size: 1em;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            margin: 5px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-success {
            background: linear-gradient(135deg, #48bb78 0%, #38a169 100%);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(135deg, #f56565 0%, #e53e3e 100%);
            color: white;
        }

        .control-panel {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 20px;
        }

        .player-info {
            background: #f7fafc;
            border-radius: 15px;
            padding: 15px;
            border-left: 5px solid;
        }

        .player-info.active {
            background: #ebf8ff;
            border-left-color: #3182ce;
            transform: scale(1.02);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .player-info.inactive {
            border-left-color: #a0aec0;
            opacity: 0.7;
        }

        .player-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .player-name {
            font-weight: 700;
            font-size: 1.2em;
        }

        .player-money {
            font-size: 1.3em;
            color: #38a169;
            font-weight: bold;
        }

        .player-properties {
            font-size: 0.9em;
            color: #4a5568;
            margin-top: 5px;
        }

        .question-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .question-card {
            background: white;
            border-radius: 20px;
            padding: 30px;
            max-width: 600px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from {
                transform: translateY(-50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .question-header {
            text-align: center;
            margin-bottom: 20px;
        }

        .question-header h2 {
            color: #2d3748;
            margin-bottom: 10px;
        }

        .question-text {
            font-size: 1.2em;
            color: #4a5568;
            margin-bottom: 20px;
            line-height: 1.6;
            background: #f7fafc;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #667eea;
        }

        .options {
            display: grid;
            gap: 10px;
        }

        .option {
            padding: 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            background: white;
        }

        .option:hover {
            background: #ebf8ff;
            border-color: #3182ce;
            transform: translateX(5px);
        }

        .option.correct {
            background: #c6f6d5;
            border-color: #38a169;
            color: #22543d;
        }

        .option.wrong {
            background: #fed7d7;
            border-color: #e53e3e;
            color: #742a2a;
        }

        .message-log {
            background: #2d3748;
            color: white;
            padding: 15px;
            border-radius: 10px;
            height: 150px;
            overflow-y: auto;
            font-family: monospace;
            font-size: 0.9em;
            margin-top: 10px;
        }

        .message-log div {
            margin-bottom: 5px;
            padding: 3px 0;
            border-bottom: 1px solid #4a5568;
        }

        .start-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            z-index: 2000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
        }

        .start-screen h1 {
            font-size: 3em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .player-setup {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            margin: 20px;
        }

        .player-input {
            margin: 10px 0;
        }

        .player-input label {
            display: block;
            margin-bottom: 5px;
        }

        .player-input input {
            padding: 10px;
            border: none;
            border-radius: 5px;
            width: 250px;
            font-size: 1em;
        }

        .property-group-1 { background: #8b4513; }
        .property-group-2 { background: #87ceeb; }
        .property-group-3 { background: #ff69b4; }
        .property-group-4 { background: #ff8c00; }
        .property-group-5 { background: #dc143c; }
        .property-group-6 { background: #ffd700; }
        .property-group-7 { background: #228b22; }
        .property-group-8 { background: #4169e1; }

        .special-space { background: linear-gradient(135deg, #9f7aea 0%, #6b46c1 100%); }
        .tax-space { background: linear-gradient(135deg, #fc8181 0%, #e53e3e 100%); }

        .house-marker {
            width: 8px;
            height: 8px;
            background: #38a169;
            border-radius: 50%;
            display: inline-block;
            margin: 0 1px;
        }

        .hotel-marker {
            width: 12px;
            height: 8px;
            background: #e53e3e;
            border-radius: 2px;
            display: inline-block;
            margin: 0 1px;
        }

        @media (max-width: 1024px) {
            .main-board {
                grid-template-columns: 80px 1fr 80px;
                grid-template-rows: 80px 1fr 80px;
            }
            .space {
                font-size: 0.5em;
                min-height: 60px;
            }
            .corner {
                font-size: 0.7em;
            }
        }
    </style>
</head>
<body>
    <div class="start-screen" id="startScreen">
        <h1>✝ Monopoli PAK SMP ✝</h1>
        <p style="font-size: 1.2em; margin-bottom: 30px;">Fase D (Kelas 7-9) - Pendidikan Agama Kristen</p>
        
        <div class="player-setup">
            <h3>Masukkan Nama Pemain (2-4 orang):</h3>
            <div class="player-input">
                <label>Pemain 1 (Merah):</label>
                <input type="text" id="p1name" placeholder="Nama Pemain 1" value="Pemain 1">
            </div>
            <div class="player-input">
                <label>Pemain 2 (Biru):</label>
                <input type="text" id="p2name" placeholder="Nama Pemain 2" value="Pemain 2">
            </div>
            <div class="player-input">
                <label>Pemain 3 (Hijau):</label>
                <input type="text" id="p3name" placeholder="Nama Pemain 3 (Opsional)">
            </div>
            <div class="player-input">
                <label>Pemain 4 (Kuning):</label>
                <input type="text" id="p4name" placeholder="Nama Pemain 4 (Opsional)">
            </div>
            <button class="btn btn-primary" onclick="startGame()" style="margin-top: 20px; width: 100%;">Mulai Permainan</button>
        </div>
        
        <div style="margin-top: 30px; text-align: center; max-width: 600px; padding: 0 20px;">
            <p style="font-size: 0.9em; opacity: 0.9;">
                Petunjuk: Jawab soal PAK dengan benar untuk mendapatkan properti. 
                Kumpulkan properti sebanyak mungkin dan jadilah juara!
            </p>
        </div>
    </div>

    <div class="game-container" id="gameContainer" style="display: none;">
        <div class="game-header">
            <h1>✝ Monopoli PAK SMP ✝</h1>
            <p>Fase D (Kelas 7-9) | Giliran: <span id="currentTurn" style="font-weight: bold; color: #667eea;">-</span></p>
        </div>

        <div class="main-board">
            <div class="corner top-left" data-pos="20">
                <div style="font-size: 2em;">🛐</div>
                <div>Parkir Bebas</div>
            </div>
            
            <div class="board-section top-row" id="topRow"></div>
            
            <div class="corner top-right" data-pos="30">
                <div style="font-size: 2em;">👮</div>
                <div>Masuk Penjara</div>
            </div>
            
            <div class="board-section left-col" id="leftCol"></div>
            
            <div class="center-area">
                <div class="dice-container">
                    <div class="dice" id="dice1">🎲</div>
                    <div class="dice" id="dice2">🎲</div>
                </div>
                <button class="btn btn-primary" id="rollBtn" onclick="rollDice()">Lempar Dadu</button>
                <button class="btn btn-success" id="buyBtn" onclick="buyProperty()" style="display: none;">Beli Properti</button>
                <button class="btn btn-danger" id="endBtn" onclick="endTurn()" style="display: none;">Akhiri Giliran</button>
                <div id="actionText" style="margin-top: 15px; font-weight: 600; color: #4a5568; text-align: center;"></div>
            </div>
            
            <div class="board-section right-col" id="rightCol"></div>
            
            <div class="corner bottom-left" data-pos="10">
                <div style="font-size: 2em;">🔒</div>
                <div>Penjara</div>
            </div>
            
            <div class="board-section bottom-row" id="bottomRow"></div>
            
            <div class="corner bottom-right" data-pos="0">
                <div style="font-size: 2em;">🚀</div>
                <div>Mulai</div>
                <div style="font-size: 0.8em;">Dapat Rp 200</div>
            </div>
        </div>

        <div class="control-panel">
            <div id="playersInfo"></div>
            <div>
                <div class="message-log" id="gameLog">
                    <div>Selamat datang di Monopoli PAK! Lempar dadu untuk memulai.</div>
                </div>
            </div>
        </div>
    </div>

    <div class="question-modal" id="questionModal">
        <div class="question-card">
            <div class="question-header">
                <h2>📚 Soal PAK</h2>
                <span id="questionCategory" style="background: #667eea; color: white; padding: 5px 15px; border-radius: 20px; font-size: 0.9em;"></span>
            </div>
            <div class="question-text" id="questionText"></div>
            <div class="options" id="optionsContainer"></div>
            <div id="explanation" style="margin-top: 15px; padding: 15px; background: #f0fff4; border-radius: 10px; display: none; border-left: 4px solid #38a169;"></div>
            <button class="btn btn-primary" id="continueBtn" onclick="closeQuestion()" style="display: none; width: 100%; margin-top: 15px;">Lanjutkan</button>
        </div>
    </div>

    <script>
        // Game Data
        const spaces = [
            { name: "Mulai", type: "corner", price: 0 },
            { name: "Kitab Suci", type: "property", group: 1, price: 60, rent: [2, 10, 30, 90, 160, 250], question: "biblical" },
            { name: "Dana Kemuliaan", type: "chest", price: 0 },
            { name: "Doa Bapa Kami", type: "property", group: 1, price: 60, rent: [4, 20, 60, 180, 320, 450], question: "prayer" },
            { name: "Perpuluhan", type: "tax", price: 200 },
            { name: "Gereja Sentral", type: "church", price: 200, rent: [25, 50, 100, 200] },
            { name: "Iman kepada Allah", type: "property", group: 2, price: 100, rent: [6, 30, 90, 270, 400, 550], question: "faith" },
            { name: "Kesempatan", type: "chance", price: 0 },
            { name: "Iman kepada Yesus", type: "property", group: 2, price: 100, rent: [6, 30, 90, 270, 400, 550], question: "faith" },
            { name: "Kasih Karunia", type: "property", group: 2, price: 120, rent: [8, 40, 100, 300, 450, 600], question: "grace" },
            { name: "Penjara", type: "corner", price: 0 },
            { name: "Mazmur 23", type: "property", group: 3, price: 140, rent: [10, 50, 150, 450, 625, 750], question: "bible_content" },
            { name: "Listrik Roh Kudus", type: "utility", price: 150, question: "holy_spirit" },
            { name: "Amsal Kebijaksanaan", type: "property", group: 3, price: 140, rent: [10, 50, 150, 450, 625, 750], question: "bible_content" },
            { name: "Pengkhotbah", type: "property", group: 3, price: 160, rent: [12, 60, 180, 500, 700, 900], question: "bible_content" },
            { name: "Katedral", type: "church", price: 200, rent: [25, 50, 100, 200] },
            { name: "Mukjizat Yesus", type: "property", group: 4, price: 180, rent: [14, 70, 200, 550, 750, 950], question: "miracles" },
            { name: "Dana Kemuliaan", type: "chest", price: 0 },
            { name: "Pengajaran Yesus", type: "property", group: 4, price: 180, rent: [14, 70, 200, 550, 750, 950], question: "teachings" },
            { name: "Paskah", type: "property", group: 4, price: 200, rent: [16, 80, 220, 600, 800, 1000], question: "holidays" },
            { name: "Parkir Bebas", type: "corner", price: 0 },
            { name: "Perjamuan Kudus", type: "property", group: 5, price: 220, rent: [18, 90, 250, 700, 875, 1050], question: "sacraments" },
            { name: "Kesempatan", type: "chance", price: 0 },
            { name: "Baptisan", type: "property", group: 5, price: 220, rent: [18, 90, 250, 700, 875, 1050], question: "sacraments" },
            { name: "Pengakuan Iman", type: "property", group: 5, price: 240, rent: [20, 100, 300, 750, 925, 1100], question: "creed" },
            { name: "Gereja Besar", type: "church", price: 200, rent: [25, 50, 100, 200] },
            { name: "Natal", type: "property", group: 6, price: 260, rent: [22, 110, 330, 800, 975, 1150], question: "holidays" },
            { name: "Pentakosta", type: "property", group: 6, price: 260, rent: [22, 110, 330, 800, 975, 1150], question: "holidays" },
            { name: "Air Kehidupan", type: "utility", price: 150, question: "theology" },
            { name: "Kenaikan Yesus", type: "property", group: 6, price: 280, rent: [24, 120, 360, 850, 1025, 1200], question: "theology" },
            { name: "Masuk Penjara", type: "corner", price: 0 },
            { name: "Suku Bangsa", type: "property", group: 7, price: 300, rent: [26, 130, 390, 900, 1100, 1275], question: "church_history" },
            { name: "Gereja Mula-mula", type: "property", group: 7, price: 300, rent: [26, 130, 390, 900, 1100, 1275], question: "church_history" },
            { name: "Dana Kemuliaan", type: "chest", price: 0 },
            { name: "Reformasi", type: "property", group: 7, price: 320, rent: [28, 150, 450, 1000, 1200, 1400], question: "church_history" },
            { name: "Kapel", type: "church", price: 200, rent: [25, 50, 100, 200] },
            { name: "Kesempatan", type: "chance", price: 0 },
            { name: "Kasih Agape", type: "property", group: 8, price: 350, rent: [35, 175, 500, 1100, 1300, 1500], question: "ethics" },
            { name: "Pajak Amal", type: "tax", price: 100 },
            { name: "Surga Baru", type: "property", group: 8, price: 400, rent: [50, 200, 600, 1400, 1700, 2000], question: "eschatology" }
        ];

        const questions = {
            biblical: [
                { q: "Berapa jumlah kitab dalam Alkitab?", options: ["66 kitab", "72 kitab", "39 kitab", "27 kitab"], correct: 0, explanation: "Alkitab terdiri dari 66 kitab: 39 kitab Perjanjian Lama dan 27 kitab Perjanjian Baru." },
                { q: "Kitab apa yang menjadi awal Alkitab?", options: ["Kejadian", "Keluaran", "Matius", "Yohanes"], correct: 0, explanation: "Kitab Kejadian adalah kitab pertama dalam Alkitab, menceritakan penciptaan dunia." },
                { q: "Siapakah penulis kitab Keluaran?", options: ["Musa", "Daud", "Salomo", "Yosua"], correct: 0, explanation: "Musa menulis kitab Keluaran yang menceritakan bangsa Israel keluar dari Mesir." }
            ],
            prayer: [
                { q: "Apa doa yang diajarkan Yesus kepada murid-murid-Nya?", options: ["Doa Bapa Kami", "Doa Salam Maria", "Doa Kudus", "Doa Malam"], correct: 0, explanation: "Doa Bapa Kami (Our Father/The Lord's Prayer) adalah doa yang diajarkan Yesus dalam Matius 6:9-13." },
                { q: "Dalam doa Bapa Kami, kita meminta 'roti' apa untuk hari ini?", options: ["Roti yang secukupnya", "Roti yang melimpah", "Roti emas", "Roti surgawi"], correct: 0, explanation: "'Berikanlah kami pada hari ini makanan kami yang secukupnya' (Matius 6:11)." },
                { q: "Apa yang kita minta dalam doa 'janganlah membawa kami ke dalam pencobaan'?", options: ["Perlindungan dari godaan", "Ujian berat", "Kekayaan", "Kekuasaan"], correct: 0, explanation: "Kita meminta perlindungan dari godaan dan pencobaan yang dapat merusak iman kita." }
            ],
            faith: [
                { q: "Apa definisi iman menurut Ibrani 11:1?", options: ["Dasar dari sesuatu yang diharapkan", "Perbuatan baik", "Pergi ke gereja", "Membaca Alkitab"], correct: 0, explanation: "Iman adalah dasar dari sesuatu yang kita harapkan dan bukti dari sesuatu yang tidak kita lihat (Ibrani 11:1)." },
                { q: "Siapakah bapa orang beriman?", options: ["Abraham", "Musa", "Daud", "Yusuf"], correct: 0, explanation: "Abraham disebut bapa orang beriman karena ketaatannya kepada Allah (Roma 4:11-12)." },
                { q: "Bagaimana cara menyelamatkan manusia?", options: ["Oleh karena iman", "Oleh perbuatan baik", "Oleh uang", "Oleh kedudukan"], correct: 0, explanation: "Karena kasih karunia kamu diselamatkan oleh iman (Efesus 2:8)." }
            ],
            grace: [
                { q: "Apa arti kasih karunia (gratia)?", options: ["Anugerah yang tidak layak diterima", "Upah atas jerih payah", "Hak yang harus didapat", "Bonus tambahan"], correct: 0, explanation: "Kasih karunia adalah anugerah Allah yang tidak layak kita terima namun diberikan secara cuma-cuma." },
                { q: "Ayat mana yang terkenal tentang kasih karunia?", options: ["Yohanes 3:16", "Mazmur 23", "Kejadian 1:1", "Wahyu 1:1"], correct: 0, explanation: "Yohanes 3:16: 'Karena begitu besar kasih Allah akan dunia ini...'" },
                { q: "Dapatkah kasih karunia Allah habis?", options: ["Tidak, kekal selamanya", "Ya, jika kita berdosa", "Ya, jika kita tidak beribadah", "Hanya untuk orang tertentu"], correct: 0, explanation: "Kasih karunia Allah adalah kekal dan baru setiap pagi (Kidung Agung 3:22-23)." }
            ],
            bible_content: [
                { q: "Mazmur 23 menggambarkan Tuhan sebagai...", options: ["Gemba yang baik", "Raja yang perkasa", "Hakim yang adil", "Nabi yang bijaksana"], correct: 0, explanation: "Mazmur 23:1 'Tuhan adalah gembalaiku, takkan kekurangan aku.'" },
                { q: "Apa tema utama kitab Pengkhotbah?", options: ["Keheningan dan kesia-siaan", "Kemenangan dan sukacita", "Perang dan damai", "Cinta dan persahabatan"], correct: 0, explanation: "Pengkhotbah (Eclesiastes) membahas kesia-siaan segala sesuatu di bawah matahari tanpa Tuhan." },
                { q: "Siapakah penulis kitab Amsal utama?", options: ["Salomo", "Daud", "Hezekiel", "Yosafat"], correct: 0, explanation: "Raja Salomo menulis sebagian besar kitab Amsal yang penuh dengan kebijaksanaan." }
            ],
            holy_spirit: [
                { q: "Apa simbol Roh Kudus pada hari Pentakosta?", options: ["Lidah api", "Merpati", "Air", "Angin"], correct: 0, explanation: "Pada Pentakosta, Roh Kudus turun dalam bentuk lidah-lidah api (Kisah Para Rasul 2:3)." },
                { q: "Apa buah Roh Kudus yang pertama?", options: ["Kasih", "Sukacita", "Damai sejahtera", "Kesabaran"], correct: 0, explanation: "Galatia 5:22 menyebut kasih sebagai buah Roh yang pertama." },
                { q: "Roh Kudus dijanjikan oleh siapa?", options: ["Yesus", "Petrus", "Paulus", "Yohanes Pembaptis"], correct: 0, explanation: "Yesus berjanji akan mengirim Roh Kudus (Penghibur) setelah Dia naik ke sorga." }
            ],
            miracles: [
                { q: "Mukjizat pertama Yesus adalah?", options: ["Mengubah air menjadi anggur", "Memberi makan 5000 orang", "Berjalan di atas air", "Membangkitkan Lazarus"], correct: 0, explanation: "Di pesta pernikahan di Kana, Yesus mengubah air menjadi anggur (Yohanes 2:1-11)." },
                { q: "Berapa jumlah roti dalam mukjizat memberi makan 5000 orang?", options: ["5 roti dan 2 ikan", "7 roti", "12 roti", "2 roti"], correct: 0, explanation: "Yesus memberi makan 5000 orang dengan 5 roti dan 2 ikan (Matius 14:13-21)." },
                { q: "Siapakah yang dibangkitkan Yesus setelah 4 hari mati?", options: ["Lazarus", "Yairus", "Anak janda Nain", "Yesus sendiri"], correct: 0, explanation: "Lazarus, sahabat Yesus, dibangkitkan setelah 4 hari di dalam kubur (Yohanes 11)." }
            ],
            teachings: [
                { q: "Apa isi khotbah di atas bukit (Matius 5-7)?", options: ["Ucapan berbahagia", "10 Perintah Allah", "Doa Bapa Kami saja", "Cerita tentang kerajaan"], correct: 0, explanation: "Khotbah di atas bukit dimulai dengan Ucapan Berbahagia (Beatitudes)." },
                { q: "Apa hukum utama menurut Yesus?", options: ["Kasihilah Tuhan dan sesama", "Jangan mencuri", "Jangan membunuh", "Hormati orang tua"], correct: 0, explanation: "Kasihilah Tuhan Allahmu... dan kasihilah sesamamu manusia (Matius 22:37-39)." },
                { q: "Apa arti 'jika pipi kiri ditampar...'?", options: ["Berikan pipi kanan", "Balas dendam", "Lari", "Marah"], correct: 0, explanation: "Yesus mengajarkan untuk tidak membalas kejahatan dengan kejahatan (Matius 5:39)." }
            ],
            holidays: [
                { q: "Apa arti Natal?", options: ["Kelahiran", "Kebangkitan", "Kenaikan", "Penciptaan"], correct: 0, explanation: "Natal (Christmas) merayakan kelahiran Yesus Kristus." },
                { q: "Apa yang dirayakan pada Paskah?", options: ["Kebangkitan Yesus", "Kelahiran Yesus", "Kenaikan Yesus", "Turunnya Roh Kudus"], correct: 0, explanation: "Paskah (Easter) merayakan kebangkitan Yesus dari kematian." },
                { q: "Hari apa Yesus bangkit dari kubur?", options: ["Hari ketiga", "Hari kedua", "Hari pertama", "Hari keempat"], correct: 0, explanation: "Yesus bangkit pada hari ketiga sesuai dengan Kitab Suci (1 Korintus 15:4)." }
            ],
            sacraments: [
                { q: "Berapa sakramen dalam Gereja Protestan umumnya?", options: ["2", "7", "5", "3"], correct: 0, explanation: "Umumnya Protestan mengakui 2 sakramen: Baptisan dan Perjamuan Kudus." },
                { q: "Apa simbol dalam Perjamuan Kudus?", options: ["Roti dan anggur", "Air dan api", "Minyak dan dupa", "Cahaya dan garam"], correct: 0, explanation: "Roti melambangkan tubuh Kristus dan anggur melambangkan darah Kristus." },
                { q: "Apa arti baptisan?", options: ["Penandaan iman", "Pembersihan dosa saja", "Masuk gereja", "Pernikahan"], correct: 0, explanation: "Baptisan adalah sakramen penandaan masuk dalam perjanjian Allah dan komunitas gereja." }
            ],
            creed: [
                { q: "Berapa pengakuan iman rasuli?", options: ["12", "10", "8", "7"], correct: 0, explanation: "Pengakuan Iman Rasuli terdiri dari 12 pasal yang meringkaskan iman Kristen." },
                { q: "Apa pengakuan pertama Iman Rasuli?", options: ["Percaya kepada Allah Bapa", "Percaya kepada Yesus", "Percaya kepada Roh Kudus", "Percaya kepada Gereja"], correct: 0, explanation: "Aku percaya kepada Allah, Bapa yang mahakuasa, Khalik langit dan bumi." },
                { q: "Pengakuan Iman Rasuli ditulis untuk?", options: ["Meringkas iman Kristen", "Menggantikan Alkitab", "Aturan gereja", "Doa harian"], correct: 0, explanation: "Pengakuan Iman Rasuli ditulis sebagai ringkasan iman Kristen yang esensial." }
            ],
            theology: [
                { q: "Apa arti 'Trinitas'?", options: ["Tiga dalam satu", "Tiga dewa", "Satu dalam tiga", "Tuhan tunggal"], correct: 0, explanation: "Trinitas mengajarkan bahwa Allah adalah Tiga Pribadi (Bapa, Anak, Roh Kudus) dalam Satu Essensi." },
                { q: "Apa yang terjadi saat Kenaikan Yesus?", options: ["Yesus naik ke sorga", "Yesus turun ke neraka", "Yesus ke surga", "Yesus ke dunia"], correct: 0, explanation: "Yesus naik ke sorga di depan murid-murid-Nya (Kisah Para Rasul 1:9)." },
                { q: "Apa tugas Roh Kudus menurut Yohanes 16?", options: ["Menuntun ke kebenaran", "Menghakimi dunia", "Memberi kekayaan", "Menghancurkan musuh"], correct: 0, explanation: "Roh Kudus akan menuntun kamu ke dalam segala kebenaran (Yohanes 16:13)." }
            ],
            church_history: [
                { q: "Siapakah rasul untuk bangsa bukan Yahudi?", options: ["Paulus", "Petrus", "Yohanes", "Yakobus"], correct: 0, explanation: "Paulus menjadi rasul bagi bangsa-bangsa (Gentiles) dan menyebarkan Injil ke seluruh dunia." },
                { q: "Kapan Gereja Mula-mula dimulai?", options: ["Hari Pentakosta", "Kelahiran Yesus", "Paskah", "Natal"], correct: 0, explanation: "Gereja Mula-mula lahir pada hari Pentakosta ketika Roh Kudus turun (Kisah 2)." },
                { q: "Siapakah tokoh Reformasi Protestan?", options: ["Martin Luther", "Yohanes Calvin", "Ulrich Zwingli", "Semua benar"], correct: 3, explanation: "Martin Luther, Yohanes Calvin, dan Ulrich Zwingli adalah tokoh-tokoh utama Reformasi." }
            ],
            ethics: [
                { q: "Apa arti kasih agape?", options: ["Kasih tanpa syarat", "Kasih romantis", "Kasih keluarga", "Kasih persahabatan"], correct: 0, explanation: "Agape adalah kasih ilahi yang tanpa syarat dan mengorbankan diri (1 Korintus 13)." },
                { q: "Apa hukum emas (Golden Rule)?", options: ["Kasihilah sesama", "Jangan berbuat zalim", "Hormati atasan", "Bekerja keras"], correct: 0, explanation: "Segala sesuatu yang kamu kehendaki supaya orang perbuat kepadamu... (Matius 7:12)." },
                { q: "Bagaimana sikap Kristen terhadap musuh?", options: ["Kasihilah mereka", "Benci mereka", "Hindari mereka", "Serang mereka"], correct: 0, explanation: "Kasihilah musuhmu dan berdoalah bagi mereka yang menganiaya kamu (Matius 5:44)." }
            ],
            eschatology: [
                { q: "Apa arti 'eschatology'?", options: ["Ilmu tentang hal-hal terakhir", "Ilmu tentang awal", "Ilmu tentang gereja", "Ilmu tentang malaikat"], correct: 0, explanation: "Eschatology (ilmu akhir zaman) mempelajari hal-hal terakhir: kematian, pengadilan, surga, neraka." },
                { q: "Apa janji Allah tentang surga baru?", options: ["Tiada air mata", "Emas di mana-mana", "Semua jadi kaya", "Tak ada kerja"], correct: 0, explanation: "Allah akan menghapus segala air mata dari mata mereka (Wahyu 21:4)." },
                { q: "Apa yang terjadi pada Kedatangan Yesus yang kedua?", options: ["Pengadilan akhir", "Natal kedua", "Paskah lagi", "Baptisan besar"], correct: 0, explanation: "Kedatangan Yesus yang kedua akan membawa pengadilan akhir dan pemisahan yang benar dan salah." }
            ]
        };

        const chanceCards = [
            "Advance to Go (Collect $200)",
            "Bank pays you dividend of $50",
            "Go back 3 spaces",
            "Go directly to Jail",
            "Make general repairs on all your property",
            "Pay poor tax of $15",
            "Take a trip to Reading Railroad",
            "You have been elected Chairman of the Board",
            "Your building loan matures"
        ];

        const chestCards = [
            "Advance to Go (Collect $200)",
            "Bank error in your favor",
            "Doctor's fee",
            "From sale of stock you get $50",
            "Get Out of Jail Free",
            "Go to Jail",
            "Grand Opera Night",
            "Holiday Fund matures",
            "Income tax refund",
            "It is your birthday",
            "Life insurance matures",
            "Pay hospital fees",
            "Receive $25 consultancy fee",
            "You are assessed for street repairs",
            "You have won second prize in a beauty contest",
            "You inherit $100"
        ];

        // Game State
        let players = [];
        let currentPlayer = 0;
        let gameStarted = false;
        let diceRolled = false;
        let currentQuestion = null;
        let spacesOwned = new Array(40).fill(null);
        let houses = new Array(40).fill(0);

        function startGame() {
            const p1 = document.getElementById('p1name').value || 'Pemain 1';
            const p2 = document.getElementById('p2name').value || 'Pemain 2';
            const p3 = document.getElementById('p3name').value;
            const p4 = document.getElementById('p4name').value;

            players = [
                { name: p1, money: 1500, position: 0, color: 'token-1', inJail: false, jailTurns: 0, properties: [] },
                { name: p2, money: 1500, position: 0, color: 'token-2', inJail: false, jailTurns: 0, properties: [] }
            ];

            if (p3) players.push({ name: p3, money: 1500, position: 0, color: 'token-3', inJail: false, jailTurns: 0, properties: [] });
            if (p4) players.push({ name: p4, money: 1500, position: 0, color: 'token-4', inJail: false, jailTurns: 0, properties: [] });

            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('gameContainer').style.display = 'block';
            
            initBoard();
            updatePlayerInfo();
            updateTokens();
            log(`${players[0].name} memulai permainan! Lempar dadu untuk bermain.`);
            document.getElementById('currentTurn').textContent = players[0].name;
        }

        function initBoard() {
            // Top row (pos 21-29)
            const topRow = document.getElementById('topRow');
            for (let i = 21; i <= 29; i++) {
                topRow.appendChild(createSpace(i));
            }

            // Right column (pos 31-39)
            const rightCol = document.getElementById('rightCol');
            for (let i = 31; i <= 39; i++) {
                rightCol.appendChild(createSpace(i));
            }

            // Bottom row (pos 9-1, reversed)
            const bottomRow = document.getElementById('bottomRow');
            for (let i = 9; i >= 1; i--) {
                bottomRow.appendChild(createSpace(i));
            }

            // Left column (pos 19-11, reversed)
            const leftCol = document.getElementById('leftCol');
            for (let i = 19; i >= 11; i--) {
                leftCol.appendChild(createSpace(i));
            }

            // Set corner positions
            document.querySelector('.corner.bottom-right').setAttribute('data-pos', '0');
            document.querySelector('.corner.bottom-left').setAttribute('data-pos', '10');
            document.querySelector('.corner.top-left').setAttribute('data-pos', '20');
            document.querySelector('.corner.top-right').setAttribute('data-pos', '30');
        }

        function createSpace(index) {
            const space = document.createElement('div');
            const data = spaces[index];
            space.className = 'space';
            space.setAttribute('data-pos', index);
            
            let headerClass = '';
            if (data.type === 'property') headerClass = `property-group-${data.group}`;
            else if (data.type === 'church') headerClass = 'special-space';
            else if (data.type === 'tax') headerClass = 'tax-space';
            else if (['chance', 'chest'].includes(data.type)) headerClass = 'special-space';

            let header = '';
            if (headerClass) {
                header = `<div class="space-header ${headerClass}">${data.group ? 'G' + data.group : data.type}</div>`;
            }

            let price = '';
            if (data.price > 0 && !['tax', 'corner'].includes(data.type)) {
                price = `<div class="space-price">Rp ${data.price}</div>`;
            } else if (data.type === 'tax') {
                price = `<div class="space-price">Bayar Rp ${data.price}</div>`;
            }

            space.innerHTML = `
                ${header}
                <div class="space-title">${data.name}</div>
                ${price}
            `;

            space.onclick = () => showSpaceInfo(index);
            return space;
        }

        function rollDice() {
            if (diceRolled) return;
            
            const d1 = Math.floor(Math.random() * 6) + 1;
            const d2 = Math.floor(Math.random() * 6) + 1;
            
            document.getElementById('dice1').textContent = d1;
            document.getElementById('dice2').textContent = d2;
            
            const total = d1 + d2;
            const player = players[currentPlayer];
            
            log(`${player.name} melempar dadu: ${d1} + ${d2} = ${total}`);
            
            if (player.inJail) {
                if (d1 === d2) {
                    player.inJail = false;
                    player.jailTurns = 0;
                    log(`${player.name} bebas dari penjara karena dadu kembar!`);
                    movePlayer(total);
                } else {
                    player.jailTurns++;
                    if (player.jailTurns >= 3) {
                        player.inJail = false;
                        player.jailTurns = 0;
                        player.money -= 50;
                        log(`${player.name} membayar denda Rp 50 untuk bebas dari penjara`);
                        movePlayer(total);
                    } else {
                        log(`${player.name} masih di penjara (giliran ${player.jailTurns}/3)`);
                        endTurn();
                    }
                }
            } else {
                movePlayer(total);
            }
            
            diceRolled = true;
            document.getElementById('rollBtn').style.display = 'none';
        }

        function movePlayer(steps) {
            const player = players[currentPlayer];
            const oldPos = player.position;
            player.position = (player.position + steps) % 40;
            
            // Pass Go
            if (player.position < oldPos && oldPos + steps >= 40) {
                player.money += 200;
                log(`${player.name} melewati Mulai dan menerima Rp 200!`);
                updatePlayerInfo();
            }
            
            updateTokens();
            handleLanding();
        }

        function updateTokens() {
            // Remove all tokens
            document.querySelectorAll('.player-token').forEach(t => t.remove());
            
            // Add tokens to current positions
            players.forEach((player, idx) => {
                const space = document.querySelector(`[data-pos="${player.position}"]`);
                if (space) {
                    const token = document.createElement('div');
                    token.className = `player-token ${player.color}`;
                    token.textContent = idx + 1;
                    token.style.left = `${10 + (idx * 15)}px`;
                    token.style.top = `${10 + (idx * 15)}px`;
                    space.appendChild(token);
                }
            });
        }

        function handleLanding() {
            const player = players[currentPlayer];
            const space = spaces[player.position];
            
            log(`${player.name} mendarat di ${space.name}`);
            
            if (space.type === 'property' || space.type === 'church' || space.type === 'utility') {
                if (spacesOwned[player.position] === null) {
                    // Available to buy - show question first
                    showQuestion(space);
                } else if (spacesOwned[player.position] !== currentPlayer) {
                    // Pay rent
                    const owner = players[spacesOwned[player.position]];
                    let rent = 0;
                    
                    if (space.type === 'property') {
                        rent = space.rent[houses[player.position]];
                    } else if (space.type === 'church') {
                        const churchesOwned = owner.properties.filter(p => spaces[p].type === 'church').length;
                        rent = space.rent[churchesOwned - 1];
                    } else if (space.type === 'utility') {
                        const dice1 = parseInt(document.getElementById('dice1').textContent) || 1;
                        const dice2 = parseInt(document.getElementById('dice2').textContent) || 1;
                        const utilitiesOwned = owner.properties.filter(p => spaces[p].type === 'utility').length;
                        rent = (dice1 + dice2) * (utilitiesOwned === 2 ? 10 : 4);
                    }
                    
                    player.money -= rent;
                    owner.money += rent;
                    log(`${player.name} membayar sewa Rp ${rent} kepada ${owner.name}`);
                    updatePlayerInfo();
                    checkBankruptcy();
                    document.getElementById('endBtn').style.display = 'inline-block';
                } else {
                    // Own property
                    document.getElementById('actionText').textContent = 'Anda memiliki properti ini';
                    document.getElementById('endBtn').style.display = 'inline-block';
                }
            } else if (space.type === 'tax') {
                player.money -= space.price;
                log(`${player.name} membayar ${space.name}: Rp ${space.price}`);
                updatePlayerInfo();
                checkBankruptcy();
                document.getElementById('endBtn').style.display = 'inline-block';
            } else if (space.type === 'chance') {
                drawChance();
            } else if (space.type === 'chest') {
                drawChest();
            } else if (space.type === 'corner') {
                if (player.position === 30) {
                    goToJail();
                } else if (player.position === 0) {
                    player.money += 200;
                    log(`${player.name} menerima Rp 200 di Mulai!`);
                    updatePlayerInfo();
                    document.getElementById('endBtn').style.display = 'inline-block';
                } else {
                    document.getElementById('endBtn').style.display = 'inline-block';
                }
            }
        }

        function showQuestion(space) {
            const category = space.question;
            const questionList = questions[category];
            const randomQ = questionList[Math.floor(Math.random() * questionList.length)];
            
            currentQuestion = { ...randomQ, space: space, spaceIndex: players[currentPlayer].position };
            
            document.getElementById('questionCategory').textContent = space.name;
            document.getElementById('questionText').textContent = randomQ.q;
            
            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';
            
            randomQ.options.forEach((opt, idx) => {
                const btn = document.createElement('div');
                btn.className = 'option';
                btn.textContent = opt;
                btn.onclick = () => answerQuestion(idx);
                optionsContainer.appendChild(btn);
            });
            
            document.getElementById('explanation').style.display = 'none';
            document.getElementById('continueBtn').style.display = 'none';
            document.getElementById('questionModal').style.display = 'flex';
        }

        function answerQuestion(answer) {
            const options = document.querySelectorAll('.option');
            options.forEach((opt, idx) => {
                opt.style.pointerEvents = 'none';
                if (idx === currentQuestion.correct) {
                    opt.classList.add('correct');
                } else if (idx === answer && idx !== currentQuestion.correct) {
                    opt.classList.add('wrong');
                }
            });
            
            const explanation = document.getElementById('explanation');
            explanation.textContent = currentQuestion.explanation;
            explanation.style.display = 'block';
            
            if (answer === currentQuestion.correct) {
                log('Jawaban benar! Anda dapat membeli properti ini.');
                document.getElementById('buyBtn').style.display = 'inline-block';
            } else {
                log('Jawaban salah! Anda tidak dapat membeli properti ini.');
                setTimeout(() => {
                    closeQuestion();
                }, 2000);
            }
            
            document.getElementById('continueBtn').style.display = 'block';
        }

        function closeQuestion() {
            document.getElementById('questionModal').style.display = 'none';
            if (!document.getElementById('buyBtn').style.display || document.getElementById('buyBtn').style.display === 'none') {
                document.getElementById('endBtn').style.display = 'inline-block';
            }
        }

        function buyProperty() {
            const player = players[currentPlayer];
            const space = currentQuestion.space;
            const idx = currentQuestion.spaceIndex;
            
            if (player.money >= space.price) {
                player.money -= space.price;
                spacesOwned[idx] = currentPlayer;
                player.properties.push(idx);
                
                // Mark space as owned
                const spaceEl = document.querySelector(`[data-pos="${idx}"]`);
                spaceEl.classList.add('owned');
                spaceEl.style.borderColor = getComputedStyle(document.querySelector(`.${player.color}`)).backgroundColor;
                
                log(`${player.name} membeli ${space.name} seharga Rp ${space.price}`);
                updatePlayerInfo();
                
                document.getElementById('buyBtn').style.display = 'none';
                document.getElementById('endBtn').style.display = 'inline-block';
                document.getElementById('actionText').textContent = `Berhasil membeli ${space.name}!`;
            } else {
                alert('Uang tidak cukup!');
                document.getElementById('buyBtn').style.display = 'none';
                document.getElementById('endBtn').style.display = 'inline-block';
            }
        }

        function drawChance() {
            const card = chanceCards[Math.floor(Math.random() * chanceCards.length)];
            log(`Kesempatan: ${card}`);
            
            const player = players[currentPlayer];
            
            if (card.includes('Go to Jail')) {
                goToJail();
            } else if (card.includes('Advance to Go')) {
                player.position = 0;
                player.money += 200;
                updateTokens();
            } else if (card.includes('go back 3')) {
                player.position = (player.position - 3 + 40) % 40;
                updateTokens();
                handleLanding();
                return;
            } else if (card.includes('dividend')) {
                player.money += 50;
            } else if (card.includes('poor tax')) {
                player.money -= 15;
            } else if (card.includes('Chairman')) {
                player.money -= 50;
                players.forEach((p, idx) => {
                    if (idx !== currentPlayer) p.money += 50;
                });
            }
            
            updatePlayerInfo();
            document.getElementById('endBtn').style.display = 'inline-block';
        }

        function drawChest() {
            const card = chestCards[Math.floor(Math.random() * chestCards.length)];
            log(`Dana Kemuliaan: ${card}`);
            
            const player = players[currentPlayer];
            
            if (card.includes('Go to Jail')) {
                goToJail();
            } else if (card.includes('Advance to Go')) {
                player.position = 0;
                player.money += 200;
                updateTokens();
            } else if (card.includes('Bank error')) {
                player.money += 200;
            } else if (card.includes('Doctor') || card.includes('hospital')) {
                player.money -= 50;
            } else if (card.includes('sale of stock') || card.includes('birthday')) {
                player.money += 50;
            } else if (card.includes('Holiday') || card.includes('insurance') || card.includes('inherit')) {
                player.money += 100;
            } else if (card.includes('refund')) {
                player.money += 20;
            } else if (card.includes('consultancy')) {
                player.money += 25;
            } else if (card.includes('beauty contest')) {
                player.money += 10;
            }
            
            updatePlayerInfo();
            document.getElementById('endBtn').style.display = 'inline-block';
        }

        function goToJail() {
            const player = players[currentPlayer];
            player.position = 10;
            player.inJail = true;
            player.jailTurns = 0;
            updateTokens();
            log(`${player.name} masuk ke penjara!`);
            document.getElementById('endBtn').style.display = 'inline-block';
        }

        function endTurn() {
            currentPlayer = (currentPlayer + 1) % players.length;
            diceRolled = false;
            
            document.getElementById('rollBtn').style.display = 'inline-block';
            document.getElementById('buyBtn').style.display = 'none';
            document.getElementById('endBtn').style.display = 'none';
            document.getElementById('actionText').textContent = '';
            document.getElementById('dice1').textContent = '🎲';
            document.getElementById('dice2').textContent = '🎲';
            
            document.getElementById('currentTurn').textContent = players[currentPlayer].name;
            updatePlayerInfo();
            
            log(`Giliran ${players[currentPlayer].name}`);
        }

        function checkBankruptcy() {
            const player = players[currentPlayer];
            if (player.money < 0) {
                alert(`${player.name} bangkrut! Permainan berakhir untuknya.`);
                // Remove player
                players.splice(currentPlayer, 1);
                if (players.length === 1) {
                    alert(`Selamat! ${players[0].name} memenangkan permainan!`);
                    location.reload();
                } else {
                    if (currentPlayer >= players.length) currentPlayer = 0;
                    updatePlayerInfo();
                }
            }
        }

        function updatePlayerInfo() {
            const container = document.getElementById('playersInfo');
            container.innerHTML = '';
            
            players.forEach((player, idx) => {
                const div = document.createElement('div');
                div.className = `player-info ${idx === currentPlayer ? 'active' : 'inactive'}`;
                
                const props = player.properties.map(p => spaces[p].name).join(', ') || 'Belum ada';
                
                div.innerHTML = `
                    <div class="player-header">
                        <span class="player-name" style="color: ${getPlayerColor(player.color)}">${player.name}</span>
                        <span class="player-money">Rp ${player.money}</span>
                    </div>
                    <div class="player-properties">Properti: ${props}</div>
                    ${player.inJail ? '<div style="color: red; font-weight: bold;">🔒 DI PENJARA</div>' : ''}
                `;
                
                container.appendChild(div);
            });
        }

        function getPlayerColor(colorClass) {
            const colors = {
                'token-1': '#e53e3e',
                'token-2': '#3182ce',
                'token-3': '#38a169',
                'token-4': '#d69e2e'
            };
            return colors[colorClass] || '#666';
        }

        function log(message) {
            const logEl = document.getElementById('gameLog');
            const entry = document.createElement('div');
            entry.textContent = `> ${message}`;
            logEl.appendChild(entry);
            logEl.scrollTop = logEl.scrollHeight;
        }

        function showSpaceInfo(index) {
            const space = spaces[index];
            let info = `${space.name}\n`;
            if (space.type === 'property') {
                info += `Harga: Rp ${space.price}\nSewa: ${space.rent.join(', ')}`;
                if (spacesOwned[index] !== null) {
                    info += `\nPemilik: ${players[spacesOwned[index]].name}`;
                }
            } else if (space.price > 0) {
                info += `Biaya: Rp ${space.price}`;
            }
            alert(info);
        }
    </script>
</body>
</html>
