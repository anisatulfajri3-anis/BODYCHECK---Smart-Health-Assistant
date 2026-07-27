# BODYCHECK---Smart-Health-Assistant
"BODYCHECK adalah asisten kesehatan pintar yang membantu memantau kondisi tubuh, memberikan rekomendasi gaya hidup sehat, serta mendukung pengguna dalam menjaga keseimbangan fisik dan mental secara praktis dan mudah."
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BODYCHECK PRO - Smart Health Assistant</title>
  
  <!-- Google Font Poppins -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  
  <!-- Font Awesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  
  <!-- html2canvas & jsPDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  
  <!-- CSS internal -->
  <style>
    * {margin:0; padding:0; box-sizing:border-box;}
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #6a11cb, #2575fc);
      color: #fff;
      scroll-behavior: smooth;
      overflow-x: hidden;
    }
    section {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 40px;
      position: relative;
    }
    h1, h2 {text-align: center;}
    .card {
      background: rgba(255,255,255,0.1);
      backdrop-filter: blur(10px);
      border-radius: 20px;
      padding: 20px;
      box-shadow: 0 4px 30px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }
    .card:hover {transform: translateY(-10px);}
    button {
      padding: 12px 24px;
      border:none;
      border-radius:30px;
      background:rgba(255,255,255,0.2);
      color:#fff;
      cursor:pointer;
      transition:0.3s;
    }
    button:hover {background:rgba(255,255,255,0.4);}
    nav {
      position:fixed; top:0; left:0; width:100%;
      background:rgba(0,0,0,0.3);
      padding:10px;
      display:flex; justify-content:center;
      transition:0.3s;
    }
    nav.scrolled {background:rgba(0,0,0,0.7);}
    nav a {color:#fff; margin:0 10px; text-decoration:none;}
    #backToTop {
      position:fixed; bottom:20px; right:20px;
      background:#fff; color:#000;
      border-radius:50%; padding:10px;
      cursor:pointer; display:none;
    }
  </style>
