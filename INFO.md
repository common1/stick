# Project stick

```
[https://www.youtube.com/watch?v=G_99ZxDhVvo]
[https://github.com/Atiqullah-Naemi/stick]
```

## 1 - Project setup & dependencies


### 1.1 Project setup
```
[https://www.youtube.com/watch?v=G_99ZxDhVvo&t=344s]
```

```
npx shadcn@4.2.0 init --preset b0 --base base --template next 
√ What is your project named? ... stick
✔ Creating a new Next.js project.
✔ Writing components.json.
✔ Checking registry.
✔ Installing dependencies.
✔ Updating fonts.
✔ Created 2 files:
  - components\ui\button.tsx
  - lib\utils.ts
✔ Updating app\globals.css

Project initialization completed.
You may now add components.
```

### 1.2 Install and configure prisma

```
Install dependencies
npm install prisma@prev tsx @types/pg --save-dev
npm install @prisma/client@7 @prisma/adapter-pg dotenv pg
```

```
Initialize prisma
npx prisma init
✔ Skills installed

Initialized Prisma in your project

  prisma/
    schema.prisma
  prisma7.config.ts
  .env
  .claude/skills/
  .windsurf/skills/
  .agents/skills/
  skills-lock.json

warn You already have a .gitignore file. Don't forget to add .env in it to not commit any private information.
```
### 1.3 Create and configure database

```
Drop database and user

psql -U postgres
postgres=# DROP DATABASE stick_db; 
postgres=# DROP USER stick_user; 
```

```
Create database and user

psql -U postgres
postgres=# CREATE DATABASE stick_db; 
postgres=# CREATE USER stick_user WITH ENCRYPTED PASSWORD 'WXYZ&6789'; 
postgres=# GRANT ALL PRIVILEGES ON DATABASE stick_db TO stick_user; 
postgres=# \c stick_db postgres;
You are now connected to database "stick_db" as user "postgres"
stick_db=# GRANT ALL ON SCHEMA public TO stick_user; 
stick_db=# ALTER USER stick_user CREATEDB;
```

```
Create DATABASE_URL in .env
DATABASE_URL="postgres://stick_user:WXYZ&6789@localhost:5432/stick_db"
```

### 1.4 Instantiate Prisma Client

```
File: lib/db.ts
import "dotenv/config"
import { PrismaPg } from "@prisma/adapter-pg"
import { PrismaClient } from "@/generated/prisma/client"

const connectionString = `${process.env.DATABASE_URL}`

const adapter = new PrismaPg({ connectionString })
export const db = new PrismaClient({ adapter })
```

```
npx prisma generate
```

### 1.5 Define your prisma schema model

```
model User {
  id Int @id @default(autoincrement())
  email String @unique
  name String?
}
```

### 1.6 Run migrations

```
npx prisma migrate dev
Loaded Prisma config from prisma7.config.ts.

Prisma schema loaded from prisma\schema.prisma.
Datasource "db": PostgreSQL database "stick_db", schema "public" at "localhost:5432"

√ Enter a name for the new migration: ... initial migration
Applying migration `20261019093041_initial_migration`

The following migration(s) have been created and applied from new schema changes:

prisma\migrations/
  └─ 20261019093041_initial_migration/
    └─ migration.sql

Your database is now in sync with your schema.
```

```
Generate prisma client
npx prisma generate
Loaded Prisma config from prisma7.config.ts.

Prisma schema loaded from prisma\schema.prisma.

✔ Generated Prisma Client (7.10.1) to .\generated\prisma in 38ms
```

```
Start prisma studio

npx prisma studio
```

```
Remove all items from the database

npx prisma migrate reset
```

