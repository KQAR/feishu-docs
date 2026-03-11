# feishu-docs CLI 技能

基于 [KQAR/feishu-docs](https://github.com/KQAR/feishu-docs) CLI 工具的飞书文档操作技能。

## 功能特性

- ✅ **文档 CRUD**：创建、读取、更新、删除文档
- ✅ **7 种更新模式**：append/overwrite/replace_range/replace_all/insert_before/insert_after/delete_range
- ✅ **Wiki 解析**：自动解析 wiki 链接获取实际文档类型和 token
- ✅ **Markdown 支持**：标题、列表、代码块、待办等
- ✅ **知识库管理**：空间、节点的完整操作

## 安装

### 下载预编译二进制

```bash
# Linux ARM64 (iSH 环境)
curl -sL -o /usr/local/bin/feishu-docs \
  "https://github.com/KQAR/feishu-docs/releases/download/v0.2.0/feishu-docs_0.2.0_linux_arm64.tar.gz"
tar -xzf /usr/local/bin/feishu-docs -C /usr/local/bin/
chmod +x /usr/local/bin/feishu-docs
```

### 配置

```bash
# 初始化配置
feishu-docs init

# 编辑配置文件
# 路径: ~/config/feishu-docs/config.json
cat > ~/config/feishu-docs/config.json << EOF
{
  "app_id": "YOUR_APP_ID",
  "app_secret": "YOUR_APP_SECRET"
}
EOF
```

## 使用示例

### 文档操作

```bash
# 创建文档
feishu-docs doc create --title "AI周报" --folder-token Fldxxx

# 获取文档内容
feishu-docs doc content --id BuModzb4aoJ9KhxJbN8cnpy6n5g

# 列出文档块
feishu-docs doc blocks --id BuModzb4aoJ9KhxJbN8cnpy6n5g

# 插入内容
feishu-docs doc insert --doc-id DOC_ID --text "新段落" --type text
feishu-docs doc insert --doc-id DOC_ID --text "## 标题" --type heading2

# 高级更新模式
feishu-docs doc update-v2 --doc-id DOC_ID --mode append --markdown "追加内容"
feishu-docs doc update-v2 --doc-id DOC_ID --mode replace_all --markdown "新内容" --selection-by-title "## 目标章节"
feishu-docs doc update-v2 --doc-id DOC_ID --mode delete_range --selection-with-ellipsis "开始...结束"
```

### Wiki 操作

```bash
# 解析 wiki 链接
feishu-docs wiki resolve --url "https://xxx.feishu.cn/wiki/ABC123"

# 列出知识空间
feishu-docs wiki spaces

# 获取节点信息
feishu-docs wiki node --token NODE_TOKEN

# 创建 wiki 节点
feishu-docs wiki create --space-id SPACE_ID --title "新页面" --obj-type docx
```

## 更新模式详解

| 模式 | 说明 | 必需参数 |
|-----|------|---------|
| `append` | 追加到文档末尾 | `--markdown` |
| `overwrite` | 完全覆盖文档（⚠️ 清空所有内容） | `--markdown` |
| `replace_range` | 定位替换指定范围 | `--markdown`, `--selection-*` |
| `replace_all` | 全文替换多处匹配 | `--markdown`, `--selection-*` |
| `insert_before` | 在目标前插入 | `--markdown`, `--selection-*` |
| `insert_after` | 在目标后插入 | `--markdown`, `--selection-*` |
| `delete_range` | 删除指定范围 | `--selection-*` |

### 定位语法

- `--selection-with-ellipsis`: 内容定位，如 `"开头文本...结尾文本"`
- `--selection-by-title`: 标题定位，如 `"## 章节标题"`

## 权限要求

请确保飞书应用已开通以下权限：

- `docx:document` - 文档读写权限
- `wiki:wiki` - 知识库读写权限

## 参考

- 飞书官方 OpenClaw 插件: https://github.com/larksuite/openclaw-lark
- 飞书开放平台文档: https://open.feishu.cn/
