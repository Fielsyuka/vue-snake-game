<template>
  <div class="App">
    <header class="header">
      <div class="title-container">
        <h1 class="nav-title">Snake Game</h1>
      </div>
      <Navigation />
    </header>
    <main class="main">
      <Field :fields="fields" />
    </main>
    <footer class="footer">
      <Button />
      <ManipulationPanel />
    </footer>
  </div>
</template>

<script>
import Navigation from './components/Navigation';
import Field from './components/Field';
import Button from './components/Button';
import ManipulationPanel from './components/ManipulationPanel';
import { initFields } from './utils';

export default {
  name: 'App',
  components: {
    Navigation,
    Field,
    Button,
    ManipulationPanel
  },
  data() {
    return {
      fields: initFields(35)
    }
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto',
    'Oxygen', 'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans',
    'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* borderを含めた幅で要素の幅が計算されるように */
*,
*:before,
*:after {
  box-sizing: border-box;
}

.App {
  max-width: calc(9px * 35 + 1px * 2); /* a. (マスの幅) * 個数 + (ボーダーの幅) * 本数 */
  margin: 0 auto;
  text-align: center;
}

#root {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
