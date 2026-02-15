<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MusicAI Transformer Pro</title>
    <script src="https://unpkg.com/wavesurfer.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;800&display=swap" rel="stylesheet">
    
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #0f172a; }
        .glass { background: rgba(30, 41, 59, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.1); }
        .gradient-text { background: linear-gradient(90deg, #6366f1, #a855f7); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        #waveform { background: #1e293b; border-radius: 12px; overflow: hidden; }
        .hidden { display: none; }
    </style>
</head>
<body class="text-white min-h-screen flex items-center justify-center p-4">

    <div class="glass w-full max-w-md rounded-[2rem] p-8 shadow-2xl">
        <header class="text-center mb-8">
            <h1 class="text-4xl font-800 gradient-text mb-2">MusicAI Pro</h1>
            <p class="text-slate-400 text-sm">Ubah Gaya, Genre, & File Musik Anda</p>
        </header>

        <div id="dropZone" class="border-2 border-dashed border-slate-700 rounded-2xl p-6 text-center mb-6 transition-all hover:border-indigo-500">
            <input type="file" id="audioUpload" hidden accept="audio/*">
            <div class="text-4xl mb-3">🎵</div>
            <button onclick="document.getElementById('audioUpload').click()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-2 rounded-full font-bold transition">
                Pilih Lagu
            </button>
            <p id="fileName" class="text-xs text-slate-500 mt-3 truncate">Belum ada file dipilih</p>
        </div>

        <div id="waveform" class="mb-6 h-20"></div>

        <div class="space-y-4 mb-8">
            <div>
                <label class="text-xs font-bold text-slate-400 uppercase mb-2 block">Pilih Genre Baru</label>
                <select id="genreSelect" class="w-full bg-slate-800 border border-slate-700 rounded-xl p-3 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <option value="edm">🎧 EDM (Dance)</option>
                    <option value="jazz">🎷 Smooth Jazz</option>
                    <option value="lofi">☁️ Lofi Hip Hop</option>
                    <option value="rock">🎸 Hard Rock</option>
                    <option value="acoustic">🎸 Acoustic Folk</option>
                </select>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <div>
                    <label class="text-xs font-bold text-slate-400 uppercase mb-2 block">Pitch (Nada)</label>
                    <input type="range" id="pitchSlider" min="-12" max="12" value="0" class="w-full accent-indigo-500">
                </div>
                <div>
                    <label class="text-xs font-bold text-slate-400 uppercase mb-2 block">Tempo</label>
                    <input type="range" id="speedSlider" min="0.5" max="2" step="0.1" value="1" class="w-full accent-purple-500">
                </div>
            </div>
        </div>

        <button id="genBtn" onclick="processAI()" class="w-full bg-gradient-to-r from-indigo-600 to-purple-600 py-4 rounded-xl font-800 text-lg shadow-lg hover:scale-[1.02] transition">
            GENERATE TRANSFORMASI
        </button>

        <button id="downloadBtn" onclick="downloadFile()" class="hidden w-full bg-emerald-600 py-4 rounded-xl font-800 text-lg shadow-lg mt-4 animate-bounce">
            ⬇️ DOWNLOAD HASIL
        </button>

        <div id="loader" class="hidden mt-6 text-center">
            <div class="inline-block animate-spin rounded-full h-8 w-8 border-4 border-indigo-500 border-t-transparent"></div>
            <p class="text-sm text-indigo-400 mt-2 font-bold italic">AI sedang menyusun ulang musik Anda...</p>
        </div>
    </div>

    <script>
        // Inisialisasi Audio Player
        const wavesurfer = WaveSurfer.create({
            container: '#waveform',
            waveColor: '#475569',
            progressColor: '#6366f1',
            cursorColor: '#fff',
            barWidth: 2,
            height: 80,
            responsive: true
        });

        let currentAudioUrl = null;

        // Input Handler
        document.getElementById('audioUpload').onchange = function(e) {
            const file = e.target.files[0];
            if (file) {
                document.getElementById('fileName').innerText = file.name;
                currentAudioUrl = URL.createObjectURL(file);
                wavesurfer.load(currentAudioUrl);
            }
        };

        // Simulasi Logika AI Sempurna
        async function processAI() {
            if (!currentAudioUrl) return alert("Pilih file lagu terlebih dahulu!");
            
            const loader = document.getElementById('loader');
            const genBtn = document.getElementById('genBtn');
            const downloadBtn = document.getElementById('downloadBtn');

            genBtn.disabled = true;
            loader.classList.remove('hidden');
            downloadBtn.classList.add('hidden');

            // Simulasi proses pengiriman ke API AI (Suno/Fadr)
            await new Promise(resolve => setTimeout(resolve, 5000));

            // Efek Suara: Merubah gaya secara instan di player (lokal)
            const speed = document.getElementById('speedSlider').value;
            wavesurfer.setPlaybackRate(speed);

            loader.classList.add('hidden');
            genBtn.disabled = false;
            downloadBtn.classList.remove('hidden');
            
            alert("Selesai! AI telah menyesuaikan gaya musik Anda.");
        }

        function downloadFile() {
            const link = document.createElement('a');
            link.href = currentAudioUrl;
            link.download = "MusicAI_Transformed.mp3";
            link.click();
        }
    </script>
</body>
</html>
