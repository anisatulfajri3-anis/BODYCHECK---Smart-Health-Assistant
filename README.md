# BODYCHECK---Smart-Health-Assistant
"BODYCHECK adalah asisten kesehatan pintar yang membantu memantau kondisi tubuh, memberikan rekomendasi gaya hidup sehat, serta mendukung pengguna dalam menjaga keseimbangan fisik dan mental secara praktis dan mudah."
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BODYCHECK PRO - Smart Health Assistant</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #6a11cb, #2575fc);
      color: #fff;
      margin:0; padding:0;
      transition: background 0.5s, color 0.5s;
    }
    body.dark {background:#111; color:#eee;}
    section {min-height:100vh; display:flex; align-items:center; justify-content:center; padding:40px;}
    .card {
      background: rgba(255,255,255,0.1);
      border-radius: 20px;
      padding: 20px;
      margin: 20px auto;
      max-width: 700px;
      text-align: center;
      backdrop-filter: blur(10px);
    }
    button {
      padding:12px 24px; border:none; border-radius:30px;
      background:#fff; color:#000; cursor:pointer; margin:5px;
    }
    #progressBar {width:100%; background:#ddd; border-radius:20px; margin-top:10px;}
    #progressFill {height:20px; width:0%; background:#00ff99; border-radius:20px; transition:width 1s ease;}
    canvas {margin-top:20px;}
    #riwayatList {text-align:left; margin-top:20px;}
    #popup {
      position:fixed; top:20px; right:20px; background:#ff0066; color:#fff;
      padding:15px; border-radius:10px; display:none; z-index:1000;
    }
    .badge {
      display:inline-block; padding:10px; border-radius:50%; margin:10px;
      font-size:24px; color:#fff;
    }
    .badge.kurus {background:#00bfff;}
    .badge.normal {background:#00ff99;}
    .badge.overweight {background:#ffa500;}
    .badge.obesitas {background:#ff0066;}
    img.icon {width:80px; margin:10px; cursor:pointer; transition:transform 0.3s;}
    img.icon:hover {transform:scale(1.2);}
  </style>
</head>
<body>

  <div id="popup">Tetap semangat menjaga kesehatan 💪</div>

  <!-- SLIDE 1: Landing -->
  <section id="landing">
    <div class="card">
      <h1>BODYCHECK PRO</h1>
      <h2>Smart Health Assistant</h2>
      <p>Aplikasi kesehatan pintar untuk memantau tubuh dan gaya hidup sehat Anda.</p>
      <img src="https://cdn-icons-png.flaticon.com/512/2966/2966486.png" class="icon" onclick="alert('Mulai perjalanan sehatmu sekarang!')">
      <br>
      <button onclick="document.getElementById('form').scrollIntoView({behavior:'smooth'})">Mulai Sekarang</button>
      <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
    </div>
  </section>

  <!-- SLIDE 2: Tentang BMI -->
  <section id="bmi">
    <div class="card">
      <h2>Tentang BMI</h2>
      <p>BMI adalah Body Mass Index, rumus: Berat (kg) / Tinggi² (m²). 
      Indeks ini digunakan untuk mengetahui kategori berat badan seseorang.</p>
      <img src="https://cdn-icons-png.flaticon.com/512/1046/1046784.png" class="icon">
    </div>
  </section>

  <!-- SLIDE 3: Fakta Kesehatan -->
  <section id="fakta">
    <div class="card">
      <h2>Fakta Kesehatan</h2>
      <p>Kesehatan tubuh dipengaruhi oleh pola hidup seimbang.</p>
      <img src="https://cdn-icons-png.flaticon.com/512/415/415733.png" class="icon" onclick="alert('Minum cukup air setiap hari!')">
      <img src="https://cdn-icons-png.flaticon.com/512/1046/1046786.png" class="icon" onclick="alert('Konsumsi buah dan sayur segar!')">
      <img src="https://cdn-icons-png.flaticon.com/512/1046/1046787.png" class="icon" onclick="alert('Tidur cukup untuk kesehatan mental!')">
    </div>
  </section>

  <!-- SLIDE 4: Form Input -->
  <section id="form">
    <div class="card">
      <h2>Form Input</h2>
      <p>Masukkan data pribadi Anda untuk menghitung BMI dan kebutuhan kalori.</p>
      <form id="healthForm">
        <input type="text" id="nama" placeholder="Nama" required><br>
        <input type="number" id="umur" placeholder="Umur (10-100)" required><br>
        <select id="gender"><option>Pria</option><option>Wanita</option></select><br>
        <input type="number" id="tinggi" placeholder="Tinggi (cm)" required><br>
        <input type="number" id="berat" placeholder="Berat (kg)" required><br>
        <button type="button" onclick="hitungBMI()">Hitung BMI</button>
        <button type="button" onclick="exportPDF()">Export PDF</button>
      </form>
      <div id="progressBar"><div id="progressFill"></div></div>
      <p id="hasilBMI">Hasil akan muncul di sini...</p>
      <div id="badgeArea"></div>
      <canvas id="bmiChart" width="400" height="200"></canvas>
    </div>
  </section>

  <!-- SLIDE 5: Hasil BMI -->
  <section id="hasil">
    <div class="card">
      <h2>Hasil BMI</h2>
      <p>Hasil ini menunjukkan kategori berat badan Anda berdasarkan perhitungan BMI.</p>
    </div>
  </section>

  <!-- SLIDE 6: Analisis Tubuh -->
  <section id="analisis">
    <div class="card">
      <h2>Analisis Tubuh</h2>
      <p>Keterangan tambahan: Kurus → risiko nutrisi, Normal → sehat, Overweight → kontrol pola makan, Obesitas → risiko penyakit.</p>
      <img src="https://cdn-icons-png.flaticon.com/512/1046/1046785.png" class="icon">
    </div>
  </section>

  <!-- SLIDE 7: Kalkulator Kalori -->
  <section id="kalori">
    <div class="card">
      <h2>Kalkulator Kalori</h2>
      <p>Masukkan data aktivitas harian untuk mengetahui kebutuhan kalori tubuh.</p>
      <input type="range" min="1" max="5" value="3" id="aktivitas" oninput="updateKalori()">
      <p id="kaloriOutput">Level aktivitas: 3</p>
    </div>
  </section>

  <!-- SLIDE 8: Rekomendasi -->
  <section id="rekomendasi">
    <div class="card">
      <h2>Rekomendasi Cerdas</h2>
      <p>Olahraga ringan 3x seminggu, konsumsi sayur dan buah, kurangi gula dan lemak.</p>
      <img src="https://cdn-icons-png.flaticon.com/512/1046/1046788.png" class="icon">
    </div>
  </section>

  <!-- SLIDE 9: Riwayat -->
  <section id="riwayat">
    <div class="card">
      <h2>Riwayat Pemeriksaan</h2>
      <ul id="riwayatList"></ul>
    </div>
  </section>

  <!-- SLIDE 10: Penutup -->
  <section id="penutup">
    <div class="card">
      <h2>Terima Kasih</h2>
      <p>Tetap jaga kesehatan Anda bersama BODYCHECK PRO.</p>
      <img src="https://cdn-icons-png.flaticon.com/
            <img src="https://cdn-icons-png.flaticon.com/512/833/833472.png" class="icon" onclick="alert('Terima kasih sudah menggunakan BODYCHECK PRO!')">
    </div>
  </section>

</body>
</html>
