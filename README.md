<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8" />
  <title>حضور و غیاب مدرسه جذب</title>
  <style>
    body { font-family: sans-serif; direction: rtl; padding: 20px; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
    select { padding: 5px; }
    button { padding: 10px 15px; margin-top: 20px; }
  </style>
</head>
<body>
  <h1>حضور و غیاب مدرسه جذب</h1>
  <label for="responsible">مسئول کلاس: </label>
  <select id="responsible">
    <option>سرگروه ۱</option>
    <option>سرگروه ۲</option>
  </select>

  <table id="attendance-table">
    <thead>
      <tr>
        <th>نام طلبه</th>
        <th>وضعیت</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <button onclick="exportToExcel()">خروجی اکسل</button>

  <script>
    const students = Array.from({length: 50}, (_, i) => `طلبه ${i + 1}`);
    const statusOptions = ["حاضر", "غیبت موجه", "غیبت غیرموجه", "تأخیر"];
    const tbody = document.querySelector('#attendance-table tbody');

    students.forEach(student => {
      const tr = document.createElement('tr');
      const tdName = document.createElement('td');
      tdName.textContent = student;

      const tdStatus = document.createElement('td');
      const select = document.createElement('select');
      statusOptions.forEach(status => {
        const option = document.createElement('option');
        option.value = status;
        option.textContent = status;
        select.appendChild(option);
      });
      tdStatus.appendChild(select);

      tr.appendChild(tdName);
      tr.appendChild(tdStatus);
      tbody.appendChild(tr);
    });

    function exportToExcel() {
      let rows = [["نام طلبه", "وضعیت"]];
      const trs = document.querySelectorAll('#attendance-table tbody tr');
      trs.forEach(tr => {
        const name = tr.cells[0].innerText;
        const status = tr.cells[1].querySelector('select').value;
        rows.push([name, status]);
      });

      let csvContent = "\uFEFF" + rows.map(e => e.join(",")).join("\n"); // BOM برای فارسی
      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.setAttribute("download", "attendance.csv");
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
  </script>
</body>
</html>
