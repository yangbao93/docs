# maven打包语句

```bash
maven package -D skipTests
```

skipTests 代表跳过测试

The packaging for this project did not assign a file to the build artifact 当maven执行install:install出现如上错误时，则尝试改用install执行 当maven执行deploy:deployl出现如上错误时，则尝试改用deployl执行

