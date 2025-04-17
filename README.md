<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bạn có muốn một miếng bánh?</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #fff5f5;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            text-align: center;
            background-image: url('https://imgur.com/QERvOth.jpg');
            background-size: cover;
            color: #5d4037;
        }

        .container {
            background-color: rgba(255, 253, 245, 0.95);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            max-width: 500px;
            border: 2px solid #ffcdd2;
        }

        h1 {
            color: #e53935;
            margin-bottom: 20px;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }

        .cake-img {
            width: 200px;
            height: 200px;
            margin: 20px auto;
            background-image: url('https://imgur.com/55NJXRj.png');
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
            transition: transform 0.3s;
        }

        .cake-img:hover {
            transform: scale(1.1);
        }

        .buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
            position: relative;
            height: 100px;
        }

        button {
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            font-size: 18px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        #no-btn {
            background-color: #ef5350;
            color: white;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
        }

        #yes-btn {
            background-color: #66bb6a;
            color: white;
            position: absolute;
        }

        #no-btn:hover {
            background-color: #e53935;
            transform: translateX(-50%) scale(1.05);
        }

        .lore {
            display: none;
            margin-top: 20px;
            padding: 15px;
            background-color: #fff9c4;
            border-radius: 10px;
            border-left: 5px solid #ffd54f;
            text-align: left;
            animation: fadeIn 0.5s;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .run-away {
            animation: runAway 0.5s forwards;
        }

        @keyframes runAway {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1) translate(var(--tx), var(--ty)); }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Bạn có muốn một miếng bánh?</h1>
        <div class="cake-img"></div>
        <div class="buttons">
            <button id="yes-btn">Có</button>
            <button id="no-btn">Không</button>
        </div>
        <div id="lore" class="lore">
            <h3>Lore về chiếc bánh:</h3>
            <p>Đây không phải là chiếc bánh bình thường. Theo truyền thuyết, ai từ chối miếng bánh này sẽ phải chịu lời nguyền ăn kiêng vĩnh viễn. Bạn chắc chứ?</p>
            <p>🍰 Chiếc bánh được làm từ: Bột mì phép thuật, đường tình yêu, và kem hạnh phúc.</p>
        </div>
    </div>

    <script>
        const yesBtn = document.getElementById('yes-btn');
        const noBtn = document.getElementById('no-btn');
        const lore = document.getElementById('lore');
        const buttonsContainer = document.querySelector('.buttons');
        
        // Kích thước container và nút
        const containerRect = buttonsContainer.getBoundingClientRect();
        const btnWidth = yesBtn.offsetWidth;
        const btnHeight = yesBtn.offsetHeight;
        
        // Vị trí ban đầu cho nút Có
        let yesBtnX = containerRect.width * 0.3;
        let yesBtnY = containerRect.height * 0.5;
        
        yesBtn.style.left = `${yesBtnX}px`;
        yesBtn.style.top = `${yesBtnY}px`;
        
        // Khoảng cách kích hoạt chạy
        const runDistance = 50;
        
        // Theo dõi vị trí chuột
        buttonsContainer.addEventListener('mousemove', (e) => {
            const mouseX = e.clientX - containerRect.left;
            const mouseY = e.clientY - containerRect.top;
            
            // Tính khoảng cách từ chuột đến nút Có
            const distance = Math.sqrt(
                Math.pow(mouseX - yesBtnX - btnWidth/2, 2) + 
                Math.pow(mouseY - yesBtnY - btnHeight/2, 2)
            );
            
            // Nếu chuột đến gần nhưng chưa chạm
            if (distance < runDistance && distance > 10) {
                moveYesButton(mouseX, mouseY);
            }
        });
        
        function moveYesButton(mouseX, mouseY) {
            // Tính hướng chạy (ngược hướng chuột)
            const angle = Math.atan2(
                yesBtnY + btnHeight/2 - mouseY, 
                yesBtnX + btnWidth/2 - mouseX
            );
            
            // Khoảng cách chạy
            const runAwayDistance = 100;
            
            // Vị trí mới
            const newX = yesBtnX + Math.cos(angle) * runAwayDistance;
            const newY = yesBtnY + Math.sin(angle) * runAwayDistance;
            
            // Giới hạn trong container
            yesBtnX = Math.max(0, Math.min(containerRect.width - btnWidth, newX));
            yesBtnY = Math.max(0, Math.min(containerRect.height - btnHeight, newY));
            
            // Áp dụng hiệu ứng chạy
            yesBtn.style.setProperty('--tx', `${yesBtnX - parseFloat(yesBtn.style.left || 0)}px`);
            yesBtn.style.setProperty('--ty', `${yesBtnY - parseFloat(yesBtn.style.top || 0)}px`);
            yesBtn.classList.add('run-away');
            
            // Cập nhật vị trí sau khi animation kết thúc
            setTimeout(() => {
                yesBtn.style.left = `${yesBtnX}px`;
                yesBtn.style.top = `${yesBtnY}px`;
                yesBtn.classList.remove('run-away');
            }, 500);
        }
        
        noBtn.addEventListener('click', () => {
            lore.style.display = lore.style.display === 'block' ? 'none' : 'block';
        });
        
        // Nút Có khi click sẽ di chuyển ngẫu nhiên
        yesBtn.addEventListener('click', () => {
            yesBtnX = Math.random() * (containerRect.width - btnWidth);
            yesBtnY = Math.random() * (containerRect.height - btnHeight);
            yesBtn.style.left = `${yesBtnX}px`;
            yesBtn.style.top = `${yesBtnY}px`;
        });
    </script>
</body>
</html>
