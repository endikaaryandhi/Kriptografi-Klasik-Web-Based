<template>
  <div class="app-wrapper">
    <header class="app-header">
      <h1>Kalkulator Kriptografi Klasik</h1>
      <nav class="app-nav">
        <button 
          v-for="tab in tabs" 
          :key="tab.id" 
          @click="currentTab = tab.id" 
          :class="{ active: currentTab === tab.id }"
        >
          {{ tab.name }}
        </button>
      </nav>
    </header>

    <main class="app-main">
      <div class="card" :class="{ 'home-card': currentTab === 'Home' }">
        <component :is="currentComponent" @navigate="currentTab = $event"></component>
      </div>
    </main>

    <footer class="app-footer">
      <p>Endika Aryandhi | 21120123130089 | Kriptografi C</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Home from './components/Home.vue'
import VigenereCipher from './components/VigenereCipher.vue'
import AffineCipher from './components/AffineCipher.vue'
import PlayfairCipher from './components/PlayfairCipher.vue'
import HillCipher from './components/HillCipher.vue'
import EnigmaCipher from './components/EnigmaCipher.vue'

const currentTab = ref('Home')

const tabs = [
  { id: 'Home', name: 'Beranda' },
  { id: 'VigenereCipher', name: 'Vigenere' },
  { id: 'AffineCipher', name: 'Affine' },
  { id: 'PlayfairCipher', name: 'Playfair' },
  { id: 'HillCipher', name: 'Hill' },
  { id: 'EnigmaCipher', name: 'Enigma' }
]

const components = {
  Home,
  VigenereCipher,
  AffineCipher,
  PlayfairCipher,
  HillCipher,
  EnigmaCipher
}

const currentComponent = computed(() => components[currentTab.value])
</script>

<style>
:root {
  --primary-color: #0ea5e9;
  --primary-hover: #0284c7;
  --bg-color: #f1f5f9;
  --text-color: #334155;
  --border-color: #cbd5e1;
  --card-bg: #ffffff;
}

html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  max-width: 100%;
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
  text-align: left;
  display: flex;
  flex-direction: column;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  background-color: var(--bg-color);
  color: var(--text-color);
  line-height: 1.5;
}

.app-wrapper {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 100vh;
}

.app-header {
  background-color: var(--card-bg);
  padding: 1.5rem 1rem;
  text-align: center;
  border-bottom: 1px solid var(--border-color);
}

.app-header h1 {
  margin: 0 0 1.5rem 0;
  font-size: 1.5rem;
  color: #0f172a;
}

.app-nav {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.app-nav button {
  background-color: #f8fafc;
  border: 1px solid var(--border-color);
  color: #000000;
  padding: 0.6rem 1.2rem;
  border-radius: 0.375rem;
  font-size: 0.95rem;
  cursor: pointer;
  transition: all 0.2s;
}

.app-nav button:hover {
  background-color: #e2e8f0;
}

.app-nav button.active {
  background-color: var(--primary-color);
  color: #ffffff;
  border-color: var(--primary-color);
}

.app-main {
  flex: 1;
  padding: 2rem 1rem;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.card {
  background-color: var(--card-bg);
  width: 100%;
  max-width: 550px;
  padding: 2rem;
  border-radius: 0.5rem;
  border: 1px solid var(--border-color);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.home-card {
  max-width: 800px;
}

.card h2 {
  margin-top: 0;
  margin-bottom: 1.5rem;
  font-size: 1.25rem;
  color: #0f172a;
  border-bottom: 1px solid var(--border-color);
  padding-bottom: 0.75rem;
}

.input-group {
  margin-bottom: 1.25rem;
}

.input-group label {
  display: block;
  font-size: 0.875rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #000000;
}

.input-group input[type="text"],
.input-group input[type="number"],
.input-group textarea {
  width: 100%;
  padding: 0.6rem 0.75rem;
  border: 1px solid var(--border-color);
  border-radius: 0.375rem;
  font-size: 0.95rem;
  font-family: inherit;
  box-sizing: border-box;
  background-color: #ffffff;
  color: #000000;
  transition: border-color 0.15s;
}

.input-group textarea {
  resize: vertical;
}

.input-group input:focus,
.input-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
}

.flex-row {
  display: flex;
  gap: 1rem;
}

.flex-1 {
  flex: 1;
}

.matrix-container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: center;
  margin-top: 0.5rem;
  padding: 1rem;
  background-color: #f8fafc;
  border: 1px solid var(--border-color);
  border-radius: 0.375rem;
}

.matrix-row {
  display: flex;
  gap: 0.5rem;
}

.matrix-input {
  width: 3.5rem !important;
  text-align: center;
  color: #000000 !important;
}

.actions {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.actions button {
  flex: 1;
  padding: 0.75rem;
  border: none;
  border-radius: 0.375rem;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s;
}

.actions button:first-child {
  background-color: var(--primary-color);
  color: #ffffff;
}

.actions button:first-child:hover {
  background-color: var(--primary-hover);
}

.actions button.decrypt {
  background-color: #e2e8f0;
  color: #000000;
}

.actions button.decrypt:hover {
  background-color: #cbd5e1;
}

.app-footer {
  text-align: center;
  padding: 1.5rem;
  background-color: var(--card-bg);
  border-top: 1px solid var(--border-color);
  color: #000000;
  font-size: 0.875rem;
  margin-top: auto;
}
</style>