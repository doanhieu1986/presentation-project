# SKILL: HTML Presentation Generator

## Khi nào dùng skill này
Khi người dùng yêu cầu tạo, chỉnh sửa, hoặc cập nhật một HTML presentation.

---

## BƯỚC 0 — Xác định Content Mode

**Trước khi làm gì, kiểm tra xem có file content không:**

```
content/<topic-slug>.md  hoặc  content/<topic-slug>.txt
```

### MODE A — Có file content (Brief-driven)
→ Đọc file, dùng làm nguồn sự thật cho nội dung  
→ Không tự thêm ý ngoài file trừ khi cần expand bullet  
→ Ghi chú: `📄 Content source: content/<file>`

### MODE B — Không có file content (Auto-generate)
→ Claude tự generate dựa trên `topic`, `audience`, `tone`  
→ Dùng kiến thức chung, cấu trúc theo best practices  
→ Ghi chú: `🤖 Content: auto-generated`

**Quy tắc ưu tiên:** File content luôn thắng. Nếu có file → luôn dùng MODE A dù prompt không nhắc đến.

---

## BƯỚC 1 — Parse yêu cầu

Trích xuất các tham số từ prompt người dùng:

| Tham số | Nguồn | Fallback |
|---|---|---|
| `topic` | Từ prompt | Bắt buộc — hỏi lại nếu thiếu |
| `subtitle` | Từ prompt | `""` |
| `author` | Từ prompt | `""` |
| `slide_count` | Từ prompt | `config.content.slide_count` |
| `audience` | Từ prompt | `config.content.audience` |
| `tone` | Từ prompt | `config.content.tone` |
| `language` | Từ prompt | `config.presentation.language` |
| `date` | Từ prompt | Ngày hiện tại |

---

## BƯỚC 2 — Lên outline nội dung

Dựa vào `topic`, `slide_count`, `audience`, `tone`, tạo outline:

```
1. Cover
2. Agenda (nếu include_agenda = true)
3–N. Content slides
N+1. Q&A / Thank you (nếu include_qa = true)
```

**Quy tắc phân bổ slide theo audience:**
- `internal`: tập trung detail, OK dùng jargon kỹ thuật
- `client`: highlight benefit, hạn chế jargon
- `clevel`: executive summary, số liệu, big picture
- `conference`: engaging, storytelling, visual-heavy

**Quy tắc chọn layout:**
- Slide 1 bullet item → `cols-2` hoặc `cols-3`
- Slide có số liệu → dùng `.stat-number`
- Slide so sánh → `cols-2` với `.card`
- Slide key message → `.highlight-box`
- Slide code → `<pre><code>`

---

## BƯỚC 3 — Thay thế placeholders trong template

Mở `templates/dark-base.html` và thay toàn bộ `{{PLACEHOLDER}}`:

| Placeholder | Giá trị |
|---|---|
| `{{LANGUAGE}}` | `vi` / `en` |
| `{{TOPIC}}` | Tiêu đề chính |
| `{{SUBTITLE}}` | Phụ đề |
| `{{SUBTITLE_BRACKET}}` | `— Subtitle` nếu có, `""` nếu không |
| `{{EYEBROW}}` | Loại deck, VD: "Báo cáo Q3 2025" |
| `{{AUTHOR}}` | Tên tác giả |
| `{{DATE}}` | Ngày định dạng DD/MM/YYYY |
| `{{REVEALJS_VERSION}}` | Từ `config.framework.version` |
| `{{TRANSITION_SLIDE}}` | Từ `config.transitions.slide` |
| `{{TRANSITION_SPEED}}` | Từ `config.transitions.speed` |
| `{{INJECT_THEME_CSS}}` | Nội dung file `themes/dark.css` (inline toàn bộ) |
| `{{AGENDA_ITEMS}}` | `<li>` cho từng section |
| `{{CONTENT_SLIDES}}` | HTML của tất cả content slides |
| `{{QA_LABEL}}` | "Câu hỏi & Thảo luận" hoặc "Q&A" |
| `{{CONTACT_INFO}}` | Email/info nếu có |

Xoá toàn bộ comment `<!-- {{IF_...}} -->` sau khi xử lý điều kiện.

---

## BƯỚC 4 — Generate từng content slide

### Template slide thông thường (bullet list):
```html
<section>
  <h2>Tiêu đề Slide</h2>
  <ul>
    <li class="fragment">Ý chính 1</li>
    <li class="fragment">Ý chính 2</li>
    <li class="fragment">Ý chính 3</li>
  </ul>
</section>
```

### Template slide 2 cột:
```html
<section>
  <h2>Tiêu đề Slide</h2>
  <div class="cols-2">
    <div class="card card-accent">
      <h3>Cột trái</h3>
      <ul>
        <li>Item A</li>
        <li>Item B</li>
      </ul>
    </div>
    <div class="card card-accent">
      <h3>Cột phải</h3>
      <ul>
        <li>Item X</li>
        <li>Item Y</li>
      </ul>
    </div>
  </div>
</section>
```

### Template slide số liệu (stats):
```html
<section>
  <h2>Tiêu đề Slide</h2>
  <div class="cols-3">
    <div class="card" style="text-align:center">
      <div class="stat-number">85%</div>
      <div class="stat-label">Mô tả số liệu</div>
    </div>
    <!-- thêm cards -->
  </div>
</section>
```

### Template section divider:
```html
<section class="slide-section">
  <h2>Tên Section</h2>
  <p>Mô tả ngắn (tùy chọn)</p>
</section>
```

### Template highlight message:
```html
<section>
  <h2>Tiêu đề</h2>
  <div class="highlight-box">
    <p><strong>Key message nổi bật ở đây.</strong></p>
  </div>
  <ul>
    <li class="fragment">Supporting point 1</li>
    <li class="fragment">Supporting point 2</li>
  </ul>
</section>
```

---

## BƯỚC 5 — Output

1. Tạo file tại `output/<topic-slug>_<YYYYMMDD>.html`
2. `topic-slug`: lowercase, replace space bằng `-`, bỏ dấu tiếng Việt
3. Báo cáo lại:
   ```
   ✅ Generated: output/ten-file.html
   📊 Slides: X slides (1 cover + Y content + 1 close)
   🎨 Theme: dark
   🔄 Transition: slide / default
   ```

---

## Khi người dùng yêu cầu chỉnh sửa visual

**"Đổi màu accent"** → Sửa `--color-accent-primary` trong `themes/dark.css`  
**"Đổi font"** → Sửa `--font-heading` / `--font-body` trong `themes/dark.css`  
**"Layout slide khác"** → Thêm CSS class mới vào `templates/dark-base.html`  
**"Chỉnh transition"** → Sửa `transitions` trong `config/presentation.config.yaml`  

Sau khi sửa template/theme: hỏi người dùng có muốn regenerate file output hiện tại không.

---

## Checklist trước khi output

- [ ] Đã kiểm tra `content/` — xác định MODE A hay MODE B
- [ ] Tất cả `{{PLACEHOLDER}}` đã được thay thế
- [ ] CSS từ `themes/dark.css` đã được inline
- [ ] Không còn comment `{{IF_...}}` nào trong file
- [ ] Slide count đúng với config (hoặc brief nếu MODE A)
- [ ] File tự mở được bằng browser (không cần server)
- [ ] Reveal.js khởi tạo đúng với plugins
- [ ] Báo cáo đúng content mode (📄 hay 🤖)
