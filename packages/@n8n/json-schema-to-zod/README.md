# Json-Schema-to-Zod

一个用于在运行时将 JSON schema（草案 4+）对象转换为 Zod 对象形式的 Zod schema 的包。

## 安装

```sh
npm install @n8n/json-schema-to-zod
```

### 简单示例

```typescript
import { jsonSchemaToZod } from "json-schema-to-zod";

const jsonSchema = {
  type: "object",
  properties: {
    hello: {
      type: "string",
    },
  },
};

const zodSchema = jsonSchemaToZod(myObject);
```

### 覆盖解析器

您可以将一个函数传递给 `overrideParser` 选项，该函数表示一个接收当前 schema 节点和引用对象的函数，并在希望替换默认输出时返回一个 zod 对象。如果节点应使用默认输出，只需返回 undefined。

## 鸣谢

这是 [`json-schema-to-zod`](https://github.com/StefanTerdell/json-schema-to-zod) 的一个分支。
