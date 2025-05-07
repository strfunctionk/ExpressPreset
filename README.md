Node.js 설치

---

[공식 사이트](https://nodejs.org/)에서 다운로드  
 설치 후 확인
`node -v npm -v`

프로젝트 초기화

---

```bash
npm init
```

Express 설치

---

```bash
npm install express
```

nodemon 설치 $\small{\color{#aaaaaa}(선택)}$

---

```bash
npm install --save-dev nodemon
```

모듈 방식 사용

---

```json
"type": "module"
```

.vscode 폴더에
settings.json

---

```json
{
  "javascript.preferences.importModuleSpecifierEnding": "js"
}
```

최종 package.json

---

```json
{
  "name": "project",
  "type": "module",
  "version": "1.0.0",
  "description": "project",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node src/index.js",
    "dev": "nodemon --exec node src/index.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^5.1.0"
  },
  "devDependencies": {
    "nodemon": "^3.1.10"
  }
}
```

src 폴더에
index.js 추가

---

```js
//index.js
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Server: http://localhost:${port}`);
});
```

프로젝트 구조

---

```markdown
📦 project
┣ 📂.node_module
┣ 📂.vscode
┃ ┗ 📜settings.json
┣ 📂src
┃ ┗ 📜index.js
┣ 📜package-lock.json
┣ 📜package.json
┗ 📜README.md
```
