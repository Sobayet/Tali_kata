<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>টালি খাতা</title>
    <!-- ফন্ট অ্যাওয়েসম লিঙ্ক -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
            color: #333;
        }
        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .container {
            background: rgba(255, 255, 255, 0.8);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 400px;
            text-align: center;
            backdrop-filter: blur(10px);
        }
        h1 {
            color: #4CAF50;
            margin-bottom: 20px;
            font-size: 24px;
        }
        input[type="text"], input[type="number"], select {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
            background: rgba(255, 255, 255, 0.9);
        }
        button {
            background: #4CAF50;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
            font-size: 16px;
            transition: background 0.3s ease;
        }
        button:hover {
            background: #45a049;
        }
        .balance {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }
        .balance span {
            font-weight: bold;
            color: #4CAF50;
        }
        .transaction-list {
            margin-top: 20px;
            text-align: left;
            max-height: 200px;
            overflow-y: auto;
            padding: 10px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 5px;
            border: 1px solid #ddd;
        }
        .transaction-item {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .transaction-item:last-child {
            border-bottom: none;
        }
        .transaction-item .description {
            font-weight: bold;
            color: #333;
        }
        .transaction-item .amount {
            font-weight: bold;
        }
        .transaction-item .type {
            font-size: 12px;
            color: #888;
        }
        .income {
            color: #4CAF50;
        }
        .expense {
            color: #f44336;
        }
        .delete-btn {
            background: none;
            border: none;
            color: #f44336;
            cursor: pointer;
            font-size: 16px;
            transition: color 0.3s ease;
        }
        .delete-btn:hover {
            color: #d32f2f;
        }
        .theme-toggle {
            position: absolute;
            top: 20px;
            right: 20px;
            background: #4CAF50;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            transition: background 0.3s ease;
        }
        .theme-toggle:hover {
            background: #45a049;
        }
        .dark-theme {
            background: linear-gradient(-45deg, #333, #555, #777, #999);
            color: #fff;
        }
        .dark-theme .container {
            background: rgba(0, 0, 0, 0.8);
            color: #fff;
        }
        .dark-theme input[type="text"], .dark-theme input[type="number"], .dark-theme select {
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            border: 1px solid #555;
        }
        .dark-theme .transaction-list {
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #555;
        }
        .dark-theme .transaction-item .description {
            color: #fff;
        }
    </style>
</head>
<body>
    <button class="theme-toggle" onclick="toggleTheme()">ডার্ক থিম</button>
    <div class="container">
        <h1>টালি খাতা</h1>
        <input type="text" id="description" placeholder="লেনদেনের বিবরণ">
        <input type="number" id="amount" placeholder="টাকার পরিমাণ">
        <select id="type">
            <option value="income">পাবো</option>
            <option value="expense">দিব</option>
        </select>
        <button onclick="addTransaction()">যোগ করুন</button>
        <div class="balance">
            <strong>মোট ব্যালেন্স:</strong> <span id="totalBalance">০ টাকা</span>
        </div>
        <div class="transaction-list" id="transactionList">
            <!-- লেনদেনের তালিকা এখানে দেখানো হবে -->
        </div>
    </div>

    <script>
        let transactions = [];
        let totalBalance = 0;

        function addTransaction() {
            const description = document.getElementById('description').value;
            const amount = parseFloat(document.getElementById('amount').value);
            const type = document.getElementById('type').value;

            if (description && !isNaN(amount)) {
                const transaction = {
                    description,
                    amount: type === 'income' ? amount : -amount,
                    type: type === 'income' ? 'পাবো' : 'দিব'
                };
                transactions.push(transaction);
                updateBalance(transaction.amount);
                updateTransactionList();
                clearInputs();
            } else {
                alert('দয়া করে সঠিক বিবরণ এবং টাকার পরিমাণ লিখুন।');
            }
        }

        function updateBalance(amount) {
            totalBalance += amount;
            document.getElementById('totalBalance').textContent = `${totalBalance} টাকা`;
        }

        function updateTransactionList() {
            const transactionList = document.getElementById('transactionList');
            transactionList.innerHTML = '';

            transactions.forEach((transaction, index) => {
                const transactionItem = document.createElement('div');
                transactionItem.className = 'transaction-item';
                transactionItem.innerHTML = `
                    <div>
                        <span class="description">${transaction.description}</span>
                        <span class="type">(${transaction.type})</span>
                    </div>
                    <span class="amount ${transaction.type === 'পাবো' ? 'income' : 'expense'}">${transaction.amount} টাকা</span>
                    <button class="delete-btn" onclick="deleteTransaction(${index})"><i class="fas fa-trash"></i></button>
                `;
                transactionList.appendChild(transactionItem);
            });
        }

        function deleteTransaction(index) {
            const deletedTransaction = transactions.splice(index, 1)[0];
            updateBalance(-deletedTransaction.amount);
            updateTransactionList();
        }

        function clearInputs() {
            document.getElementById('description').value = '';
            document.getElementById('amount').value = '';
        }

        function toggleTheme() {
            const body = document.body;
            body.classList.toggle('dark-theme');
            const themeToggle = document.querySelector('.theme-toggle');
            if (body.classList.contains('dark-theme')) {
                themeToggle.textContent = 'লাইট থিম';
            } else {
                themeToggle.textContent = 'ডার্ক থিম';
            }
        }
    </script>
</body>
</html>
