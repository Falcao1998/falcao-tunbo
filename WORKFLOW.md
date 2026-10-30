# TUNBO — Quy trình làm việc

Theme TUNBO cho store **tunbo.shop**, fork từ Shopify Horizon 4.3.0.
Quy ước kỹ thuật của theme: xem `AGENTS.md`. File này mô tả quy trình phát hành.

## Môi trường

| Thành phần | Giá trị |
|---|---|
| Repo | github.com/falcao1998/falcao-tunbo (private) |
| Nhánh `main` | = theme **live** trên store (Shopify GitHub integration, theme "falcao-tunbo/main") |
| Nhánh `dev` | thử nghiệm, kết nối thành theme unpublished "falcao-tunbo/dev" |
| Folder local | ~/Desktop/falcao-tunbo |
| Shopify CLI | `shopify theme dev` để xem trước, `shopify theme check` để lint |

Không dùng `shopify theme push --live`. Deploy = `git push` lên `main`.

## Nhịp làm việc hằng ngày

```bash
git checkout dev
git pull                         # lấy commit Theme Editor (nếu có) + thay đổi mới
shopify theme dev --store tunbo.shop     # sửa và xem trực tiếp
shopify theme check              # lint trước khi commit
git add -A
git commit -m "feat(home): thêm section lookbook"
git push                         # theme falcao-tunbo/dev tự cập nhật → preview trong Admin
```

## Phát hành một bản

```bash
git checkout main && git pull
git merge dev
# 1. sửa "theme_version" trong config/settings_schema.json (vd 1.1.0)
# 2. thêm mục vào release-notes.md
git commit -am "chore(release): v1.1.0"
git tag -a v1.1.0 -m "Mô tả ngắn"
git push && git push --tags
```

Store tự cập nhật trong vài giây. Kiểm tra lại trang chủ, trang sản phẩm, giỏ hàng trên desktop và mobile.

## Quay lui

| Tình huống | Cách làm |
|---|---|
| Sửa file chưa commit, muốn bỏ | `git restore <file>` hoặc `git restore .` |
| Hoàn tác một commit đã lên live | `git revert <commit>` rồi `git push` |
| Lấy lại một file từ bản cũ | `git checkout v1.0.0 -- sections/header.liquid` |
| Xem code tại một mốc | `git checkout v1.0.0` … `git checkout main` |
| Khẩn cấp | Admin → Themes → Publish lại theme backup |

## Quy ước

- Commit message: `feat:` `fix:` `style:` `chore:` `docs:` + phạm vi trong ngoặc, ví dụ `fix(cart): …`. Mỗi commit một mục đích.
- Version toàn bộ theme, kể cả `templates/*.json` và `config/settings_data.json`. `.gitignore` chỉ bỏ `.shopify/`, file hệ điều hành, file nén.
- Ảnh trong template phải nằm ở Admin → Content → Files; tham chiếu `shopify://shop_images/<ten-file>`.
- Chỉnh gì trong Theme Editor trên live thì Shopify tự commit vào `main`; trước khi làm việc local luôn `git pull`.
- Giữ nguyên ID section/block trong `templates/*.json` (cấu hình merchant phụ thuộc vào ID).
- Theme check: 2 lỗi `UnknownFilter` và vài warning là của Horizon gốc, bỏ qua.
- Mỗi lần publish bản mới: tăng `theme_version`, ghi `release-notes.md`, gắn tag.
