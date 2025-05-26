<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>হোল্ডিং ট্যাক্স কালেকশন</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #f4f7fa;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    form {
      max-width: 600px;
      margin: 0 auto 30px auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgb(0 0 0 / 0.1);
    }
    form div {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: 600;
    }
    input[type="text"],
    input[type="number"],
    input[type="date"] {
      width: 100%;
      padding: 10px;
      border-radius: 4px;
      border: 1px solid #ccc;
      font-size: 16px;
      box-sizing: border-box;
    }
    button {
      background-color: #2980b9;
      color: white;
      padding: 12px 20px;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #1f618d;
    }
    table {
      width: 90%;
      margin: 0 auto;
      border-collapse: collapse;
      box-shadow: 0 0 10px rgb(0 0 0 / 0.1);
      background: #fff;
      border-radius: 8px;
      overflow: hidden;
    }
    th, td {
      padding: 12px 15px;
      border-bottom: 1px solid #ddd;
      text-align: left;
    }
    th {
      background-color: #2980b9;
      color: white;
    }
    tr:hover {
      background-color: #f1f1f1;
    }
    .invoice-btn {
      background-color: #27ae60;
      border: none;
      padding: 8px 12px;
      color: white;
      border-radius: 4px;
      cursor: pointer;
      font-weight: 600;
      transition: background-color 0.3s ease;
    }
    .invoice-btn:hover {
      background-color: #1e8449;
    }
    @media (max-width: 700px) {
      form, table {
        width: 100%;
      }
    }
  </style>
</head>
<body>

  <h1>হোল্ডিং ট্যাক্স কালেকশন</h1>

  <form id="taxForm">
    <div>
      <label for="name">নাগরিকের নাম</label>
      <input type="text" id="name" placeholder="নাম লিখুন" required />
    </div>
    <div>
      <label for="holdingNo">হোল্ডিং নম্বর</label>
      <input type="text" id="holdingNo" placeholder="হোল্ডিং নম্বর" required />
    </div>
    <div>
      <label for="address">ঠিকানা</label>
      <input type="text" id="address" placeholder="ঠিকানা লিখুন" required />
    </div>
    <div>
      <label for="taxAmount">ট্যাক্সের পরিমাণ (টাকা)</label>
      <input type="number" id="taxAmount" placeholder="ট্যাক্সের পরিমাণ" required min="0" />
    </div>
    <div>
      <label for="dueDate">পেমেন্টের শেষ তারিখ</label>
      <input type="date" id="dueDate" required />
    </div>
    <button type="submit">নাগরিক যোগ করুন</button>
  </form>

  <table id="citizenTable" style="display:none;">
    <thead>
      <tr>
        <th>নাম</th>
        <th>হোল্ডিং নম্বর</th>
        <th>ঠিকানা</th>
        <th>ট্যাক্স</th>
        <th>শেষ তারিখ</th>
        <th>চালান</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    const form = document.getElementById('taxForm');
    const table = document.getElementById('citizenTable');
    const tbody = table.querySelector('tbody');

    form.addEventListener('submit', function (e) {
      e.preventDefault();

      // তথ্য সংগ্রহ
      const name = document.getElementById('name').value.trim();
      const holdingNo = document.getElementById('holdingNo').value.trim();
      const address = document.getElementById('address').value.trim();
      const taxAmount = document.getElementById('taxAmount').value.trim();
      const dueDate = document.getElementById('dueDate').value;

      // নতুন সারি তৈরি
      const tr = document.createElement('tr');

      tr.innerHTML = `
        <td>${name}</td>
        <td>${holdingNo}</td>
        <td>${address}</td>
        <td>${taxAmount} টাকা</td>
        <td>${dueDate}</td>
        <td><button class="invoice-btn">চালান দেখুন</button></td>
      `;

      tbody.appendChild(tr);
      table.style.display = 'table';

      // ফর্ম রিসেট
      form.reset();

      // চালান বোতামে ক্লিক ইভেন্ট
      const invoiceBtn = tr.querySelector('.invoice-btn');
      invoiceBtn.addEventListener('click', () => {
        showInvoice({ name, holdingNo, address, taxAmount, dueDate });
      });
    });

    function showInvoice(data) {
      const invoiceWindow = window.open('', 'চালান', 'width=600,height=700');
      invoiceWindow.document.write(`
        <html lang="bn">
          <head>
            <title>হোল্ডিং ট্যাক্স চালান</title>
            <style>
              body { font-family: 'SolaimanLipi', Arial, sans-serif; padding: 30px; }
              h2 { text-align: center; color: #2980b9; }
              .invoice-box {
                border: 2px solid #2980b9;
                padding: 20px;
                border-radius: 10px;
                max-width: 500px;
                margin: auto;
              }
              .invoice-box table {
                width: 100%;
                border-collapse: collapse;
              }
              .invoice-box table td {
                padding: 8px;
                border: 1px solid #ddd;
              }
              .invoice-box .title {
                font-size: 24px;
                font-weight: bold;
                margin-bottom: 20px;
                text-align: center;
              }
              .center {
                text-align: center;
              }
              .footer {
                margin-top: 30px;
                font-size: 14px;
                color: #555;
                text-align: center;
              }
            </style>
          </head>
          <body>
            <div class="invoice-box">
              <div class="title">হোল্ডিং ট্যাক্স চালান</div>
              <table>
                <tr><td>নাম</td><td>${data.name}</td></tr>
                <tr><td>হোল্ডিং নম্বর</td><td>${data.holdingNo}</td></tr>
                <tr><td>ঠিকানা</td><td>${data.address}</td></tr>
                <tr><td>ট্যাক্সের পরিমাণ</td><td>${data.taxAmount} টাকা</td></tr>
                <tr><td>পেমেন্টের শেষ তারিখ</td><td>${data.dueDate}</td></tr>
              </table>
              <p class="center">আপনি অনুগ্রহ করে নির্দিষ্ট তারিখের মধ্যে ট্যাক্স প্রদান করবেন।</p>
              <div class="footer">সর্বস্বত্ব সংরক্ষিত © 2025</div>
            </div>
          </body>
        </html>
      `);
      invoiceWindow.document.close();
      invoiceWindow.focus();
    }
  </script>

</body>
</html>
 -
