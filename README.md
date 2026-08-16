# profile-content

Dữ liệu cho site Astro (`profile-web`). Sửa gì thì sửa ở đây rồi push `main`.

## Cấu trúc

```
data/       JSON theo schema (profile, about, experience, education, projects, skills, contact, social)
posts/      Markdown. Tên file = YYYY-MM-DD-slug.md
images/     avatar, og-cover, projects/<id>/, posts/<slug>/
files/      PDF và tài liệu tải về
```

## Bài viết

- `slug` trong frontmatter phải khớp phần cuối tên file.
- `status: draft | published` — chỉ dùng 1 field này.
- `status: draft` bị ẩn khỏi danh sách và RSS lúc build production.

## Ảnh

- Cover ~1600px rộng, ảnh trong bài ~800–1000px.
- Ưu tiên `.webp`, fallback `.jpg`.
- Không commit ảnh > 1–2MB.

## Workflow

1. Sửa JSON hoặc thêm `posts/YYYY-MM-DD-slug.md`.
2. Push `main`.
3. `notify-web.yml` bắn `repository_dispatch` sang `profile-web` (cần secret `PAT_TOKEN`).
