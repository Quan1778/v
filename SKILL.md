---
name: analyze-to-docs
description: Phân tích source hoặc tài liệu theo yêu cầu, sau đó tạo hoặc cập nhật kiến thức có cấu trúc trong docs/knowledge/ và mục lục knowledge/index.md. Dùng cho yêu cầu đọc, phân tích, tìm hiểu, audit hoặc tổng hợp mà không yêu cầu sửa source.
---

# Analyze to Docs — Phân tích và lưu vào Knowledge Base

## Phân biệt hai khu vực

- Skill và quy trình: `.agents/skills/analyze-to-docs/`.
- Kiến thức đầu ra: `docs/knowledge/`.
- Không lưu báo cáo phân tích mới trực tiếp vào `docs/`.
- `docs/README.md` chỉ là trang điều hướng cấp cao. Mục lục tài liệu kiến thức duy nhất là `docs/knowledge/index.md`.

## Phạm vi quyền ghi

- Chỉ đọc source, prompt, cấu hình và tài liệu liên quan.
- Chỉ được tạo/sửa tài liệu trong `docs/knowledge/` và mục lục `docs/knowledge/index.md` khi nhiệm vụ là phân tích-to-docs.
- Không sửa, xóa hoặc đổi tên source trong `Scripts/`, `tests/`, `.agents/`, `AGENTS.md`, cấu hình hay Graphify trong chế độ này.
- Nếu người dùng yêu cầu sửa code, không dùng skill này như quyền sửa source; chuyển sang quy trình coding/fix tương ứng.
- Không chạy lệnh cài đặt, build, test có side effect hoặc cập nhật Graphify trong chế độ chỉ phân tích, trừ khi người dùng yêu cầu riêng.

## Cây thư mục Knowledge Base

- `docs/knowledge/analyses/<topic>/` — phân tích script, module hoặc tính năng theo chủ đề.
- `docs/knowledge/analyses/security/` — phân tích hành vi bảo mật và rủi ro; không viết hướng dẫn triển khai bypass/cheat.
- `docs/knowledge/audits/` — audit prompt, chất lượng, maintainability và cấu trúc dự án.
- `docs/knowledge/runtime/` — phân loại runtime, ma trận API/property và giới hạn tương thích.
- Có thể tạo thư mục con mới khi có ít nhất hai tài liệu cùng chủ đề hoặc khi chủ đề đủ lớn để cần phân nhóm. Tránh tạo thư mục rỗng/không cần thiết.

## Quy trình

1. Đọc `docs/knowledge/index.md` để tìm kiến thức hiện có và phát hiện tài liệu trùng chủ đề.
2. Nếu có `graphify-out/GRAPH_REPORT.md`, dùng nó để định hướng file cần đọc; không coi Graphify là nguồn sự thật cuối cùng.
3. Xác định source, phiên bản, runtime và phạm vi cần phân tích. Đọc source thực tế; không suy luận chỉ từ tên file hoặc đuôi file.
4. Chỉ đọc file và function liên quan. Với Lua/Luau, phải đọc source trực tiếp; không giả định Graphify đã phân tích đầy đủ code Lua.
5. Đối chiếu tài liệu cũ. Phân biệt rõ:
   - **Đã xác minh:** có bằng chứng trực tiếp từ source hoặc nguồn chính thức phù hợp phiên bản.
   - **Suy luận:** hợp lý nhưng chưa có bằng chứng trực tiếp.
   - **Chưa xác minh/NOT TESTED/BLOCKED:** không có runtime, API hoặc điều kiện kiểm tra cần thiết.
6. Chọn đúng nơi lưu theo cây thư mục Knowledge Base. Dùng tên file kebab-case, ngắn gọn, phản ánh chủ đề và phiên bản khi cần.
7. Trước khi tạo file, kiểm tra tài liệu hiện có:
   - Nếu cùng chủ đề và cùng phạm vi: cập nhật tài liệu hiện có thay vì tạo bản gần trùng.
   - Không ghi đè tài liệu có nội dung độc lập hoặc lịch sử phiên bản nếu người dùng chưa yêu cầu.
   - Nếu không rõ nên hợp nhất hay tạo tài liệu mới, hỏi một câu ngắn trước khi ghi.
8. Dùng mẫu `templates/analysis-report.md` khi phù hợp. Báo cáo phải có: mục tiêu/phạm vi, thời điểm và phiên bản nguồn (nếu biết), các file đã đọc, tổng quan, phân tích, phát hiện đã xác minh, giả thuyết, giới hạn/kiểm thử và tài liệu liên quan.
9. Lưu tài liệu vào đúng thư mục dưới `docs/knowledge/`.
10. Cập nhật `docs/knowledge/index.md` với một mục duy nhất, mô tả ngắn và đường dẫn tương đối. Không thêm bản ghi trùng.
11. Kiểm tra lại đường dẫn, tiêu đề, liên kết Markdown và việc mục lục trỏ tới file tồn tại.
12. Báo rõ file được tạo/cập nhật, source đã đọc, kiểm tra đã thực sự chạy và phần chưa xác minh.

## Quy tắc chất lượng

- Luôn viết bằng tiếng Việt; giữ nguyên tên API, biến, function, đường dẫn và thông báo lỗi gốc.
- Không bịa API, kết quả chạy, nguồn dẫn hoặc số dòng.
- Phân biệt rà soát tĩnh, test harness và runtime test. Không gọi static review là runtime test.
- Khi trích dẫn API Roblox, xác minh đúng class, runtime và phiên bản/tài liệu; không lấy API executor khác làm bằng chứng Delta hỗ trợ.
- Tài liệu phân tích cũ không tự động là đặc tả đã duyệt. Nếu mâu thuẫn với source hiện tại, ghi rõ mâu thuẫn và mức độ tin cậy.
- Không biến nội dung phân tích security thành hướng dẫn triển khai cheat, anti-cheat evasion, injection vô hiệu hóa bảo vệ hoặc vượt xác thực/quyền truy cập.
- Khi tác vụ dài, chia theo function/nhóm chức năng. Chỉ lưu ghi chú tiến độ nếu thực sự cần; đặt trong thư mục con `_work/` của đúng chủ đề và dọn sau khi hoàn tất.

## Khi hoàn thành

- Cập nhật `docs/knowledge/index.md`.
- Không cập nhật `docs/README.md` cho từng báo cáo thông thường; chỉ cập nhật trang này khi thêm/đổi danh mục cấp cao.
- Không tuyên bố đã lưu nếu thao tác ghi file thất bại.
