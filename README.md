<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>kulucky - สถิติต่ำ-สูง</title>
  <style>
    body {
      font-family: 'Sarabun', sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background: linear-gradient(to bottom, #ffecd2, #fcb69f);
    }
    h1 {
      margin-top: 30px;
      color: #ffffff;
      text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
    }
    h2 {
      margin-top: 10px;
      color: #333;
    }
    .container {
      background: white;
      max-width: 400px;
      margin: 30px auto;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }
    input {
      width: 220px;
      font-size: 18px;
      padding: 5px;
      margin-top: 10px;
      border: 2px solid #ff7f50;
      border-radius: 5px;
    }
    button {
      font-size: 18px;
      margin: 10px 5px;
      padding: 8px 20px;
      cursor: pointer;
      background-color: #ff7f50;
      color: white;
      border: none;
      border-radius: 5px;
      transition: 0.2s;
    }
    button:hover {
      background-color: #ff5722;
    }
    #result {
      margin-top: 20px;
      font-size: 20px;
      color: #222;
    }
    img {
      width: 100px;
      margin-top: 20px;
      animation: float 3s infinite ease-in-out;
    }
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
  </style>
</head>
<body>
  <h1>🎲 kulucky 🎲</h1>
  <div class="container">
    <h2>กรอกเลข 10 หลักล่าสุด</h2>
    <input type="text" id="inputNumber" maxlength="10" placeholder="เช่น 6538520643">
    <br>
    <button onclick="analyze()">วิเคราะห์</button>
    <button onclick="resetForm()">ล้างเลข</button>

    <div id="result"></div>
    <img id="emojiImage" src="https://cdn-icons-png.flaticon.com/512/3082/3082030.png" alt="lucky icon">
  </div>

  <!-- เสียงเอฟเฟกต์ -->
  <audio id="analyzeSound" src="https://cdn.pixabay.com/download/audio/2022/03/10/audio_7751e7e5f2.mp3?filename=correct-answer-2-46134.mp3"></audio>
  <audio id="resetSound" src="https://cdn.pixabay.com/download/audio/2022/03/10/audio_5fd7a39b2b.mp3?filename=button-click-13884.mp3"></audio>

  <script>
    // ประวัติ 10 ตาล่าสุด
    let history = [];

    function analyze() {
      const numStr = document.getElementById("inputNumber").value.trim();

      // ตรวจว่ากรอกเป็นเลข 10 หลัก
      if (!/^\d{10}$/.test(numStr)) {
        alert("กรุณากรอกตัวเลข 10 หลักให้ถูกต้อง");
        return;
      }

      document.getElementById("analyzeSound").play();

      let low = 0;  // 0-4
      let high = 0; // 5-9
      let even = 0; // เลขคู่
      let odd = 0;  // เลขคี่

      // วิเคราะห์เลขแต่ละหลัก
      for (let char of numStr) {
        const digit = parseInt(char);
        if (digit <= 4) {
          low++;
        } else {
          high++;
        }

        // ตรวจสอบเลขคู่/คี่
        if (digit % 2 === 0) {
          even++;
        } else {
          odd++;
        }
      }

      // เพิ่มผลการวิเคราะห์ลงใน history
      if (history.length >= 10) {
        history.shift(); // ลบข้อมูลเก่าที่เกิน 10
      }
      history.push({ low, high, even, odd });

      // ทำนายตาถัดไป
      let predictedLowHigh = (high > low) ? "สูง" : (low > high ? "ต่ำ" : "เท่ากัน");
      let predictedEvenOdd = (even > odd) ? "คู่" : (odd > even ? "คี่" : "เท่ากัน");

      // สรุปผล
      let resultHTML = `ต่ำ (0-4): ${low} ครั้ง<br>สูง (5-9): ${high} ครั้ง<br>`;
      if (low > high) {
        resultHTML += "✅ ออกต่ำเยอะกว่า";
        document.getElementById("emojiImage").src = "https://cdn-icons-png.flaticon.com/512/1031/1031881.png";
      } else if (high > low) {
        resultHTML += "✅ ออกสูงเยอะกว่า";
        document.getElementById("emojiImage").src = "https://cdn-icons-png.flaticon.com/512/1031/1031923.png";
      } else {
        resultHTML += "⚖️ ต่ำ-สูง ออกเท่ากัน";
        document.getElementById("emojiImage").src = "https://cdn-icons-png.flaticon.com/512/616/616408.png";
      }

      resultHTML += `<br><strong>จำนวนเลขคู่: ${even}</strong><br>`;
      resultHTML += `<strong>จำนวนเลขคี่: ${odd}</strong><br>`;
      resultHTML += `<br><strong>ทำนายตาถัดไป: ${predictedLowHigh} และ ${predictedEvenOdd}</strong><br>`;

      document.getElementById("result").innerHTML = resultHTML;
    }

    function resetForm() {
      document.getElementById("inputNumber").value = "";
      document.getElementById("result").innerHTML = "";
      document.getElementById("emojiImage").src = "https://cdn-icons-png.flaticon.com/512/3082/3082030.png";
      document.getElementById("resetSound").play();
    }
  </script>
</body>
</html>
