cors 설치

---

```bash
npm install cors
```

http-status-codes 설치

---

```bash
npm install http-status-codes
```

dotenv 설치

---

```bash
npm install dotenv
```

.gitignore 설정

---

```markdown
node_modules/
.env
.env.*
```

환경변수 설정

---

``` markdown
PORT=3000
```

.error 설정
``` js
export class 에러이름 extends Error {
    errorCode = "에러코드";

    constructor(reason, data) {
        super(reason);
        this.reason = reason;
        this.data = data;
    }
}
```
---

index.js 설정
1. 모듈 불러오기
    ``` js
    import express from 'express'
    import dotenv from 'dotenv'
    import cors from 'cors'

    dotenv.config();
    ```
2. 포트 env로 설정
    ``` js
    const port = process.env.PORT
    ```
3. cors 설정정
    ``` js
    const corsOptions = {
      origin: '*',
      methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
      preflightContinue: false,
      optionsSuccessStatus: 204
    }

    app.use(cors(corsOptions));
    ```
4. 정적 파일 설정
    ```js
    app.use(express.static('public'));
    app.use(express.json());
    app.use(express.urlencoded({ extended: false }));
    ```
5. 에러처리
    ``` js
    app.use((req, res, next) => {
        res.success = (success) => {
            return res.json({ resultType: "SUCCESS", error: null, success });
        };
        res.error = ({ errorCode = "unknown", reason = null, data = null }) => {
            return res.json({
                resultType: "FAIL",
                error: { errorCode, reason, data },
                success: null,
            });
        };
        next();
    });
    ```
    (err, req, res, next)는 항상 마지막에 사용합니다.
    ``` js
    app.use((err, req, res, next) => {
        if (res.headersSent) {
            return next(err);
        }

        res.status(err.statusCode || 500).error({
            errorCode: err.errorCode || "unknown",
            reason: err.reason || err.message || null,
            data: err.data || null,
        });
    });
    ```
프로젝트 구조

---

```markdown
📦 project
┣ 📂.node_module
┣ 📂.vscode
┃ ┗ 📜settings.json
┃ 📂public
┣ 📂src
┃ ┣📜error.js
┃ ┗📜index.js
┣ 📜.env
┣ 📜.gitignore
┣ 📜package-lock.json
┣ 📜package.json
┗ 📜README.md
```
