# Tududi 懒猫微服包

自托管任务与生活管理：项目、任务、笔记、领域与习惯。

- 上游：<https://github.com/chrisvel/tududi>
- 镜像：`chrisvel/tududi`
- 许可：MIT（全文见 content/licenses/LICENSE）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `tududi` | `chrisvel/tududi` | 应用本体，监听 3002 |
| `db` | `postgres:16-alpine` | 数据库 |

## 关键配置说明

- **DSN 里的密码用十六进制编码**：`DATABASE_URL` 按 URL 解析，密码里若含 `@ : /`
  会破坏连接串，所以用 `printf "%x" (stable_secret ...)` 转成纯字母数字，
  数据库侧的 `POSTGRES_PASSWORD` 用同一个值保持一致。
- **管理员账号来自部署参数**：上游默认关闭自助注册
  （`registrationService.js` 中 `registration_enabled` 默认 false），
  因此首个可用账号由 `TUDUDI_USER_EMAIL` / `TUDUDI_USER_PASSWORD` 预置。
- `TUDUDI_SESSION_SECRET` 用稳定密钥，变化会让所有人被登出。
- `TUDUDI_TRUST_PROXY=true`：入口在容器前面转发，需信任代理头。
- 应用自带账号体系，因此开放 `public_path`，由应用自己鉴权。

## 未验证部分

未在真机上实测：登录、创建任务与笔记、上传附件、重启后数据留存。
