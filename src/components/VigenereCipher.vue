<template>
  <div>
    <h2>Vigenere Cipher</h2>
    <div class="input-group">
      <label>Teks:</label>
      <textarea v-model="text" rows="3"></textarea>
    </div>
    <div class="input-group">
      <label>Kunci (Huruf saja):</label>
      <input v-model="key" type="text" />
    </div>
    <div class="actions">
      <button @click="process('encrypt')">Enkripsi</button>
      <button class="decrypt" @click="process('decrypt')">Dekripsi</button>
    </div>
    <div class="input-group">
      <label>Hasil:</label>
      <textarea v-model="result" rows="3" readonly></textarea>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const text = ref('')
const key = ref('')
const result = ref('')

const process = (mode) => {
  const t = text.value.toUpperCase().replace(/[^A-Z]/g, '')
  const k = key.value.toUpperCase().replace(/[^A-Z]/g, '')
  
  if (!t || !k) return

  let out = ''
  let j = 0

  for (let i = 0; i < t.length; i++) {
    const c = t.charCodeAt(i) - 65
    const shift = k.charCodeAt(j % k.length) - 65
    
    let resChar
    if (mode === 'encrypt') {
      resChar = (c + shift) % 26
    } else {
      resChar = (c - shift + 26) % 26
    }
    
    out += String.fromCharCode(resChar + 65)
    j++
  }

  result.value = out
}
</script>