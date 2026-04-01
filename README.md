# 📊 Presentation Project

HTML presentations với Reveal.js, dark theme, generate bằng Claude Code.

## Cách dùng

### Tạo presentation mới
```
"Tạo presentation về [chủ đề]"
"Tạo presentation về [chủ đề], [X] slides, audience: [client|internal|clevel|conference]"
```

### Tùy chỉnh nhanh
```
"Tạo presentation về [chủ đề], tác giả: [tên], ngày: [DD/MM/YYYY]"
"Tạo presentation về [chủ đề], thêm subtitle: [phụ đề]"
```

### Chỉnh sửa visual
```
"Đổi màu accent sang #ff6b35"
"Đổi font heading sang Poppins"
"Thêm layout 3 cột vào template"
"Chỉnh transition sang zoom"
```

## Cấu trúc project

```
├── CLAUDE.md                     ← Context cho Claude Code
├── config/
│   └── presentation.config.yaml ← Tham số mặc định (chỉnh tại đây)
├── templates/
│   └── dark-base.html            ← Base HTML template
├── themes/
│   └── dark.css                  ← CSS variables (màu, font, spacing)
├── skills/
│   └── SKILL.md                  ← Workflow guide cho Claude
└── output/                       ← Files được generate (gitignore?)
```

## Tham số hóa

Mọi thứ đều có thể override qua prompt. Giá trị mặc định ở `config/presentation.config.yaml`.

| Muốn thay đổi | Cách thay đổi |
|---|---|
| Số slide mặc định | `config.content.slide_count` |
| Tác giả mặc định | `config.presentation.author` |
| Transition | `config.transitions.slide` |
| Màu sắc / Font | `themes/dark.css` |
| Layout mới | `templates/dark-base.html` |
