<script setup lang="ts">
import { ref, onMounted } from 'vue'
import ListContainer from './components/ListContainer.vue'
import LightThemeIcon from './components/icons/LightThemeIcon.vue'
import DarkThemeIcon from './components/icons/DarkThemeIcon.vue'
import GitHubIcon from './components/icons/GitHubIcon.vue'
import SiteLinkIcon from './components/icons/SiteLinkIcon.vue'

type UserTheme = 'light' | 'dark'

const userTheme = ref<UserTheme>(getTheme())

onMounted(() => setTheme(userTheme.value || getMediaPreference()))

function setTheme(theme: UserTheme) {
  localStorage.setItem('user-theme', theme)
  userTheme.value = theme
  document.documentElement.className = theme
}

function getTheme(): UserTheme {
  return localStorage.getItem('user-theme') as UserTheme
}

function toggleTheme() {
  const activeTheme = localStorage.getItem('user-theme')
  if (activeTheme === 'light') {
    setTheme('dark')
  } else {
    setTheme('light')
  }
}

function getMediaPreference(): UserTheme {
  const hasDarkPreference = window.matchMedia('(prefers-color-scheme: dark)').matches
  return hasDarkPreference ? 'dark' : 'light'
}
</script>

<template>
  <header>
    <div class="title" data-test="title">
      <h1><a href="/">Formula2TeX</a></h1>
      <h2><i>Copy the TeX source of various formulas.</i></h2>
    </div>
    <div class="theme">
      <button @click="toggleTheme" class="theme-btn">
        <span v-if="userTheme === 'dark'">
          <LightThemeIcon />
        </span>
        <span v-if="userTheme === 'light'">
          <DarkThemeIcon />
        </span>
      </button>
    </div>
  </header>
  <main>
    <ListContainer />
  </main>
  <footer>
    <div>
      <div class="footer-icons">
        <a href="https://github.com/rivea0">
          <GitHubIcon :fill-color="userTheme === 'light' ? '#10131a' : '#eff1f5'" :width="28" :height="28" />
        </a>
        <a href="https://edaeren.com">
          <SiteLinkIcon :fill-color="userTheme === 'light' ? '#10131a' : '#eff1f5'" :width="28" :height="28" />
        </a>
      </div>
      <p class="footer-primary" data-test="footer-primary">
        Made with &#128156; & &#9749; by Eda Eren. &copy; {{ new Date().getFullYear() }}
      </p>
      <p>
        Rubik (fav)icon from
        <a href="https://www.svgrepo.com/collection/eighties-3/">the Eighties 3 collection</a> from
        SVG Repo | Icons from <a href="https://phosphoricons.com">Phosphor Icons</a>.
      </p>
    </div>
  </footer>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Imprima&display=swap');

header {
  display: flex;
  justify-content: space-between;
  padding: 1rem 2rem;
  background-color: var(--accent-color);
  border-bottom: 2px solid var(--text-primary-color);
}

h1 > a {
  color: var(--text-primary-color);
  text-decoration: none;
  font-size: 3.6rem;
}

h1 > a:hover {
  border-bottom: 6px solid var(--text-primary-color);
}

h2 {
  color: var(--text-secondary-color);
}

.theme {
  align-self: center;
  border: 2px solid var(--text-primary-color);
  border-radius: 12px;
  padding: 4px;
}

.theme-btn {
  background-color: inherit;
  border: none;
}

.footer-icons {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
  justify-content: center;
}

footer {
  text-align: center;
  padding: 1rem;
}

@media (orientation: portrait) {
  header {
    flex-direction: column;
    gap: 18px;
    padding: 1rem;
  }

  .theme {
    align-self: flex-start;
  }
}
</style>
