# 学科评估结果数据导入Supabase工具

这个工具可以帮助你将学科评估结果表格数据导入到Supabase数据库中。

## 准备工作

### 1. 安装Python依赖

```bash
pip install -r requirements.txt
```

### 2. 获取Supabase项目信息

1. 登录到 [Supabase控制台](https://app.supabase.com)
2. 选择你的项目 `songweibiao`
3. 在左侧菜单中点击 "Settings" → "API"
4. 复制以下信息：
   - **Project URL**: 格式类似 `https://xxx.supabase.co`
   - **anon public key**: 一个很长的字符串

## 使用步骤

### 第一步：配置Supabase连接

运行配置脚本：

```bash
python setup_supabase.py
```

这个脚本会：
1. 要求你输入Supabase URL和API Key
2. 测试连接是否正常
3. 检查表 `pinggu` 是否存在
4. 如果表不存在，会显示创建表的SQL语句
5. 保存配置到 `.env` 文件

### 第二步：创建数据库表（如果需要）

如果表 `pinggu` 不存在，请在Supabase控制台中执行以下步骤：

1. 在Supabase控制台中，点击左侧的 "SQL Editor"
2. 点击 "New query"
3. 复制并粘贴配置脚本显示的SQL语句
4. 点击 "Run" 执行

表结构说明：
- `id`: 主键，自动递增
- `discipline_code`: 一级学科代码（如：0101）
- `discipline_name`: 一级学科名称（如：哲学）
- `evaluation_grade`: 评估等级（如：A+、A、A-、B+等）
- `school_code`: 学校代码
- `school_name`: 学校名称
- `created_at`: 创建时间
- `updated_at`: 更新时间

### 第三步：导入数据

运行导入脚本：

```bash
python import_data_to_supabase.py
```

这个脚本会：
1. 解析 `学科评估结果表格.md` 文件
2. 提取所有学科评估数据
3. 显示数据预览
4. 询问确认后批量导入到Supabase

## 数据格式

工具会从markdown表格中提取以下格式的数据：

```
| 一级学科代码 | 一级学科名称 | 评估等级 | 学校代码 | 学校名称 |
|---|---|---|---|---|
| 0101 | 哲学 | A+ | 10001 | 北京大学 |
```

## 功能特点

- ✅ 自动解析markdown表格格式
- ✅ 批量导入，提高效率
- ✅ 错误处理和重试机制
- ✅ 导入进度显示
- ✅ 数据预览功能
- ✅ 连接测试
- ✅ 环境变量管理

## 注意事项

1. **数据安全**: `.env` 文件包含敏感信息，请不要提交到版本控制系统
2. **数据备份**: 导入前建议备份现有数据
3. **重复数据**: 如果表中已有数据，可能会出现重复，建议先清空表或添加唯一约束
4. **网络连接**: 确保网络连接稳定，导入大量数据时可能需要较长时间

## 故障排除

### 连接失败
- 检查Supabase URL和API Key是否正确
- 确认网络连接正常
- 检查Supabase项目是否处于活跃状态

### 导入失败
- 检查表结构是否正确创建
- 确认数据格式是否符合要求
- 查看错误信息，可能是数据类型不匹配

### 数据格式问题
- 确保markdown文件格式正确
- 检查表格中是否有特殊字符
- 验证所有必需字段都有值

## 文件说明

- `import_data_to_supabase.py`: 主要的数据导入脚本
- `setup_supabase.py`: Supabase配置和连接测试脚本
- `requirements.txt`: Python依赖包列表
- `学科评估结果表格.md`: 源数据文件
- `.env`: 环境变量配置文件（运行后生成）

## 支持

如果遇到问题，请检查：
1. Python版本（建议3.7+）
2. 网络连接
3. Supabase项目状态
4. 数据文件格式

导入完成后，你可以在Supabase控制台的 "Table Editor" 中查看导入的数据。