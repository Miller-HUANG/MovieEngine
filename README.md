# MovieEngine

电影网站，基于 Spring Boot + MyBatis-Plus + Thymeleaf。

## 环境要求

| 软件 | 版本 |
|------|------|
| JDK | 21+ |
| MySQL | 8.0+ |
| Redis | 7.x+ |
| Neo4j | 5.x+（可选，关系图谱功能需要） |

## 启动步骤

### 1. 创建数据库并导入数据

```bash
mysql -u root -p -e "CREATE DATABASE movie_engine DEFAULT CHARSET utf8mb4;"
mysql -u root -p --default-character-set=utf8mb4 movie_engine < src/main/resources/sql/init.sql
mysql -u root -p --default-character-set=utf8mb4 movie_engine < src/main/resources/sql/data.sql
```

### 2. 启动 Redis

```bash
redis-server
```

### 3. 启动 Neo4j（可选）

```bash
neo4j console
```

默认连接 `bolt://localhost:7687`，用户名 `neo4j`，密码 `neo4j`（首次登录需修改）。

如不使用 Neo4j，应用仍可正常启动，仅主创关系图谱页面无数据。

### 4. 设置环境变量

项目通过环境变量读取敏感配置，`application.yml` 中已设好默认值可直接启动。

**必须设置的：**

```bash
# Windows PowerShell
$env:DB_PASSWORD="你的MySQL密码"
$env:NEO4J_PASSWORD="你的Neo4j密码"

# Windows CMD
set DB_PASSWORD=你的MySQL密码
set NEO4J_PASSWORD=你的Neo4j密码

# Linux / macOS
export DB_PASSWORD=你的MySQL密码
export NEO4J_PASSWORD=你的Neo4j密码
```

**可选设置（不设则对应功能不生效，不影响其他功能）：**

```bash
# Spring AI 大模型（启用后还需删除 application.yml 中 OpenAiChatAutoConfiguration 和 ChatClientAutoConfiguration 的 exclude）
export AI_API_KEY=你的API密钥
export AI_BASE_URL=所选的url
export AI_MODEL=所选的model

# 支付宝沙箱（不设则自动降级为 Mock 支付）
export ALIPAY_APP_ID=你的沙箱APPID
export ALIPAY_PRIVATE_KEY=你的应用私钥
export ALIPAY_PUBLIC_KEY=支付宝公钥
```

### 5. 运行项目

```bash
mvn spring-boot:run
```

访问 http://localhost:8080

## 测试账号

| 用户名 | 密码 | 说明 |
|--------|------|------|
| vip1 | 123456 | VIP 用户，可观看全部影片 |
| user1 | 123456 | 普通用户，只能观看免费影片 |
