<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎵 MuzikPlayer</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        
        :root {
            --primary: #1db954;
            --dark: #121212;
            --dark2: #181818;
            --dark3: #282828;
            --gray: #b3b3b3;
            --white: #fff;
        }
        
        body {
            background: var(--dark);
            color: var(--white);
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .sidebar {
            width: 240px;
            background: var(--dark);
            height: 100vh;
            position: fixed;
            padding: 24px;
            left: 0;
            top: 0;
            display: none;
        }
        
        @media (min-width: 768px) {
            .sidebar { display: block; }
        }
        
        .logo {
            color: var(--primary);
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 40px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .menu-item {
            padding: 12px 16px;
            color: var(--gray);
            cursor: pointer;
            border-radius: 8px;
            margin-bottom: 8px;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .menu-item:hover, .menu-item.active {
            background: var(--dark3);
            color: var(--white);
        }
        
        .main {
            margin-left: 0;
            padding: 24px;
            padding-bottom: 100px;
        }
        
        @media (min-width: 768px) {
            .main { margin-left: 240px; padding: 24px 32px; }
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .search-box {
            background: var(--dark3);
            padding: 12px 20px;
            border-radius: 50px;
            display: flex;
            align-items: center;
            gap: 10px;
            flex: 1;
            max-width: 300px;
        }
        
        .search-box input {
            background: transparent;
            border: none;
            color: var(--white);
            outline: none;
            width: 100%;
        }
        
        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
        }
        
        .upload-section {
            background: linear-gradient(135deg, #1db954 0%, #1ed760 100%);
            border-radius: 16px;
            padding: 40px;
            margin-bottom: 40px;
            position: relative;
            overflow: hidden;
        }
        
        .upload-section h2 {
            font-size: 28px;
            margin-bottom: 15px;
            color: #000;
        }
        
        .upload-section p {
            opacity: 0.8;
            margin-bottom: 25px;
            color: #333;
        }
        
        .upload-btn {
            background: var(--dark);
            color: var(--white);
            padding: 14px 32px;
            border-radius: 50px;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.3s;
        }
        
        .upload-btn:hover { transform: scale(1.05); }
        
        .file-input { display: none; }
        
        .section-title {
            font-size: 24px;
            margin-bottom: 20px;
        }
        
        .songs-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
        }
        
        .song-card {
            background: var(--dark2);
            padding: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .song-card:hover {
            background: var(--dark3);
            transform: translateY(-5px);
        }
        
        .song-card .cover {
            width: 100%;
            aspect-ratio: 1;
            background: linear-gradient(135deg, #333, #555);
            border-radius: 8px;
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            position: relative;
            overflow: hidden;
        }
        
        .song-card .cover i { color: var(--gray); }
        
        .song-card h3 {
            font-size: 14px;
            margin-bottom: 8px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .song-card p {
            color: var(--gray);
            font-size: 12px;
        }
        
        .player-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(to right, #181818, #181818);
            padding: 16px 24px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-top: 1px solid #282828;
            z-index: 100;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .now-playing {
            display: flex;
            align-items: center;
            gap: 15px;
            min-width: 150px;
        }
        
        .now-playing img {
            width: 50px;
            height: 50px;
            border-radius: 4px;
            background: #333;
            display: none;
        }
        
        .now-playing .info h4 { font-size: 14px; }
        .now-playing .info p { font-size: 12px; color: var(--gray); }
        
        .player-controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            flex: 1;
        }
        
        .control-buttons {
            display: flex;
            align-items: center;
            gap: 20px;
        }
        
        .control-buttons i {
            color: var(--gray);
            font-size: 16px;
            cursor: pointer;
            transition: color 0.3s;
        }
        
        .control-buttons i:hover { color: var(--white); }
        
        .control-buttons .play-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--white);
            color: var(--dark);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
        }
        
        .progress-bar {
            width: 100%;
            max-width: 400px;
            height: 4px;
            background: #4d4d4d;
            border-radius: 2px;
            cursor: pointer;
            position: relative;
        }
        
        .progress-bar .progress {
            width: 0%;
            height: 100%;
            background: var(--white);
            border-radius: 2px;
            position: relative;
        }
        
        .progress-bar:hover .progress { background: var(--primary); }
        
        .volume-controls {
            display: flex;
            justify-content: flex-end;
            align-items: center;
            gap: 10px;
            min-width: 100px;
        }
        
        .volume-slider {
            width: 80px;
            height: 4px;
            background: #4d4d4d;
            border-radius: 2px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <div class="logo">
            <i class="fas fa-music"></i>
            MuzikPlayer
        </div>
        
        <div class="menu-item active">
            <i class="fas fa-home"></i>
            Ana Sayfa
        </div>
        <div class="menu-item">
            <i class="fas fa-search"></i>
            Ara
        </div>
        <div class="menu-item">
            <i class="fas fa-book"></i>
            Kütüphane
        </div>
    </div>
    
    <div class="main">
        <div class="header">
            <div class="search-box">
                <i class="fas fa-search"></i>
                <input type="text" placeholder="Şarkı ara">
            </div>
            <div class="user-avatar">S</div>
        </div>
        
        <div class="upload-section">
            <h2>Müziklerini Yükle</h2>
            <p>Kendi müzik koleksiyonunu oluştur!</p>
            <button class="upload-btn" onclick="document.getElementById('fileInput').click()">
                <i class="fas fa-upload"></i> Şarkı Yükle
            </button>
            <input type="file" id="fileInput" class="file-input" accept="audio/*" multiple onchange="handleFiles(this.files)">
        </div>
        
        <h2 class="section-title">🎵 Şarkılarım</h2>
        <div class="songs-grid" id="songsGrid">
            <p style="color: var(--gray);">Henüz şarkı yok. Yukarıdan yükle!</p>
        </div>
    </div>
    
    <div class="player-bar">
        <div class="now-playing">
            <div class="info">
                <h4 id="currentTitle">Şarkı seçilmedi</h4>
                <p id="currentArtist">-</p>
            </div>
        </div>
        
        <div class="player-controls">
            <div class="control-buttons">
                <i class="fas fa-step-backward" onclick="prevSong()"></i>
                <i class="fas fa-play play-btn" id="playBtn" onclick="togglePlay()"></i>
                <i class="fas fa-step-forward" onclick="nextSong()"></i>
            </div>
            <div class="progress-bar" onclick="seek(event)">
                <div class="progress" id="progress"></div>
            </div>
        </div>
        
        <div class="volume-controls">
            <i class="fas fa-volume-up"></i>
            <div class="volume-slider" onclick="setVolume(event)"></div>
        </div>
    </div>
    
    <audio id="audioPlayer"></audio>
    
    <script>
        let songs = [];
        let currentSongIndex = -1;
        let isPlaying = false;
        const audioPlayer = document.getElementById('audioPlayer');
        
        function handleFiles(files) {
            Array.from(files).forEach(file => {
                const song = {
                    title: file.name.replace(/\.[^/.]+$/, ''),
                    artist: 'Bilinmiyor',
                    url: URL.createObjectURL(file)
                };
                songs.push(song);
            });
            displaySongs();
            alert('✅ ' + files.length + ' şarkı yüklendi!');
        }
        
        function displaySongs() {
            const grid = document.getElementById('songsGrid');
            if (songs.length === 0) {
                grid.innerHTML = '<p style="color: var(--gray);">Henüz şarkı yok!</p>';
                return;
            }
            grid.innerHTML = '';
            songs.forEach((song, index) => {
                grid.innerHTML += `
                    <div class="song-card" onclick="playSong(${index})">
                        <div class="cover">
                            <i class="fas fa-music"></i>
                        </div>
                        <h3>${song.title}</h3>
                        <p>${song.artist}</p>
                    </div>
                `;
            });
        }
        
        function playSong(index) {
            currentSongIndex = index;
            const song = songs[index];
            audioPlayer.src = song.url;
            audioPlayer.play();
            isPlaying = true;
            document.getElementById('currentTitle').textContent = song.title;
            document.getElementById('currentArtist').textContent = song.artist;
            document.getElementById('playBtn').classList.remove('fa-play');
            document.getElementById('playBtn').classList.add('fa-pause');
        }
        
        function togglePlay() {
            if (currentSongIndex === -1 && songs.length > 0) {
                playSong(0);
                return;
            }
            if (isPlaying) {
                audioPlayer.pause();
                document.getElementById('playBtn').classList.remove('fa-pause');
                document.getElementById('playBtn').classList.add('fa-play');
            } else {
                audioPlayer.play();
                document.getElementById('playBtn').classList.remove('fa-play');
                document.getElementById('playBtn').classList.add('fa-pause');
            }
            isPlaying = !isPlaying;
        }
        
        function prevSong() {
            if (songs.length === 0) return;
            currentSongIndex = (currentSongIndex - 1 + songs.length) % songs.length;
            playSong(currentSongIndex);
        }
        
        function nextSong() {
            if (songs.length === 0) return;
            currentSongIndex = (currentSongIndex + 1) % songs.length;
            playSong(currentSongIndex);
        }
        
        audioPlayer.ontimeupdate = function() {
            if (audioPlayer.duration) {
                const progress = (audioPlayer.currentTime / audioPlayer.duration) * 100;
                document.getElementById('progress').style.width = progress + '%';
            }
        }
        
        function seek(e) {
            const bar = e.currentTarget;
            const progress = (e.offsetX / bar.offsetWidth) * 100;
            if (audioPlayer.duration) {
                audioPlayer.currentTime = (progress / 100) * audioPlayer.duration;
            }
        }
        
        function setVolume(e) {
            const volume = e.offsetX / e.target.offsetWidth;
            audioPlayer.volume = volume;
        }
        
        audioPlayer.onended = function() {
            nextSong();
        };
    </script>
</body>
</html>
