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
    .card {
      background: rgba(255,255,255,0.1);
      border-radius: 20px;
      padding: 20px;
      margin: 20px auto;
      max-width: 700px;
      text-align: center;
    }
    input, select {padding:10px; margin:5px; border-radius:10px; border:none;}
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
  </style>
</head>
<body>

  <div id="popup">Tetap semangat menjaga kesehatan 💪</div>

  <div class="card">
    <h1>BODYCHECK PRO</h1>
    <h2>Smart Health Assistant</h2>
    <p>Masukkan data Anda untuk analisis kesehatan.</p>
    <input type="text" id="nama" placeholder="Nama" required><br>
    <input type="number" id="umur" placeholder="Umur (10-100)" required><br>
    <select id="gender"><option>Pria</option><option>Wanita</option></select><br>
    <input type="number" id="tinggi" placeholder="Tinggi (cm)" required><br>
    <input type="number" id="berat" placeholder="Berat (kg)" required><br>
    <button onclick="hitungBMI()">Hitung BMI</button>
    <button onclick="exportPDF()">Export PDF</button>
    <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
    
    <div id="progressBar"><div id="progressFill"></div></div>
    <p id="hasilBMI">Hasil akan muncul di sini...</p>
    <div id="badgeArea"></div>
    <canvas id="bmiChart" width="400" height="200"></canvas>
    
    <h3>Riwayat Pemeriksaan</h3>
    <ul id="riwayatList"></ul>
  </div>

<script>
function hitungBMI(){
  let nama = document.getElementById('nama').value;
  let umur = parseInt(document.getElementById('umur').value);
  let tinggi = document.getElementById('tinggi').value / 100;
  let berat = document.getElementById('berat').value;
  let bmi = (berat / (tinggi * tinggi)).toFixed(1);
  let status = "";
  let rekomendasi = "";

  if(bmi < 18.5) status = "Kurus";
  else if(bmi < 25) status = "Normal";
  else if(bmi < 30) status = "Overweight";
  else status = "Obesitas";

  if(status === "Kurus"){
    rekomendasi = umur < 20 ? "Perbanyak nutrisi untuk pertumbuhan." : "Perhatikan asupan kalori agar tidak kekurangan energi.";
  } else if(status === "Normal"){
    rekomendasi = umur < 30 ? "Pertahankan pola hidup sehat dan aktif." : "Jaga pola makan seimbang dan rutin cek kesehatan.";
  } else if(status === "Overweight"){
    rekomendasi = umur < 30 ? "Kurangi makanan cepat saji, tingkatkan olahraga." : "Kontrol pola makan dan konsultasi bila perlu.";
  } else {
    rekomendasi = umur < 30 ? "Segera atur pola makan dan olahraga intensif." : "Waspada risiko penyakit, konsultasi dokter disarankan.";
  }

  document.getElementById('hasilBMI').innerHTML = 
    `${nama}, Umur ${umur} tahun<br>BMI: ${bmi} (${status})<br><b>Rekomendasi:</b> ${rekomendasi}`;

  let progress = Math.min((bmi/40)*100,100);
  document.getElementById('progressFill').style.width = progress+"%";

  const ctx = document.getElementById('bmiChart').getContext('2d');
  if(window.bmiChartInstance) window.bmiChartInstance.destroy();
  window.bmiChartInstance = new Chart(ctx, {
    type: 'radar',
    data: {
      labels: ['Nutrisi','Aktivitas','Tidur','Mental','BMI'],
      datasets: [{
        label: 'Profil Kesehatan',
        data: [Math.random()*10,Math.random()*10,Math.random()*10,Math.random()*10,bmi/3],
        backgroundColor: 'rgba(0,255,153,0.3)',
        borderColor: '#00ff99'
      }]
    }
  });

  let riwayat = JSON.parse(localStorage.getItem('riwayat')) || [];
  riwayat.push({nama, umur, bmi, status, rekomendasi});
  localStorage.setItem('riwayat', JSON.stringify(riwayat));
  tampilkanRiwayat();

  showPopup();
  tampilkanBadge(status);
}

function tampilkanRiwayat(){
  let riwayat = JSON.parse(localStorage.getItem('riwayat')) || [];
  let list = document.getElementById('riwayatList');
  list.innerHTML = "";
  riwayat.forEach(r => {
    let li = document.createElement('li');
    li.innerHTML = `${r.nama} (Umur ${r.umur}) - BMI: ${r.bmi} (${r.status})`;
    list.appendChild(li);
  });
}

function exportPDF(){
  html2canvas(document.querySelector(".card")).then(canvas=>{
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jspdf.jsPDF();
    pdf.addImage(imgData, 'PNG', 10, 10, 180, 160);
    pdf.save("hasil_bodycheck.pdf");
  });
}

function toggleDarkMode(){
  document.body.classList.toggle("dark");
}

function showPopup(){
  let popup = document.getElementById("popup");
  popup.style.display = "block";
  setTimeout(()=>{popup.style.display="none";},3000);
}

function tampilkanBadge(status){
  let badgeArea = document.getElementById("badgeArea");
  badgeArea.innerHTML = "";
  let badge = document.createElement("div");
  badge.classList.add("badge");
  if(status==="Kurus") badge.classList.add("kurus"), badge.innerHTML="🥗";
  else if(status==="Normal") badge.classList.add("normal"), badge.innerHTML="🏆";
  else if(status==="Overweight") badge.classList.add("overweight"), badge.innerHTML="⚖️";
  else badge.classList.add("obesitas"), badge.innerHTML="❤️‍🔥";
  badgeArea.appendChild(badge);
}

tampilkanRiwayat();
</script>

</body>
</html>
