# Middle-Examp-js

// Data Input berdasarkan soal
const lahan = [
    ["subur", "kering", "subur", "subur"],
    ["tandus", "kering", "kering", "subur"],
    ["subur", "subur", "subur", "kering"],
    ["kering", "kering", "kering", "kering"]
];

const weatherData = {
    temperature: 28,
    humidity: 55,
    windSpeed: 12
};

// --- Fungsi Pemeriksaan Cuaca ---
function isWeatherIdeal(data) {
    // Kondisi ideal:
    // Suhu antara 20°C hingga 30°C [cite: 27]
    const isTempIdeal = data.temperature >= 20 && data.temperature <= 30;
    // Kelembapan lebih dari 50% [cite: 28]
    const isHumidityIdeal = data.humidity > 50;
    // Kecepatan angin kurang dari 15 km/h [cite: 29]
    const isWindIdeal = data.windSpeed < 15;

    return isTempIdeal && isHumidityIdeal && isWindIdeal;
}

// --- Fungsi Utama Manajemen Lahan ---
function kelolaLahan(lahanMatrix, cuaca) {
    let totalSubur = 0;
    let totalDitanami = 0;
    
    // 1. Validasi Baris Lahan dan Perbarui Status
    const lahanTerbaru = [];
    
    for (let i = 0; i < lahanMatrix.length; i++) {
        const row = lahanMatrix[i];
        let suburCount = 0;
        
        // Hitung petak subur dalam baris
        for (let j = 0; j < row.length; j++) {
            if (row[j] === "subur") {
                suburCount++;
            }
        }
        
        const totalPetak = row.length;
        const suburPercentage = suburCount / totalPetak;
        
        // Aturan a: Setiap baris harus mengandung setidaknya 50% petak subur[cite: 19].
        if (suburPercentage < 0.5) {
            // Jika tidak memenuhi syarat, semua petak di baris itu menjadi "kering"[cite: 20].
            // Catatan: Petak 'tandus' (tidak dapat dipulihkan) tidak berubah.
            const newRow = row.map(petak => (petak === "tandus" ? "tandus" : "kering"));
            lahanTerbaru.push(newRow);
        } else {
            // Baris memenuhi syarat, status petak tetap
            lahanTerbaru.push(row);
        }
    }
    
    // 2. Hitung Total Petak Subur setelah validasi
    for (const row of lahanTerbaru) {
        for (const petak of row) {
            if (petak === "subur") {
                totalSubur++;
            }
        }
    }
    
    // 3. Pengecekan Cuaca dan Penanaman
    const cuacaIdeal = isWeatherIdeal(cuaca);
    
    if (cuacaIdeal) {
        // Aturan b: Petak subur akan ditanami jika kondisi cuaca cocok[cite: 21].
        totalDitanami = totalSubur; 
        console.log("Cuaca cocok untuk bercocok tanam. Tidak ada peringatan.");
    } else {
        // Aturan d: Jika cuaca tidak sesuai, keluarkan peringatan[cite: 33].
        totalDitanami = 0; // Tidak ada yang ditanami
        console.log("Peringatan: Cuaca tidak cocok untuk bercocok tanam.");
    }
    
    // 4. Keluarkan Hasil (Aturan e)
    console.log(`\nTotal petak subur: ${totalSubur}`); // [cite: 35]
    console.log(`Total petak yang ditanami: ${totalDitanami}`); // [cite: 36]
}

// Jalankan program dengan input yang diberikan
console.log("--- Hasil Manajemen Lahan ---");
kelolaLahan(lahan, weatherData);
