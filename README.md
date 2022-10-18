![Banner Ignite - Forgma for devs](https://user-images.githubusercontent.com/12506432/196316375-4930864d-f51f-44ca-a42f-1f1761fd5cb4.png)

# Ignite - Figma for Devs

Curso disponibilizado pela [RocketSeat](https://www.rocketseat.com.br/)
<p>Tema: Figma for Devs</p> 
<img alt="GitHub Contributors" src="https://img.shields.io/github/contributors/claudneysessa/RocketSeat-ignite-lab-design-system" />

## Configurando style-dictionary

- [https://github.com/amzn/style-dictionary](https://github.com/amzn/style-dictionary)

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

### Configurar o GitHub para integração direto do figma

Na página do GitHub vá no seu perfil e escolha a opção:

- perfil->Settings->Developer Settings->Personal Access Token
- Gere um Token e guarde ele para iniciar a integração

![image](https://user-images.githubusercontent.com/12506432/196316838-92036370-d0f4-4b2f-a38a-f43a048f0dee.png)

#### Criar arquivo de configuração para o workflow do processo de publicação dos tokens

- github/workflows/update-tokens.yml

```yml
name: Generate design tokens

on:
  repository_dispatch:
    types: update-tokens

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Create input JSON
        id: create-json
        uses: jsdaniell/create-json@1.1.2
        with:
          name: ${{ github.event.client_payload.filename }}
          json: ${{ github.event.client_payload.tokens }}
          dir: 'input'

      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Create PR
        uses: peter-evans/create-pull-request@v4
        with:
          commit-message: "style: Update design tokens"
          title: ${{ github.event.client_payload.commitMessage || 'Update design tokens' }}
          body: "Design tokens have been updated via Figma and need to be reviewed."
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH_NAME: 'main'
```

### Configurando o figma

No plugin Design Tokens:

- Adicione a url do GitHub
  - [https://api.github.com/repos/claudneysessa/figma-tokens/dispatches](https://api.github.com/repos/claudneysessa/figma-tokens/dispatches)
  - Adicione a Key gerada
  - Adicione o commit message

<img width="567" alt="image" src="https://user-images.githubusercontent.com/12506432/196099035-b60a7c09-9bf5-4ebe-b126-640fa8870959.png">

Após salvar, o github irá rodar a Action e gerar os arquivos

<img width="1438" alt="image" src="https://user-images.githubusercontent.com/12506432/196099159-5bef8489-dd6f-4fd2-a67c-acff6e8a0549.png">

Será gerado um Pull-Request após a conclusão com os arquivos alterados

<img width="1438" alt="image" src="https://user-images.githubusercontent.com/12506432/196099308-03ae366d-a5e2-46e6-98ca-8deaa7299f95.png">

## Where to find me

<div>
    <a href="https://www.youtube.com/channel/UC52P3HVpmOMHjkmIhcjLRyQ" target="_blank">
      <img src="https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white" target="_blank">
    </a>
    <a href="https://instagram.com/claudneysessa" target="_blank">
      <img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank">
    </a>
    <a href = "mailto:claudneysartisessa@gmail.com">
      <img src="https://img.shields.io/badge/-Gmail-%23333?style=for-the-badge&logo=gmail&logoColor=white" target="_blank">
    </a>
    <a href="https://www.linkedin.com/in/claudneysessa/" target="_blank">
      <img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank">
    </a>
    <a href="https://medium.com/@claudneysartisessa" target="_blank">
      <img alt="Medium" src="https://img.shields.io/badge/medium-%2312100E.svg?&style=for-the-badge&logo=medium&logoColor=white" />
    </a>
</div>

## Veja o meu Perfil no Github

https://github.com/claudneysessa

## Contact
 - Instagram: [https://www.instagram.com/claudneysessa/](https://www.instagram.com/claudneysessa/)
 - Linkedin: [https://www.linkedin.com/in/claudneysessa/](https://www.linkedin.com/in/claudneysessa/)
