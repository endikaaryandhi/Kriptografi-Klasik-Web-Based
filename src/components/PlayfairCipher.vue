<template>
  <div>
    <h2>Playfair Cipher</h2>
    <div class="input-group">
      <label>Teks:</label>
      <textarea v-model="text" rows="3"></textarea>
    </div>
    <div class="input-group">
      <label>Kunci:</label>
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

const generateMatrix = (k) => {
  const alphabet = 'ABCDEFGHIKLMNOPQRSTUVWXYZ'
  const matrixStr = [...new Set((k.toUpperCase().replace(/J/g, 'I') + alphabet).replace(/[^A-Z]/g, ''))].join('')
  const matrix = []
  for (let i = 0; i < 5; i++) {
    matrix.push(matrixStr.slice(i * 5, i * 5 + 5).split(''))
  }
  return matrix
}

const findPosition = (matrix, char) => {
  for (let r = 0; r < 5; r++) {
    for (let c = 0; c < 5; c++) {
      if (matrix[r][c] === char) return [r, c]
    }
  }
  return [0, 0]
}

const formatPlaintext = (t) => {
  t = t.toUpperCase().replace(/J/g, 'I').replace(/[^A-Z]/g, '')
  let res = ''
  for (let i = 0; i < t.length; i += 2) {
    let char1 = t[i]
    let char2 = t[i + 1]
    if (!char2) {
      res += char1 + 'X'
    } else if (char1 === char2) {
      res += char1 + 'X'
      i--
    } else {
      res += char1 + char2
    }
  }
  return res
}

const process = (mode) => {
  const k = key.value || 'KEY'
  const matrix = generateMatrix(k)
  const t = mode === 'encrypt' ? formatPlaintext(text.value) : text.value.toUpperCase().replace(/[^A-Z]/g, '')
  
  if (!t || t.length % 2 !== 0 && mode === 'decrypt') return

  let out = ''

  for (let i = 0; i < t.length; i += 2) {
    const [r1, c1] = findPosition(matrix, t[i])
    const [r2, c2] = findPosition(matrix, t[i + 1])

    if (r1 === r2) {
      const shift = mode === 'encrypt' ? 1 : 4
      out += matrix[r1][(c1 + shift) % 5] + matrix[r2][(c2 + shift) % 5]
    } else if (c1 === c2) {
      const shift = mode === 'encrypt' ? 1 : 4
      out += matrix[(r1 + shift) % 5][c1] + matrix[(r2 + shift) % 5][c2]
    } else {
      out += matrix[r1][c2] + matrix[r2][c1]
    }
  }

  result.value = out
}
</script>