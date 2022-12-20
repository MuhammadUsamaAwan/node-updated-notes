```
npm init -y
npm i express dotenv
npm i -D nodemon typescript ts-node @types/node @types/express
npx tsc --init
```

```json
// package.json

"start": "node dist/server.js",
"dev": "nodemon src/server.ts",
"build": "npx tsc -p ."
```

```json
// tsconfig.json

"rootDir": "./src",
"outDir": "./dist"
```

```ts
// server.ts

import { config } from 'dotenv';
config();

import express from 'express';
const PORT = process.env.PORT || 5000;

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.listen(PORT, () => console.log(`Server Started on Port ${PORT}`));
```
