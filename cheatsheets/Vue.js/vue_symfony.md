# Installer Vue.js dans un projet Symfony

1. Installer Vue.js et TypeScript

```bash
yarn add vue@^3 vue-loader vue-template-compiler ts-loader typescript --dev
```

2. Dans webpack.config.js :

Décommenter la ligne ~63
```js
    .enableTypeScriptLoader()
```

Ajouter la ligne
```js
    .enableVueLoader(() => {}, { runtimeCompilerBuild: false })
```

3. Dans assets/app.js, ajouter :

```js
import { createApp } from 'vue';
import App from './js/App.vue';

createApp(App).mount('#vue-app');
```