## swagger 문서 설치

```bash
npm add swagger-autogen swagger-ui-express
```

## swagger 문서 설정정

```js
//index.js
import swaggerAutogen from "swagger-autogen";
import swaggerUiExpress from "swagger-ui-express";

app.use(
  "/docs",
  swaggerUiExpress.serve,
  swaggerUiExpress.setup(
    {},
    {
      swaggerOptions: {
        url: "/openapi.json",
      },
    }
  )
);

app.get("/openapi.json", async (req, res, next) => {
  // #swagger.ignore = true
  const options = {
    openapi: "3.0.0",
    disableLogs: true,
    writeOutputFile: false,
  };
  const outputFile = "/dev/null";
  const routes = ["./src/index.js"];
  const protocol = req.protocol;
  const host = req.get("host");
  const doc = {
    info: {
      title: "제목",
      description: "설명",
    },
    host: `${protocol}://${host}`,
  };

  const result = await swaggerAutogen(options)(outputFile, routes, doc);
  res.json(result ? result.data : null);
});
```

## swagger 문서 작성

```js
#swagger.tags = ['User']
#swagger.summary = '회원가입'
#swagger.description = '회원가입을 위한 API입니다. 이메일, 이름, 아이디, 비밀번호를 포함해 요청해야 합니다. 선택적으로 avatar URL을 포함할 수 있습니다.'

    #swagger.requestBody = {
      required: true,
      content: {
        'application/json': {
          schema: {
            type: 'object',
            properties: {
              email: { type: 'string', example: 'email@email.com' },
              name: { type: 'string', example: '이름' },
              username: { type: 'string', example: '아이디' },
              password: { type: 'string', example: '비밀번호' },
              avatar: { type: 'string', example: 'https://example.com/avatar.png' }
            },
            required: ['email', 'name', 'username', 'password']
          }
        }
      }
    }

    #swagger.responses[200] = {
      description: '회원가입 성공',
      content: {
        'application/json': {
          schema: {
            type: 'object',
            properties: {
              resultType: { type: 'string', example: 'SUCCESS' },
              error: { type: 'object', example: null },
              success: {
                type: 'object',
                properties: {
                  id: { type: 'number', example: 1 },
                  email: { type: 'string', example: 'email@email.com' },
                  name: { type: 'string', example: '이름' },
                  username: { type: 'string', example: '아이디' },
                  password: { type: 'string', example: '비밀번호' },
                  avatar: { type: 'string', example: 'https://example.com/avatar.png' },
                  createdAt: { type: 'string', example: '2023-01-01T00:00:00.000Z' },
                  updatedAt: { type: 'string', example: '2023-01-01T00:00:00.000Z' }
                }
              }
            }
          }
        }
      }
    }

    #swagger.responses[409] = {
      description: '이메일 중복',
      content: {
        'application/json': {
          schema: {
            type: 'object',
            properties: {
              resultType: { type: 'string', example: 'FAIL' },
              error: {
                type: 'object',
                properties: {
                  errorCode: { type: 'string', example: 'duplicate_email' },
                  reason: { type: 'string', example: '이미 존재하는 이메일입니다.' },
                  data: { type: 'object', example: null }
                }
              },
              success: { type: 'object', example: null }
            }
          }
        }
      }
    }

    #swagger.responses[400] = {
      description: '잘못된 요청',
      content: {
        'application/json': {
          schema: {
            type: 'object',
            properties: {
              resultType: { type: 'string', example: 'FAIL' },
              error: {
                type: 'object',
                properties: {
                  errorCode: { type: 'string', example: 'invalid_request' },
                  reason: { type: 'string', example: '요청 데이터가 잘못되었습니다.' },
                  data: { type: 'object', example: null }
                }
              },
              success: { type: 'object', example: null }
            }
          }
        }
      }
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
