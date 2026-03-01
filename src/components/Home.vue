<template>
  <div class="home-container">
    <h2>Selamat Datang</h2>
    <p class="subtitle">Aplikasi Kalkulator Kriptografi Klasik untuk keperluan edukasi dan simulasi enkripsi/dekripsi teks.</p>
    
    <div class="cipher-cards">
      <div 
        class="cipher-card" 
        v-for="cipher in ciphers" 
        :key="cipher.id" 
        @click="$emit('navigate', cipher.id)"
      >
        <h3>{{ cipher.name }}</h3>
        <p>{{ cipher.desc }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const ciphers = [
  { 
    id: 'VigenereCipher', 
    name: 'Vigenere Cipher', 
    desc: 'Metode menyandikan teks alfabet dengan menggunakan deretan sandi Caesar berdasarkan huruf-huruf pada kata kunci.' 
  },
  { 
    id: 'AffineCipher', 
    name: 'Affine Cipher', 
    desc: 'Jenis sandi substitusi monoalfabetik di mana setiap huruf dipetakan ke fungsi matematika (ax + b) mod 26.' 
  },
  { 
    id: 'PlayfairCipher', 
    name: 'Playfair Cipher', 
    desc: 'Teknik enkripsi simetrik yang mengenkripsi pasangan huruf (digram) menggunakan matriks 5x5.' 
  },
  { 
    id: 'HillCipher', 
    name: 'Hill Cipher', 
    desc: 'Kriptografi substitusi polialfabetik yang bekerja berdasarkan aljabar linier (perkalian matriks).' 
  },
  { 
    id: 'EnigmaCipher', 
    name: 'Enigma Cipher', 
    desc: 'Simulasi mesin Enigma yang menggunakan sistem perputaran rotor elektromekanis untuk enkripsi.' 
  }
]
</script>

<style scoped>
.home-container {
  text-align: center;
}

.subtitle {
  color: #475569;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}

.cipher-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  text-align: left;
}

.cipher-card {
  background-color: #f8fafc;
  border: 1px solid var(--border-color);
  padding: 1.25rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.cipher-card:hover {
  background-color: #f1f5f9;
  border-color: var(--primary-color);
  transform: translateY(-2px);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.cipher-card h3 {
  margin-top: 0;
  margin-bottom: 0.5rem;
  color: var(--primary-color);
  font-size: 1.1rem;
}

.cipher-card p {
  font-size: 0.85rem;
  margin-bottom: 0;
  color: #334155;
  line-height: 1.4;
}
</style>