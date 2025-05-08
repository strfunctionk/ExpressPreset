## Prisma 설치

```bash
npm install prisma @prisma/client
```

## Prisma 설정

```bash
npm exec prisma init
```

prisma 폴더에 schema.prisma 생성
provider mysql로 수정

```sql
generator client {
  provider = "prisma-client-js"
  output   = "../src/generated/prisma"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

.env 추가

```
DATABASE_URL="mysql://root:{비밀번호}@localhost:3306/{데이터베이스이름}"
```

package.json 수정

```json
  "dev": "nodemon -e js,json,prisma --exec \"prisma generate && node src/index.js\""
```

prisma에서 output 설정이 되어있으면, 무한 반복이 될 수 있음

```sql
output = "../src/generated/prisma"
```

db.congig.js 추가

```js
import { PrismaClient } from "@prisma/client";

export const prisma = new PrismaClient();
```

## 프로젝트 구조

```markdown
📦 project
┣ 📂.node_module
┣ 📂.vscode
┃ ┗ 📜settings.json
┃ 📂prisma
┃ ┗ 📜schema.prisma
┃ 📂public
┣ 📂src
┃ ┣ 📂controllers
┃ ┣ 📂services
┃ ┣ 📂repositories
┃ ┣ 📂dtos
┃ ┣📜db.config.js
┃ ┣📜error.js
┃ ┗📜index.js
┣ 📜.env
┣ 📜.gitignore
┣ 📜package-lock.json
┣ 📜package.json
┗ 📜README.md
```
