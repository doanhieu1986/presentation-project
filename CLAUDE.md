# Presentation Project — Claude Code Context

## Mục đích project
Tạo HTML presentations theo yêu cầu. Mỗi presentation là 1 file HTML duy nhất, tự chứa (inline CSS + JS), dark theme mặc định. Hỗ trợ 2 định dạng: **slide** (Reveal.js) và **interactive** (HTML tương tác như website).

## Quy trình làm việc (LUÔN theo thứ tự này)

1. **Đọc config** → `presentation.config.yaml`
2. **Đọc skill** → `SKILL.md`
3. **Đọc theme** → `dark.css`
4. **Nếu `format: slide`** → đọc base template `dark-base.html`
5. **Generate** → output vào `output/<topic-slug>.html`

## Khi nhận yêu cầu tạo presentation mới

Người dùng sẽ cung cấp (tối thiểu): **chủ đề** (topic).  
Các tham số còn lại lấy default từ `presentation.config.yaml`.

**Bước đầu tiên — kiểm tra content mode:**
1. Tìm file `content/<topic-slug>.md` hoặc `content/<topic-slug>.txt`
2. Nếu có → **MODE A**: dùng file làm nguồn nội dung
3. Nếu không có → **MODE B**: tự generate từ topic + config

**Quy tắc số slide:**
- `slide_count` có giá trị số → generate đúng số slide đó
- `slide_count` để trống (`""`) → tự quyết dựa theo độ phức tạp và lượng nội dung

Nếu người dùng truyền thêm tham số, override config tương ứng.

**Ví dụ prompt:**
- `"Tạo presentation về AI trong giáo dục"` → dùng toàn bộ default
- `"Tạo presentation về blockchain, 12 slides, audience: C-level"` → override slide_count và audience
- `"Tạo presentation về DevOps, format interactive"` → override format

## Khi nhận yêu cầu chỉnh sửa visual / template

- Chỉnh sửa `dark.css` nếu là màu sắc / font / spacing
- Chỉnh sửa `dark-base.html` nếu là layout / structure (chỉ ảnh hưởng format `slide`)
- **Không được** hardcode style vào file output — luôn dùng CSS variables từ theme

## Cấu trúc file output

- Tên file: `output/<topic-slug>_<YYYYMMDD>.html`
- Self-contained: tất cả CSS inline trong `<style>`, JS dùng CDN hoặc inline

**Hai định dạng output (`output.format`):**

- `"slide"` → Reveal.js slideshow (mặc định). Dùng framework + theme từ config, đọc template `templates/dark-base.html`
- `"interactive"` → HTML tùy biến, tương tác như website: có thể có tabs, sections, sidebar, accordion, v.v. Không dùng Reveal.js. Tự thiết kế layout phù hợp với nội dung, ưu tiên trải nghiệm đọc/khám phá thay vì trình chiếu. CSS và JS viết inline trong file.

## Quy tắc nội dung slide

- Mỗi slide: tối đa **5–6 bullet points** hoặc **1 visual chính**
- Không nhồi text — ưu tiên ngắn gọn, súc tích
- Slide cuối: luôn có "Q&A / Thank you" hoặc tương đương
- Ngôn ngữ: theo `config.language` (default: tiếng Việt)

## Ghi chú quan trọng

- Sau khi generate xong, báo lại: tên file, số slide, theme dùng
- Nếu config có gì mâu thuẫn với yêu cầu, hỏi lại trước khi generate
- Khi người dùng nói "update template" → sửa file template/theme, KHÔNG tạo file mới
