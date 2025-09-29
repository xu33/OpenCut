当然可以，以下是你的 README.md 中文翻译：

---

在按照本指南操作前，请确保你已经完成了[主项目 README](../../README.md)中“可选：自动字幕（转写）设置”部分的步骤。

打开终端，确保你当前在 transcription 目录下。

1. 创建虚拟环境

```bash
python -m venv env
```

2. 激活虚拟环境

**Windows:**

```bash
env\Scripts\activate
```

**macOS/Linux:**

```bash
source env/bin/activate
```

> 注意：如果你在 VS Code/Cursor 里看到导入模块报错，  
> 你可能需要按 CTRL + Shift + P -> Python: Select Interpreter -> 输入解释器路径 -> 查找 env -> scripts -> python.exe

3. 安装依赖库/包

```bash
pip install -r requirements.txt
```

4. 确保你有一个 Modal 账号。如果没有：[注册一个](https://modal.com/)

> 如果你不知道 Modal 是什么：它允许我们用大量内存处理音频并用 Whisper 进行转写，Modal 提供了运行 Python 代码的基础设施，价格也比较实惠。

5. 有了 Modal 账号后，运行：

```bash
python -m modal setup
```

这会打开浏览器让你进行认证。

6. 测试一下，确保一切正常：

```bash
modal run transcription.py
```

6. 部署函数！

```bash
modal deploy transcription.py
```

7. 在 Modal 设置所需的密钥（Secrets）

我们刚刚部署的脚本需要和 Cloudflare 交互，做两件事：

- 下载音频文件（用于 Whisper 转写）
- 处理完后删除文件（保护隐私）

为此，脚本需要访问以下环境变量：

```bash
CLOUDFLARE_ACCOUNT_ID=your-account-id
R2_ACCESS_KEY_ID=your-access-key-id
R2_SECRET_ACCESS_KEY=your-secret-access-key
R2_BUCKET_NAME=opencut-transcription
```

这些变量我们之前已经在 `.env.local` 里设置过。

现在来操作：

- 打开 [Modal Secrets](https://modal.com/secrets/mazewinther/main)
- 点击“Custom”，名称填写 "opencut-r2-secrets"
- 点击 "Import .env"，然后复制粘贴你 `.env.local` 文件里的这 4 个变量。只复制如下内容：
  ```bash
  CLOUDFLARE_ACCOUNT_ID=your-account-id
  R2_ACCESS_KEY_ID=your-access-key-id
  R2_SECRET_ACCESS_KEY=your-secret-access-key
  R2_BUCKET_NAME=opencut-transcription
  ```
- 点击 "Done"，你应该会看到一些很酷的粒子动画！

---
