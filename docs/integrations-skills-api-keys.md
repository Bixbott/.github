# Tích hợp, Skill và ủy quyền API key

Trang này bổ sung tài liệu cho ba mảng nền tảng của Bixbott: **trang tích hợp**, **skill** và **ủy quyền API key**. Mục tiêu là giúp workspace kết nối dịch vụ ngoài một cách an toàn, đồng thời cho phép agent dùng đúng năng lực trong đúng phạm vi.

> Trạng thái: bản thiết kế tài liệu. Dùng làm cơ sở cho UI, CLI và API sau này.

## Trang tích hợp

Trang tích hợp là nơi cấu hình các kết nối giữa Bixbott và hệ sinh thái phát triển.

### Nhóm tích hợp đề xuất

| Nhóm | Ví dụ | Dữ liệu/quyền cần thiết |
| --- | --- | --- |
| Source control | GitHub, GitLab | Repository, issue, pull request, commit status |
| Deploy | Vercel, Netlify, Cloudflare | Project, environment, preview deployment |
| Observability | Sentry, Datadog, OpenTelemetry | Error, trace, release marker |
| Communication | Slack, Discord, Email | Notification, approval request |
| AI provider | OpenAI-compatible provider | Model, usage, API key scope |

### Trạng thái tích hợp

| Trạng thái | Ý nghĩa |
| --- | --- |
| `connected` | Kết nối hoạt động bình thường |
| `needs_action` | Cần cấp lại quyền, refresh token hoặc cấu hình thiếu |
| `disabled` | Tích hợp bị tắt thủ công |
| `error` | Kết nối lỗi và cần kiểm tra log |

## Skill

Skill là đơn vị năng lực có thể gán cho agent hoặc workspace để xử lý các nhóm tác vụ cụ thể.

### Metadata skill

```yaml
name: docs-writer
description: Tạo và cập nhật tài liệu kỹ thuật cho repository.
version: 0.1.0
capabilities:
  - docs.generate
  - docs.review
  - markdown.edit
required_integrations:
  - github
allowed_tags:
  - docs
  - area:docs
```

### Vòng đời skill

1. **Khám phá**: Workspace liệt kê skill có sẵn hoặc được cài từ registry nội bộ.
2. **Cài đặt**: Owner/Maintainer bật skill cho workspace.
3. **Cấu hình**: Chọn repo, tag, agent và giới hạn quyền.
4. **Sử dụng**: Agent chỉ dùng skill khi tác vụ phù hợp với tag/quyền.
5. **Kiểm toán**: Ghi log lần dùng skill, input chính và output quan trọng.

## Ủy quyền API key

API key không nên được chia sẻ trực tiếp cho mọi agent. Bixbott nên dùng cơ chế ủy quyền theo phạm vi để giảm rủi ro lộ khóa.

### Nguyên tắc bảo mật

- Chỉ cấp quyền tối thiểu cần thiết cho từng tác vụ hoặc skill.
- Không hiển thị lại secret sau khi tạo.
- Hỗ trợ xoay vòng, thu hồi và hết hạn tự động.
- Ghi audit log cho mọi lần agent truy cập secret hoặc gọi provider.
- Tách API key theo môi trường: `development`, `preview`, `production`.

### Scope đề xuất

| Scope | Mục đích |
| --- | --- |
| `repo:read` | Đọc repository và metadata |
| `repo:write` | Tạo branch, commit, pull request |
| `issue:write` | Tạo/cập nhật issue và comment |
| `deploy:preview` | Tạo preview deployment |
| `secret:read:delegated` | Cho agent đọc secret đã được ủy quyền |
| `model:invoke` | Gọi AI provider theo quota workspace |

### Mẫu cấu hình ủy quyền

```json
{
  "apiKeyId": "key_openai_workspace",
  "delegatedTo": "agent_docs",
  "scopes": ["model:invoke", "repo:read"],
  "expiresAt": "2026-07-01T00:00:00Z",
  "allowedSkills": ["docs-writer"],
  "allowedTags": ["docs", "area:docs"]
}
```

## CLI dự kiến

```bash
bixbott integrations list
bixbott integrations connect github
bixbott skills list
bixbott skills enable docs-writer --workspace current
bixbott api-keys create openai --scope model:invoke --env preview
bixbott api-keys delegate key_openai_workspace --to agent_docs --skill docs-writer --expires 14d
```

## Checklist triển khai

- [ ] Trang tích hợp có bộ lọc theo nhóm và trạng thái.
- [ ] Màn hình chi tiết tích hợp hiển thị quyền đã cấp và log đồng bộ.
- [ ] Skill registry hiển thị metadata, version và quyền yêu cầu.
- [ ] API key vault hỗ trợ tạo, xoay vòng, thu hồi và hết hạn.
- [ ] Cơ chế delegation bắt buộc scope, thời hạn và audit log.
