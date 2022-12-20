## Code Formatting

```json
// .prettierrc
{
  "useTabs": false,
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 120,
  "tabWidth": 2,
  "arrowParens": "avoid"
}
```

## Environment Variables

```sh
#.env

NODE_ENV=development
PORT=5000
```

## .gitignore

```sh
node_modules
*.log
.env
dist
```

## Error Handling

```
npm i express-async-errors
```

```ts
// server.ts

import 'express-async-errors';
```

```ts
// middleware/errorHandler.ts

import { Request, Response, NextFunction } from 'express';

const errorHandler = (err: Error, req: Request, res: Response, next: NextFunction) => {
  const status = res.statusCode || 500;
  if (process.env.NODE_ENV === 'development') console.log(err.stack);
  return res.status(status).json({ message: err.message });
};

export default errorHandler;
```

```ts
// server.ts

// below all routes
app.use(errorHandler);
```

## Not Found

```ts
// server.ts

app.all('*', (req: Request, res: Response) => {
  res.status(404).json({ message: '404 - No Route Found' });
});
```

## CORS

```
npm i cors
npm i -D @types/cors
```

```ts
// config/allowedOrigins.ts

const allowedOrigins: string[] = ['http://localhost:3000'];

export default allowedOrigins;
```

```ts
// config/corsOptions.ts

import allowedOrigins from './allowedOrigins';

const corsOptions = {
  origin: (origin: any, callback: Function) => {
    if (allowedOrigins.indexOf(origin) !== -1 || !origin) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true,
  optionsSuccessStatus: 200,
};

export default corsOptions;
```

```ts
// server.ts

app.use(cors(corsOptions));
```
