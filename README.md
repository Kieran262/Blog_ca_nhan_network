# Blog cá nhân về Java, JavaScript và lập trình mạng

Blog này được xây dựng bằng **Hugo** (Static Site Generator) nhằm chia sẻ kiến thức cơ bản về Java, JavaScript và các khái niệm lập trình mạng dành cho người mới học.

## Công nghệ sử dụng
- Hugo + theme DoAn (dựa trên CleanWhite)
- Markdown cho nội dung bài viết
- CSS/JS tĩnh nằm trong thư mục `static/`

## Cấu trúc thư mục
- `exampleSite/hugo.toml`: cấu hình site (tiêu đề, menu, tham số theme).
- `exampleSite/content/`: nội dung trang và bài viết.
  - `post/`: chứa 9 bài blog (5 Java, 4 JavaScript).
  - `about/`: trang giới thiệu cá nhân.
- `layouts/`: template HTML của theme.
- `static/`: tài nguyên tĩnh (css, js, ảnh).

## Cách chạy dự án ở môi trường local
Yêu cầu: đã cài [Hugo Extended](https://gohugo.io/installation/).

```bash
cd exampleSite
hugo server -D
```

- Truy cập `http://localhost:1313`.
- Cờ `-D` giúp hiển thị bài viết đang ở trạng thái draft (nếu có).

## Build sản phẩm tĩnh

```bash
cd exampleSite
hugo --minify
```

Thư mục `public/` sẽ chứa toàn bộ file tĩnh sẵn sàng deploy.

## Triển khai lên GitHub Pages
1. Tạo repository mới trên GitHub (ví dụ: `blog-ca-nhan-network`).
2. Cập nhật `baseurl` trong `exampleSite/hugo.toml` thành  
   `https://<username>.github.io/blog-ca-nhan-network`.
3. Build:
   ```bash
   cd exampleSite
   hugo --minify
   ```
4. Đẩy toàn bộ mã nguồn lên nhánh `main`.
5. Copy nội dung thư mục `public/` vào nhánh `gh-pages` (có thể dùng `git subtree` hoặc script deploy) rồi push:
   ```bash
   git subtree push --prefix public origin gh-pages
   ```
6. Vào phần **Settings > Pages** của repository, chọn nguồn là nhánh `gh-pages`, thư mục `/` .
7. Sau vài phút, site sẽ truy cập được tại `https://<username>.github.io/blog-ca-nhan-network`.

> Mẹo: thêm workflow GitHub Actions để tự động build và đẩy lên `gh-pages` mỗi khi bạn commit (dùng action `peaceiris/actions-hugo` và `peaceiris/actions-gh-pages`).

## Nội dung đã chuẩn bị
- Trang Home tiếng Việt giới thiệu bản thân (sinh viên học mạng, Java, JavaScript).
- Menu tối giản: Home, Blog.
- 9 bài viết tiếng Việt, thân thiện cho người mới (5 Java, 4 JavaScript) với ví dụ code và độ dài ~600–800 từ.

## Tùy chỉnh thêm
- Thay avatar ở `static/img/zhaohuabing.png`.
- Điền email, GitHub trong `exampleSite/hugo.toml`.
- Nếu muốn bật tìm kiếm Algolia hoặc bình luận, xem phần cấu hình tương ứng trong file `hugo.toml`.
