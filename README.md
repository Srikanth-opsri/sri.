<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>sriChef Kitchen - Billing Counter</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #fff8f0;
      margin: 0;
      padding: 0;
      color: #333;
    }

    header {
      background-color: #d35400;
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 2.5rem;
      font-weight: bold;
      letter-spacing: 3px;
      font-family: 'Georgia', serif;
      user-select: none;
      box-shadow: 0 3px 8px rgba(211, 84, 0, 0.6);
    }

    header span.logo {
      color: #f39c12;
      font-style: italic;
    }

    main {
      max-width: 900px;
      margin: 30px auto;
      background: white;
      padding: 20px 30px;
      border-radius: 10px;
      box-shadow: 0 5px 20px rgba(211, 84, 0, 0.15);
    }

    h2 {
      color: #d35400;
      margin-top: 0;
      margin-bottom: 15px;
    }

    /* Menu Grid */
    #menu {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(140px,1fr));
      gap: 20px;
      margin-bottom: 40px;
    }

    .menu-item {
      border: 2px solid #d35400;
      border-radius: 10px;
      overflow: hidden;
      cursor: pointer;
      transition: box-shadow 0.3s ease;
      background: #fff5eb;
      text-align: center;
      user-select: none;
    }

    .menu-item:hover {
      box-shadow: 0 0 15px #d35400;
    }

    .menu-item img {
      width: 100%;
      height: 110px;
      object-fit: cover;
      display: block;
      border-bottom: 2px solid #d35400;
    }

    .menu-item .item-name {
      font-weight: 600;
      padding: 8px 5px 4px;
      font-size: 1rem;
      color: #c0392b;
    }

    .menu-item .item-price {
      font-size: 0.9rem;
      padding-bottom: 8px;
      color: #e67e22;
    }

    /* Billing Form */
    form {
      display: flex;
      gap: 15px;
      flex-wrap: wrap;
      margin-bottom: 25px;
      align-items: center;
    }

    form input[type="text"],
    form input[type="number"] {
      padding: 10px;
      font-size: 1rem;
      border: 2px solid #d35400;
      border-radius: 6px;
      flex: 1 1 150px;
    }

    form button {
      background-color: #d35400;
      color: white;
      border: none;
      padding: 12px 25px;
      font-size: 1rem;
      border-radius: 6px;
      cursor: pointer;
      transition: background-color 0.3s ease;
      flex: 0 0 120px;
    }

    form button:hover {
      background-color: #e67e22;
    }

    /* Order Table */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 25px;
    }

    table th,
    table td {
      border: 1px solid #d35400;
      padding: 12px 15px;
      text-align: center;
      font-size: 1rem;
    }

    table th {
      background-color: #f39c12;
      color: white;
    }

    /* Total */
    .total {
      font-size: 1.5rem;
      font-weight: bold;
      text-align: right;
      margin-bottom: 20px;
      color: #d35400;
    }

    /* Buttons */
    .clear-btn,
    #generateReceiptBtn {
      background-color: #d35400;
      color: white;
      border: none;
      padding: 12px 30px;
      font-size: 1rem;
      border-radius: 6px;
      cursor: pointer;
      margin-right: 10px;
      transition: background-color 0.3s ease;
    }

    .clear-btn:hover,
    #generateReceiptBtn:hover {
      background-color: #e67e22;
    }

    /* Receipt styling */
    #receipt {
      background: #fff5e6;
      border: 2px solid #d35400;
      padding: 20px 30px;
      margin-top: 30px;
      border-radius: 10px;
      display: none;
      white-space: pre-wrap;
      font-family: 'Courier New', Courier, monospace;
      color: #6e2c00;
    }

    #printReceiptBtn {
      margin-top: 15px;
      background-color: #c0392b;
      border: none;
      color: white;
      padding: 10px 25px;
      font-size: 1rem;
      border-radius: 6px;
      cursor: pointer;
      display: none;
      transition: background-color 0.3s ease;
    }
    #printReceiptBtn:hover {
      background-color: #e74c3c;
    }

    @media (max-width: 600px) {
      form {
        flex-direction: column;
      }
      form button {
        flex: 1 1 100%;
      }
      .clear-btn,
      #generateReceiptBtn {
        margin-bottom: 10px;
        width: 100%;
      }
      #printReceiptBtn {
        width: 100%;
      }
    }
  </style>
