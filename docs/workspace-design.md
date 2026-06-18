# Thiết kế Workspace: Tag tác vụ và tab nhân sự

Tài liệu này mô tả thiết kế nền tảng cho trải nghiệm quản lý công việc trong Bixbott Workspace, tập trung vào **nhiều tag tác vụ** và **tab nhân sự** để điều phối agent, thành viên và quyền truy cập.

> Trạng thái: bản thiết kế sản phẩm. Các tên trường và endpoint có thể được tinh chỉnh khi triển khai UI/API chính thức.

## Mục tiêu

- Cho phép một tác vụ có nhiều tag để lọc, ưu tiên và phân tuyến cho agent phù hợp.
- Tách tab nhân sự thành nơi quản lý thành viên, vai trò, agent được gán và quyền API key.
- Chuẩn hóa metadata để đồng bộ giữa CLI, web app, GitHub issue/PR và automation.

## Tag tác vụ

Mỗi tác vụ có thể gắn nhiều tag theo nhóm ngữ nghĩa khác nhau.

| Nhóm tag | Ví dụ | Mục đích |
| --- | --- | --- |
| Loại việc | `feature`, `bug`, `docs`, `refactor`, `test` | Xác định bản chất công việc |
| Độ ưu tiên | `priority:high`, `priority:medium`, `priority:low` | Sắp xếp backlog và SLA |
| Khu vực | `area:web`, `area:api`, `area:agent`, `area:docs` | Lọc theo module hoặc package |
| Kỹ năng | `skill:frontend`, `skill:security`, `skill:devops` | Gợi ý agent/thành viên phù hợp |
| Trạng thái phụ | `blocked`, `needs-review`, `ready-to-ship` | Bổ sung trạng thái workflow |

### Quy tắc đề xuất

1. Một tác vụ nên có ít nhất một tag loại việc và một tag khu vực.
2. Tag ưu tiên chỉ nên có một giá trị tại một thời điểm.
3. Tag kỹ năng có thể có nhiều giá trị để hỗ trợ phân công cho nhiều agent.
4. Tag đồng bộ từ GitHub label nên giữ nguyên tên khi có thể để tránh mất ngữ cảnh.

### Mô hình dữ liệu gợi ý

```json
{
  "id": "task_123",
  "title": "Add API key delegation page",
  "tags": ["feature", "area:docs", "skill:security", "priority:high"],
  "assignees": ["user_001", "agent_docs"],
  "status": "in_progress"
}
```

## Tab nhân sự

Tab nhân sự tập trung vào các đối tượng có thể tham gia xử lý công việc: người dùng, nhóm và agent.

### Thành phần chính

| Khu vực | Nội dung |
| --- | --- |
| Danh sách thành viên | Tên, email, vai trò, trạng thái hoạt động |
| Nhóm/chuyên môn | Frontend, Backend, Docs, Security, DevOps |
| Agent được bật | Agent coding, review, docs, deploy |
| Quyền truy cập | Repository, workspace, môi trường, API key |
| Lịch sử phân công | Tác vụ đã nhận, PR liên quan, lần hoạt động gần nhất |

### Vai trò mặc định

| Vai trò | Quyền đề xuất |
| --- | --- |
| Owner | Quản lý workspace, billing, API key, thành viên |
| Maintainer | Quản lý repo, merge PR, cấu hình agent |
| Developer | Nhận/giao tác vụ, tạo branch, mở PR |
| Reviewer | Review code, yêu cầu thay đổi, phê duyệt |
| Viewer | Chỉ xem dashboard, docs và trạng thái tác vụ |

## Luồng phân công tác vụ

1. Người dùng tạo tác vụ hoặc đồng bộ từ GitHub issue.
2. Bixbott phân tích nội dung và đề xuất tag.
3. Workspace gợi ý người hoặc agent dựa trên tag kỹ năng/khu vực.
4. Maintainer xác nhận assignee trong tab nhân sự hoặc từ màn hình tác vụ.
5. Agent cập nhật trạng thái, PR và kết quả review về tác vụ.

## Checklist triển khai UI

- [ ] Bộ lọc multi-select cho tag tác vụ.
- [ ] Trang quản trị tag cho phép tạo, đổi màu, gộp và vô hiệu hóa tag.
- [ ] Tab nhân sự có bảng thành viên và agent.
- [ ] Drawer chi tiết nhân sự hiển thị quyền, tác vụ và API key được ủy quyền.
- [ ] Đồng bộ GitHub label hai chiều với tag tác vụ.
