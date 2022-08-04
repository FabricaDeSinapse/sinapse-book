# TypeScript - Criando um Projeto

![image](https://user-images.githubusercontent.com/66227147/182867883-64eb69b3-6bb3-43dd-8aec-346565986a10.png)

1. Inicializar um projeto NodeJS
```bash
npm init -y
```
2. Criar um arquivo `index.ts`
3. Adicionar um trecho de código
```ts
const nome: string = "Paulo Salvatore 2";
console.log(nome);
```
4. Instalar o TypeScript como DevDependency
```bash
npm i -D typescript
```
5. Inicializar um projeto TypeScript, criando o arquivo `tsconfig.json`
```bash
npx tsc --init
```
6. Transpilar os arquivos `.TS` para arquivos `.JS`
```bash
npx tsc
```
7. Executar o arquivo `.JS` que foi gerado
```bash
node index.js
```

## Rodando diretamente o arquivo `.TS`
- Se quiser rodar diretamente o arquivo `.TS` sem transpilar
```bash
npx ts-node index.ts
```

## Scripts para facilitar a utilização do compilador do TypeScript (TypeScript Compiler - TSC) e do TS-Node
```json
  "scripts": {
    "start": "node dist/index.js",
    "dev": "npx ts-node src/index.ts",
    "build": "rimraf dist && npx tsc",
    "build-and-run": "npm run build && npm start"
  },
```

## Bônus: pasta `dist` pasta `src`
- Arquivos `.JS` são gerados geralmente na pasta `"dist/"`
  - Para fazer isso, basta alterar a propriedade `"outDir"` do `tsconfig.json` para `"./dist"`
- Arquivos `.TS` geralmente ficam dentro da pasta `"src/"`
  - O compilador do TypeScript (TSC) identifica automaticamente todos os arquivos `.ts` e monta a estrutura da pasta `outDir` também de forma automática
