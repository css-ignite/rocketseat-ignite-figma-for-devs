# Ignite - Figma for Devs

## Install style-dictionary

```bash
npm install style-dictionary
npx style-dictionary init basic 
```

### Remover as pastas

- build
- tokens

### Ajustando o arquivo config.json

- Altero a pasta tokens para a minha pasta input
- Altero as configurações de scss para css
- Altero o destination removendo o "_" do variables

### Antes

```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "scss": {
      "transformGroup": "scss",
      "buildPath": "build/scss/",
      "files": [{
        "destination": "_variables.scss",
        "format": "scss/variables"
      }]
    }
  }
}
```

### Depois

```json
{
  "source": ["input/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "build/css/",
      "files": [{
        "destination": "tokens-css.css",
        "format": "css/variables"
      }]
    }
  }
}
```

### Ajustando o package.json

- Adiciono o comando de build do style-dictionary

```bash
style-dictionary build
```

```json
{
  "dependencies": {
    "style-dictionary": "^3.7.1"
  },
  "name": "figma-tokens",
  "description": "Projeto criado para estudos da exportação de tokens do figma",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "build": "style-dictionary build"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/claudneysessa/figma-tokens.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/claudneysessa/figma-tokens/issues"
  },
  "homepage": "https://github.com/claudneysessa/figma-tokens#readme"
}
```

### Executo o build do style-dictionary

```bash
npm run build
```

### Criado o build/css/tokens-css.css (inicial)

```css
/**
 * Do not edit directly
 * Generated on Mon, 17 Oct 2022 04:05:12 GMT
 */

:root {
  --color-gray-200: #898996;
  --color-gray-400: #50505c;
  --color-gray-700: #1c1c1f;
  --color-gray-900: #121214;
  --color-purple-500: #8b5cf6;
  --color-white-900: #ffffff;
  --color-black-900: #000000;
}
```

### Incrementando o arquivo config.json

- Adicionando configurações para o JavaScript Flat
- Adicionando configurações para o Flutter com Dart

```json
{
  "source": ["input/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "build/css/",
      "files": [{
        "destination": "tokens-css.css",
        "format": "css/variables"
      }]
    },
    "js": {
      "transformGroup": "js",
      "buildPath": "build/js/",
      "files": [{
        "destination": "tokens-js.js",
        "format": "javascript/module-flat"
      }]
    },
    "flutter": {
      "transformGroup": "flutter",
      "buildPath": "build/flutter/",
      "files": [{
        "destination": "tokens-flutter.dart",
        "format": "flutter/class.dart"
      }]
    }
  }
}
```

Após aplicada a configuração, o style-dictionary gera os arquivos conforme configurado:

- tokens-css.css
- tokens-js.js
- tokens-flutter.dart

## Iniciando a publicação

---

### Ajustando o .gitignore

- Removo a pasta node_modules
  - node_modules
- Removo a pasta input e todos os arquivos que json dentro dela
  - input/*.json
- Adiciono na pasta input o arquivo .gitkeep para a pasta continuar a existir

```gitignore
node_modules
input/*.json
```
