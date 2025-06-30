<!DOCTYPE html>
<html>
<head>
  <title>ระบบบันทึกรายการเสื้อกีฬา</title>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-storage.js"></script>
</head>
<body>
  <h2>เพิ่มรายการสินค้าเสื้อกีฬา</h2>

  <form id="itemForm">
    วันที่: <input type="date" id="date"><br><br>
    N/R: <input type="text" id="nr"><br><br>
    เลขจ็อบ: <input type="text" id="job"><br><br>
    ชื่องาน: <input type="text" id="name"><br><br>
    กำหนดส่ง: <input type="date" id="deadline"><br><br>
    สถานะ:
    <select id="status">
      <option>รอแอฟพรูพ</option>
      <option>พิมพ์</option>
      <option>ฮีต</option>
      <option>เย็บ</option>
      <option>พับแพ็ค</option>
      <option>เสร็จแล้ว</option>
      <option>ส่งแล้ว</option>
      <option>รอซ่อม</option>
    </select><br><br>
    หมายเหตุ: <input type="text" id="note"><br><br>
    รูปตัวอย่าง: <input type="file" id="image"><br><br>

    <button type="submit">บันทึก</button>
  </form>

  <hr>
  <h3>รายการที่บันทึกไว้</h3>
  <table border="1" id="dataTable">
    <thead>
      <tr>
        <th>วันที่</th><th>N/R</th><th>Job</th><th>งาน</th><th>ส่ง</th><th>สถานะ</th><th>หมายเหตุ</th><th>รูป</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    // === Firebase Configuration ===
    var firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://YOUR_PROJECT.firebaseio.com",
      projectId: "YOUR_PROJECT",
      storageBucket: "YOUR_PROJECT.appspot.com",
      messagingSenderId: "SENDER_ID",
      appId: "APP_ID"
    };
    firebase.initializeApp(firebaseConfig);
    var db = firebase.database();

    // === Add Item with Image Upload ===
    document.getElementById("itemForm").addEventListener("submit", function(e) {
      e.preventDefault();

      var fileInput = document.getElementById("image");
      var file = fileInput.files[0];

      var item = {
        date: document.getElementById("date").value,
        nr: document.getElementById("nr").value,
        job: document.getElementById("job").value,
        name: document.getElementById("name").value,
        deadline: document.getElementById("deadline").value,
        status: document.getElementById("status").value,
        note: document.getElementById("note").value,
        image: ""
      };

      if (file) {
        // Upload the image to Firebase Storage
        var storageRef = firebase.storage().ref('images/' + Date.now() + "_" + file.name);
        storageRef.put(file).then(function(snapshot) {
          snapshot.ref.getDownloadURL().then(function(url) {
            item.image = url;
            db.ref("items").push(item);
            alert("บันทึกแล้ว");
            document.getElementById("itemForm").reset();
          });
        }).catch(function(error) {
          alert("เกิดข้อผิดพลาดในการอัปโหลดรูป: " + error.message);
        });
      } else {
        item.image = "";
        db.ref("items").push(item);
        alert("บันทึกแล้ว");
        document.getElementById("itemForm").reset();
      }
    });

    // === Load Items ===
    db.ref("items").on("value", function(snapshot) {
      var tbody = document.querySelector("#dataTable tbody");
      tbody.innerHTML = "";
      snapshot.forEach(function(child) {
        var item = child.val();
        var row = `<tr>
          <td>${item.date}</td><td>${item.nr}</td><td>${item.job}</td>
          <td>${item.name}</td><td>${item.deadline}</td>
          <td>${item.status}</td><td>${item.note}</td>
          <td>${item.image ? `<img src="${item.image}" width="80"/>` : ""}</td>
        </tr>`;
        tbody.innerHTML += row;
      });
    });
  </script>
</body>
</html>
