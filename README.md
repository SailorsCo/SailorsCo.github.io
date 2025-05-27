<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TV Account Setup Banner</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        .banner {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: white;
            padding: 60px;
            border-radius: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            max-width: 1500px;
            margin: 5px auto;
        }
        .banner-content {
            max-width: 50%;
        }
        .banner h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }
        .banner p {
            font-size: 16px;
            margin-bottom: 20px;
        }
        .banner a {
            background-color: black;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            font-size: 16px;
            border-radius: 5px;
            text-decoration: none;
            display: inline-block;
        }
        .banner a:hover {
            opacity: 0.8;
        }
        .banner img {
            max-width: 45%;
            border-radius: 10px;
        }
        .top-boxes {
            display: flex;
            justify-content: space-between;
            margin: 20px auto;
            max-width: 900px;
            padding: 0 20px;
        }
        .top-box {
            flex: 1;
            margin: 0 10px;
            padding: 20px;
            text-align: center;
            background-color: #f0f0f0;
            border-radius: 10px;
            cursor: pointer;
            min-height: 60px;
        }
        .top-box.editing {
            background-color: #fff8dc;
        }
        @media (max-width: 768px) {
            .banner {
                flex-direction: column;
                text-align: center;
            }
            .banner-content, .banner img {
                max-width: 100%;
            }
            .banner img {
                margin-top: 20px;
            }
            .top-boxes {
                flex-direction: column;
                align-items: center;
            }
            .top-box {
                width: 100%;
                margin: 10px 0;
            }
        }
    </style>
</head>
<body>
    <div class="banner">
        <div class="banner-content">
            <h1>Complete TV Account Setup</h1>
            <p>Complete your Sailors Smart TV account setup to have the best experience while browsing through your TV.</p>
            <a href="https://sites.google.com/view/sailors-smart-tv/account-setup">Complete Account Setup</a>
        </div>
        <img src="https://dl.dropboxusercontent.com/scl/fi/ox4skedg3gevozo2fb8m2/Untitled-design-63.png?rlkey=zh7rure348h3mph64h47vpf4p&e=1&st=0agv0lcj&dl=0" alt="TV Setup Image">
    </div>

    <div class="top-boxes" style="display: none;">
        <div class="top-box" id="left-box">DOUBLE TAP THIS BOX TO ADD NUMBER</div>
        <div class="top-box" id="middle-box">Emergency Service: 112</div>
        <div class="top-box" id="right-box">DOUBLE TAP THIS BOX TO ADD NUMBER</div>
    </div>

    <script>
        function enableEditing(boxId) {
            const box = document.getElementById(boxId);
            box.classList.add('editing');
            const currentText = box.innerText;
            const input = document.createElement('input');
            input.type = 'text';
            input.value = localStorage.getItem(boxId) || '';
            input.style.width = '100%';
            input.style.fontSize = '16px';
            input.style.padding = '10px';

            input.addEventListener('blur', () => {
                const value = input.value.trim() || 'DOUBLE TAP THIS BOX TO ADD NUMBER';
                localStorage.setItem(boxId, value);
                box.innerText = value;
                box.classList.remove('editing');
            });

            box.innerHTML = '';
            box.appendChild(input);
            input.focus();
        }

        document.addEventListener('DOMContentLoaded', () => {
            const leftBox = document.getElementById('left-box');
            const rightBox = document.getElementById('right-box');
            const middleBox = document.getElementById('middle-box');

            // Restore saved values
            [leftBox, rightBox].forEach(box => {
                const saved = localStorage.getItem(box.id);
                if (saved) box.innerText = saved;
            });

            [leftBox, rightBox].forEach(box => {
                let lastTap = 0;
                box.addEventListener('click', () => {
                    const currentTime = new Date().getTime();
                    if (currentTime - lastTap < 300) {
                        enableEditing(box.id);
                    }
                    lastTap = currentTime;
                });
            });

            // Hide banner if already clicked
            if (localStorage.getItem('tvBannerClicked')) {
                const banner = document.querySelector('.banner');
                const topBoxes = document.querySelector('.top-boxes');
                if (banner) banner.style.display = 'none';
                if (topBoxes) topBoxes.style.display = 'flex';
            }

            // Track button click
            const setupButton = document.querySelector('.banner a');
            if (setupButton) {
                setupButton.addEventListener('click', () => {
                    localStorage.setItem('tvBannerClicked', 'true');
                });
            }
        });
    </script>
</body>
</html>
