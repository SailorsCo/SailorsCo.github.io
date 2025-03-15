<!DOCTYPEs html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voice Search Button</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: transparent;
        }
        button {
            font-size: 18px;
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <button onclick="startVoiceSearch()">Click to Speak</button>
    <script>
        function startVoiceSearch() {
            const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = 'en-US';
            recognition.start();
            
            recognition.onresult = function(event) {
                const speech = event.results[0][0].transcript;
                window.location.href = `https://www.google.com/search?q=${encodeURIComponent(speech)}`;
            };
            
            recognition.onerror = function(event) {
                alert('Error occurred: ' + event.error);
            };
        }
    </script>
</body>
</html>
