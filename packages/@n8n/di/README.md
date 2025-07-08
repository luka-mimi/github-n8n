## @n8n/di

`@n8n/di` 是一个依赖注入（DI）容器库，基于 [`typedi`](https://github.com/typestack/typedi)。

n8n 不再使用 `typedi`，因为：

- `typedi` 不再正式维护
- 需要面向未来的功能，例如 stage-3 装饰器
- 足够小，值得维护的负担
- 更容易定制，例如简化单元测试

### 用法

```typescript
// 来自 https://github.com/typestack/typedi/blob/develop/README.md
import { Container, Service } from 'typedi';

@Service()
class ExampleInjectedService {
  printMessage() {
    console.log('我活着！');
  }
}

@Service()
class ExampleService {
  constructor(
    // 因为我们用 @Service() 注解了 ExampleInjectedService
    // 装饰器 TypeDI 会在请求 ExampleService 类时
    // 自动在此处注入 ExampleInjectedService 的实例
    public injectedService: ExampleInjectedService
  ) {}
}

const serviceInstance = Container.get(ExampleService);
// 我们从 TypeDI 请求 ExampleService 的一个实例

serviceInstance.injectedService.printMessage();
// 在控制台记录 "我活着！"
```

需要在 `tsconfig.json` 中启用以下标志：

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```
