# Presentation Project

HTML presentations, generate bằng Claude Code. Hỗ trợ 2 định dạng output: **slide** (Reveal.js) và **interactive** (HTML tương tác như website).

## Cách dùng

### Tạo presentation mới — prompt nhanh
```
"Tạo presentation về [chủ đề]"
"Tạo presentation về [chủ đề], [X] slides, audience: [client|internal|clevel|conference]"
"Tạo presentation về [chủ đề], format: interactive"
```

### Dùng brief file (khuyến nghị cho nội dung phức tạp)
1. Sao chép `_template.md` → đặt vào `content/<topic-slug>.md`
2. Điền nội dung, meta, ghi chú cho Claude
3. Prompt: `"Tạo presentation về [chủ đề]"` — Claude tự tìm file

Xem `_example.md` để biết cách viết brief.

### Chỉnh sửa visual
```
"Đổi màu accent sang #da35ffff"
"Đổi font heading sang Poppins"
"Thêm layout 3 cột vào template"
"Chỉnh transition sang zoom"
```

## Hai định dạng output

| Format | Mô tả | Khi nào dùng |
|---|---|---|
| `slide` | Reveal.js slideshow, dark theme | Trình chiếu, thuyết trình |
| `interactive` | HTML tương tác (tabs, accordion...), self-contained | Tài liệu đọc, gửi qua email/link |

Đặt mặc định tại `config.output.format`, hoặc override qua prompt / brief file.

## Số slide

| `slide_count` | Kết quả |
|---|---|
| Số cụ thể (VD: `10`) | Generate đúng số slide đó |
| Để trống (`""`) hoặc `0` | Claude tự quyết dựa theo độ phức tạp nội dung |

## Cấu trúc project

```
├── CLAUDE.md                      ← Context cho Claude Code
├── README.md                      ← File này
├── _template.md                   ← Mẫu brief file — sao chép để dùng
├── _example.md                    ← Ví dụ brief file đã điền
├── presentation.config.yaml       ← Tham số mặc định (chỉnh tại đây)
├── content/                       ← Brief files theo từng topic
│   └── <topic-slug>.md
├── templates/
│   └── dark-base.html             ← Base HTML template (chỉ dùng cho format: slide)
├── themes/
│   └── dark.css                   ← CSS variables (màu, font, spacing)
├── skills/
│   └── SKILL.md                   ← Workflow guide cho Claude
└── output/                        ← Files được generate
```

## Tham số mặc định

Mọi thứ đều có thể override qua prompt hoặc brief file. Giá trị mặc định ở `presentation.config.yaml`.

| Muốn thay đổi | Chỉnh ở đâu |
|---|---|
| Số slide mặc định | `content.slide_count` |
| Định dạng output mặc định | `output.format` |
| Tác giả mặc định | `presentation.author` |
| Transition | `transitions.slide` |
| Màu sắc / Font | `themes/dark.css` |
| Layout slide mới | `templates/dark-base.html` |
