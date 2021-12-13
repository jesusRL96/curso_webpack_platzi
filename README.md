# Curso Webpack

## Primer build con Webpack
Instalación
```bash
npm init -y
npm i webpack webpack-cli -D
npx webpack
```
Modo de compilación
```bash
npx webpack --mode production
npx webpack --mode development
```

### Configuración de webpack
Crear archivo llamado webpack.config.js y agregar configuración básica:
```js
const path = require('path');
module.exports = {
    entry: "./src/index.js",
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'main.js',
    },
    resolve: {
        extensions: ['.js']
    }
}
```
Para utilizar la configuración del archivo se ocupa el comando:
```bash
npx webpack --mode production --config webpack.config.js
```
