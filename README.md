# 个人知识管理系统（PKM）

基于 **Spring Boot 3** 的个人知识管理系统，集文档管理、**Lucene 中文全文检索**、**知识图谱可视化**、数据备份与操作审计于一体，前后端不分离架构（Thymeleaf + 原生 JS），JWT 令牌认证。

![Java](https://img.shields.io/badge/Java-17-blue) ![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.7-brightgreen) ![MyBatis](https://img.shields.io/badge/MyBatis-3.0.5-red) ![Lucene](https://img.shields.io/badge/Lucene-9.x-orange) ![MySQL](https://img.shields.io/badge/MySQL-8.x-lightgrey)

## 功能特性

- **用户认证**：注册 / 登录，JWT 令牌鉴权，密码加密存储
- **文档管理**：Markdown 文档的创建、编辑、删除，支持分类（Category）与标签（Tag）多维组织
- **全文检索**：基于 Lucene + SmartCN 中文分词的站内全文搜索，检索历史记录
- **知识图谱**：文档、分类、标签之间的关联关系可视化
- **收藏夹**：文档收藏与收藏数统计
- **回收站**：文档软删除与恢复
- **数据备份**：一键全量导出 JSON 备份文件
- **操作日志**：用户关键操作自动审计记录
- **统计仪表盘**：文档数量、分类分布、近期动态等可视化看板

## 技术栈

| 层次 | 技术 |
|---|---|
| 框架 | Spring Boot 3.5.7、Spring MVC |
| 前端 | 原生 HTML/CSS/JavaScript（模块化组织，RESTful 接口对接） |
| 检索 | Apache Lucene 9.x + SmartCN 中文分析器 |
| 认证 | JJWT（JWT 令牌） |
| 持久层 | MyBatis 3.0.5、MySQL 8.x |
| 工具 | Lombok、Maven |

## 项目结构

```
src/main/java/com/dzy666/demo/
├── DemoApplication.java      # 启动类
├── config/                   # 配置（Web、拦截器等）
├── controller/               # 控制层（14 个模块控制器）
├── service/                  # 业务层（12 个服务：文档/搜索/图谱/备份...）
├── mapper/                   # MyBatis 数据访问接口
├── entity/                   # 数据库实体
├── dto/                      # 数据传输对象
└── util/                     # 工具类（JWT 等）

src/main/resources/
├── application.yml           # 应用配置（数据库、端口）
└── static/                   # 前端页面与静态资源
    ├── index.html            # 主界面（文档/搜索/图谱/看板）
    ├── login.html            # 登录注册
    └── *.js / *.css          # 模块化前端逻辑与样式
```

## 快速开始

### 环境要求

- JDK 17+
- MySQL 8.x
- Maven 3.6+（或使用项目自带 Wrapper）

### 运行步骤

```bash
# 1. 创建数据库
mysql -u root -p -e "CREATE DATABASE pkm_db DEFAULT CHARACTER SET utf8mb4;"

# 2. 修改 src/main/resources/application.yml 中的数据库账号密码

# 3. 构建并启动
mvn spring-boot:run
```

启动后浏览器访问 **http://localhost:8080**，注册账号即可使用。

### 构建

```bash
mvn clean package
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

## 核心模块说明

| 模块 | 说明 |
|---|---|
| SearchService | 文档变更时增量维护 Lucene 索引，搜索时 SmartCN 分词多字段查询 |
| KnowledgeGraphService | 抽取文档-分类-标签关联关系，构建图谱节点与边 |
| BackupService | 全量数据序列化为 JSON 快照，支持历史备份列表与恢复 |
| OperationLogService | AOP 式关键操作审计，记录操作人、行为与时间 |
