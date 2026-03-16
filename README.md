# feishu-docs-cli

基于飞书开放平台 Golang SDK 的命令行工具，支持文档(Docx)和知识库(Wiki)的增删改查操作。

> 🚀 **v0.2.0 更新**：新增 7 种文档更新模式、Wiki 链接解析、更完善的 Markdown 支持

## 特性

- ✅ **文档 CRUD**：创建、读取、更新、删除文档
- ✅ **7 种更新模式**：append/overwrite/replace_range/replace_all/insert_before/insert_after/delete_range
- ✅ **Wiki 解析**：自动解析 wiki 链接获取实际文档类型和 token
- ✅ **Markdown 支持**：标题、列表、代码块、待办等
- ✅ **知识库管理**：空间、节点的完整操作
- ✅ **跨平台**：支持 macOS、Linux (amd64/arm64)

## 安装

### 通过 Homebrew（推荐）

```bash
brew tap KQAR/tap
brew install feishu-docs-cli
```

升级：

```bash
brew upgrade feishu-docs-cli
```

### 从源码编译

```bash
git clone https://github.com/KQAR/feishu-docs-cli.git
cd feishu-docs-cli
go install .
```

## 配置

运行初始化命令创建配置模板：

```bash
feishu-docs-cli init
```

编辑 `~/.config/feishu-docs/config.json`，填入飞书应用凭证：

```json
{
  "app_id": "cli_xxxx",
  "app_secret": "xxxx"
}
```

> 凭证获取方式：前往 [飞书开放平台开发者控制台](https://open.feishu.cn/app) 创建应用并获取。

## 使用

### 文档操作

```bash
# 创建文档
feishu-docs-cli doc create --title "我的文档" --folder-token <folder_token>

# 获取文档信息
feishu-docs-cli doc get --id <document_id>

# 获取文档纯文本内容
feishu-docs-cli doc content --id <document_id>

# 列出文档所有块
feishu-docs-cli doc blocks --id <document_id>

# 获取单个块的详细信息
feishu-docs-cli doc block --doc-id <document_id> --block-id <block_id>

# 插入内容块 (支持 text/heading1~9/bullet/ordered/code/todo)
feishu-docs-cli doc insert --doc-id <document_id> --text "Hello World"
feishu-docs-cli doc insert --doc-id <document_id> --text "标题" --type heading2 --index 0

# 更新块内容
feishu-docs-cli doc update --doc-id <document_id> --block-id <block_id> --text "新内容"

# 高级更新模式（支持7种模式）
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode append --markdown "追加内容"
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode overwrite --markdown "覆盖全部" --new-title "新标题"
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode replace_range --markdown "替换内容" --selection-with-ellipsis "开头...结尾"
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode replace_all --markdown "替换内容" --selection-by-title "## 章节标题"
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode insert_before --markdown "插入内容" --selection-by-title "## 目标标题"
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode insert_after --markdown "插入内容" --selection-with-ellipsis "定位文本..."
feishu-docs-cli doc update-v2 --doc-id <document_id> --mode delete_range --selection-with-ellipsis "开始...结束"

# 删除文档子块
feishu-docs-cli doc delete-blocks --doc-id <document_id> --block-id <block_id> --start 0 --end 2
```

### 知识库(Wiki)操作

```bash
# 列出知识空间
feishu-docs-cli wiki spaces

# 获取知识空间信息
feishu-docs-cli wiki space --id <space_id>

# 获取节点信息
feishu-docs-cli wiki node --token <node_token>

# 解析 wiki 链接获取实际文档
feishu-docs-cli wiki resolve --url "https://xxx.feishu.cn/wiki/ABC123"
feishu-docs-cli wiki resolve --url "wiki/ABC123"

# 列出子节点
feishu-docs-cli wiki nodes --space-id <space_id> [--parent <parent_node_token>]

# 创建节点
feishu-docs-cli wiki create --space-id <space_id> --title "新页面" [--obj-type docx] [--parent <parent_token>]

# 移动节点
feishu-docs-cli wiki move --space-id <space_id> --node-token <token> --target-parent <target_token>

# 复制节点
feishu-docs-cli wiki copy --space-id <space_id> --node-token <token> --target-parent <target_token> --target-space <target_space_id>
```

## 权限要求

请确保飞书应用已开通以下权限：

- `docx:document` - 文档读写权限
- `wiki:wiki` - 知识库读写权限

## 发布新版本

```bash
git tag -a v0.1.0 -m "Release v0.1.0"
git push origin v0.1.0
```

推送 tag 后，GitHub Actions 会自动构建多平台二进制并更新 Homebrew tap。

## 依赖

- [oapi-sdk-go v3](https://github.com/larksuite/oapi-sdk-go) - 飞书开放平台 Golang SDK
- [cobra](https://github.com/spf13/cobra) - CLI 框架
