# Vue3-vanilla

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
pnpm install
```

### Compile and Hot-Reload for Development

```sh
pnpm dev
```

### Compile and Minify for Production

```sh
pnpm build
```

### 安装Element-Plus

```sh
pnpm install element-plus 
```

### 按需引入

```sh
pnpm install -D unplugin-vue-components unplugin-auto-import 
```

### vite.config.js 添加配置文件
```js
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// plugins 中添加以下配置
AutoImport({
    resolvers: [ElementPlusResolver()],
}),
Components({
    resolvers: [ElementPlusResolver()],
})
```
