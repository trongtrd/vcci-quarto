# quarto-vcci

Extension Quarto cho tài liệu của Liên đoàn Thương mại và Công nghiệp Việt Nam (VCCI). Có ba định dạng: báo cáo tư vấn (`vcci-report`), working paper (`vcci-wp`) và tờ trình (`vcci-memo`). Mỗi định dạng xuất PDF bằng Typst (bản chính, khổ A4) và Word (để đồng nghiệp sửa), theo Bộ nhận diện thương hiệu VCCI. Kèm theo là theme biểu đồ cho Python (matplotlib), R (ggplot2) và Stata.

*English: Quarto formats for VCCI (Vietnam Chamber of Commerce and Industry) consulting reports, working papers and memos, rendered to PDF (Typst, A4) and Word, with chart themes for matplotlib, ggplot2 and Stata. Documentation is in Vietnamese; every format also supports `lang: en`.*

Phiên bản 1.0.0. Cần Quarto ≥ 1.6 (đã có sẵn Typst). Font Lora, Be Vietnam Pro và JetBrains Mono đi kèm extension, nên không cần cài font.

## Cài đặt

Tạo dự án mới từ mẫu, gồm extension, tài liệu mẫu và theme biểu đồ:

```bash
quarto use template trongtrd/quarto-vcci
```

Lệnh trên tạo các file sau:

| File | Nội dung |
|---|---|
| `<tên-thư-mục>.qmd` | Báo cáo tư vấn mẫu (`vcci-report`) |
| `wp.qmd`, `references.bib` | Working paper mẫu (`vcci-wp`) |
| `memo.qmd` | Tờ trình mẫu (`vcci-memo`) |
| `charts.qmd` | Báo cáo có biểu đồ ggplot2 (cần R) |
| `charts/` | Theme biểu đồ cho matplotlib, ggplot2, Stata |
| `_extensions/trongtrd/` | Extension (ba định dạng) |

Chỉ thêm extension vào một dự án đã có:

```bash
quarto add trongtrd/quarto-vcci
```

Ghim một phiên bản: `quarto use template trongtrd/quarto-vcci@v1.0.0`. Cập nhật extension: chạy lại `quarto add trongtrd/quarto-vcci`.

Dựng tài liệu:

```bash
quarto render ten-tai-lieu.qmd
```

Mỗi mẫu khai báo cả hai đầu ra (`vcci-report-typst` cho PDF, `vcci-report-docx` cho Word). Chỉ dựng một đầu ra: `quarto render ten-tai-lieu.qmd --to vcci-report-typst`.

## Dùng `vcci-report`

Bắt đầu từ `<tên-thư-mục>.qmd` do `quarto use template` tạo ra (trong repo này là `template.qmd`).

