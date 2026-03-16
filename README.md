<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Monopoli Buah Roh - Kelas 8</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js"></script>
    <style>
        .board-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            grid-template-rows: repeat(6, 1fr);
            gap: 4px;
            aspect-ratio: 1 / 1;
        }
        .cell {
            border: 2px solid #e2e8f0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
            text-align: center;
            padding: 4px;
            background: white;
            position: relative;
            min-height: 60px;
        }
        .player-token {
            width: 18px;
            height: 18px;
            border-radius: 50%;
            border: 2px solid white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.3);
            position: absolute;
            transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
            z-index: 10;
        }
        .cell-start { background-color: #fecaca; font-weight: bold; }
        .cell-fruit { border-top: 4px solid #4f46e5; }
        .cell-theory { border-top: 4px solid #10b981; }
        .cell-special { background-color: #fef3c7; }
        
        @media (max-width: 640px) {
            .cell { font-size: 0.55rem; padding: 2px; }
            .player-token { width: 14px; height: 14px; }
        }
    </style>
</head>
<body class="bg-slate-100 min-h-screen font-sans">

<div id="app" class="max-w-4xl mx-auto p-4">
    <header class="text-center mb-6">
        <h1 class="text-3xl font-bold text-indigo-700">Monopoli Buah Roh</h1>
        <p class="text-slate-600 italic">Implementasi Nilai Kristiani - Galatia 5:22-23</p>
    </header>

    <!-- Lobi -->
    <div id="lobby" class="bg-white p-8 rounded-2xl shadow-xl mb-6 max-w-md mx-auto">
        <h2 class="text-2xl font-semibold mb-2 text-center">Lobi Permainan</h2>
        <p class="text-center text-slate-500 mb-6 text-sm">Gunakan ID yang sama untuk bermain bersama teman.</p>
        <div class="space-y-4">
            <div>
                <label class="block text-sm font-medium text-slate-700 mb-1">ID Ruangan</label>
                <input id="room-id" type="text" placeholder="Contoh: kelas8-a" class="w-full border border-slate-300 p-3 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none">
            </div>
            <button id="join-btn" onclick="joinRoom()" class="w-full bg-indigo-600 text-white py-3 rounded-lg font-bold hover:bg-indigo-700 transition transform active:scale-95 shadow-md">
                MULAI BERMAIN
            </button>
        </div>
    </div>

    <!-- Area Game -->
    <div id="game-area" class="hidden">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <!-- Papan -->
            <div class="md:col-span-2 bg-white p-4 rounded-2xl shadow-lg border-4 border-indigo-50">
                <div id="monopoly-board" class="board-grid bg-slate-200 p-1 rounded-lg">
                    <!-- Cell diisi via JS -->
                </div>
            </div>

            <!-- Kontrol -->
            <div class="flex flex-col gap-4">
                <div class="bg-white p-5 rounded-2xl shadow-lg border border-slate-100">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-slate-800">Daftar Pemain</h3>
                        <span id="room-display" class="text-xs bg-indigo-100 text-indigo-700 px-2 py-1 rounded"></span>
                    </div>
                    <div id="player-list" class="space-y-3"></div>
                </div>

                <div class="bg-white p-6 rounded-2xl shadow-lg text-center border border-slate-100">
                    <div class="text-sm text-slate-500 uppercase tracking-widest mb-1">Hasil Dadu</div>
                    <div id="dice-result" class="text-5xl font-black text-indigo-600 mb-4">-</div>
                    <button id="roll-btn" onclick="rollDice()" class="w-full bg-green-600 text-white py-4 rounded-xl font-bold text-lg hover:bg-green-700 transition disabled:opacity-50 disabled:cursor-not-allowed shadow-lg">
                        KOCOK DADU
                    </button>
                    <p id="turn-indicator" class="mt-3 text-sm font-bold animate-pulse"></p>
                </div>

                <div id="log-box" class="bg-slate-800 text-slate-200 p-4 rounded-2xl h-40 overflow-y-auto font-mono text-xs shadow-inner">
                    <p class="text-indigo-400">> Menunggu pemain...</p>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- Modal -->
<div id="modal" class="fixed inset-0 bg-slate-900/80 hidden items-center justify-center p-4 z-50 backdrop-blur-sm">
    <div class="bg-white max-w-sm w-full p-8 rounded-3xl shadow-2xl transform transition-all scale-100">
        <div id="modal-icon" class="text-4xl mb-4 text-center">📖</div>
        <h2 id="modal-title" class="text-2xl font-bold mb-3 text-center text-slate-800">Refleksi</h2>
        <p id="modal-desc" class="text-slate-600 mb-8 text-center leading-relaxed"></p>
        <button onclick="closeModal()" class="w-full bg-indigo-600 text-white py-3 rounded-xl font-bold hover:bg-indigo-700 transition shadow-lg">
            SAYA MENGERTI
        </button>
    </div>
</div>

<script>
    // Firebase Config
    const firebaseConfig = JSON.parse(__firebase_config);
    const app = firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore(app);
    const auth = firebase.auth(app);
    const appId = typeof __app_id !== 'undefined' ? __app_id : 'monopoly-buah-roh-v2';

    let currentRoomId = '';
    let currentUser = null;
    let gameState = null;
    const INITIAL_COINS = 1000;

    const boardData = [
        { name: "MULAI", type: "start", color: "cell-start", val: 200 },
        { name: "Kasih", type: "fruit", desc: "Membantu teman tanpa pamrih adalah wujud kasih Kristus.", val: 100 },
        { name: "Material", type: "theory", desc: "Nilai yang berguna bagi jasmani (makanan, pakaian).", val: 50 },
        { name: "Sukacita", type: "fruit", desc: "Tetap bersyukur walau sedang dalam kesulitan.", val: 100 },
        { name: "Refleksi", type: "card", desc: "Ambil kartu pertanyaan iman." },
        { name: "Damai Sejahtera", type: "fruit", desc: "Menjadi pembawa damai di tengah pertengkaran.", val: 120 },
        { name: "Kesabaran", type: "fruit", desc: "Menunggu waktu Tuhan dengan tetap berdoa.", val: 120 },
        { name: "Vital", type: "theory", desc: "Nilai yang berguna untuk aktivitas (kekuatan, kesehatan).", val: 50 },
        { name: "Kemurahan", type: "fruit", desc: "Berbagi berkat kepada orang yang membutuhkan.", val: 140 },
        { name: "Refleksi", type: "card", desc: "Ambil kartu pertanyaan iman." },
        { name: "Kebaikan", type: "fruit", desc: "Melakukan kebenaran sesuai Firman Tuhan.", val: 140 },
        { name: "Kesetiaan", type: "fruit", desc: "Tetap setia beribadah dan melayani Tuhan.", val: 160 },
        { name: "Gurun", type: "jail", desc: "Berhenti sejenak untuk berdoa (Lewati giliran)." },
        { name: "Kelemahlembutan", type: "fruit", desc: "Menjawab kata-kata kasar dengan kesantunan.", val: 180 },
        { name: "Kerohanian", type: "theory", desc: "Nilai tertinggi yang berasal dari Tuhan.", val: 50 },
        { name: "Penguasaan Diri", type: "fruit", desc: "Mampu menahan amarah dan nafsu duniawi.", val: 200 },
        { name: "Refleksi", type: "card", desc: "Ambil kartu pertanyaan iman." },
        { name: "Ibadah", type: "bonus", desc: "Mendengar Firman menumbuhkan iman. (+100)", val: 100 },
        { name: "Egois", type: "penalty", desc: "Sifat mementingkan diri sendiri merusak buah roh. (-100)", val: -100 },
        { name: "Integritas", type: "bonus", desc: "Melakukan apa yang benar di mata Tuhan. (+150)", val: 150 }
    ];

    const reflectionQuestions = [
        "Sebutkan satu contoh penguasaan diri saat teman mengejekmu.",
        "Mengapa mendengarkan Firman Tuhan bisa menumbuhkan iman?",
        "Apa perbedaan nilai Vital dan nilai Kerohanian dalam hidupmu?",
        "Bagaimana cara mempraktikkan 'Sabar' saat antre di kantin?",
        "Berikan contoh 'Kasih' yang nyata kepada teman beda agama."
    ];

    function renderBoard() {
        const boardEl = document.getElementById('monopoly-board');
        boardEl.innerHTML = '';
        const perimeterMap = [
            [0,0], [0,1], [0,2], [0,3], [0,4], [0,5],
            [1,5], [2,5], [3,5], [4,5], [5,5],
            [5,4], [5,3], [5,2], [5,1], [5,0],
            [4,0], [3,0], [2,0], [1,0]
        ];

        for(let r=0; r<6; r++) {
            for(let c=0; c<6; c++) {
                const cell = document.createElement('div');
                cell.id = `cell-${r}-${c}`;
                cell.className = 'bg-slate-50 border border-slate-200';
                boardEl.appendChild(cell);
            }
        }

        perimeterMap.forEach((pos, index) => {
            const data = boardData[index];
            if(!data) return;
            const cell = document.getElementById(`cell-${pos[0]}-${pos[1]}`);
            cell.className = `cell shadow-inner ${data.color || 'bg-white'} ${data.type === 'fruit' ? 'cell-fruit' : ''} ${data.type === 'theory' ? 'cell-theory' : ''}`;
            cell.innerHTML = `<span class="font-bold">${data.name}</span><span class="text-[10px] mt-1 text-slate-400">${data.val ? data.val : ''}</span>`;
        });
    }

    async function joinRoom() {
        const id = document.getElementById('room-id').value.trim().toLowerCase();
        if(!id) return;
        
        currentRoomId = id;
        document.getElementById('join-btn').innerText = "MENYAMBUNGKAN...";
        document.getElementById('join-btn').disabled = true;

        try {
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                const result = await firebase.auth().signInWithCustomToken(__initial_auth_token);
                currentUser = result.user;
            } else {
                const result = await firebase.auth().signInAnonymously();
                currentUser = result.user;
            }

            const roomRef = db.collection('artifacts').doc(appId).collection('public').doc('data').collection('rooms').doc(currentRoomId);
            
            roomRef.onSnapshot(doc => {
                if(doc.exists) {
                    gameState = doc.data();
                    if(!gameState.players[currentUser.uid]) {
                        addNewPlayer();
                    }
                    updateUI();
                } else {
                    initRoom();
                }
            }, err => {
                console.error("Firestore Error:", err);
                document.getElementById('join-btn').innerText = "COBA LAGI";
                document.getElementById('join-btn').disabled = false;
            });

            document.getElementById('lobby').classList.add('hidden');
            document.getElementById('game-area').classList.remove('hidden');
            document.getElementById('room-display').innerText = `ID: ${currentRoomId}`;
            renderBoard();
        } catch (e) {
            console.error(e);
            alert("Gagal masuk. Coba lagi.");
        }
    }

    async function initRoom() {
        const roomRef = db.collection('artifacts').doc(appId).collection('public').doc('data').collection('rooms').doc(currentRoomId);
        const initialData = {
            players: {
                [currentUser.uid]: {
                    id: currentUser.uid,
                    name: "Kamu (" + currentUser.uid.substring(0,3) + ")",
                    pos: 0,
                    coins: INITIAL_COINS,
                    color: '#4f46e5',
                    lastAction: 'Baru bergabung'
                }
            },
            turn: currentUser.uid,
            lastRoll: 0,
            updatedAt: Date.now()
        };
        await roomRef.set(initialData);
    }

    async function addNewPlayer() {
        const roomRef = db.collection('artifacts').doc(appId).collection('public').doc('data').collection('rooms').doc(currentRoomId);
        const playerColor = `hsl(${Math.random() * 360}, 70%, 50%)`;
        await roomRef.update({
            [`players.${currentUser.uid}`]: {
                id: currentUser.uid,
                name: "Pemain " + currentUser.uid.substring(0,3),
                pos: 0,
                coins: INITIAL_COINS,
                color: playerColor,
                lastAction: 'Baru bergabung'
            }
        });
    }

    function updateUI() {
        if(!gameState) return;

        // Update list pemain
        const list = document.getElementById('player-list');
        list.innerHTML = '';
        
        // Bersihkan token lama
        document.querySelectorAll('.player-token').forEach(t => t.remove());

        const players = Object.values(gameState.players);
        players.forEach((p, idx) => {
            const isTurn = gameState.turn === p.id;
            const pDiv = document.createElement('div');
            pDiv.className = `p-3 rounded-xl flex justify-between items-center border ${isTurn ? 'border-indigo-500 bg-indigo-50 ring-2 ring-indigo-200' : 'border-slate-100 bg-white'}`;
            pDiv.innerHTML = `
                <div class="flex items-center gap-3">
                    <div class="w-4 h-4 rounded-full" style="background:${p.color}"></div>
                    <div class="flex flex-col">
                        <span class="text-sm font-bold ${p.id === currentUser.uid ? 'text-indigo-600' : 'text-slate-700'}">${p.name} ${p.id === currentUser.uid ? '(Anda)' : ''}</span>
                        <span class="text-[10px] text-slate-400">${p.lastAction}</span>
                    </div>
                </div>
                <div class="text-right">
                    <div class="font-black text-green-600">${p.coins}</div>
                    <div class="text-[9px] uppercase text-slate-400 tracking-tighter">Koin Iman</div>
                </div>
            `;
            list.appendChild(pDiv);

            // Letakkan token di papan
            const coords = getCoords(p.pos);
            const cell = document.getElementById(`cell-${coords[0]}-${coords[1]}`);
            if(cell) {
                const token = document.createElement('div');
                token.className = 'player-token';
                token.style.backgroundColor = p.color;
                // Offset agar tidak menumpuk
                const offset = (idx * 5) - 10;
                token.style.left = `calc(50% + ${offset}px)`;
                token.style.top = `calc(50% + ${offset}px)`;
                cell.appendChild(token);
            }
        });

        const myTurn = gameState.turn === currentUser.uid;
        document.getElementById('dice-result').innerText = gameState.lastRoll || '-';
        document.getElementById('turn-indicator').innerText = myTurn ? "GILIRAN ANDA!" : `Giliran ${gameState.players[gameState.turn]?.name || 'Pemain Lain'}`;
        document.getElementById('turn-indicator').className = `mt-3 text-sm font-bold ${myTurn ? 'text-green-600 animate-bounce' : 'text-slate-400 animate-pulse'}`;
        document.getElementById('roll-btn').disabled = !myTurn;
    }

    function getCoords(index) {
        const perimeterMap = [
            [0,0], [0,1], [0,2], [0,3], [0,4], [0,5],
            [1,5], [2,5], [3,5], [4,5], [5,5],
            [5,4], [5,3], [5,2], [5,1], [5,0],
            [4,0], [3,0], [2,0], [1,0]
        ];
        return perimeterMap[index % perimeterMap.length];
    }

    async function rollDice() {
        if(gameState.turn !== currentUser.uid) return;

        const roll = Math.floor(Math.random() * 6) + 1;
        const player = gameState.players[currentUser.uid];
        let nextPos = (player.pos + roll) % boardData.length;
        let newCoins = player.coins;

        // Bonus Lewat Mulai
        if(nextPos < player.pos) {
            newCoins += 200;
            addToLog("Melewati MULAI, berkat +200 koin!");
        }

        const land = boardData[nextPos];
        let actionMsg = `Maju ${roll} langkah ke ${land.name}`;

        if(land.type === 'fruit' || land.type === 'theory' || land.type === 'bonus') {
            newCoins += land.val;
            showModal(land.name, land.desc, land.type === 'fruit' ? '🍎' : '📖');
        } else if(land.type === 'penalty') {
            newCoins += land.val;
            showModal("Peringatan", land.desc, '⚠️');
        } else if(land.type === 'card') {
            const q = reflectionQuestions[Math.floor(Math.random() * reflectionQuestions.length)];
            newCoins += 50;
            showModal("Kartu Refleksi", q, '💡');
        } else if(land.type === 'jail') {
            showModal("Padang Gurun", "Berhenti sejenak untuk refleksi dan doa.", '⏳');
        }

        // Giliran selanjutnya
        const ids = Object.keys(gameState.players);
        const nextId = ids[(ids.indexOf(currentUser.uid) + 1) % ids.length];

        const roomRef = db.collection('artifacts').doc(appId).collection('public').doc('data').collection('rooms').doc(currentRoomId);
        await roomRef.update({
            turn: nextId,
            lastRoll: roll,
            [`players.${currentUser.uid}.pos`]: nextPos,
            [`players.${currentUser.uid}.coins`]: newCoins,
            [`players.${currentUser.uid}.lastAction`]: actionMsg
        });

        addToLog(`Kamu mengocok ${roll} dan mendarat di ${land.name}`);
    }

    function showModal(title, desc, icon) {
        document.getElementById('modal-title').innerText = title;
        document.getElementById('modal-desc').innerText = desc;
        document.getElementById('modal-icon').innerText = icon || '📖';
        document.getElementById('modal').classList.remove('hidden');
        document.getElementById('modal').classList.add('flex');
    }

    function closeModal() {
        document.getElementById('modal').classList.add('hidden');
        document.getElementById('modal').classList.remove('flex');
    }

    function addToLog(msg) {
        const log = document.getElementById('log-box');
        const p = document.createElement('p');
        p.className = "mb-1";
        p.innerText = `> ${msg}`;
        log.prepend(p);
    }

    // Init UI awal
    renderBoard();
</script>

</body>
</html>

