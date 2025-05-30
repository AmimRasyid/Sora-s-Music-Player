# Sora-s-Music-Player

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Sora's Player</title>
  <link rel="icon" type="image/x-icon" href="profile.jpg">
  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      overflow: hidden;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      color: #fff;
      /* Background gradasi kuning ke putih */
      background: linear-gradient(358deg, #1e1e1e 0%, #3e3d00 100%);
    }
    .sidebar {
      width: 260px;
      background: #1e1e1e;
      display: flex;
      flex-direction: column;
      padding: 15px;
      box-sizing: border-box;
      overflow: hidden;
    }
    .sidebar h2 {
      margin: 0 0 15px 0; /* Menambah margin bawah */
      font-size: 24px; /* Memperbesar ukuran font */
      padding: 10px 0; /* Menambah padding atas/bawah */
      color: #ffeb3b;
      text-align: center; /* Menengahkan teks */
    }
    .search-container {
      margin-bottom: 8px;
      position: relative;
    }
    .search-input {
      width: 100%;
      padding: 6px 35px 6px 10px;
      border-radius: 6px;
      border: none;
      font-size: 14px;
      background: #333;
      color: #ffeb3b;
      box-sizing: border-box;
    }
    .search-input::placeholder {
      color: #ffeb3b99;
    }
    .search-icon {
      position: absolute;
      top: 7px;
      right: 10px;
      pointer-events: none;
      color: #ffeb3b99;
      font-size: 16px;
      user-select: none;
    }
    .playlist {
      flex: 1;
      overflow-y: auto;
      border: 1px solid #444;
      border-radius: 6px;
      background: #222;
      padding: 8px;
      user-select: none;
      /* Custom Scrollbar Styles */
      scrollbar-width: thin; /* Firefox */
      scrollbar-color: #ffeb3b #222; /* Firefox (thumb track) */
    }
    /* Webkit (Chrome, Safari) scrollbar styles */
    .playlist::-webkit-scrollbar {
      width: 8px;
    }
    .playlist::-webkit-scrollbar-track {
      background: #222;
      border-radius: 10px;
    }
    .playlist::-webkit-scrollbar-thumb {
      background-color: #ffeb3b;
      border-radius: 10px;
      border: 2px solid #222;
    }
    .playlist-item {
      padding: 6px 6px 6px 48px;
      border-radius: 4px;
      margin-bottom: 6px;
      cursor: grab; /* Change cursor for draggable items */
      color: #ddd;
      display: flex;
      align-items: center;
      transition: background 0.25s;
      position: relative;
      background-color: #333; /* Make items more distinct */
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }
    .playlist-item:hover {
      background: #555519;
      color: #ffeb3b;
    }
    .playlist-item.selected {
      background: #ffeb3b;
      color: #121212;
      font-weight: 700;
    }
    .playlist-item.dragging {
      opacity: 0.7;
      border: 2px dashed #ffeb3b;
    }
    .playlist-item button {
      /* Adjusted styles for a smaller, less obtrusive remove button */
      background: none; /* No background */
      border: none;
      color: #ffeb3b; /* Yellow text for visibility */
      cursor: pointer;
      font-size: 18px; /* Slightly larger X for clickability */
      padding: 0 5px; /* Reduced padding */
      border-radius: 4px;
      user-select: none;
      opacity: 0.7; /* Slightly transparent when not hovered */
      transition: opacity 0.2s, color 0.2s;
      position: absolute;
      right: 8px;
      top: 50%;
      transform: translateY(-50%);
      z-index: 10;
      font-weight: bold;
    }
    .playlist-item button:hover {
      opacity: 1;
      color: #fdd835; /* Brighter yellow on hover */
    }
    .thumb {
      position: absolute;
      left: 6px;
      top: 50%;
      transform: translateY(-50%);
      width: 36px;
      height: 24px;
      background: #444;
      border-radius: 3px;
      overflow: hidden;
      flex-shrink: 0;
      box-shadow: 0 0 4px #000 inset;
    }
    .thumb img, .thumb video {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }
    .upload-container {
      margin-top: 10px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }
    .upload-label, .clear-all-button { /* Apply similar styling to new button */
      cursor: pointer;
      background: #ffeb3b;
      color: #121212;
      padding: 7px 14px;
      border-radius: 6px;
      font-weight: 600;
      box-shadow: 0 0 8px #ffeb3baa;
      transition: background 0.3s;
      user-select: none;
      border: none;
      font-size: 14px;
    }
    .upload-label:hover, .clear-all-button:hover {
      background: #fdd835;
    }
    #videoUpload {
      display: none;
    }
    .main-container {
      flex: 1;
      display: flex;
      flex-direction: column;
      padding: 15px;
      box-sizing: border-box;
      overflow: hidden;
      position: relative; /* Penting untuk posisi absolut profil dan indikator volume */
      align-items: center;
    }
    .video-header {
      width: 100%;
      display: flex;
      justify-content: space-between; /* Untuk menempatkan item di ujung-ujung */
      align-items: center;
      background: #333;
      padding: 10px 15px;
      border-radius: 0px;
      margin-bottom: 10px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    }
    .header-title {
        font-size: 20px;
        font-weight: 600;
        color: #ffeb3b;
        user-select: none;
    }
    .header-button {
      background: #ffeb3b;
      color: #121212;
      padding: 8px 15px;
      border-radius: 6px;
      font-weight: 600;
      border: none;
      cursor: pointer;
      transition: transform 0.2s ease-in-out, background 0.3s;
      user-select: none;
    }
    .header-button:hover {
      transform: scale(1.05);
      background: #fdd835;
    }
    .profile-container {
      position: absolute;
      top: 15px; /* Sesuaikan jarak dari atas */
      right: 15px; /* Sesuaikan jarak dari kanan */
      z-index: 20; /* Pastikan di atas elemen lain */
    }
    .profile-container img {
      width: 40px; /* Ukuran foto profil */
      height: 40px; /* Ukuran foto profil */
      border-radius: 50%; /* Membuat foto melingkar */
      object-fit: cover;
      border: 3px solid #ffeb3b; /* Border kuning */
      box-shadow: 0 0 10px rgba(255, 235, 59, 0.5);
      transition: transform 0.3s ease, box-shadow 0.3s ease; /* Transisi untuk hover */
    }
    .profile-container img:hover {
      transform: scale(1.1); /* Membesar saat dihover */
      box-shadow: 0 0 20px rgba(255, 235, 59, 0.8), 0 0 30px rgba(255, 235, 59, 0.6); /* Cahaya lebih terang */
    }
    video {
      width: 100%;
      max-height: 370px; /* reduced max height */
      border-radius: 10px;
      background: black;
      box-shadow: 0 0 30px #ffeb3bcc;
      user-select: none;
      margin-bottom: 20px;
      transition: max-height 0.3s ease;
      object-fit: contain;
    }
    canvas.waveform {
      width: 100%;
      height: 100px;
      background: #222;
      border-radius: 8px;
      box-shadow: inset 0 0 12px #ffeb3b88;
      margin-bottom: 10px;
    }
    .video-title-display {
      width: 100%;
      text-align: center;
      font-size: 18px;
      color: #ffeb3b;
      margin-bottom: 10px; /* Space between title and progress bar */
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      user-select: none;
    }
    .progress-container {
      width: 100%;
      display: flex;
      align-items: center;
      gap: 12px;
      user-select: none;
      margin-top: 10px;
    }
    .progress-bar {
      flex: 1;
      appearance: none;
      height: 10px;
      border-radius: 6px;
      background: #555;
      cursor: pointer;
      outline: none;
    }
    .progress-bar::-webkit-slider-thumb {
      appearance: none;
      width: 18px;
      height: 18px;
      background: #ffeb3b;
      border-radius: 50%;
      cursor: pointer;
      border: none;
      margin-top: -5px;
      transition: background 0.3s;
    }
    .progress-bar::-moz-range-thumb {
      width: 18px;
      height: 18px;
      background: #ffeb3b;
      border-radius: 50%;
      cursor: pointer;
      border: none;
      transition: background 0.3s;
    }
    .progress-bar:hover::-webkit-slider-thumb {
      background: #fdd835;
    }
    .controls {
      display: flex;
      justify-content: center;
      gap: 16px;
      flex-wrap: wrap;
      margin-top: 12px;
    }
    .control-button {
      background: #ffeb3b;
      border: none;
      border-radius: 50%;
      width: 52px;
      height: 52px;
      cursor: pointer;
      color: #121212;
      font-size: 24px;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: 0 0 10px #ffeb3bcc;
      transition: background 0.3s;
      user-select: none;
      position: relative;
    }
    .control-button:hover {
      background: #fdd835;
    }
    .control-button:active {
      background: #d6bc00;
      box-shadow: 0 0 18px #a78b00;
    }
    .time-display {
      min-width: 90px;
      font-variant-numeric: tabular-nums;
      font-size: 16px;
      text-align: center;
      color: #ffeb3b;
      user-select: none;
      white-space: nowrap;
    }
    /* New volume slider styling */
    .volume-container {
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .volume-slider {
      appearance: none;
      width: 120px; /* Adjust width as needed */
      height: 8px;
      border-radius: 4px;
      background: #555;
      cursor: pointer;
      outline: none;
    }
    .volume-slider::-webkit-slider-thumb {
      appearance: none;
      width: 16px;
      height: 16px;
      background: #ffeb3b;
      border-radius: 50%;
      cursor: pointer;
      transition: background 0.3s;
    }
    .volume-slider::-moz-range-thumb {
      width: 16px;
      height: 16px;
      background: #ffeb3b;
      border-radius: 50%;
      cursor: pointer;
      transition: background 0.3s;
    }
    .volume-slider:hover::-webkit-slider-thumb {
      background: #fdd835;
    }

    /* Volume Indicator Styles */
    .volume-indicator {
      position: absolute;
      top: 10px;
      left: 10px;
      background-color: rgba(0, 0, 0, 0.7);
      color: #fff;
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 14px;
      opacity: 0;
      transition: opacity 0.3s ease-in-out;
      z-index: 10;
      user-select: none;
    }
  </style>
</head>
<body>
  <div class="sidebar" role="complementary" aria-label="Playlist">
    <h2>Playlist</h2>
    <div class="search-container">
      <input type="text" id="searchInput" class="search-input" placeholder="Search videos..." aria-label="Search videos" />
      <div class="search-icon" aria-hidden="true">&#128269;</div>
    </div>
    <div class="playlist" id="playlist" aria-live="polite" aria-relevant="additions removals"></div>
    <div class="upload-container">
      <label for="videoUpload" class="upload-label" title="Select a single or multiple video files to queue">Add Video(s)</label>
      <input type="file" id="videoUpload" accept="video/mp4,video/webm,video/ogg" multiple />
      <button id="clearAllBtn" class="clear-all-button" title="Clear all videos from playlist">Clear All</button>
    </div>
  </div>

  <div class="main-container">
    <div class="profile-container">
      <a href="https://www.instagram.com/rsyd.mp4?utm_source=ig_web_button_share_sheet&igsh=ZDNlZDc0MzIxNw==" target="_blank" title="Visit my profile">
        <img src="profile.jpg" alt="Profile Picture">
      </a>
    </div>

    <div class="video-header">
      <div class="header-title">Sora's Media Player</div>
    </div>

    <div id="volumeIndicator" class="volume-indicator"></div>

    <video id="video" preload="metadata" crossorigin="anonymous" controlsList="nodownload" playsinline></video>

    <canvas id="waveform" class="waveform" aria-label="Audio waveform visualization"></canvas>

    <div class="video-title-display" id="currentTitle" aria-live="polite" aria-atomic="true">No video loaded</div>

    <div class="progress-container">
      <input type="range" id="progress" class="progress-bar" min="0" max="100" value="0" step="0.1" aria-label="Video progress bar" />
      <div class="time-display" id="timeDisplay" aria-live="off">00:00 / 00:00</div>
    </div>

    <div class="controls" role="group" aria-label="Video controls">
      <button id="rewindBtn" class="control-button" title="Rewind 10 seconds" aria-label="Rewind 10 seconds">&#9664;&#9664;</button>
      <button id="playPauseBtn" class="control-button" title="Play/Pause" aria-label="Play or Pause">&#9658;</button>
      <button id="forwardBtn" class="control-button" title="Forward 10 seconds" aria-label="Forward 10 seconds">&#9654;&#9654;</button>

      <div class="volume-container">
        <input type="range" id="volumeSlider" class="volume-slider" min="0" max="1" step="0.05" value="1" aria-label="Volume slider" />
        <button id="muteBtn" class="control-button" title="Mute/Unmute" aria-label="Mute or Unmute">&#128263;</button>
      </div>
      
      <button id="restartBtn" class="control-button" title="Restart" aria-label="Restart Video">&#8634;</button>
      <button id="pipBtn" class="control-button" title="Picture-in-Picture" aria-label="Toggle Picture-in-Picture">&#9635;</button> <button id="shuffleBtn" class="control-button" title="Shuffle Playlist" aria-label="Shuffle Playlist">&#128256;</button>
      <button id="fullscreenBtn" class="control-button" title="Fullscreen" aria-label="Toggle Fullscreen">&#x26F6;</button>
    </div>
  </div>

  <script>
    const video = document.getElementById('video');
    const progressBar = document.getElementById('progress');
    const playPauseBtn = document.getElementById('playPauseBtn');
    const rewindBtn = document.getElementById('rewindBtn');
    const forwardBtn = document.getElementById('forwardBtn');
    const muteBtn = document.getElementById('muteBtn');
    const volumeSlider = document.getElementById('volumeSlider');
    const restartBtn = document.getElementById('restartBtn');
    const shuffleBtn = document.getElementById('shuffleBtn');
    const fullscreenBtn = document.getElementById('fullscreenBtn');
    const pipBtn = document.getElementById('pipBtn'); // Get the new PiP button
    const canvas = document.getElementById('waveform');
    const ctx = canvas.getContext('2d');
    const timeDisplay = document.getElementById('timeDisplay');
    const playlistElement = document.getElementById('playlist');
    const videoUpload = document.getElementById('videoUpload');
    const searchInput = document.getElementById('searchInput');
    const currentTitle = document.getElementById('currentTitle');
    const volumeIndicator = document.getElementById('volumeIndicator');
    const clearAllBtn = document.getElementById('clearAllBtn');

    let volumeIndicatorTimeout; 

    let playlist = [];
    let originalPlaylistOrder = []; // To store original order for unshuffling (optional)
    let currentIndex = -1;

    let dragSrcEl = null; // For drag and drop

    function resizeCanvas() {
      const rect = canvas.getBoundingClientRect();
      const dpr = window.devicePixelRatio || 1;
      canvas.width = rect.width * dpr;
      canvas.height = rect.height * dpr;
      ctx.scale(dpr, dpr);
    }
    resizeCanvas();
    window.addEventListener('resize', () => {
      ctx.setTransform(1, 0, 0, 1, 0, 0);
      resizeCanvas();
    });

    let audioContext;
    let analyser;
    let sourceNode;
    let dataArray;
    let bufferLength;
    let animationId = null;

    function setupAudioContext() {
      if(!audioContext){
        audioContext = new (window.AudioContext || window.webkitAudioContext)();
        analyser = audioContext.createAnalyser();
        analyser.fftSize = 2048;
        bufferLength = analyser.frequencyBinCount;
        dataArray = new Uint8Array(bufferLength);
      }
    }

    function connectAudio() {
      if(sourceNode){
        try { sourceNode.disconnect(); } catch {}
      }
      setupAudioContext();
      try {
        sourceNode = audioContext.createMediaElementSource(video);
        sourceNode.connect(analyser);
        analyser.connect(audioContext.destination);
      } catch(e){
        console.warn('AudioContext source error:', e);
      }
    }

    function drawWaveform() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      if (!analyser) return;
      analyser.getByteTimeDomainData(dataArray);
      const width = canvas.width / (window.devicePixelRatio || 1);
      const height = canvas.height / (window.devicePixelRatio || 1);
      ctx.lineWidth = 2;
      ctx.strokeStyle = '#ffeb3b';
      ctx.beginPath();
      let sliceWidth = width / bufferLength;
      let x = 0;
      for(let i=0; i<bufferLength; i++){
        let v = dataArray[i]/128.0;
        let y = v * height / 2;
        if(i===0) ctx.moveTo(x,y);
        else ctx.lineTo(x,y);
        x += sliceWidth;
      }
      ctx.lineTo(width, height/2);
      ctx.stroke();
    }

    function formatTime(seconds){
      if(isNaN(seconds) || !isFinite(seconds)) return '00:00';
      const m = Math.floor(seconds/60);
      const s = Math.floor(seconds%60);
      return m.toString().padStart(2,'0') + ':' + s.toString().padStart(2,'0');
    }

    function createThumbnail(file) {
      return new Promise(resolve => {
        const tempVideo = document.createElement('video');
        tempVideo.preload = 'metadata';
        tempVideo.muted = true;
        tempVideo.src = URL.createObjectURL(file);
        const canvasThumb = document.createElement('canvas');
        const ctxThumb = canvasThumb.getContext('2d');
        const cleanup = ()=>{URL.revokeObjectURL(tempVideo.src); tempVideo.remove(); canvasThumb.remove();};
        tempVideo.addEventListener('loadedmetadata', ()=>{
          canvasThumb.width = 160; canvasThumb.height = 90;
          tempVideo.currentTime = 0.1;
        });
        tempVideo.addEventListener('seeked', ()=>{
          try{
            ctxThumb.drawImage(tempVideo,0,0,canvasThumb.width,canvasThumb.height);
            canvasThumb.toBlob(blob=>{
              const url=URL.createObjectURL(blob);
              cleanup();
              resolve(url);
            },'image/jpeg',0.75);
          }catch{
            cleanup();
            resolve(null);
          }
        });
        tempVideo.onerror = ()=>{cleanup(); resolve(null);};
      });
    }

    function updateVideoAspectRatio(){
      if(video.videoWidth && video.videoHeight){
        const ratio = video.videoWidth / video.videoHeight;
        let maxHeight = 370; /* reduced height */
        let maxWidth = video.parentElement.offsetWidth - 30; /* Ambil lebar parent dan kurangi padding */
        let width = Math.min(maxWidth, maxHeight * ratio);
        let height = width / ratio;
        video.style.width = width + 'px';
        video.style.height = height + 'px';
      }
    }

    function loadVideoAt(index){
      if(index < 0 || index >= playlist.length) return;
      currentIndex = index;
      const item = playlist[index];
      video.src = item.url;
      video.load();
      video.play();
      currentTitle.textContent = item.name;
      updateVideoAspectRatio();
      updatePlaylistUI();
    }

    function updatePlaylistUI(){
      const filter = searchInput.value.trim().toLowerCase();
      playlistElement.innerHTML = '';
      playlist.forEach((item, index) => {
        if (!item.name.toLowerCase().includes(filter)) return;
        const div = document.createElement('div');
        div.classList.add('playlist-item');
        if (index === currentIndex) div.classList.add('selected');
        div.setAttribute('tabindex', '0');
        div.setAttribute('role', 'button');
        div.setAttribute('aria-pressed', index === currentIndex ? 'true' : 'false');
        div.setAttribute('draggable', 'true'); // Make item draggable
        div.dataset.index = index; // Store original index for drag and drop

        // Drag and drop event listeners
        div.addEventListener('dragstart', handleDragStart);
        div.addEventListener('dragover', handleDragOver);
        div.addEventListener('dragleave', handleDragLeave);
        div.addEventListener('drop', handleDrop);
        div.addEventListener('dragend', handleDragEnd);

        const thumbDiv = document.createElement('div');
        thumbDiv.classList.add('thumb');
        if (item.thumbnailUrl) {
          const img = document.createElement('img');
          img.src = item.thumbnailUrl;
          img.alt = `Thumbnail of ${item.name}`;
          thumbDiv.appendChild(img);
        } else {
          thumbDiv.style.background = '#444';
        }
        div.appendChild(thumbDiv);

        const nameSpan = document.createElement('span');
        nameSpan.textContent = item.name;
        nameSpan.style.flex = '1';
        nameSpan.style.userSelect = 'none';
        nameSpan.style.marginRight = '25px'; /* Add margin to prevent text overlap with button */
        nameSpan.style.overflow = 'hidden';
        nameSpan.style.whiteSpace = 'nowrap';
        nameSpan.style.textOverflow = 'ellipsis';
        div.appendChild(nameSpan);

        const btnRemove = document.createElement('button');
        btnRemove.innerHTML = '&#10006;'; // X icon for remove
        btnRemove.title = 'Remove from queue';
        btnRemove.setAttribute('aria-label', 'Remove ' + item.name + ' from queue');
        btnRemove.addEventListener('click', e => {
          e.stopPropagation();
          removeFromPlaylist(index);
        });
        div.appendChild(btnRemove);

        div.addEventListener('click', () => {
          if (index !== currentIndex) loadVideoAt(index);
        });
        div.addEventListener('keydown', e => {
          if (e.key === "Enter" || e.key === " ") {
            e.preventDefault();
            if (index !== currentIndex) loadVideoAt(index);
          }
        });

        playlistElement.appendChild(div);
      });
    }

    function removeFromPlaylist(index){
      if(index<0 || index>=playlist.length) return;
      const item=playlist[index];
      if(item.file) URL.revokeObjectURL(item.url);
      if(item.thumbnailUrl) URL.revokeObjectURL(item.thumbnailUrl);
      playlist.splice(index,1);
      
      // Update originalPlaylistOrder if the shuffled list is not active
      if (playlist.every((val, i) => val === originalPlaylistOrder[i])) {
        originalPlaylistOrder.splice(index, 1);
      } else {
        // If shuffled, remove from originalPlaylistOrder too, though it's less critical for shuffle
        const originalIndex = originalPlaylistOrder.findIndex(originalItem => originalItem === item);
        if (originalIndex !== -1) {
          originalPlaylistOrder.splice(originalIndex, 1);
        }
      }

      if(index === currentIndex){
        if(playlist.length === 0){
          currentIndex = -1;
          video.pause();
          video.removeAttribute('src');
          video.load();
          updatePlaylistUI();
          updateProgress();
          currentTitle.textContent = 'No video loaded';
          video.style.width = '';
          video.style.height = '';
        } else if(index >= playlist.length){
          loadVideoAt(playlist.length-1);
        } else {
          loadVideoAt(index);
        }
      } else if(index < currentIndex){
        currentIndex--;
        updatePlaylistUI();
      } else {
        updatePlaylistUI();
      }
    }

    function clearAllVideos() {
      if (confirm('Apakah kamu yakin akan menghapus seluruh playlist?')) {
        playlist.forEach(item => {
          if (item.file) URL.revokeObjectURL(item.url);
          if (item.thumbnailUrl) URL.revokeObjectURL(item.thumbnailUrl);
        });
        playlist = [];
        originalPlaylistOrder = [];
        currentIndex = -1;
        video.pause();
        video.removeAttribute('src');
        video.load();
        updatePlaylistUI();
        updateProgress();
        currentTitle.textContent = 'No video loaded';
        video.style.width = '';
        video.style.height = '';
      }
    }

    function togglePlayPause(){
      if(playlist.length === 0) return; /* Prevent playing if no video loaded */
      if(video.paused || video.ended) video.play();
      else video.pause();
    }
    function updatePlayPauseIcon(){
      if(video.paused || video.ended) playPauseBtn.innerHTML = '&#9658;';
      else playPauseBtn.innerHTML = '&#10073;&#10073;';
    }
    function updateMuteIcon(){
      if(video.muted || video.volume === 0) muteBtn.innerHTML = '&#128263;'; /* Muted icon */
      else muteBtn.innerHTML = '&#128266;'; /* Unmuted icon */
    }
    function updateProgress(){
      if(!progressBar.dragging){
        progressBar.value = (video.currentTime / video.duration)*100 || 0;
      }
      timeDisplay.textContent = `${formatTime(video.currentTime)} / ${formatTime(video.duration)}`;
    }
    function seekVideo(){
      video.currentTime = (progressBar.value / 100) * video.duration;
    }
    function animate(){
      if(video.paused || video.ended){
        cancelAnimationFrame(animationId);
        animationId = null;
        return;
      }
      drawWaveform();
      animationId = requestAnimationFrame(animate);
    }
    function playNextInPlaylist(){
      if(currentIndex + 1 < playlist.length) {
        loadVideoAt(currentIndex+1);
      } else {
        video.pause();
        // Optional: Loop playlist if desired
        // if (playlist.length > 0) loadVideoAt(0);
      }
    }
    function startAudioContextOnPlay(){
      if(!audioContext){
        setupAudioContext();
        connectAudio();
      }
    }
    function addVideoToPlaylist(file){
      createThumbnail(file).then(thumbUrl=>{
        const url = URL.createObjectURL(file);
        const newItem = {name:file.name, url:url, file:file, thumbnailUrl: thumbUrl};
        playlist.push(newItem);
        originalPlaylistOrder.push(newItem); // Add to original order
        if(currentIndex === -1) loadVideoAt(0);
        updatePlaylistUI();
      }).catch(() => {
        const url = URL.createObjectURL(file);
        const newItem = {name:file.name, url:url, file:file, thumbnailUrl: null};
        playlist.push(newItem);
        originalPlaylistOrder.push(newItem); // Add to original order
        if(currentIndex === -1) loadVideoAt(0);
        updatePlaylistUI();
      });
    }

    // Function to shuffle the playlist
    function shufflePlaylist() {
      if (playlist.length <= 1) return;

      let currentVideo = playlist[currentIndex];
      let newPlaylist = [...playlist];

      // Fisher-Yates (Knuth) shuffle algorithm
      for (let i = newPlaylist.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newPlaylist[i], newPlaylist[j]] = [newPlaylist[j], newPlaylist[i]];
      }

      playlist = newPlaylist;
      currentIndex = playlist.findIndex(item => item === currentVideo);

      updatePlaylistUI();
    }

    // Function to display volume indicator
    function displayVolumeIndicator(volumeLevel) {
      clearTimeout(volumeIndicatorTimeout);
      volumeIndicator.textContent = `Volume: ${Math.round(volumeLevel * 100)}%`;
      volumeIndicator.style.opacity = '1';

      volumeIndicatorTimeout = setTimeout(() => {
        volumeIndicator.style.opacity = '0';
      }, 1500);
    }

    // Drag and Drop functions for playlist items
    function handleDragStart(e) {
      dragSrcEl = this;
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('text/html', this.innerHTML);
      this.classList.add('dragging');
    }

    function handleDragOver(e) {
      e.preventDefault(); // Necessary to allow drop
      e.dataTransfer.dropEffect = 'move';
      if (this !== dragSrcEl && this.classList.contains('playlist-item')) {
        this.style.borderTop = '2px solid #ffeb3b'; // Visual cue for reordering
      }
    }

    function handleDragLeave(e) {
      this.style.borderTop = ''; // Remove visual cue
    }

    function handleDrop(e) {
      e.stopPropagation();
      if (dragSrcEl !== this) {
        const dragIndex = Array.from(playlistElement.children).indexOf(dragSrcEl);
        const dropIndex = Array.from(playlistElement.children).indexOf(this);

        const [removed] = playlist.splice(dragIndex, 1);
        playlist.splice(dropIndex, 0, removed);

        // Adjust currentIndex if necessary
        if (currentIndex === dragIndex) {
            currentIndex = dropIndex;
        } else if (currentIndex > dragIndex && currentIndex <= dropIndex) {
            currentIndex--;
        } else if (currentIndex < dragIndex && currentIndex >= dropIndex) {
            currentIndex++;
        }
        
        updatePlaylistUI();
        if (playlist[currentIndex] === removed) { // Ensure the currently playing video is still selected
            playlistElement.children[currentIndex].classList.add('selected');
        }
      }
      this.style.borderTop = '';
      return false;
    }

    function handleDragEnd(e) {
      this.classList.remove('dragging');
      const items = playlistElement.querySelectorAll('.playlist-item');
      items.forEach(item => {
        item.style.borderTop = '';
      });
    }

    // PiP function
    function togglePictureInPicture() {
      if (document.pictureInPictureElement) {
        document.exitPictureInPicture()
          .catch(error => console.error('Failed to exit Picture-in-Picture:', error));
      } else {
        if (video.requestPictureInPicture) {
          video.requestPictureInPicture()
            .catch(error => console.error('Failed to enter Picture-in-Picture:', error));
        } else {
          alert('Picture-in-Picture is not supported by your browser.');
        }
      }
    }

    playPauseBtn.addEventListener('click', togglePlayPause);
    video.addEventListener('play', () => {
      updatePlayPauseIcon();
      if(audioContext && audioContext.state === 'suspended') audioContext.resume();
      animate();
    });
    video.addEventListener('pause', () => {
      updatePlayPauseIcon();
      if(animationId) {
        cancelAnimationFrame(animationId);
        animationId = null;
      }
    });
    video.addEventListener('timeupdate', updateProgress);
    video.addEventListener('volumechange', () => {
      updateMuteIcon();
      volumeSlider.value = video.volume;
    });
    video.addEventListener('ended', playNextInPlaylist);
    rewindBtn.addEventListener('click', () => {
      video.currentTime = Math.max(0, video.currentTime-10);
    });
    forwardBtn.addEventListener('click', () => {
      video.currentTime = Math.min(video.duration, video.currentTime+10);
    });
    muteBtn.addEventListener('click', () => {
      video.muted = !video.muted;
      displayVolumeIndicator(video.volume);
    });
    volumeSlider.addEventListener('input', () => {
      video.volume = volumeSlider.value;
      if (video.volume > 0) {
        video.muted = false;
      }
      displayVolumeIndicator(video.volume);
    });

    restartBtn.addEventListener('click', () => {
      video.currentTime = 0;
      if(video.paused) video.play();
    });
    shuffleBtn.addEventListener('click', shufflePlaylist);
    fullscreenBtn.addEventListener('click', () => {
      if(!document.fullscreenElement){
        video.requestFullscreen().catch(err => alert(`Fullscreen error: ${err.message} (${err.name})`));
      } else {
        document.exitFullscreen();
      }
    });
    pipBtn.addEventListener('click', togglePictureInPicture); // Add event listener for PiP button

    progressBar.addEventListener('input', () => {progressBar.dragging=true;});
    progressBar.addEventListener('change', () => {progressBar.dragging=false; seekVideo();});
    video.addEventListener('loadedmetadata', () => {
      updateProgress();
      updateVideoAspectRatio();
      volumeSlider.value = video.volume;
      updateMuteIcon();
    });
    video.addEventListener('play', () => {
      startAudioContextOnPlay();
    });
    
    // Modified visibilitychange listener for better background playback attempt
    document.addEventListener('visibilitychange', () => {
      if (document.hidden) {
        // Tab is hidden
        // Browsers might pause media automatically, especially if not in PiP.
        // You might consider pausing here if you explicitly want to save resources
        // or if you don't want audio playing when the tab is completely out of view.
        // For 'not pausing on app/tab switch', we actually don't want to explicitly pause here.
        // The browser will manage this based on its policies.
      } else {
        // Tab is visible
        if (video.paused && currentIndex !== -1) {
          // If the video was paused when the tab was hidden and a video is loaded, attempt to resume.
          // This helps if the browser auto-paused it.
          // video.play().catch(e => console.warn('Autoplay prevented on tab focus:', e));
        }
      }
    });

    videoUpload.addEventListener('change', (e) => {
      const files = e.target.files;
      for(let file of files) addVideoToPlaylist(file);
      videoUpload.value = '';
    });
    searchInput.addEventListener('input', updatePlaylistUI);
    clearAllBtn.addEventListener('click', clearAllVideos);

    // Tambahkan event listener untuk mengaktifkan play/pause saat video diklik
    video.addEventListener('click', togglePlayPause);

    // Keyboard shortcuts
    window.addEventListener('keydown', e => {
      if(e.target.tagName === 'INPUT' || e.target.tagName === 'BUTTON') return;
      if(e.code === 'Space'){
        e.preventDefault();
        togglePlayPause();
      } else if(e.ctrlKey && e.code === 'ArrowRight'){
        e.preventDefault();
        video.currentTime = Math.min(video.duration, video.currentTime + 10);
      } else if(e.ctrlKey && e.code === 'ArrowLeft'){
        e.preventDefault();
        video.currentTime = Math.max(0, video.currentTime - 10);
      } else if(e.code === 'ArrowRight'){
        e.preventDefault();
        video.currentTime = Math.min(video.duration, video.currentTime + 5);
      } else if(e.code === 'ArrowLeft'){
        e.preventDefault();
        video.currentTime = Math.max(0, video.currentTime - 5);
      } else if(e.code === 'KeyF'){
        e.preventDefault();
        if(!document.fullscreenElement){
          video.requestFullscreen().catch(err => alert(`Fullscreen error: ${err.message} (${err.name})`));
        } else {
          document.exitFullscreen();
        }
      } else if (e.code === 'KeyP' && e.ctrlKey) { // Ctrl + P for Picture-in-Picture
          e.preventDefault();
          togglePictureInPicture();
      } else if (e.code === 'ArrowUp') {
        e.preventDefault();
        video.volume = Math.min(1, video.volume + 0.05);
        if(video.volume > 0) video.muted = false;
        displayVolumeIndicator(video.volume);
      } else if (e.code === 'ArrowDown') {
        e.preventDefault();
        video.volume = Math.max(0, video.volume - 0.05);
        if(video.volume === 0) video.muted = true;
        displayVolumeIndicator(video.volume);
      } else if (e.code === 'KeyM') {
        e.preventDefault();
        video.muted = !video.muted;
        displayVolumeIndicator(video.volume);
      } else if (e.code === 'KeyO') {
        e.preventDefault();
        videoUpload.click();
      }
    });

    // Tambahkan event listener untuk drag and drop files from file explorer
    playlistElement.addEventListener('dragover', (e) => {
      e.preventDefault(); // Mencegah perilaku default untuk memungkinkan drop
      playlistElement.style.borderColor = '#ffeb3b'; // Efek visual saat drag over
    });

    playlistElement.addEventListener('dragleave', (e) => {
      playlistElement.style.borderColor = '#444'; // Kembalikan border saat drag leave
    });

    playlistElement.addEventListener('drop', (e) => {
      e.preventDefault(); // Mencegah browser membuka file
      playlistElement.style.borderColor = '#444'; // Kembalikan border setelah drop

      const files = e.dataTransfer.files; // Mengakses file yang dijatuhkan
      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        // Pastikan file adalah video sebelum menambahkannya
        if (file.type.startsWith('video/')) {
          addVideoToPlaylist(file); // Panggil fungsi addVideoToPlaylist() yang sudah ada
        } else {
          console.warn('File dropped is not a video:', file.name);
        }
      }
    });
  </script>
</body>
</html>
