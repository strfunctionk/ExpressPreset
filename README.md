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

## User 회원가입 예시

index

```js
//index.js
import { handleUserSignUp } from "./controllers/user.controller.js";

app.post("/v1/api/signup", handleUserSignUp);
```

error

```js
// error.js
export class DuplicateUserEmailError extends Error {
  errorCode = "001";

  constructor(reason, data) {
    super(reason);
    this.reason = reason;
    this.data = data;
  }
}
```

controller

```js
import { StatusCodes } from "http-status-codes";
import { bodyToUser } from "../dtos/user.dto.js";
import { userSignUp } from "../services/user.service.js";

export const handleUserSignUp = async (req, res, next) => {
  try {
    console.log("회원가입을 요청했습니다!");
    console.log("body:", req.body); // 값이 잘 들어오나 확인하기 위한 테스트용

    const user = await userSignUp(bodyToUser(req.body));
    res.status(StatusCodes.OK).success(user);
  } catch (err) {
    return next(err);
  }
};
```

dto

```js
export const bodyToUser = (body) => {
  return {
    name: body.name,
    email: body.email,
  };
};
export const responseFromUser = ({ user }) => {
  return {
    email: user.email,
    name: user.name,
  };
};
```

service

```js
import { responseFromUser } from "../dtos/user.dto.js";
import { addUser, getUser } from "../repositories/user.repository.js";
import { DuplicateUserEmailError } from "../errors.js";

export const userSignUp = async (data) => {
  const UserId = await addUser({
    name: data.name,
    email: data.email,
  });

  if (UserId === null) {
    throw new DuplicateUserEmailError("이미 존재하는 이메일입니다.", data);
  }

  const user = await getUser(UserId);
  return responseFromUser({
    user,
  });
};
```

repository

```js
import { prisma } from "../db.config.js";
export const addUser = async (data) => {
  const user = await prisma.user.findFirst({ where: { email: data.email } });
  if (user) {
    return null;
  }
  const created = await prisma.user.create({ data: data });
  return created.id;
};
export const getUser = async (userId) => {
  const user = await prisma.user.findFirstOrThrow({ where: { id: userId } });
  return user;
};
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
┃ ┃ ┗ 📜user.controller.js
┃ ┣ 📂services
┃ ┃ ┗ 📜user.services.js
┃ ┣ 📂repositories
┃ ┃ ┗ 📜user.repository.js
┃ ┣ 📂dtos
┃ ┃ ┗ 📜user.dto.js
┃ ┣📜db.config.js
┃ ┣📜error.js
┃ ┗📜index.js
┣ 📜.env
┣ 📜.gitignore
┣ 📜package-lock.json
┣ 📜package.json
┗ 📜README.md
```
