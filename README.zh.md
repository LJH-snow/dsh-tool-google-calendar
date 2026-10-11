# @libai168/dsh-tool-google-calendar

面向 DeepSeek Harness 的只读 Google Calendar 插件，使用 Google Calendar API v3，不创建、修改或删除日历数据。

## 工具

- `gcal_auth_test`：验证 access token 或 refresh token，只返回受限的主日历摘要。
- `gcal_list_calendars`：分页读取日历列表。
- `gcal_list_events`：按日历和 RFC 3339 时间窗读取事件。
- `gcal_search_events`：使用 Calendar API 的 `q` 参数搜索事件。
- `gcal_get_event`：读取受限的单个事件摘要，不返回附件、会议数据或文件链接。

所有工具都是读取或搜索操作；描述、参与者数量和分页均有限制，分页 token 只作为不透明值传递。

## 配置

插件支持短期 `accessToken`，也支持 OAuth Web Server 凭据：

```yaml
- name: 'github:LJH-snow/dsh-tool-google-calendar'
  config:
    clientId: 'your-client-id.apps.googleusercontent.com'
    clientSecret: 'your-client-secret'
    refreshToken: 'your-refresh-token'
```

使用包内 OAuth 辅助命令：

```bash
npm exec dsh-google-calendar-auth -- --client-id "..." --client-secret "..."
```

辅助命令申请只读 Calendar scope，并打印 Cordis 配置片段；client secret 不会放入授权 URL。不要把 client secret 或 refresh token 提交到代码仓库。

## 安全边界

- 默认 endpoint 为 `https://www.googleapis.com/calendar/v3` 和 `https://oauth2.googleapis.com/token`。
- 自定义 endpoint 会拒绝 URL 凭据、query、fragment、localhost、私网/保留地址，以及解析到受阻地址的 DNS 结果。
- 测试注入 `fetch` 和 DNS lookup，不访问 Google 账号。

## 开发

```bash
npm ci
npm run typecheck
npm test
npm run build
npm pack --dry-run
```

详见 [DEVELOPMENT.md](DEVELOPMENT.md)。
