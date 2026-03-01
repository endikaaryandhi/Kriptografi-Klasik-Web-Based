<template>
  <div>
    <h2>Enigma Cipher (Sederhana)</h2>
    <div class="input-group">
      <label>Posisi Rotor Awal (3 Huruf):</label>
      <input v-model="rotorPositions" type="text" maxlength="3" />
    </div>
    <div class="input-group">
      <label>Teks:</label>
      <textarea v-model="text" rows="3"></textarea>
    </div>
    <div class="actions">
      <button @click="process">Proses (Enkripsi/Dekripsi)</button>
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
const rotorPositions = ref('AAA')
const result = ref('')

const rotors = [
  { wiring: 'EKMFLGDQVZNTOWYHXUSPAIBRCJ', notch: 'Q' },
  { wiring: 'AJDKSIRUXBLHWTMCQGZNPYFVOE', notch: 'E' },
  { wiring: 'BDFHJLCPRTXVZNYEIWGAKMUSQO', notch: 'V' }
]
const reflector = 'YRUHQSLDPXNGOKMIEBFZCWVJAT'

const process = () => {
  const t = text.value.toUpperCase().replace(/[^A-Z]/g, '')
  let pos = rotorPositions.value.toUpperCase().replace(/[^A-Z]/g, '')
  if (pos.length !== 3) pos = 'AAA'

  let posArr = [pos.charCodeAt(0) - 65, pos.charCodeAt(1) - 65, pos.charCodeAt(2) - 65]
  let out = ''

  for (let i = 0; i < t.length; i++) {
    let c = t.charCodeAt(i) - 65

    let atNotch1 = String.fromCharCode(posArr[1] + 65) === rotors[1].notch
    let atNotch2 = String.fromCharCode(posArr[2] + 65) === rotors[2].notch

    if (atNotch1) {
      posArr[0] = (posArr[0] + 1) % 26
      posArr[1] = (posArr[1] + 1) % 26
    } else if (atNotch2) {
      posArr[1] = (posArr[1] + 1) % 26
    }
    posArr[2] = (posArr[2] + 1) % 26

    for (let r = 2; r >= 0; r--) {
      const idx = (c + posArr[r]) % 26
      const mapped = rotors[r].wiring.charCodeAt(idx) - 65
      c = (mapped - posArr[r] + 26) % 26
    }

    c = reflector.charCodeAt(c) - 65

    for (let r = 0; r <= 2; r++) {
      const idx = (c + posArr[r]) % 26
      const mapped = rotors[r].wiring.indexOf(String.fromCharCode(idx + 65))
      c = (mapped - posArr[r] + 26) % 26
    }

    out += String.fromCharCode(c + 65)
  }
  
  result.value = out
}
</script>