Bản mẫu đã dựng: [`examples/report.pdf`](examples/report.pdf) và [`report.docx`](https://github.com/trongtrd/quarto-vcci/releases/download/v1.0.0/report.docx).

### Thông tin trong YAML

| Khóa | Ý nghĩa | Hiện ở |
|---|---|---|
| `title`, `subtitle` | Tiêu đề, phụ đề | Bìa, trang thông tin |
| `report-label` | Nhãn trên tiêu đề, mặc định "Báo cáo" (ví dụ "Báo cáo tư vấn") | Bìa |
| `short-title` | Tên rút gọn | Đầu trang |
| `client` | Bên đặt hàng | Bìa, trang thông tin |
| `author` (kèm `affiliations`), `team` | Tác giả; `team` thay danh sách nhóm thực hiện nếu cần | Bìa, trang thông tin |
| `date`, `version` | Ngày ("10 tháng 10 năm 2026"), phiên bản | Bìa, trang thông tin |
| `status` | Ví dụ "Bản thảo", "Mật": nhãn trên bìa và dấu chìm mọi trang. Bỏ khi phát hành | Bìa, mọi trang |
| `hero` | Hero icon trên bìa, số 1–24 (xem `brand-assets/05_Icons`) | Bìa |
| `citation-text` | Gợi ý trích dẫn (khóa `citation` đã được Quarto dùng) | Trang thông tin |
| `disclaimer` | Lưu ý, miễn trừ | Trang thông tin |
| `contact` | Thay địa chỉ liên hệ mặc định của VCCI | Bìa sau |
| `cover: false`, `back-cover: false` | Bỏ bìa trước (cùng trang thông tin) hoặc bìa sau | |
| `toc: true` | Mục lục | Sau trang thông tin |
| `bibliography` | Tài liệu tham khảo; trích dẫn kiểu Chicago chú thích cuối trang | Chú thích, cuối tài liệu |

### Khối nội dung VCCI

```markdown
::: lead
Hai, ba câu nêu thông điệp chính (Lora, màu xanh).
:::

::: figures
64 ngày | Thời gian hoàn tất thủ tục
38% | Doanh nghiệp đi lại từ ba lần trở lên
:::

::: {.highlight title="Ba khuyến nghị ưu tiên"}
Hộp nhấn mạnh nền Blue Tint, gạch xanh bên trái.
:::

::: label
Nhãn chữ in hoa nhỏ
:::
```

Hộp `callout-note`, `-tip`, `-warning`, `-important`, `-caution` của Quarto dùng màu trạng thái VCCI (thông tin, thành công, cảnh báo, lỗi). Mỗi chương (`#`) bắt đầu trang mới với nhãn "CHƯƠNG 01" và gạch vàng; `{.unnumbered}` cho chương không đánh số (Tóm tắt, Phụ lục).

### Lưu ý

- **Bảng rộng:** Pandoc chia độ rộng cột theo số dấu `-` ở dòng phân cách khi bảng có dòng dài. Cột có chữ dài thì cho nhiều dấu `-` hơn, ví dụ `|-----------|--:|--:|`.
- **Trích dẫn:** báo cáo và tờ trình dùng Chicago chú thích cuối trang (lần đầu đầy đủ, các lần sau rút gọn); luôn viết trong ngoặc vuông, `[@khoa]` hay `[@khoa, tr. 12]`, vì `@khoa` đứng trần sẽ in cả nguồn giữa câu. Working paper dùng APA (tác giả–năm), nơi `@khoa` cho "Tác giả (năm)" trong câu. Kiểu Chicago là bản tùy biến `vcci-chicago-notes.csl`: không đặt dấu phẩy trước "và" khi có hai tác giả. Với `@techreport`, thêm `type = {Báo cáo kỹ thuật}` để không hiện "technical report". Trong file `.bib`, viết tên người Việt trong ngoặc nhọn, `author = {{Nguyễn Thị Hạnh} and {Trần Đức Long}}`, để tên không bị đảo thành "Thị Hạnh Nguyễn" hay viết tắt thành "Nguyễn, T. H.".
- **Tài liệu tham khảo** được Quarto đặt ở cuối tài liệu (sau phụ lục) với tiêu đề "Tài liệu tham khảo"; không tự thêm mục này.
- **Bản Word:**
  - Không có bìa màu như PDF; trang đầu là tiêu đề, tác giả, ngày.
  - Mục lục cần cập nhật một lần: nhấp vào mục lục rồi nhấn F9.
  - Danh sách đặt trong `::: highlight` nằm ngoài hộp, vì Pandoc chỉ áp style cho đoạn văn, không cho mục danh sách.
- **Phương trình** dùng font toán của Typst (PDF) hoặc Cambria Math (Word), vì Lora và Be Vietnam Pro không có ký hiệu toán.
- **Mã nguồn** (`` `...` `` và khối ```` ``` ````) dùng JetBrains Mono, được đóng gói kèm extension và nhúng vào bản Word. Tô cú pháp theo màu VCCI (theme `vcci.theme`, sinh từ `brand-tokens.json`), nền Ivory; ligature tắt nên `<-` của R vẫn là hai ký tự. Trong khối mã không có chữ nghiêng, vì chỉ đóng gói kiểu Regular và Bold.

## Dùng `vcci-wp` (working paper)

Bắt đầu từ `wp.qmd` (kèm `references.bib`). Bản mẫu: [`examples/wp.pdf`](examples/wp.pdf), [`wp.docx`](https://github.com/trongtrd/quarto-vcci/releases/download/v1.0.0/wp.docx), bản tiếng Anh [`examples/wp-en.pdf`](examples/wp-en.pdf).

Trang đầu gồm: tên bộ và số hiệu, tiêu đề, tác giả (đơn vị, email), ngày, tóm tắt kèm từ khóa (một hoặc hai ngôn ngữ), mã JEL. Chân trang đầu có trích dẫn đề nghị, tác giả liên hệ, lời miễn trừ. Thân bài đánh số mục 1, 1.1; phương trình đánh số; tài liệu tham khảo kiểu APA ở cuối.

| Khóa | Ý nghĩa |
|---|---|
| `wp-number` | Số hiệu, ví dụ `"2026/01"`: hiện "VCCI Working Paper số 2026/01" (tiếng Anh: "No. 2026/01") |
| `wp-series` | Tên bộ, mặc định "VCCI Working Paper" |
| `lang` | `vi` (mặc định) hoặc `en`. Bản tiếng Anh nên thêm `date-format: "D MMMM YYYY"` |
| `author` | `name`, `email`, `affiliations`; `attributes: {corresponding: true}` đánh dấu \* tác giả liên hệ |
| `abstract`, `keywords` | Tóm tắt và từ khóa theo ngôn ngữ của bài |
| `abstract-en`, `keywords-en` | Tóm tắt tiếng Anh cho bài tiếng Việt (bài tiếng Anh dùng `abstract-vi`, `keywords-vi`) |
| `jel` | Mã JEL, ví dụ `[D22, K23]` |
| `thanks` | Lời cảm ơn: chú thích † ở tiêu đề |
| `citation-text` | Thay gợi ý trích dẫn tự sinh ("Tác giả (năm). Tiêu đề. VCCI Working Paper số …. Hà Nội: VCCI.") |
| `disclaimer` | Thay lời miễn trừ mặc định |
| `status`, `short-title`, `version`, `toc` | Như `vcci-report` |

Trong bản Word, tên bộ và số hiệu nằm trên dòng ngày; từ khóa, tóm tắt thứ hai, mã JEL, trích dẫn và lời miễn trừ nằm ở đầu thân bài.

## Dùng `vcci-memo` (tờ trình)

Bắt đầu từ `memo.qmd`. Bản mẫu: [`examples/memo.pdf`](examples/memo.pdf), [`memo.docx`](https://github.com/trongtrd/quarto-vcci/releases/download/v1.0.0/memo.docx), bản tiếng Anh hai trang [`examples/memo-en.pdf`](examples/memo-en.pdf).

| Khóa | Ý nghĩa |
|---|---|
| `title` | Nội dung "Về việc" |
| `recipient`, `sender`, `copy-to` | Gửi, Từ, Đồng gửi (`to` và `from` là khóa riêng của Pandoc nên không dùng được); không có `sender` thì lấy `author` |
| `date` | Ngày |
| `priority` | Ví dụ "Khẩn": nhãn cạnh chữ "TỜ TRÌNH" |
| `status` | Ví dụ "Nội bộ", "Mật": dấu chìm mọi trang |
| `signature` | `name` và `title` (chức vụ), đặt bên phải cuối memo |
| `attachments` | Danh sách tài liệu kèm theo |
| `memo-label` | Thay chữ "Tờ trình" (tiếng Anh: "Memo") |
| `lang: en` | Memo tiếng Anh ("MEMO", To, From, Subject…); thêm `date-format: "D MMMM YYYY"` |

Tiêu đề mục (`#`) của memo nhỏ hơn chương của báo cáo và không đánh số. Từ trang 2, đầu trang nhắc lại "Về việc".

## Biểu đồ: Python, R, Stata

Thư mục [`charts/`](charts/) có theme biểu đồ theo phần 08 của hướng dẫn thương hiệu: `vcci_charts.py` và `vcci.mplstyle` (matplotlib), `theme_vcci.R` (ggplot2), `scheme-vcci.scheme` và `vcci_graph.do` (Stata). Cách dùng và ví dụ cho từng công cụ: [`charts/README.md`](charts/README.md).

Trong tài liệu Quarto, gọi file theme trong `charts/` (hoặc chép file cạnh file `.qmd`):

- **R** (knitr): `source("charts/theme_vcci.R")` rồi `vcci_setup()` trong khối mã đầu tiên. Biểu đồ trong bản PDF là SVG (chữ dựng bằng font của tài liệu); bản Word dùng PNG 300 dpi. Mẫu: `charts.qmd`, kết quả ở [`examples/charts.pdf`](examples/charts.pdf).
- **Python** (Jupyter): `sys.path.insert(0, "charts")`, `import vcci_charts as vcci` rồi `vcci.use()`. Thêm `fig-format: svg` cho định dạng Typst và `fig-format: png` cho Word.
- **Stata:** vẽ trong Stata (`adopath + "charts"` để Stata tìm thấy scheme, rồi `do charts/vcci_graph.do`), xuất `graph export ten.svg`, rồi chèn `![Chú thích](ten.svg)`.

Chú thích hình (`fig-cap`) nêu thông điệp của biểu đồ; nguồn số liệu ghi dưới biểu đồ (`caption` trong ggplot, `vcci.source()`, `note()` trong Stata).
