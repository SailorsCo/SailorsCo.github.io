<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube Recommended Videos</title>
    <script src="https://apis.google.com/js/api.js"></script>
    <script src="https://accounts.google.com/gsi/client" async defer></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .carousel-container { width: 80%; margin: auto; overflow: hidden; position: relative; }
        .carousel { display: flex; transition: transform 0.5s ease-in-out; }
        .video-item { flex: 0 0 20%; padding: 10px; cursor: pointer; }
        .popup { 
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(0, 0, 0, 0.8); justify-content: center; align-items: center;
        }
        .popup-content { position: relative; }
        .popup iframe { width: 80vw; height: 45vw; }
        .close-btn { position: absolute; top: -20px; right: -20px; background: red; color: white; border: none; padding: 10px; cursor: pointer; }
        .nav-button { position: absolute; top: 50%; transform: translateY(-50%); font-size: 24px; cursor: pointer; }
        .prev { left: 10px; }
        .next { right: 10px; }
    </style>
</head>
<body>
    <button onclick="authenticate()">Login with Google</button>
    <div class="carousel-container">
        <div class="carousel" id="videoCarousel"></div>
        <div class="nav-button prev" onclick="moveCarousel(-1)">&#10094;</div>
        <div class="nav-button next" onclick="moveCarousel(1)">&#10095;</div>
    </div>

    <div class="popup" id="videoPopup">
        <div class="popup-content">
            <button class="close-btn" onclick="closePopup()">X</button>
            <iframe id="popupIframe" frameborder="0" allowfullscreen></iframe>
        </div>
    </div>

    <script>
        const API_KEY = 'AIzaSyD3sefiRIcPVnMNwn84qQGTgact7rSyIbs';
        let accessToken = '';
        let carousel = document.getElementById("videoCarousel");
        let currentIndex = 0;

        function authenticate() {
            google.accounts.oauth2.initTokenClient({
                client_id: '238216183607-o0nnlq1ee75s1roo1cc6btf319ro0e1e.apps.googleusercontent.com',
                scope: 'https://www.googleapis.com/auth/youtube.force-ssl',
                callback: (response) => {
                    accessToken = response.access_token;
                    fetchRecommendedVideos();
                }
            }).requestAccessToken();
        }

        function fetchRecommendedVideos() {
            fetch(`https://www.googleapis.com/youtube/v3/videos?part=snippet&chart=mostPopular&regionCode=US&maxResults=10&key=${API_KEY}`, {
                headers: { 'Authorization': `Bearer ${accessToken}` }
            })
            .then(response => response.json())
            .then(data => {
                carousel.innerHTML = '';
                data.items.forEach(video => {
                    let videoItem = document.createElement("div");
                    videoItem.classList.add("video-item");
                    videoItem.dataset.video = `https://www.youtube.com/embed/${video.id}`;
                    videoItem.innerHTML = `<img src="${video.snippet.thumbnails.medium.url}" alt="${video.snippet.title}">`;
                    videoItem.onclick = openPopup;
                    carousel.appendChild(videoItem);
                });
            });
        }

        function moveCarousel(direction) {
            let totalItems = document.querySelectorAll(".video-item").length;
            currentIndex = (currentIndex + direction + totalItems) % totalItems;
            let offset = -currentIndex * 20;
            carousel.style.transform = `translateX(${offset}%)`;
        }

        function openPopup() {
            document.getElementById("popupIframe").src = this.dataset.video;
            document.getElementById("videoPopup").style.display = "flex";
        }

        function closePopup() {
            document.getElementById("videoPopup").style.display = "none";
            document.getElementById("popupIframe").src = "";
        }
    </script>
</body>
</html>
