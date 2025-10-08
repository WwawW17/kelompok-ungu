<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Perkalian Sederhana</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f8ff;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 400px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            color: #333;
        }
        #soal {
            font-size: 2em;
            margin: 20px 0;
            color: #007bff;
        }
        #counter {
            font-size: 1.2em;
            color: #666;
            margin: 10px 0;
        }
        input[type="number"] {
            font-size: 1.5em;
            padding: 10px;
            width: 100px;
            margin: 10px;
            border: 2px solid #ddd;
            border-radius: 5px;
        }
        button {
            font-size: 1.2em;
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        button:hover {
            background-color: #218838;
        }
        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        #skor {
            font-size: 1.5em;
            margin: 20px 0;
            color: #28a745;
        }
        #feedback {
            font-size: 1.2em;
            margin: 10px 0;
            min-height: 30px;
        }
        .benar {
            color: green;
        }
        .salah {
            color: red;
        }
        #gameOver {
            display: none;
            font-size: 1.5em;
            color: #007bff;
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Game Perkalian</h1>
        <p>Jawab soal perkalian berikut! Target: 10 soal.</p>
        
        <div id="soal">Siap untuk soal pertama...</div>
        <div id="counter">Soal 0/10</div>
        <input type="number" id="jawaban" placeholder="Jawabanmu" disabled aria-label="Masukkan jawaban perkalian">
        <br>
        <button id="mulaiBtn" onclick="mulaiGame()">Mulai Game</button>
        <button id="submitBtn" onclick="cekJawaban()" disabled aria-label="Kirim jawaban">Submit Jawaban</button>
        <button id="nextBtn" onclick="soalBaru()" disabled aria-label="Soal berikutnya">Soal Baru</button>
        <button id="resetBtn" onclick="resetGame()" disabled aria-label="Mulai ulang game">Reset Game</button>
        
        <div id="skor">Skor: 0</div>
        <div id="feedback"></div>
        <div id="gameOver"></div>
    </div>

    <script>
        let angka1, angka2, jawabanBenar;
        let skor = 0;
        let soalCount = 0;
        let maxSoal = 10;
        let gameMulai = false;

        function mulaiGame() {
            gameMulai = true;
            skor = 0;
            soalCount = 0;
            document.getElementById('skor').innerText = 'Skor: 0';
            document.getElementById('counter').innerText = 'Soal 0/10';
            document.getElementById('mulaiBtn').disabled = true;
            document.getElementById('resetBtn').disabled = false;
            document.getElementById('submitBtn').disabled = true; // Disabled awal
            document.getElementById('nextBtn').disabled = true;
            document.getElementById('jawaban').disabled = false;
            document.getElementById('jawaban').value = '';
            document.getElementById('feedback').innerText = '';
            document.getElementById('gameOver').style.display = 'none';
            soalBaru();
        }

        function soalBaru() {
            if (soalCount >= maxSoal) {
                akhiriGame();
                return;
            }
            
            angka1 = Math.floor(Math.random() * 12) + 1; // Angka 1-12
            angka2 = Math.floor(Math.random() * 12) + 1;
            jawabanBenar = angka1 * angka2;
            soalCount++;
            document.getElementById('soal').innerText = `${angka1} Ã— ${angka2} = ?`;
            document.getElementById('counter').innerText = `Soal ${soalCount}/${maxSoal}`;
            document.getElementById('jawaban').value = '';
            document.getElementById('jawaban').focus();
            document.getElementById('feedback').innerText = '';
            document.getElementById('submitBtn').disabled = true; // Disabled sampai input valid
            document.getElementById('nextBtn').disabled = true;
            
            // Trigger input event untuk disable submit
            document.getElementById('jawaban').dispatchEvent(new Event('input'));
        }

        function cekJawaban() {
            const inputElement = document.getElementById('jawaban');
            const jawabanUser  = parseInt(inputElement.value);
            const feedback = document.getElementById('feedback');

            if (isNaN(jawabanUser ) || inputElement.value === '') {
                feedback.innerText = 'Masukkan jawaban angka yang valid!';
                feedback.className = 'salah';
                inputElement.focus();
                return; // Hentikan eksekusi
            }

            if (jawabanUser  === jawabanBenar) {
                skor++;
                feedback.innerText = 'Benar! Kerja bagus!';
                feedback.className = 'benar';
            } else {
                feedback.innerText = `Salah! Jawaban benar adalah ${jawabanBenar}.`;
                feedback.className = 'salah';
            }

            document.getElementById('skor').innerText = `Skor: ${skor}`;
            inputElement.value = '';
            document.getElementById('submitBtn').disabled = true;
            document.getElementById('nextBtn').disabled = false;
        }

        function akhiriGame() {
            const gameOver = document.getElementById('gameOver');
            const persentase = Math.round((skor / maxSoal) * 100);
            gameOver.innerText = `Game Selesai! Skor akhir: ${skor}/${maxSoal} (${persentase}%).`;
            gameOver.style.display = 'block';
            document.getElementById('submitBtn').disabled = true;
            document.getElementById('nextBtn').disabled = true;
            document.getElementById('jawaban').disabled = true;
            document.getElementById('resetBtn').disabled = false;
        }

        function resetGame() {
            gameMulai = false;
            skor = 0;
            soalCount = 0;
            document.getElementById('soal').innerText = 'Siap untuk soal pertama...';
            document.getElementById('counter').innerText = 'Soal 0/10';
            document.getElementById('skor').innerText = 'Skor: 0';
            document.getElementById('feedback').innerText = '';
            document.getElementById('gameOver').style.display = 'none';
            document.getElementById('mulaiBtn').disabled = false;
            document.getElementById('submitBtn').disabled = true;
            document.getElementById('nextBtn').disabled = true;
            document.getElementById('resetBtn').disabled = true;
            document.getElementById('jawaban').disabled = true;
            document.getElementById('jawaban').value = '';
        }

        // Event listener untuk Enter key
        document.getElementById('jawaban').addEventListener('keypress', function(event) {
            if (event.key === 'Enter' && !document.getElementById('submitBtn').disabled) {
                cekJawaban();
            }
        });

        // Event listener untuk enable/disable submit berdasarkan input
        document.getElementById('jawaban').addEventListener('input', function() {
            const submitBtn = document.getElementById('submitBtn');
            const value = this.value.trim();
            const isValid = value !== '' && !isNaN(parseInt(value));
            submitBtn.disabled = !isValid;
        });
    </script>
</body>
</html>
