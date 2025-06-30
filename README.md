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

      });
    });
  </script>
</body>
</html>
