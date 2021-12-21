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

## Copia de archivos con Webpacks
```bash
npm i copy-webpack-plugin -D
```
Agregar configuración en webpack.config.js y cambiar el path de las imagenes a la asignada en la configuración
```js
const CopyPlugin = require('copy-webpack-plugin');
// plugins
new CopyPlugin({
        patterns:[
            {
                from: path.resolve(__dirname, "src", "assets/images"),
                to: "assets/images"
            }
        ]
    }),
```
```html
<img src="assets/images/github.png" />
```
## Loaders de imágenes
No es necesario instalar ninguna dependencia, webpack ya lo tiene incluido debemos agregar la siguiente configuración en module para los recursos.
```js
{
    test: /\.png$/i,
    type: 'asset/resource'
}
```
## Loaders de fuentes
Instalación
```bash
npm install url-loader file-loader -D
```
Agregar a css la fuente descargada y eliminar la importada
```css
@font-face {
	font-family: 'Ubuntu';
	src: url('../assets/fonts/ubuntu-regular.woff2') format('woff2'), url('../assets/fonts/ubuntu-regular.woff') format('woff');
	font-weight: 400;
	font-style: normal;
}
```
En module.exports especificamos la ruta donde se deben guardar los archivos de imagen.
```js
output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    assetModuleFilename: 'assets/images/[hash][ext][query]'
},
```
Agregamos la configuración del URL-Loader.
```js
{
    test: /\.(woff|woff2|eot|ttf|otf)$/i,
    type: "asset/resource",
    generator: {
        filename: "assets/fonts/[hash][ext]",
    },
}
```
## Optimización: hashes, compresión y minificación de archivos
Instalar unas dependencias
```bash
npm install css-minimizer-webpack-plugin terser-webpack-plugin -D
```
Agregar las opciones de optimización y hashes a archivos.
```js
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');
optimization: {
        minimize:true,
        minimizer: [
            new CssMinimizerPlugin(),
            new TerserPlugin()
        ]
    }
```

## Variables de entorno
Instalar dotenv para webpack y crear archivo .env
```bash
npm install -D dotenv-webpack
```
Agregar configuración en webpack.config.js (plugins)
```js
const Dotenv = require("dotenv-webpack");
new Dotenv(),
```
Reemplazar codigo por variables de entorno:
```js
const API = process.env.API;
```
## Webpack cleaner
Instalar plugin webpack cleaner
```bash
npm install -D clean-webpack-plugin
```
Agregarlo a plugins en webpack.config.js
```js
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
new CleanWebpackPlugin(),
```
## Webpack watch
para utilizar el podo watch, se puede hacer de dos formas, agregando directamente en el archivowebpack.config.js
```js
watch: true,
```
O con script que agregue esa opción en el comando en package.json
```js
"build:watch": "webpack --watch --config webpack.config.js"
```
## Deploy a Netlify
Crear archivo netlify.toml con la siguiente configuración
```
[build]
    publish = "dist"
    command = "npm run build"

```
## Webpack Dev Server
Instalar webpack dev server con
```bash
npm install webpack-dev-server -D
```
Agregar la configuración a el archivo de webpack.config.dev.js
```js
devServer: {
    static: path.join(__dirname, 'dist'),
    compress: true,
    historyApiFallback: true,
    port: 3006,
},
```
Y el comando para correr webpack serve en package.json
```js
"start": "webpack serve --config webpack.config.dev.js"
```
No es necesario tener el modo watch activo

## Webpack Bundle Analyzer
Instalar bundle analyzer
```bash
npm install -D webpack-bundle-analyzer
```
Crear configuración en webpack.config.dev.js en plugins
```js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

new BundleAnalyzerPlugin()
```

Para generar el repote
```bash
npx webpack --profile --json > stats.json # linux
npx webpack --profile --json | Out-file 'stats.json' -Encoding OEM # windows
```
Para mostrar reporte
```bash
npx webpack-bundle-analyzer .\stats.json
```
# Webpack react
## Instalación y configuración de React
```bash
# Inicializar npm
npm init -y
# instalar dependencias
npm install react react-dom
```
