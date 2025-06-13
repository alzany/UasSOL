<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FIFO Algorithm Demo</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(120deg, #a8edea 0%, #fed6e3 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .main-container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 25px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        .title-section {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            text-align: center;
            padding: 40px 20px;
        }

        .title-section h1 {
            font-size: 3em;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .title-section p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .content-wrapper {
            padding: 30px;
        }

        .students-section {
            margin-bottom: 40px;
        }

        .section-title {
            text-align: center;
            font-size: 2em;
            color: #333;
            margin-bottom: 25px;
            font-weight: bold;
        }

        .students-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 25px;
            margin-bottom: 30px;
        }

        .student-box {
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 100%);
            padding: 25px;
            border-radius: 20px;
            color: #333;
            text-align: center;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }

        .student-box:hover {
            transform: translateY(-5px);
        }

        .student-box h3 {
            font-size: 1.6em;
            margin-bottom: 15px;
            color: #2c3e50;
        }

        .student-detail {
            font-size: 1.1em;
            margin: 8px 0;
            font-weight: 500;
        }

        .fifo-demo {
            background: #f8f9fa;
            border-radius: 20px;
            padding: 30px;
            margin-top: 30px;
        }

        .explanation {
            background: #e3f2fd;
            border-left: 6px solid #2196f3;
            padding: 20px;
            margin-bottom: 25px;
            border-radius: 10px;
        }

        .explanation h3 {
            color: #1976d2;
            margin-bottom: 10px;
            font-size: 1.4em;
        }

        .explanation p {
            color: #555;
            line-height: 1.7;
            font-size: 1.05em;
        }

        .input-section {
            display: flex;
            gap: 15px;
            margin-bottom: 25px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .input-field {
            padding: 15px;
            border: 3px solid #ddd;
            border-radius: 12px;
            font-size: 16px;
            width: 250px;
            transition: all 0.3s ease;
        }

        .input-field:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }

        .btn {
            padding: 15px 25px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 150px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-secondary {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(135deg, #ff6b6b 0%, #feca57 100%);
            color: white;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }

        .queue-visualization {
            background: white;
            border: 4px solid #667eea;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 20px;
            min-height: 150px;
        }

        .queue-header {
            text-align: center;
            font-size: 1.4em;
            font-weight: bold;
            color: #333;
            margin-bottom: 20px;
        }

        .queue-elements {
            display: flex;
            gap: 12px;
            justify-content: center;
            flex-wrap: wrap;
            align-items: center;
        }

        .queue-element {
            background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
            color: #333;
            padding: 15px 20px;
            border-radius: 30px;
            font-weight: bold;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            animation: fadeIn 0.5s ease;
            font-size: 1.1em;
            border: 2px solid #fff;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        .empty-queue {
            color: #999;
            font-style: italic;
            text-align: center;
            font-size: 1.2em;
            padding: 20px;
        }

        .notification {
            text-align: center;
            padding: 15px;
            border-radius: 12px;
            font-weight: bold;
            margin-top: 20px;
            font-size: 1.1em;
            transition: all 0.3s ease;
        }

        .notification.success {
            background: #d4edda;
            color: #155724;
            border: 2px solid #c3e6cb;
        }

        .notification.error {
            background: #f8d7da;
            color: #721c24;
            border: 2px solid #f5c6cb;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 25px;
        }

        .stat-box {
            background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .stat-number {
            font-size: 2em;
            font-weight: bold;
            color: #333;
        }

        .stat-label {
            color: #666;
            font-weight: 500;
            margin-top: 5px;
        }

        @media (max-width: 768px) {
            .students-grid {
                grid-template-columns: 1fr;
            }
            
            .input-section {
                flex-direction: column;
                align-items: center;
            }
            
            .input-field {
                width: 100%;
                max-width: 300px;
            }
            
            .title-section h1 {
                font-size: 2.2em;
            }
        }
    </style>
</head>
<body>
    <div class="main-container">
        <div class="title-section">
            <h1>🚀 UAS SISTEM OPERASI LANJUT - FIFO</h1>
            <p>First In, First Out - Simulasi Antrian Digital</p>
        </div>

        <div class="content-wrapper">
            <div class="students-section">
                <h2 class="section-title">👨‍🎓 Data Mahasiswa</h2>
                <div class="students-grid">
                    <div class="student-box">
                        <h3>📚 Mahasiswa Pertama</h3>
                        <div class="student-detail"><strong>Nama:</strong> Desi Alzany</div>
                        <div class="student-detail"><strong>NIM:</strong> 11240013</div>
                        <div class="student-detail"><strong>Kelas:</strong> IF-TI 2.A</div>
                    </div>
                    <div class="student-box">
                        <h3>💻 Mahasiswa Kedua</h3>
                        <div class="student-detail"><strong>Nama:</strong> Ihban Muhammad Rayyan</div>
                        <div class="student-detail"><strong>NIM:</strong> 11240081</div>
                        <div class="student-detail"><strong>Kelas:</strong> IF-TI 2.A</div>
                    </div>
                    <div class="student-box">
                        <h4>💻 Mahasiswa Ketiga</h4>
                        <div class="student-detail"><strong>Nama:</strong> Mohammad Ikbal</div>
                        <div class="student-detail"><strong>NIM:</strong> 11240080</div>
                        <div class="student-detail"><strong>Kelas:</strong> IF-TI 2.A</div>
                    </div>
                </div>
            </div>

            <div class="fifo-demo">
                <h2 class="section-title">🔄 UAS SISTEM OPERASI LANJUT - FIFO</h2>
                
                
                <div class="input-section">
                    <input type="text" id="dataInput" class="input-field" placeholder="Masukkan data..." maxlength="15">
                    <button class="btn btn-primary" onclick="tambahData()">➕ Tambahkan</button>
                    <button class="btn btn-secondary" onclick="keluarkanData()">➖ Keluarkan</button>
                    <button class="btn btn-danger" onclick="resetQueue()">🗑️ Reset</button>
                </div>

                <div class="queue-visualization">
                    <div class="queue-header">📋 Visualisasi Antrian</div>
                    <div class="queue-elements" id="queueDisplay">
                        <div class="empty-queue">Antrian kosong - Silakan tambahkan data</div>
                    </div>
                </div>

                <div class="stats">
                    <div class="stat-box">
                        <div class="stat-number" id="totalItems">0</div>
                        <div class="stat-label">Total Item</div>
                    </div>
                    <div class="stat-box">
                        <div class="stat-number" id="totalEnqueue">0</div>
                        <div class="stat-label">Total Enqueue</div>
                    </div>
                    <div class="stat-box">
                        <div class="stat-number" id="totalDequeue">0</div>
                        <div class="stat-label">Total Dequeue</div>
                    </div>
                </div>

                <div id="notification" class="notification" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let fifoQueue = [];
        let enqueueCount = 0;
        let dequeueCount = 0;

        function updateQueueDisplay() {
            const display = document.getElementById('queueDisplay');
            
            if (fifoQueue.length === 0) {
                display.innerHTML = '<div class="empty-queue">Antrian kosong - Silakan tambahkan data</div>';
            } else {
                display.innerHTML = fifoQueue.map((item, index) => 
                    `<div class="queue-element">${index + 1}. ${item}</div>`
                ).join('');
            }
            
            updateStats();
        }

        function updateStats() {
            document.getElementById('totalItems').textContent = fifoQueue.length;
            document.getElementById('totalEnqueue').textContent = enqueueCount;
            document.getElementById('totalDequeue').textContent = dequeueCount;
        }

        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.style.display = 'block';
            
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3500);
        }

        function tambahData() {
            const input = document.getElementById('dataInput');
            const data = input.value.trim();
            
            if (data === '') {
                showNotification('⚠️ Mohon masukkan data terlebih dahulu!', 'error');
                return;
            }
            
            if (fifoQueue.length >= 8) {
                showNotification('⚠️ Antrian penuh! Maksimal 8 item.', 'error');
                return;
            }
            
            fifoQueue.push(data);
            enqueueCount++;
            input.value = '';
            updateQueueDisplay();
            showNotification(`✅ "${data}" berhasil ditambahkan ke antrian!`);
        }

        function keluarkanData() {
            if (fifoQueue.length === 0) {
                showNotification('⚠️ Antrian kosong! Tidak ada data untuk dikeluarkan.', 'error');
                return;
            }
            
            const removedData = fifoQueue.shift(); // FIFO: keluarkan elemen pertama
            dequeueCount++;
            updateQueueDisplay();
            showNotification(`✅ "${removedData}" berhasil dikeluarkan dari antrian!`);
        }

        function resetQueue() {
            if (fifoQueue.length === 0) {
                showNotification('⚠️ Antrian sudah kosong!', 'error');
                return;
            }
            
            fifoQueue = [];
            updateQueueDisplay();
            showNotification('🔄 Antrian berhasil direset!');
        }

        // Event listener untuk tombol Enter
        document.getElementById('dataInput').addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                tambahData();
            }
        });

        // Inisialisasi tampilan
        updateQueueDisplay();
    </script>
</body>
</html>
