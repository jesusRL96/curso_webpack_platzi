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