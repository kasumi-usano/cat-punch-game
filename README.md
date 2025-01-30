<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>猫パンチでお金を貯める</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
        }
        #money {
            font-size: 24px;
            margin-bottom: 20px;
        }
        button {
            font-size: 20px;
            padding: 10px 20px;
            cursor: pointer;
            margin: 10px;
        }
    </style>
</head>
<body>
    <h1>猫パンチ💥でお金を貯めよう！</h1>
    <div id="money">現在の残高: ¥0.00</div>
    <button id="punchBtn">猫パンチ！</button>
    <button id="withdrawBtn" disabled>出金申請</button>

    <script>
        const username = "test_user"; // 仮のユーザー名
        const balanceDisplay = document.getElementById("money");
        const punchBtn = document.getElementById("punchBtn");
        const withdrawBtn = document.getElementById("withdrawBtn");
        const minWithdraw = 300.00;

        async function fetchBalance() {
            const response = await fetch(`http://localhost:5000/balance/${username}`);
            const data = await response.json();
            balanceDisplay.textContent = `現在の残高: ¥${data.balance.toFixed(2)}`;
            withdrawBtn.disabled = data.balance < minWithdraw;
        }

        punchBtn.addEventListener("click", async () => {
            await fetch("http://localhost:5000/click", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ username })
            });
            fetchBalance();
        });

        withdrawBtn.addEventListener("click", async () => {
            const response = await fetch("http://localhost:5000/withdraw", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ username })
            });
            const data = await response.json();
            alert(data.message);
            fetchBalance();
        });

        fetchBalance();
    </script>
</body>
</html>
