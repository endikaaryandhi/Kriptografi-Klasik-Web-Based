<template>
  <div>
    <h2>Playfair Cipher</h2>
    <div class="input-group">
      <label>Kunci:</label>
      <input v-model="key" type="text" />
    </div>

    <div class="input-group" v-if="displayedMatrix.length">
      <label>Matriks Kunci (5x5):</label>
      <div class="matrix-grid-container">
        <div v-for="(row, i) in displayedMatrix" :key="i" class="matrix-row">
          <div v-for="(char, j) in row" :key="j" class="matrix-cell">
            {{ char }}
          </div>
        </div>
      </div>
    </div>

    <div class="input-group">
      <label>Teks:</label>
      <textarea v-model="text" rows="3"></textarea>
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
import { ref, watch, onMounted } from 'vue'

const key = ref('KEYWORD')
const text = ref('')
const result = ref('')
const displayedMatrix = ref([])

const generateMatrixInternal = (k) => {
  const alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
  const sanitizedKey = (k.toUpperCase() + alphabet).replace(/J/g, 'I').replace(/[^A-Z]/g, '')
  const uniqueChars = [...new Set(sanitizedKey)]
  
  const matrix = []
  for (let i = 0; i < 5; i++) {
    matrix.push(uniqueChars.slice(i * 5, i * 5 + 5))
  }
  return matrix
}

onMounted(() => {
  displayedMatrix.value = generateMatrixInternal(key.value)
})

watch(key, (newKey) => {
  displayedMatrix.value = generateMatrixInternal(newKey)
})

const process = (mode) => {
  const alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
  const uniqueKey = [...new Set((key.value.toUpperCase() + alphabet).replace(/J/g, 'I').replace(/[^A-Z]/g, ''))]
  
  const matrix = []
  const charMap = {}
  for (let i = 0; i < 5; i++) {
    matrix.push(uniqueKey.slice(i * 5, i * 5 + 5))
    for (let j = 0; j < 5; j++) {
      charMap[matrix[i][j]] = [i, j]
    }
  }

  const formatPlaintext = (t) => {
    t = t.toUpperCase().replace(/J/g, 'I').replace(/[^A-Z]/g, '')
    let formatted = ''
    for (let i = 0; i < t.length; i++) {
      formatted += t[i]
      if (t[i] === t[i + 1]) formatted += 'X'
    }
    if (formatted.length % 2 !== 0) formatted += 'X'
    return formatted
  }

  const t = mode === 'encrypt' ? formatPlaintext(text.value) : text.value.toUpperCase().replace(/J/g, 'I').replace(/[^A-Z]/g, '')
  
  if (t.length % 2 !== 0) {
    alert('Ciphertext must have an even length!')
    return
  }

  let out = ''
  for (let i = 0; i < t.length; i += 2) {
    const char1 = t[i]
    const char2 = t[i + 1]
    const pos1 = charMap[char1]
    const pos2 = charMap[char2]

    if (!pos1 || !pos2) continue

    let r1 = pos1[0], c1 = pos1[1]
    let r2 = pos2[0], c2 = pos2[1]

    if (r1 === r2) {
      if (mode === 'encrypt') {
        c1 = (c1 + 1) % 5
        c2 = (c2 + 1) % 5
      } else {
        c1 = (c1 + 4) % 5
        c2 = (c2 + 4) % 5
      }
    } else if (c1 === c2) {
      if (mode === 'encrypt') {
        r1 = (r1 + 1) % 5
        r2 = (r2 + 1) % 5
      } else {
        r1 = (r1 + 4) % 5
        r2 = (r2 + 4) % 5
      }
    } else {
      const tempC1 = c1
      c1 = c2
      c2 = tempC1
    }
    out += matrix[r1][c1] + matrix[r2][c2]
  }
  result.value = out
}
</script>

<style scoped>
.matrix-grid-container {
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin-top: 0.5rem;
  padding: 1rem;
  background-color: #f8fafc;
  border: 1px solid var(--border-color);
  border-radius: 0.375rem;
  width: fit-content;
  margin-left: auto;
  margin-right: auto;
}

.matrix-row {
  display: flex;
  gap: 2px;
}

.matrix-cell {
  width: 2.5rem;
  height: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #cbd5e1;
  background-color: #ffffff;
  font-weight: 700;
  font-family: 'Courier New', Courier, monospace;
  font-size: 1.2rem;
  color: var(--primary-color);
}
</style>