# Bixbott Command Docs

Tài liệu này phác thảo hệ thống lệnh dự kiến cho Bixbott CLI/Agent IDE. Mục tiêu là tạo một trang docs nền tảng trước, sau đó có thể tách từng nhóm lệnh thành các trang chi tiết riêng.

> Trạng thái: bản thiết kế tài liệu ban đầu. Một số lệnh có thể thay đổi khi CLI chính thức được triển khai.

## Cách đọc tài liệu

Mỗi lệnh được mô tả theo cấu trúc:

- **Mục đích**: lệnh dùng để làm gì.
- **Cú pháp**: cách gọi lệnh.
- **Tùy chọn quan trọng**: các flag thường dùng.
- **Ví dụ**: tình huống sử dụng thực tế.

## Cài đặt

```bash
npm install -g bixbott
```

Kiểm tra phiên bản:

```bash
bixbott --version
```

Xem trợ giúp tổng quát:

```bash
bixbott --help
```

## Tổng quan nhóm lệnh

| Nhóm lệnh | Mục đích |
| --- | --- |
| `bixbott init` | Khởi tạo workspace hoặc cấu hình Bixbott cho dự án |
| `bixbott build` | Tạo ứng dụng/tính năng từ mô tả tự nhiên |
| `bixbott review` | Review code, diff hoặc pull request |
| `bixbott fix` | Đề xuất và áp dụng bản sửa lỗi |
| `bixbott scaffold` | Sinh cấu trúc dự án, module hoặc component |
| `bixbott deploy` | Chuẩn bị hoặc hỗ trợ deploy |
| `bixbott github` | Làm việc với GitHub repository, issue, PR và commit |
| `bixbott config` | Quản lý cấu hình cục bộ/toàn cục |
| `bixbott doctor` | Kiểm tra môi trường phát triển |
| `bixbott tasks` | Quản lý tác vụ, tag và phân công |
| `bixbott personnel` | Quản lý thành viên, agent và vai trò |
| `bixbott integrations` | Quản lý trang tích hợp dịch vụ ngoài |
| `bixbott skills` | Liệt kê, bật/tắt và cấu hình skill |
| `bixbott api-keys` | Tạo, xoay vòng và ủy quyền API key |

## `bixbott init`

Khởi tạo Bixbott trong một repository hiện có hoặc tạo workspace mới.

```bash
bixbott init
```

### Tùy chọn

| Flag | Mô tả |
| --- | --- |
| `--template <name>` | Chọn template khởi tạo |
| `--framework <name>` | Chỉ định framework chính, ví dụ `next`, `react`, `vue`, `node` |
| `--yes` | Chấp nhận giá trị mặc định |

### Ví dụ

```bash
bixbott init --framework next --yes
```

## `bixbott build`

Yêu cầu AI Agent tạo ứng dụng hoặc tính năng từ mô tả bằng ngôn ngữ tự nhiên.

```bash
bixbott build "Create a landing page for a SaaS product"
```

### Tùy chọn

| Flag | Mô tả |
| --- | --- |
| `--target <path>` | Thư mục hoặc package cần thay đổi |
| `--framework <name>` | Framework ưu tiên |
| `--plan-only` | Chỉ tạo kế hoạch, chưa sửa file |
| `--apply` | Áp dụng thay đổi được đề xuất |

### Ví dụ

```bash
bixbott build "Add authentication pages" --target apps/web --plan-only
```

## `bixbott review`

Review code hiện tại, diff local hoặc pull request.

```bash
bixbott review
```

### Tùy chọn

| Flag | Mô tả |
| --- | --- |
| `--staged` | Chỉ review các file đã stage |
| `--pr <number>` | Review một pull request trên GitHub |
| `--format <type>` | Định dạng output, ví dụ `markdown` hoặc `json` |

### Ví dụ

```bash
bixbott review --staged
```

```bash
bixbott review --pr 42 --format markdown
```

## `bixbott fix`

Đề xuất hoặc áp dụng bản sửa cho lỗi, test fail hoặc vấn đề do review phát hiện.

```bash
bixbott fix "Fix failing login tests"
```

### Tùy chọn

| Flag | Mô tả |
| --- | --- |
| `--from-review` | Dùng kết quả review gần nhất làm đầu vào |
| `--test <command>` | Lệnh test cần chạy sau khi sửa |
| `--apply` | Áp dụng patch sau khi tạo |

### Ví dụ

```bash
bixbott fix "Resolve TypeScript errors" --test "npm run typecheck" --apply
```

## `bixbott scaffold`

Sinh nhanh cấu trúc file/thư mục cho project, module, route, API hoặc component.

```bash
bixbott scaffold component Button
```

### Loại scaffold phổ biến

| Loại | Ví dụ |
| --- | --- |
| `app` | `bixbott scaffold app dashboard --framework next` |
| `component` | `bixbott scaffold component Button` |
| `api` | `bixbott scaffold api users` |
| `package` | `bixbott scaffold package ui` |
| `worker` | `bixbott scaffold worker webhook` |

## `bixbott deploy`

Hỗ trợ chuẩn bị deploy, kiểm tra cấu hình môi trường và tạo hướng dẫn triển khai.

