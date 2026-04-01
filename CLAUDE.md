# Presentation Project — Claude Code Context

## Mục đích project
Tạo HTML presentations theo yêu cầu. Mỗi presentation là 1 file HTML duy nhất, tự chứa (inline CSS + JS), dùng Reveal.js CDN, dark theme mặc định.

## Quy trình làm việc (LUÔN theo thứ tự này)

1. **Đọc config** → `config/presentation.config.yaml`
2. **Đọc skill** → `skills/SKILL.md`
3. **Đọc base template** → `templates/dark-base.html`
4. **Đọc theme** → `themes/dark.css`
5. **Generate** → output vào `output/<topic-slug>.html`

## Khi nhận yêu cầu tạo presentation mới

Người dùng sẽ cung cấp (tối thiểu): **chủ đề** (topic).  
Các tham số còn lại lấy default từ `presentation.config.yaml`.

Nếu người dùng truyền thêm tham số, override config tương ứng.

**Ví dụ prompt:**
- `"Tạo presentation về AI trong giáo dục"` → dùng toàn bộ default
- `"Tạo presentation về blockchain, 12 slides, audience: C-level"` → override slide_count và audience

## Khi nhận yêu cầu chỉnh sửa visual / template

- Chỉnh sửa `themes/dark.css` nếu là màu sắc / font / spacing
- Chỉnh sửa `templates/dark-base.html` nếu là layout / structure
- **Không được** hardcode style vào file output — luôn dùng CSS variables từ theme

## Cấu trúc file output

- Tên file: `output/<topic-slug>_<YYYYMMDD>.html`
- Self-contained: tất cả CSS inline trong `<style>`, JS dùng CDN
- Reveal.js version: xem `config/presentation.config.yaml`

## Quy tắc nội dung slide

- Mỗi slide: tối đa **5–6 bullet points** hoặc **1 visual chính**
- Không nhồi text — ưu tiên ngắn gọn, súc tích
- Slide cuối: luôn có "Q&A / Thank you" hoặc tương đương
- Ngôn ngữ: theo `config.language` (default: tiếng Việt)

## Ghi chú quan trọng

- Sau khi generate xong, báo lại: tên file, số slide, theme dùng
- Nếu config có gì mâu thuẫn với yêu cầu, hỏi lại trước khi generate
- Khi người dùng nói "update template" → sửa file template/theme, KHÔNG tạo file mới
