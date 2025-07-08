## 调试不稳定的端到端测试 - 用法

要调试不稳定的端到端（E2E）测试，请使用以下命令：

```bash
pnpm run debug:flaky:e2e -- <grep_filter> <burn_count>
```

**参数：**

* `<grep_filter>`：（可选）一个字符串，用于通过 `it()` 或 `describe()` 块标题或使用 `@cypress/grep` 插件的标签来过滤测试。如果省略，将运行所有测试。
* `<burn_count>`：（可选）运行过滤测试的次数。如果未提供，默认为 5。

**示例：**

1.  **运行所有标记为 `CAT-726` 的测试十次：**

    ```bash
    pnpm run debug:flaky:e2e CAT-726 10
    ```

2.  **运行所有包含"login"的测试五次（默认烧录次数）：**

    ```bash
    pnpm run debug:flaky:e2e login
    ```

3.  **运行所有测试五次（默认 grep 和烧录次数）：**

    ```bash
    pnpm run debug:flaky:e2e
    ```
