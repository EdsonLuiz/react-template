# Github explorer (Notas)
Coisas que achei importante anotar durante o desenvolvimento deste projeto.

## React 🤤 
- React: Biblioteca para criação de componentes.
  ```shell
  $ npm i react react-dom
  ```

## Babel 💪🏾
- Babel: Compila o código de desenvolvimento para Javascript em alguma versão
  pré-definida.
  ```shell
  $ npm i @babel/core @babel/cli @babel/preset-env babel-loader -D
  ```
### Configure seu babel
- Crie um arquivo `babel.config.js`
  ```js
  module.exports = {
    presets: [
      '@babel/preset-env'
    ]
  }
  ```
### Gerando bundle de forma manual
```shell
$ npx babel src/index.js --out-file dist/bundle.js
```
### Babel entendendo código React
```shell
$ npm i @babel/preset-react -D
```
### Altere seu babel
- Adicione no arquivo `babel.config.js`
  ```js
  module.exports = {
    presets: [
      '@babel/preset-env',
      '@babel/preset-react'
    ]
  }
  ```

## Webpack 🤢 🤮
- Transforma um conjunto de arquivos, de tipos direntes, em algo que o navegador consegue executar/ entender.
```shell
$ npm i webpack webpack-cli webpack-dev-server -D
```

### Configurando webpack
- Crie um arquivo `webpack.config.js`
```js
const path = require('path')

module.exports = {
  entry: path.resolve(__dirname, 'src', 'index.jsx'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  resolve: {
    extensions: [
      '.js', '.jsx'
    ]
  },
  module: {
    rules: [
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },
    ]
  }
}
```
  - entry: informa o arquivo de entrada da aplicação. É uma boa prática utilizar o `path.resolve` para, de forma automática, resolver o sistema de paths, de acordo com cada Sistema Operacional.
  - output: informa onde os arquivos gerados pelo webpack devem ficar. Lembre-se do `path.resolve`
    - path: folder onde serão gerados os arquivos de saída.
    - filename: nome do arquivo gerado.
  - resolve: informa outros tipos de arquivos que o webpack pode ler sem a necessidade de digitar a extensão do arquivo, o padrão são arquivos `.js`
    - extensions: um array de tipos de arquivo.
  - module: configura o comportamento do webpack de acordo com o tipo do arquivo.
    - rules: um array de objetos, sendo um objeto para cada tipo de arquivo. Este objeto deve conter todas as regras que devem ser seguidas para tratamento deste tipo de arquivo.
      - test: uma regex que vai determinar o arquivo afetado pela regra especificada.
      - exclude: folder por onde o `test` não deve passar.
      - use: define o loader que será executado nesta regra
    
## Removendo a necessidade de importar React
Adicione no arquivo `babel.config.js`
```js
module.exports = {
  presets: [
    '@babel/preset-env',
    ['@babel/preset-react', {
      runtime: 'automatic'
    }]
  ]
}
```

## Production or development mode
| production         | development         |
|--------------------|---------------------|
| builds mais lentos | builds mais rapidos |
| bundle menor       | bundle maior        |
|                    |                     |

Adicione no arquivo `webpack.config.js`
```js
module.exports = {
  mode: 'development',
  // other configurations
}
```

## Injetar Javascript no index.html
```shell
$ npm i html-webpack-plugin -D
```

Adicione no arquivo `webpack.config.js`
```js
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  // other configurations
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'public', 'index.html')
    })
  ]
  // other configurations
}
```

## Devserver
```shell
$ npm i webpack-dev-server -D
```

Adicione no arquivo `webpack.config.js`
```js
module.exports = {
  // other config settings
  devServer: {
    contentBase: path.resolve(__dirname, 'public')
  },
  // other config settings
}
```

Inicie o servidor de desenvolvimento com:
```shell
npx webpack serve
```

## Sourcemaps
Adicione no arquivo `webpack.config.js`
```js
module.exports = {
  // other config settings
  devtool: 'eval-source-map'
  // other config settings
}
```

## Ambientes
Altere no arquivo `webpack.config.js`
```js

const isDevelopment = process.env.NODE_ENV !== 'production'

module.exports = {
  mode: isDevelopment ? 'development' : 'production',
  devtool: isDevelopment ? 'eval-source-map' : 'source-map',
  // other config settings
}
```

É necessário instalar o cross-env para definir variáveis de ambiente, independente do sistema operacional
```shell
$ npm i cross-env -D
```

Criar scripts no `package.json`
```json
{
  // other config settings
  "scripts": {
    "start": "npm run start:dev",
    "start:dev": "webpack serve",
    "start:build": "cross-env NODE_ENV=production webpack"
  },
  // other config settings
}

```

## Arquivos CSS
É necessário criar uma nova regra no `webpack.config.js` para informar qual loader os arquivos `.css` devem utilizar.

Intalar loaders
```shell
npm i style-loader css-loader -D
```

```js
  module: {
    rules: [
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: ['style-loader', 'css-loader']
      },
    ]
  }
```