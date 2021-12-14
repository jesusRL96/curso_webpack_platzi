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
Agregar script en package.json
```js
"build": "webpack --mode production"
```

## Babel loader para JS
```bash
npm install -D babel-loader @babel/core @babel/preset-env @babel/plugin-transform-runtime
```
- babel-loader nos permite usar babel con webpack
- @babel/core es babel en general
- @babel/preset-env trae y te permite usar las ultimas características de JavaScript
- @babel/plugin-transform-runtime te permite trabajar con todo el tema de asincronismo como ser async y await
- Debes crear el archivo de configuración de babel el cual tiene como nombre .babelrc

## HTML en Webpack
Instalar plugin
```bash
npm install html-webpack-plugin -D
```
Agregar configuración de plugin
```js
plugins: [
        new HtmlWebpackPlugin({
            inject: true,
            template: './public/index.html',
            filename: './index.html'
        })
    ]
```

## Loaders para CSS y preprocesadores de CSS
```bash
npm install mini-css-extract-plugin css-loader -D
```

Se eliminan las etiquetas style del archivo index.html y se agrega la configuración en webpack.config.js.

*Module*:
```js
{
    test: /\.css$/i,
    use: [
        MiniCssExtractPlugin.loader,
        'css-loader'
    ],
}
```
*Plugins*:
```js
new MiniCssExtractPlugin(),
```
Para trabajar con pre procesadores e.g. stylus
```bash
npm install stylus stylus-loader -D
```
Y agregar la configuración en webpack.config.js
```js
{
    test: /\.css|.styl$/i,
    use: [
        MiniCssExtractPlugin.loader,
        'css-loader',
        'stylus-loader'
    ],
}
```