```bash
bixbott deploy
```

### Tùy chọn

| Flag | Mô tả |
| --- | --- |
| `--provider <name>` | Nền tảng deploy, ví dụ `vercel`, `cloudflare`, `netlify` |
| `--check` | Chỉ kiểm tra khả năng deploy |
| `--preview` | Tạo bản preview nếu provider hỗ trợ |

### Ví dụ

```bash
bixbott deploy --provider cloudflare --check
```

## `bixbott github`

Nhóm lệnh làm việc với GitHub.

```bash
bixbott github <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott github issue list` | Liệt kê issue |
| `bixbott github issue plan <number>` | Lập kế hoạch xử lý issue |
| `bixbott github pr review <number>` | Review pull request |
| `bixbott github pr summarize <number>` | Tóm tắt pull request |
| `bixbott github commit suggest` | Gợi ý commit message |

### Ví dụ

```bash
bixbott github issue plan 12
```

```bash
bixbott github pr review 18
```

## `bixbott config`

Quản lý cấu hình Bixbott.

```bash
bixbott config <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott config get <key>` | Đọc một giá trị cấu hình |
| `bixbott config set <key> <value>` | Ghi một giá trị cấu hình |
| `bixbott config list` | Liệt kê cấu hình hiện tại |
| `bixbott config reset` | Đặt lại cấu hình về mặc định |

### Ví dụ

```bash
bixbott config set defaultFramework next
```


## `bixbott tasks`

Quản lý tác vụ và nhiều tag cho từng tác vụ.

```bash
bixbott tasks <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott tasks list --tag docs --tag area:api` | Lọc tác vụ theo nhiều tag |
| `bixbott tasks tag add <task-id> <tag>` | Thêm tag cho tác vụ |
| `bixbott tasks assign <task-id> --to <user-or-agent>` | Phân công tác vụ cho người hoặc agent |

## `bixbott personnel`

Quản lý tab nhân sự gồm thành viên, nhóm, agent và vai trò trong workspace.

```bash
bixbott personnel <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott personnel list` | Liệt kê thành viên và agent |
| `bixbott personnel invite <email> --role developer` | Mời thành viên mới |
| `bixbott personnel role set <id> <role>` | Cập nhật vai trò |
| `bixbott personnel agent enable <agent>` | Bật agent cho workspace |

## `bixbott integrations`

Quản lý các tích hợp như GitHub, deploy provider, observability và kênh thông báo.

```bash
bixbott integrations <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott integrations list` | Liệt kê tích hợp và trạng thái |
| `bixbott integrations connect github` | Kết nối GitHub |
| `bixbott integrations disconnect <name>` | Ngắt kết nối tích hợp |
| `bixbott integrations doctor <name>` | Kiểm tra lỗi cấu hình tích hợp |

## `bixbott skills`

Liệt kê, bật/tắt và cấu hình skill cho agent hoặc workspace.

```bash
bixbott skills <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott skills list` | Liệt kê skill có sẵn |
| `bixbott skills enable <name>` | Bật skill cho workspace |
| `bixbott skills disable <name>` | Tắt skill |
| `bixbott skills inspect <name>` | Xem metadata, quyền và tích hợp cần thiết |

## `bixbott api-keys`

Quản lý API key và ủy quyền khóa cho agent theo scope, skill và thời hạn.

```bash
bixbott api-keys <subcommand>
```

### Subcommands

| Lệnh | Mục đích |
| --- | --- |
| `bixbott api-keys create <provider> --scope <scope>` | Tạo API key theo provider/scope |
| `bixbott api-keys rotate <key-id>` | Xoay vòng API key |
| `bixbott api-keys revoke <key-id>` | Thu hồi API key |
| `bixbott api-keys delegate <key-id> --to <agent> --skill <skill>` | Ủy quyền key cho agent/skill |

## `bixbott doctor`

Kiểm tra môi trường phát triển và phát hiện thiếu sót phổ biến.

```bash
bixbott doctor
```

Các kiểm tra có thể bao gồm:

- Phiên bản Node.js.
- Trình quản lý package.
- Git status.
- Cấu hình GitHub CLI/token.
- File môi trường cần thiết.
- Cấu hình framework/deploy provider.

## Quy trình đề xuất cho người mới

1. Chạy `bixbott doctor` để kiểm tra môi trường.
2. Chạy `bixbott init` để cấu hình dự án.
3. Dùng `bixbott build --plan-only` để xem kế hoạch trước khi sửa code.
4. Dùng `bixbott review --staged` trước khi commit.
5. Dùng `bixbott github pr summarize` để chuẩn bị mô tả pull request.

## Gợi ý tách trang tiếp theo

Sau trang tổng quan này, nên tách dần thành các trang:

- `docs/commands/init.md`
- `docs/commands/build.md`
- `docs/commands/review.md`
- `docs/commands/fix.md`
- `docs/commands/scaffold.md`
- `docs/commands/deploy.md`
- `docs/commands/github.md`
- `docs/commands/config.md`
- `docs/commands/doctor.md`
- `docs/workspace-design.md`
- `docs/integrations-skills-api-keys.md`