</head>
<body>
  <!-- Navbar -->
  <nav id="navbar">
    <a href="#landing">Home</a>
    <a href="#bmi">BMI</a>
    <a href="#fakta">Fakta</a>
    <a href="#form">Form</a>
    <a href="#hasil">Hasil</a>
    <a href="#analisis">Analisis</a>
    <a href="#kalori">Kalori</a>
    <a href="#rekomendasi">Rekomendasi</a>
    <a href="#riwayat">Riwayat</a>
    <a href="#penutup">Penutup</a>
  </nav>

  <!-- SLIDE 1: Landing -->
  <section id="landing">
    <div>
      <h1>BODYCHECK PRO</h1>
      <h2>Smart Health Assistant</h2>
      <p>Aplikasi kesehatan pintar untuk memantau tubuh dan gaya hidup sehat Anda.</p>
      <button onclick="document.getElementById('bmi').scrollIntoView({behavior:'smooth'})">Mulai Sekarang</button>
    </div>
  </section>

  <!-- SLIDE 2: Tentang BMI -->
  <section id="bmi">
    <div class="card">
      <h2>Tentang BMI</h2>
      <p>BMI adalah Body Mass Index, rumus: Berat (kg) / Tinggi² (m²). 
      Indeks ini digunakan untuk mengetahui kategori berat badan seseorang.</p>
    </div>
  </section>

  <!-- SLIDE 3: Fakta Kesehatan -->
  <section id="fakta">
    <div class="card">
      <h2>Fakta Kesehatan</h2>
      <p>💧 Air Putih, 🥗 Nutrisi, 😴 Tidur, 🏃 Olahraga, ❤️ Jantung, 🧠 Mental, ☀ Vitamin D, 🍎 Buah</p>
      <p>Kesehatan tubuh dipengaruhi oleh pola hidup seimbang: cukup minum, makan bergizi, tidur teratur, dan olahraga rutin.</p>
    </div>
  </section>

  <!-- SLIDE 4: Form Input -->
  <section id="form">
    <div class="card">
      <h2>Form Input</h2>
      <p>Masukkan data pribadi Anda untuk menghitung BMI dan kebutuhan kalori.</p>
      <form id="healthForm">
        <input type="text" id="nama" placeholder="Nama" required><br><br>
        <input type="number" id="umur" placeholder="Umur (10-100)" required><br><br>
        <select id="gender"><option>Pria</option><option>Wanita</option></select><br><br>
        <input type="number" id="tinggi" placeholder="Tinggi (cm)" required><br><br>
        <input type="number" id="berat" placeholder="Berat (kg)" required><br><br>
        <button type="button" onclick="hitungBMI()">Hitung</button>
      </form>
    </div>
  </section>

  <!-- SLIDE 5: Hasil BMI -->
  <section id="hasil">
    <div class="card">
      <h2>Hasil BMI</h2>
      <p id="hasilBMI">Isi setelah dihitung...</p>
      <p>Hasil ini menunjukkan kategori berat badan Anda berdasarkan perhitungan BMI.</p>
    </div>
  </section>

  <!-- SLIDE 6: Analisis Tubuh -->
  <section id="analisis">
    <div class="card">
      <h2>Analisis Tubuh</h2>
      <p>Keterangan tambahan:  
      - Kurus → risiko kekurangan nutrisi  
      - Normal → kondisi sehat  
      - Overweight → perlu kontrol pola makan  
      - Obesitas → risiko penyakit jantung/diabetes</p>
    </div>
  </section>

  <!-- SLIDE 7: Kalkulator Kalori -->
  <section id="kalori">
    <div class="card">
      <h2>Kalkulator Kalori</h2>
      <p>Masukkan data aktivitas harian untuk mengetahui kebutuhan kalori tubuh. 
      Kalkulator ini membantu Anda menjaga pola makan sesuai kebutuhan energi.</p>
    </div>
  </section>

  <!-- SLIDE 8: Rekomendasi -->
  <section id="rekomendasi">
    <div class="card">
      <h2>Rekomendasi Cerdas</h2>
      <p>Disarankan olahraga ringan 3x seminggu, perbanyak konsumsi sayur dan buah, 
      serta kurangi makanan tinggi gula dan lemak.</p>
    </div>
  </section>

  <!-- SLIDE 9: Riwayat -->
  <section id="riwayat">
    <div class="card">
      <h2>Riwayat Pemeriksaan</h2>
      <p>Riwayat pemeriksaan Anda akan tersimpan di sini untuk memantau progres kesehatan dari waktu ke waktu.</p>
    </div>
  </section>

  <!-- SLIDE 10: Penutup -->
  <section id="penutup">
    <div class="card">
      <h2>Terima Kasih</h2>
      <p>Tetap jaga kesehatan Anda bersama BODYCHECK PRO.</p>
    </div>
  </section>

  <!-- Back to top -->
  <div id="backToTop" onclick="window.scrollTo({top:0,behavior:'smooth'})"><i class="fa fa-arrow-up"></i></div>

  <!-- Script internal -->
  <script>
    window.addEventListener('scroll',()=>{
      document.getElementById('navbar').classList.toggle('scrolled',window.scrollY>50);
      document.getElementById('backToTop').style.display = window.scrollY>200 ? 'block':'none';
    });

    function hitungBMI(){
  let nama = document.getElementById('nama').value;
  let umur = document.getElementById('umur').value;
  let tinggi = document.getElementById('tinggi').value / 100;
  let berat = document.getElementById('berat').value;
  let bmi = (berat / (tinggi * tinggi)).toFixed(1);
  let status = "";
  
  if(bmi < 18.5) status = "Kurus";
  else if(bmi < 25) status = "Normal";
  else if(bmi < 30) status = "Overweight";
  else status = "Obesitas";
  
  document.getElementById('hasilBMI').innerHTML = 
    `${nama}, Umur ${umur} tahun<br>BMI: ${bmi} (${status})`;
}
