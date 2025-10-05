# nodejsblog

基于 Node.js + Express + MongoDB 的个人博客系统

[![Build Status](https://travis-ci.org/sxyx2008/nodejsblog.png?branch=master)](https://travis-ci.org/sxyx2008/nodejsblog)
[![项目依赖状态](https://david-dm.org/sxyx2008/nodejsblog.png)](https://david-dm.org/sxyx2008/nodejsblog)

## 项目简介

nodejsblog 是一个使用 Node.js、Express 框架和 MongoDB 数据库开发的个人博客系统示例。项目实现了博客的核心功能，包括用户注册登录、发表文章、评论互动等，是学习 Node.js Web 开发的优秀实践项目。

## 技术栈

- **运行环境**: Node.js
- **Web 框架**: Express 3.2.5
- **数据库**: MongoDB
- **模板引擎**: EJS (Embedded JavaScript)
- **会话管理**: connect-mongo (MongoDB session store)
- **提示消息**: connect-flash
- **Markdown 支持**: markdown 模块
- **密码加密**: crypto (Node.js 内置)

## 功能特性

### 用户功能
- 用户注册（密码 MD5 加密）
- 用户登录/登出
- 会话管理（Session）
- Flash 消息提示

### 博客功能
- 发表文章（支持 Markdown）
- 文章列表展示（分页）
- 文章详情查看
- 文章评论
- 按用户筛选文章
- 文章归档（按时间）

### 其他功能
- 响应式布局
- 表单验证
- 错误处理
- Travis CI 持续集成

## 项目结构

```
nodejsblog/
├── app.js                    # 应用入口文件
├── package.json             # 项目依赖配置
├── settings.js              # 应用配置文件
├── .travis.yml              # Travis CI 配置
├── models/                  # 数据模型
│   ├── db.js               # 数据库连接
│   ├── user.js             # 用户模型
│   ├── post.js             # 文章模型
│   └── comment.js          # 评论模型
├── routes/                  # 路由控制
│   └── index.js            # 路由定义
├── views/                   # 视图模板（EJS）
│   ├── index.ejs           # 首页
│   ├── reg.ejs             # 注册页
│   ├── login.ejs           # 登录页
│   ├── post.ejs            # 发表文章
│   ├── article.ejs         # 文章详情
│   ├── user.ejs            # 用户文章列表
│   ├── archive.ejs         # 归档页面
│   ├── header.ejs          # 页头
│   ├── footer.ejs          # 页尾
│   └── ...
├── public/                  # 静态资源
│   ├── stylesheets/        # CSS 文件
│   ├── javascripts/        # JavaScript 文件
│   └── images/             # 图片资源
├── node_modules/           # 依赖包
└── Screenshots/            # 项目截图
    ├── one.jpg
    └── two.jpg
```

## 数据模型

### User (用户)
- `name`: 用户名
- `password`: 密码（MD5 加密）
- `email`: 邮箱（可选）

### Post (文章)
- `name`: 作者名
- `title`: 文章标题
- `post`: 文章内容（支持 Markdown）
- `time`: 发布时间
- `comments`: 评论数组

### Comment (评论)
- `name`: 评论者
- `email`: 邮箱（可选）
- `website`: 网站（可选）
- `time`: 评论时间
- `content`: 评论内容

## 核心路由

| 路径 | 方法 | 功能 | 权限 |
|-----|------|------|------|
| `/` | GET | 首页（文章列表） | 公开 |
| `/reg` | GET/POST | 用户注册 | 未登录 |
| `/login` | GET/POST | 用户登录 | 未登录 |
| `/logout` | GET | 用户登出 | 已登录 |
| `/post` | GET/POST | 发表文章 | 已登录 |
| `/u/:name` | GET | 用户文章列表 | 公开 |
| `/p/:name/:day/:title` | GET/POST | 文章详情和评论 | 公开 |
| `/archive` | GET | 文章归档 | 公开 |

## 依赖要求

- **Node.js**: 0.10.x 或更高版本（建议使用 LTS 版本）
- **MongoDB**: 2.4 或更高版本
- **npm**: Node.js 包管理器

### 主要依赖包

```json
{
  "express": "3.2.5",        // Web 框架
  "ejs": "*",                // 模板引擎
  "mongodb": "*",            // MongoDB 驱动
  "connect-mongo": "*",      // MongoDB session 存储
  "connect-flash": "*",      // Flash 消息
  "markdown": "*"            // Markdown 支持
}
```

## 安装和运行

### 1. 克隆项目

```bash
cd /Users/luowei/projects/My_Github/apps/nodejsblog
```

### 2. 安装依赖

```bash
npm install
```

### 3. 启动 MongoDB

**Windows**:
```bash
mongod --dbpath=C:\data\db
```

**Linux/Mac**:
```bash
mongod --dbpath=/data/db
```

### 4. 配置数据库

编辑 `settings.js` 配置文件：

```javascript
module.exports = {
    cookieSecret: 'myblog',      // Cookie 密钥
    db: 'blog',                  // 数据库名
    host: 'localhost'            // MongoDB 主机
};
```

数据库连接配置在 `app.js` 中：

```javascript
store: new MongoStore({
    db: settings.db,
    collection: "sessions",
    auto_reconnect: true,
    url: 'mongodb://root:root@localhost:27017/blog'  // 根据实际情况修改
})
```

### 5. 启动应用

```bash
# 启动开发服务器
npm start

# 或直接运行
node app.js
```

### 6. 访问应用

打开浏览器访问：http://localhost:3000

## 使用说明

### 注册新用户

1. 访问 http://localhost:3000/reg
2. 输入用户名和密码
3. 确认密码后提交注册

### 发表文章

1. 登录账号
2. 访问 http://localhost:3000/post
3. 输入文章标题和内容（支持 Markdown 格式）
4. 提交发表

### 查看和评论文章

1. 在首页点击文章标题
2. 阅读文章内容
3. 在底部填写评论信息
4. 提交评论

### 查看用户文章

- 点击作者名或访问 `/u/:username` 查看该用户的所有文章

### 文章归档

- 访问 `/archive` 查看按时间归档的所有文章

## 开发要点

### 中间件配置 (app.js)

```javascript
// 会话管理
app.use(express.session({
    secret: settings.cookieSecret,
    key: settings.db,
    cookie: {maxAge: 1000 * 60 * 60 * 24 * 30},  // 30天
    store: new MongoStore({
        db: settings.db,
        collection: "sessions",
        auto_reconnect: true,
        url: 'mongodb://root:root@localhost:27017/blog'
    })
}));

// Flash 消息
app.use(flash());
```

### 访问控制

```javascript
// 检查未登录
function checkNotLogin(req, res, next) {
    if (req.session.user) {
        req.flash('error', '已登录!');
        return res.redirect('back');
    }
    next();
}

// 检查已登录
function checkLogin(req, res, next) {
    if (!req.session.user) {
        req.flash('error', '未登录!');
        return res.redirect('/login');
    }
    next();
}
```

### 密码加密

```javascript
var crypto = require('crypto');
var md5 = crypto.createHash('md5');
var password = md5.update(req.body.password).digest('hex');
```

### 分页实现

```javascript
// 获取第 page 页的 10 篇文章
Post.getTen(null, page, function(err, posts) {
    // 处理分页逻辑
});
```

## 项目截图

### 主页

![主页](Screenshots/one.jpg)

### 文章内容

![文章内容](Screenshots/two.jpg)

## 扩展建议

- 添加文章编辑和删除功能
- 实现文章标签和分类
- 添加文章搜索功能
- 支持图片上传
- 增强 Markdown 编辑器
- 添加用户头像功能
- 实现评论回复功能
- 添加文章点赞功能
- 优化前端界面（Bootstrap/Tailwind CSS）
- 升级到 Express 4.x 或 5.x
- 使用 Mongoose ODM
- 添加 RESTful API

## 注意事项

- 当前使用的是 Express 3.2.5，较为老旧，建议学习后升级到最新版本
- MongoDB 连接字符串包含明文密码，生产环境应使用环境变量
- 密码使用 MD5 加密不够安全，建议使用 bcrypt
- 会话存储在 MongoDB，确保数据库正常运行
- 代码中使用了同步操作，大规模应用应使用异步

## 开发和调试

### 开发模式

```bash
# 使用 nodemon 实现热重载
npm install -g nodemon
nodemon app.js
```

### 调试

```bash
# 启用调试模式
DEBUG=* node app.js
```

### 测试

项目配置了 Travis CI，可查看 `.travis.yml` 文件了解测试配置。

## 技术特点

1. **MVC 架构**: 清晰的模型-视图-控制器分离
2. **会话管理**: 使用 MongoDB 存储 Session
3. **Flash 消息**: 友好的用户提示
4. **Markdown 支持**: 文章内容支持 Markdown 格式
5. **分页功能**: 文章列表分页展示
6. **访问控制**: 路由级别的权限验证

## 许可证

本项目仅供学习和参考使用。

## 相关资源

- [Express 官方文档](https://expressjs.com/)
- [MongoDB 官方文档](https://docs.mongodb.com/)
- [EJS 模板引擎](https://ejs.co/)
- [Markdown 语法指南](https://www.markdownguide.org/)
