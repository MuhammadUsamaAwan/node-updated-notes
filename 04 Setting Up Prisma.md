```
npm i -D prisma
npm i @prisma/client
npx prisma init
```

After that change the env & then create a prisma model

```prisma
// example model

model User {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  email String @unique
  password  String
  firstName String?
  lastName  String?
}
```

```json
// package.json

"prisma-migrate": "npx prisma migrate dev"
```

```
npm run prisma-migrate
```

```ts
// use prisma

import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

// enjoy
```
