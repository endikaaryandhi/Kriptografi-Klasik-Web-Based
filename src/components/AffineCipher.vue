<template>
  <div>
    <h2>Affine Cipher</h2>
    <div class="input-group">
      <label>Teks:</label>
      <textarea v-model="text" rows="3"></textarea>
    </div>
    <div class="input-group flex-row">
      <div class="flex-1">
        <label>Kunci a (Koprima 26):</label>
        <input v-model.number="a" type="number" />
      </div>
      <div class="flex-1">
        <label>Kunci b:</label>
        <input v-model.number="b" type="number" />
      </div>
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
const a = ref(5)
const b = ref(8)
const result = ref('')

const gcd = (x, y) => {
  while (y) {
    let t = y
    y = x % y
    x = t
  }
  return x
}

const modInverse = (a, m) => {
  a = ((a % m) + m) % m
  for (let x = 1; x < m; x++) {
    if ((a * x) % m === 1) return x
  }
  return 1
}

const process = (mode) => {
  const t = text.value.toUpperCase().replace(/[^A-Z]/g, '')
  if (!t) return

  if (gcd(a.value, 26) !== 1) {
    alert('Kunci "a" harus koprima dengan 26!')
    return
  }

  let out = ''
  const aInv = modInverse(a.value, 26)

  for (let i = 0; i < t.length; i++) {
    const p = t.charCodeAt(i) - 65
    let c
    if (mode === 'encrypt') {
      c = (a.value * p + b.value) % 26
    } else {
      c = (aInv * (p - b.value)) % 26
    }
    if (c < 0) c += 26
    out += String.fromCharCode(c + 65)
  }

  result.value = out
}
</script>