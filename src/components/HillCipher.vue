<template>
  <div>
    <h2>Hill Cipher</h2>
    <div class="input-group flex-row">
      <div class="flex-1">
        <label>Ukuran Matriks (N x N):</label>
        <input type="number" v-model.number="n" min="2" max="5" @change="resizeMatrix" />
      </div>
    </div>
    
    <div class="input-group">
      <label>Key Matriks:</label>
      <div class="matrix-container">
        <div v-for="(row, i) in matrix" :key="i" class="matrix-row">
          <input 
            v-for="(val, j) in row" 
            :key="j" 
            type="number" 
            v-model.number="matrix[i][j]" 
            class="matrix-input"
          />
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
import { ref } from 'vue'

const n = ref(2)
const matrix = ref([[3, 3], [2, 5]])
const text = ref('')
const result = ref('')

const resizeMatrix = () => {
  const size = n.value
  const newMat = []
  for (let i = 0; i < size; i++) {
    newMat.push(new Array(size).fill(0))
    for (let j = 0; j < size; j++) {
      if (matrix.value[i] && matrix.value[i][j] !== undefined) {
        newMat[i][j] = matrix.value[i][j]
      }
    }
  }
  matrix.value = newMat
}

const mod = (n, m) => ((n % m) + m) % m

const getDeterminant = (mat) => {
  if (mat.length === 2) return mat[0][0] * mat[1][1] - mat[0][1] * mat[1][0]
  let det = 0
  for (let i = 0; i < mat.length; i++) {
    const minor = mat.slice(1).map(r => r.filter((_, j) => j !== i))
    det += Math.pow(-1, i) * mat[0][i] * getDeterminant(minor)
  }
  return det
}

const modInverse = (a, m) => {
  a = mod(a, m)
  for (let x = 1; x < m; x++) {
    if (mod(a * x, m) === 1) return x
  }
  return -1
}

const getAdjugate = (mat) => {
  const size = mat.length
  if (size === 2) {
    return [
      [mat[1][1], -mat[0][1]],
      [-mat[1][0], mat[0][0]]
    ]
  }
  const adj = []
  for (let i = 0; i < size; i++) {
    adj.push([])
    for (let j = 0; j < size; j++) {
      const minor = mat.filter((_, r) => r !== j).map(row => row.filter((_, c) => c !== i))
      adj[i][j] = Math.pow(-1, i + j) * getDeterminant(minor)
    }
  }
  return adj
}

const gcd = (a, b) => {
  a = Math.abs(a)
  b = Math.abs(b)
  while (b) {
    let t = b
    b = a % b
    a = t
  }
  return a
}

const process = (mode) => {
  let t = text.value.toUpperCase().replace(/[^A-Z]/g, '')
  if (!t) return

  while (t.length % n.value !== 0) {
    t += 'X'
  }

  let processMat = matrix.value
  const det = mod(getDeterminant(processMat), 26)

  if (mode === 'decrypt') {
    const invDet = modInverse(det, 26)
    if (invDet === -1) {
      alert('Matriks kunci tidak invertible (determinan tidak koprima dengan 26)!')
      return
    }
    const adj = getAdjugate(processMat)
    processMat = adj.map(row => row.map(val => mod(val * invDet, 26)))
  } else if (gcd(det, 26) !== 1) {
     alert('Peringatan: Matriks ini tidak bisa didekripsi karena determinan tidak koprima dengan 26.')
  }

  let out = ''
  for (let i = 0; i < t.length; i += n.value) {
    const block = []
    for (let j = 0; j < n.value; j++) {
      block.push(t.charCodeAt(i + j) - 65)
    }
    for (let r = 0; r < n.value; r++) {
      let sum = 0
      for (let c = 0; c < n.value; c++) {
        sum += processMat[r][c] * block[c]
      }
      out += String.fromCharCode(mod(sum, 26) + 65)
    }
  }
  result.value = out
}
</script>