</head>
<body>

  <header>
    <span class="logo">sriChef</span> Kitchen - Billing Counter
  </header>

  <main>

    <h2>Menu - Click an item to add to order</h2>
    <div id="menu">
      <!-- Sample Indian dishes -->
      <div class="menu-item" data-name="Butter Chicken" data-price="250">
        <img src="https://source.unsplash.com/400x300/?butter-chicken" alt="Butter Chicken" />
        <div class="item-name">Butter Chicken</div>
        <div class="item-price">₹250</div>
      </div>
      <div class="menu-item" data-name="Paneer Tikka" data-price="180">
        <img src="https://source.unsplash.com/400x300/?paneer-tikka" alt="Paneer Tikka" />
        <div class="item-name">Paneer Tikka</div>
        <div class="item-price">₹180</div>
      </div>
      <div class="menu-item" data-name="Masala Dosa" data-price="100">
        <img src="https://source.unsplash.com/400x300/?masala-dosa" alt="Masala Dosa" />
        <div class="item-name">Masala Dosa</div>
        <div class="item-price">₹100</div>
      </div>
      <div class="menu-item" data-name="Biryani" data-price="220">
        <img src="https://source.unsplash.com/400x300/?biryani" alt="Biryani" />
        <div class="item-name">Biryani</div>
        <div class="item-price">₹220</div>
      </div>
      <div class="menu-item" data-name="Gulab Jamun" data-price="80">
        <img src="https://source.unsplash.com/400x300/?gulab-jamun" alt="Gulab Jamun" />
        <div class="item-name">Gulab Jamun</div>
        <div class="item-price">₹80</div>
      </div>
      <div class="menu-item" data-name="Naan" data-price="40">
        <img src="https://source.unsplash.com/400x300/?naan" alt="Naan" />
        <div class="item-name">Naan</div>
        <div class="item-price">₹40</div>
      </div>
    </div>

    <h2>Add Item Manually</h2>
    <form id="itemForm">
      <input type="text" id="itemName" placeholder="Item Name" required />
      <input type="number" id="itemQty" placeholder="Quantity" min="1" value="1" required />
      <input type="number" id="itemPrice" placeholder="Price per Item (₹)" min="0" step="0.01" required />
      <button type="submit">Add Item</button>
    </form>

    <table id="orderTable">
      <thead>
        <tr>
          <th>Item</th>
          <th>Quantity</th>
          <th>Price (₹)</th>
          <th>Total (₹)</th>
        </tr>
      </thead>
      <tbody>
        <!-- Items added will appear here -->
      </tbody>
    </table>

    <div class="total" id="totalAmount">Total: ₹0.00</div>

    <button class="clear-btn" id="clearBtn">Clear Order</button>
    <button id="generateReceiptBtn">Generate Receipt</button>

    <pre id="receipt"></pre>
    <button id="printReceiptBtn">Print Receipt</button>

  </main>

  <script>
    const itemForm = document.getElementById('itemForm');
    const orderTableBody = document.querySelector('#orderTable tbody');
    const totalAmount = document.getElementById('totalAmount');
    const clearBtn = document.getElementById('clearBtn');
    const generateReceiptBtn = document.getElementById('generateReceiptBtn');
    const receipt = document.getElementById('receipt');
    const printReceiptBtn = document.getElementById('printReceiptBtn');
    const menuItems = document.querySelectorAll('.menu-item');

    let orderItems = [];

    function updateTable() {
      orderTableBody.innerHTML = '';
      let total = 0;

      orderItems.forEach(({ name, qty, price }) => {
        const itemTotal = qty * price;
        total += itemTotal;

        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${name}</td>
          <td>${qty}</td>
          <td>₹${price.toFixed(2)}</td>
          <td>₹${itemTotal.toFixed(2)}</td>
        `;
        orderTableBody.appendChild(tr);
      });

      totalAmount.textContent = `Total: ₹${total.toFixed(2)}`;
    }

    function addItem(name, qty, price) {
      // Check if item exists already in order, increase qty if so
      const existingIndex = orderItems.findIndex(i => i.name.toLowerCase() === name.toLowerCase());
      if (existingIndex >= 0) {
        orderItems[existingIndex].qty += qty;
      } else {
        orderItems.push({ name, qty, price });
      }
      updateTable();
    }

    // Add manual form submit
    itemForm.addEventListener('submit', e => {
      e.preventDefault();
      const name = document.getElementById('itemName').value.trim();
      const qty = parseInt(document.getElementById('itemQty').value);
      const price = parseFloat(document.getElementById('itemPrice').value);
      if (name && qty > 0 && price >= 0) {
        addItem(name, qty, price);
        itemForm.reset();
        document.getElementById('itemQty').value = 1; // reset qty
      }
    });

    // Add menu item click handlers
    menuItems.forEach(item => {
      item.addEventListener('click', () => {
        const name = item.getAttribute('data-name');
        const price = parseFloat(item.getAttribute('data-price'));
        addItem(name, 1, price);
      });
    });

    clearBtn.addEventListener('click', () => {
      orderItems = [];
      updateTable();
      receipt.style.display = 'none';
      printReceiptBtn.style.display = 'none';
      generateReceiptBtn.style.display = 'inline-block';
    });

    // Receipt generation
    generateReceiptBtn.addEventListener('click', () => {
      if (orderItems.length === 0) {
        alert('Please add some items before generating receipt.');
        return;
      }

      let receiptText = '';
      const shopName = 'sriChef Kitchen';
      const address = '100 Gourmet Lane, Culinary City, India';
      const contact = 'Phone: +91 9985545009';

      const date = new Date();
      const dateStr = date.toLocaleString();

      receiptText += `*** ${shopName} ***\n`;
      receiptText += `${address}\n${contact}\n`;
      receiptText += `Date: ${dateStr}\n`;
      receiptText += `------------------------------------\n`;
      receiptText += `Item               Qty    Price    Total\n`;
      receiptText += `------------------------------------\n`;

      let total = 0;
      orderItems.forEach(({ name, qty, price }) => {
        const itemTotal = qty * price;
        total += itemTotal;
        // Format spacing (fixed width approx)
        let itemName = name.length > 15 ? name.slice(0, 15) + '.' : name;
        let qtyStr = qty.toString().padStart(3, ' ');
        let priceStr = `₹${price.toFixed(2)}`.padStart(7, ' ');
        let itemTotalStr = `₹${itemTotal.toFixed(2)}`.padStart(8, ' ');
        receiptText += `${itemName.padEnd(17, ' ')}${qtyStr}  ${priceStr} ${itemTotalStr}\n`;
      });

      receiptText += `------------------------------------\n`;
      receiptText += `TOTAL AMOUNT: ₹${total.toFixed(2)}\n`;
      receiptText += `------------------------------------\n`;
      receiptText += `Thank you for dining with us!\n`;
      receiptText += `Visit Again!\n`;

      receipt.textContent = receiptText;
      receipt.style.display = 'block';
      printReceiptBtn.style.display = 'inline-block';
      generateReceiptBtn.style.display = 'none';
    });

    // Print receipt
    printReceiptBtn.addEventListener('click', () => {
      const printWindow = window.open('', '', 'width=400,height=600');
      printWindow.document.write('<pre style="font-family: Courier New, monospace; font-size: 16px;">');
      printWindow.document.write(receipt.textContent.replace(/\n/g, '<br>'));
      printWindow.document.write('</pre>');
      printWindow.document.close();
      printWindow.focus();
      printWindow.print();
      printWindow.close();
    });

    // Initialize empty table
    updateTable();
  </script>

</body>
</html>
