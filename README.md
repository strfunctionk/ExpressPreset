## Prisma 유저 테이블 추가

```js
model User {
  id        Int      @id @default(autoincrement())
  name      String   @db.VarChar(255)
  username  String   @unique @db.VarChar(255)
  email     String   @unique(map: "email") @db.VarChar(255)
  password  String   @db.VarChar(255)
  avatar    String?  @db.VarChar(255)
  createdAt DateTime @default(now()) @map("created_at") @db.DateTime(6)
  updatedAt DateTime @default(now()) @updatedAt @map("updated_at") @db.DateTime(6)

  @@map("user")
}
```

## Prisma 설명

```js
model RelationA {
  ...생략
}

model RelationB {
  id        Int       @id @default(autoincrement())
  ...생략
}

model Multi {
  ...생략
}

model 모델명 {
  // primary key
  id         Int       @id @default(autoincrement())
  //optional
  option     Int?
  // relation scalar
  relationA  RelationA
  // multifield
  multis     Multi[]
  // relation
  relationB  RelationB @relation(fields: [relationId], references: [id])
  relationId Int       @map("relation_id")
  // updatedAt
  updatedAt  DateTime  @updatedAt

  // index
  @@index([fkeyId], map: "fkey_id")
  // map
  @@map("model")
}
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
