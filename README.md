
# Iniciando um projeto Backend 

Neste repositório indico como iniciar um projeto Backend utilizando typescript express jest supertest express mysql2 eslint


## Iniciando configurações do Projeto

#### º Crie o diretório 

```bash
mkdir nome-do-projeto
```
#### º Entre na pasta do diretório

```bash
cd nome-do-projeto
```
#### º Inicie o git (não se esqueça de ir comitando)

```bash
cd nome-do-projeto
```
#### º Crie o ficheiro gitignore e adicione os seguintes elementos

### .gitignore 
```bash
    node_modules
    .env
```
#### º Criar pasta Api e mover para dentro todos os ficheiros

#### º Instalar as seguintes dependências não esquecer de verificar onde esta o terminal

```bash
npm install D @types/express @types/jest @types/supertest jest supertest ts-jest ts-node-dev typescript
```
#### º Instalar as seguintes dependências não esquecer de verificar onde esta o terminal

```bash
npm install dotenv express mysql2
```
#### º Instalar o eslint

```bash
npm install eslint
```
#### º Iniciar a configuração do eslint

```bash
npx eslint --init (escolher as seguintes opções)
```
- To check syntax, find problems , and enforce code
- Javascript
- None of these
- node
- Answer question about your style
    
    - Json
    - Tabs
    - Single
    - Unix
    - Yes
    - npm
#### º No ficheiro package.json adicionar os seguintes scripts
### package.json
```bash
"scripts": {
    "prebuild": "rm -rf dist",
    "build": "tsc",
    "postbuild": "cp ./src/database/*.sql ./dist/src/database",
    "test": "jest test --runInBand",
    "test:unit": "jest test/unit",
    "test:integration": "jest test/integration --runInBand",
    "lint": "eslint . --ext .ts",
    "dev": "ts-node-dev src/server.ts",
    "prestart": "npm run build",
    "start": "node dist/src/server.js"
  },
```
#### º Configurar o ficheiro tsconfig.json
### tsconfig.json

```bash
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "rootDir": "./",
    "outDir": "./dist",
    "allowJs": true,
    "moduleResolution": "node",
    "types":[
      "@types/jest"
    ],
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
  },
  "exclude": ["./dist"],
  "include": ["src/**/*"]
}
```
#### º Criação do Dockerfile na pasta Api
## Dockerfile

```bash
FROM node:16-alpine

WORKDIR /app/backend

COPY package*.json .

RUN npm install

COPY . .

```

#### º Criação do Dockercompose na pasta Api
## docker-compose-yml

```bash
version: '3'

services:
  api:
    build: .
    container_name: api
    tty: true
    stdin_open: true
    command: sh
    working_dir: /app/backend
    volumes:
      - ./:/app/backend
    ports:
      - 3001:3001
    env_file:
      - .env
    depends_on:
      - db
    
  db:
    image: mysql:8.0.32
    container_name: database
    environment:
      - MYSQL_ROOT_PASSWORD={MYSQL_ROOT_PASSWORD}
    ports:
      - 3306:3306

```
#### º Criação do ficheito .env
### .env

```bash
MYSQLUSER = 'root'
MYSQLPASSWORD = 'root'
MYSQL_ROOT_PASSWORD = 'root'
MYSQLPORT = 3306
MYSQLDB = db
```

#### º Rodar o docker no terminal
```bash
docker compose up -d
```
#### º Parar o docker caso precise
```bash
docker compose down
```

#### º Abrir o docker na linha de comando
```bash
docker exec -it api sh
```

## Iniciando desenvolvimento

#### º Criar a pasta src na raiz e o ficheiro App.ts dentro dela
### App.ts
```bash
import express from 'express';
const app = express();

app.use(express.json());

app.get('/health', (_req, res) => {
	res.status(200).send('ok');
});

export default app;
```
#### º Criar o servidor dentro de src/Server.ts
### Server.ts
```bash
import app from './app';
const PORT = process.env.PORT || 3001;

const server = app.listen(PORT, () => {
	console.log(`Server running on port ${PORT}`);
});

export default server;
```
#### º Testar se o servidor está a rodar
```bash
docker exec -it api sh
npm run dev
```
Se obtiver "Server running on port 3001" está tudo certo.


## Testando o app com Insomnia
### º Criar uma pasta para o request dos pedidos (Nome do app)

### Fazer requisição Get health para ver se api está a funcionar
```bash
http://localhoost:3001/health
```
Se o returno for status 200 e mensagem 'Ok' está tudo certo.

### ! Dica criar variável de ambiente http://localhost:3001 para facilitar proximos requests











## Autores

- [@FranciscoCosta](https://www.github.com/FranciscoCosta)

