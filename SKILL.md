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
| `slide_count` | Từ prompt hoặc brief | `config.content.slide_count` |
| `audience` | Từ prompt | `config.content.audience` |
| `tone` | Từ prompt | `config.content.tone` |
| `language` | Từ prompt | `config.presentation.language` |
| `date` | Từ prompt | Ngày hiện tại |
| `format` | Từ prompt hoặc brief | `config.output.format` |

**Quy tắc `slide_count`:**
- Có giá trị số (khác 0 / khác `""`) → generate đúng số slide đó
- Bằng `0`, `""`, hoặc không có → tự quyết dựa theo độ phức tạp và lượng nội dung

---

## BƯỚC 1B — Xác định Format

Sau khi parse tham số, xác định luồng generate:

| `format` | Luồng |
|---|---|
| `"slide"` | → Tiếp tục BƯỚC 2 → BƯỚC 5 (Reveal.js) |
| `"interactive"` | → Bỏ qua BƯỚC 2-4, chuyển sang **BƯỚC 2I** |

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

## BƯỚC 2I — Generate Interactive HTML (chỉ khi `format: "interactive"`)

Không dùng Reveal.js. Tạo một file HTML tự chứa với layout tương tác phù hợp nội dung.

**Chọn layout dựa theo nội dung:**
- Nhiều sections rõ ràng → **Tab navigation** (tabs ngang hoặc sidebar)
- Nội dung dạng checklist / quy trình → **Accordion / Stepper**
- So sánh nhiều phương án → **Card grid**
- Nội dung hỗn hợp → **Single-page scroll** với anchor nav

**Yêu cầu kỹ thuật:**
- CSS và JS viết inline trong `<style>` và `<script>`
- Không dùng CDN ngoài (self-contained hoàn toàn)
- Responsive (mobile-friendly)
- Dark theme, dùng color palette từ `themes/dark.css` làm tham chiếu (nhưng viết lại inline, không import file)
- Có smooth transition khi chuyển tab/section
- Tối ưu cho đọc trên màn hình, không phải trình chiếu

**Báo cáo sau khi generate:**
```
✅ Generated: output/ten-file.html
📄 Format: interactive (tabs / accordion / scroll — tuỳ loại đã chọn)
📑 Sections: X sections
🎨 Theme: dark (inline)
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
- [ ] Đã xác định `format` — slide hay interactive
- [ ] **Nếu slide:** Tất cả `{{PLACEHOLDER}}` đã được thay thế, CSS inline, Reveal.js khởi tạo đúng
- [ ] **Nếu interactive:** CSS + JS inline hoàn toàn, không dùng CDN ngoài, responsive
- [ ] Không còn comment `{{IF_...}}` nào trong file
- [ ] `slide_count` đúng — hoặc đã tự quyết nếu config để trống
- [ ] File tự mở được bằng browser (không cần server)
- [ ] Báo cáo đúng content mode (📄 hay 🤖) và format
