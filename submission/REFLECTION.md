# Reflection — Lab 19

**Tên:** Phạm Triều Dương
**MSSV:** 2A202600833
**Cohort:** 2
**Path đã chạy:** lite

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Trên 50 golden queries, Precision@10 trung bình: BM25 77.8%, vector 73.2%,
hybrid **78.6%** — hybrid thắng nhờ ổn định trên mọi loại query.

Cắt theo loại:
- **exact (15):** BM25 = hybrid = 96.7% > vector 88.7%. Query chứa thuật ngữ
  verbatim nên signal từ khóa đã đủ mạnh, embedding không thêm gì.
- **paraphrase (15):** BM25 33.3% > hybrid 32.0% > vector 24.0%. Bất ngờ:
  vector *không* thắng. Lý do là model `bge-small-en-v1.5` được train cho
  tiếng Anh nên semantic recall trên paraphrase tiếng Việt yếu — đổi sang
  `bge-m3` (multilingual) sẽ giúp vector vươn lên. "Embedding model phải khớp
  ngôn ngữ corpus" là bài học rõ nhất ở đây.
- **mixed (20):** hybrid 100% > vector 98.5% > BM25 97.0%. Query vừa có term
  exact vừa có ý paraphrase → RRF gộp được cả hai signal.

**Khi không dùng hybrid:** khi latency/chi phí bị ràng buộc và query thuần
exact-keyword — NB3 cho thấy BM25 P99 chỉ 3.8ms so với hybrid 20.2ms mà chất
lượng ngang nhau, nên chạy 2 retriever là lãng phí. Ngược lại, nếu embedding
khớp ngôn ngữ và query thuần ngữ nghĩa, pure vector là đủ.

---

## Điều ngạc nhiên nhất khi làm lab này

Vector *không* thắng paraphrase như kỳ vọng — vì model embedding tiếng Anh
kéo điểm xuống. Cùng một pipeline, đổi model là đảo kết quả: chọn model
quan trọng hơn chọn thuật toán fusion.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
