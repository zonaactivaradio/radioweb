const metadataUrl = "https://api.zeno.fm/mounts/metadata/subscribe/pm7pdvh09vzuv";
const currentSongEl = document.getElementById('current-song');
const historyList = document.getElementById('song-history');

// Función para manejar el historial en LocalStorage
function updateHistory(songTitle) {
    const today = new Date().toLocaleDateString();
    let history = JSON.parse(localStorage.getItem('radioHistory')) || { date: today, songs: [] };

    // Si cambió el día, limpiar historial
    if (history.date !== today) {
        history = { date: today, songs: [] };
    }

    // Evitar duplicados consecutivos
    if (history.songs[0] !== songTitle) {
        history.songs.unshift(songTitle); // Agregar al inicio
        if (history.songs.length > 20) history.songs.pop(); // Limitar a 20 temas
        localStorage.setItem('radioHistory', JSON.stringify(history));
        renderHistory(history.songs);
    }
}

function renderHistory(songs) {
    historyList.innerHTML = songs.map(song => `<li>${song}</li>`).join('');
}

// Conexión a la API de Zeno.fm
const eventSource = new EventSource(metadataUrl);

eventSource.onmessage = function(event) {
    try {
        const data = JSON.parse(event.data);
        if (data && data.streamTitle) {
            const songName = data.streamTitle;
            currentSongEl.innerText = songName;
            updateHistory(songName);
        }
    } catch (e) {
        console.error("Error parseando metadatos", e);
    }
};

eventSource.onerror = function() {
    console.log("Reconectando a la API de metadatos...");
};

// Cargar historial al iniciar
window.onload = () => {
    const savedHistory = JSON.parse(localStorage.getItem('radioHistory'));
    if (savedHistory && savedHistory.date === new Date().toLocaleDateString()) {
        renderHistory(savedHistory.songs);
    }
};
