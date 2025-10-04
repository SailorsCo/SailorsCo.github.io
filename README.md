<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Search Bar with Suggestions + Voice</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: url("https://SailorsCo.github.io/cdn/img/Black-Camo-Wallpaper.png")
        no-repeat center center fixed;
      background-size: cover;
      margin: 0;
      flex-direction: column;
    }
    .search-container {
      display: flex;
      border: 5px solid #bb9fff;
      border-radius: 40px;
      overflow: hidden;
      background-color: white;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      transition: border-color 0.3s;
      width: 600px;
      max-width: 90%;
      position: relative;
      flex-direction: column;
    }
    .search-bar {
      display: flex;
      width: 100%;
      align-items: center;
    }
    .search-bar button {
      background: none;
      border: none;
      cursor: pointer;
      padding: 10px;
      margin: 3.7px;
    }
    .search-bar button img {
      width: 30px;
      height: 30px;
    }
    .search-bar input[type="text"] {
      border: none;
      padding: 12px 20px;
      font-size: 16px;
      outline: none;
      flex: 1;
      color: #6f34ff;
    }
    .search-bar input[type="text"]::placeholder {
      color: #6f34ff;
      opacity: 1;
    }
    .search-container:focus-within {
      border-color: #6f34ff;
    }
    .suggestions {
      display: none;
      width: 100%;
      background: white;
      border-top: 1px solid #bb9fff;
      border-radius: 0 0 20px 20px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      z-index: 10;
    }
    .suggestions div {
      padding: 10px;
      cursor: pointer;
      color: #6f34ff;
    }
    .suggestions div:hover {
      background-color: #f0f0f0;
    }

    /* ✅ Clock */
    #clock {
      position: absolute;
      top: 15px;
      left: 15px;
      background-color: white;
      color: #8654ff;
      border: 2px solid #8654ff;
      border-radius: 10px;
      padding: 8px 15px;
      font-size: 18px;
      font-weight: bold;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
      font-family: "Courier New", monospace;
    }

    /* ✅ Shortcuts */
    .shortcuts-container {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      gap: 15px;
      margin-top: 25px;
    }
    .shortcut {
      width: 70px;
      height: 70px;
      background: white;
      border: 2px solid #8654ff;
      border-radius: 15px;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      cursor: pointer;
      transition: transform 0.2s ease;
    }
    .shortcut:hover {
      transform: scale(1.05);
    }
    .shortcut img {
      width: 35px;
      height: 35px;
      border-radius: 8px;
    }
    .delete-btn {
      position: absolute;
      top: -10px;
      right: -10px;
      background: red;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 10px;
      padding: 3px 5px;
      cursor: pointer;
      display: none;
    }
    footer {
      position: absolute;
      bottom: 10px;
      text-align: center;
      color: white;
      font-size: 14px;
    }
    footer a {
      color: #bb9fff;
      font-weight: bold;
      text-decoration: none;
    }
    footer a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>
  <div id="clock">00:00:00</div>

  <form class="search-container" id="searchForm">
    <div class="search-bar">
      <button type="submit">
        <img src="https://SailorsCo.github.io/cdn/img/Search-Icon.png" alt="Search" />
      </button>

      <input
        type="text"
        id="searchInput"
        name="query"
        placeholder="Search the Internet..."
        required
      />

      <button type="button" id="voiceButton">
        <img src="https://SailorsCo.github.io/cdn/img/Microphone-Icon.png" alt="Voice Search" />
      </button>
    </div>
    <div class="suggestions" id="suggestions"></div>
  </form>

  <!-- ✅ Shortcuts row -->
  <div class="shortcuts-container" id="shortcuts"></div>

  <footer>
    Copyright © 2025
    <a
      href="https://sites.google.com/view/SailorsCo/products/Sailors-Browser/Copyright"
      target="_blank"
      ><b>Sailors Browser</b></a
    >
    All rights reserved
  </footer>

  <script>
    const searchForm = document.getElementById("searchForm");
    const searchInput = document.getElementById("searchInput");
    const suggestionsBox = document.getElementById("suggestions");
    const voiceButton = document.getElementById("voiceButton");
    const shortcutsContainer = document.getElementById("shortcuts");

    // ✅ Clock
    function updateClock() {
      const clock = document.getElementById("clock");
      const now = new Date();
      const hours = String(now.getHours()).padStart(2, "0");
      const minutes = String(now.getMinutes()).padStart(2, "0");
      const seconds = String(now.getSeconds()).padStart(2, "0");
      clock.textContent = `${hours}:${minutes}:${seconds}`;
    }
    setInterval(updateClock, 1000);
    updateClock();

    // ✅ Search Suggestions
    function showSuggestions(query) {
      if (!query) {
        suggestionsBox.style.display = "none";
        return;
      }
      suggestionsBox.innerHTML = `
        <div id="openUrl">🔗 Open as URL (${query})</div>
        <div id="googleSearch">🌐 Search on Google: ${query}</div>
      `;
      suggestionsBox.style.display = "block";

      document.getElementById("openUrl").onclick = () => {
        let url =
          query.startsWith("http://") || query.startsWith("https://")
            ? query
            : "http://" + query;
        window.location.href = url;
      };
      document.getElementById("googleSearch").onclick = () => {
        window.location.href =
          "https://www.google.com/search?q=" + encodeURIComponent(query);
      };
    }

    searchInput.addEventListener("input", () => {
      showSuggestions(searchInput.value.trim());
    });

    searchForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const query = searchInput.value.trim();
      if (query) {
        window.location.href =
          "https://www.google.com/search?q=" + encodeURIComponent(query);
      }
    });

    document.addEventListener("click", (e) => {
      if (!searchForm.contains(e.target)) {
        suggestionsBox.style.display = "none";
      }
    });

    // ✅ Voice
    voiceButton.addEventListener("click", () => {
      const SpeechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) {
        alert("Your browser does not support voice recognition.");
        return;
      }
      const recognition = new SpeechRecognition();
      recognition.lang = "en-US";
      recognition.interimResults = false;
      recognition.maxAlternatives = 1;
      recognition.start();

      recognition.onresult = (event) => {
        const spokenText = event.results[0][0].transcript;
        searchInput.value = spokenText;
        showSuggestions(spokenText);
      };
    });

    // ✅ Shortcuts
    const defaultShortcuts = [
      "https://youtube.com",
      "https://chatgpt.com",
      "https://sites.google.com/view/SailorsCo",
      "https://news.google.com",
      "https://github.com",
      "https://chat.google.com",
    ];

    function loadShortcuts() {
      let shortcuts = JSON.parse(localStorage.getItem("shortcuts")) || defaultShortcuts;
      renderShortcuts(shortcuts);
    }

    function renderShortcuts(shortcuts) {
      shortcutsContainer.innerHTML = "";
      shortcuts.forEach((url, index) => {
        const shortcutDiv = document.createElement("div");
        shortcutDiv.className = "shortcut";
        shortcutDiv.innerHTML = `<img src="https://www.google.com/s2/favicons?domain=${url}" alt="icon" />`;
        shortcutDiv.onclick = () => window.open(url, "_blank");

        // Hover delete
        let hoverTimer;
        let deleteBtn = document.createElement("button");
        deleteBtn.textContent = "Delete";
        deleteBtn.className = "delete-btn";
        deleteBtn.onclick = (e) => {
          e.stopPropagation();
          shortcuts.splice(index, 1);
          localStorage.setItem("shortcuts", JSON.stringify(shortcuts));
          renderShortcuts(shortcuts);
        };
        shortcutDiv.appendChild(deleteBtn);

        shortcutDiv.addEventListener("mouseenter", () => {
          hoverTimer = setTimeout(() => {
            deleteBtn.style.display = "block";
          }, 2000);
        });

        shortcutDiv.addEventListener("mouseleave", () => {
          clearTimeout(hoverTimer);
          deleteBtn.style.display = "none";
        });

        shortcutsContainer.appendChild(shortcutDiv);
      });

      // Add shortcut button
      if (shortcuts.length < 8) {
        const addDiv = document.createElement("div");
        addDiv.className = "shortcut";
        addDiv.innerHTML = `<img src="https://sailorsco.github.io/cdn/img/Plus-Icon.png" alt="Add" />`;
        addDiv.onclick = () => {
          const newUrl = prompt("Enter the URL of the shortcut you want to add:");
          if (newUrl && shortcuts.length < 8) {
            shortcuts.push(newUrl);
            localStorage.setItem("shortcuts", JSON.stringify(shortcuts));
            renderShortcuts(shortcuts);
          }
        };
        shortcutsContainer.appendChild(addDiv);
      }
    }

    loadShortcuts();
  </script>
</body>
</html>
