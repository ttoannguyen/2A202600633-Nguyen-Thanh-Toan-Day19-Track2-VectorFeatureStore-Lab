# Reflection — Lab 19

**Tên:** Nguyễn Thành Toàn - 2A202600633
**Cohort:** A20
**Path đã chạy:** lite

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Trên golden set 50 query (lite path: `bge-small-en-v1.5` + BM25 + RRF k=60), Precision@10:

- **exact (15):** BM25 và hybrid bằng nhau 96.7%, semantic 88.7%. Thuật ngữ xuất hiện nguyên văn → tín hiệu từ khóa đã đủ mạnh.
- **paraphrase (15):** cả ba đều yếu (kw 33.3%, hyb 32.0%, sem 24.0%). Đáng lẽ semantic thắng, nhưng embedding English-trained embed tiếng Việt kém — bằng chứng "chọn embedding model quan trọng" (đổi `bge-m3` sẽ tốt hơn).
- **mixed (20):** hybrid thắng rõ 100% (vs 97–98.5%). Câu vừa có từ exact vừa có ý paraphrase → RRF tận dụng cả hai tín hiệu.

Trung bình hybrid 78.6% > keyword 77.8% > semantic 73.2%: hybrid thắng nhờ **robust trên mọi loại query**, không nhờ vượt trội ở một loại.

**Khi không dùng hybrid:** (1) query thuần exact (mã lỗi, ID, tên hàm) → pure BM25 đủ tốt mà nhanh/rẻ hơn (P99 1.2ms vs 10.6ms); (2) cần matching ngữ nghĩa với embedding mạnh nhưng budget latency/chi phí eo hẹp → pure vector; (3) corpus nhỏ hoặc một tín hiệu áp đảo → retriever thứ hai chỉ tăng phức tạp hạ tầng.

---

## Điều ngạc nhiên nhất khi làm lab này

Semantic search **thua** keyword trên paraphrase tiếng Việt (24.0% vs 33.3%) — ngược với kỳ vọng. Nguyên nhân là `bge-small-en-v1.5` train chủ yếu tiếng Anh nên embed tiếng Việt yếu; với corpus tiếng Việt thật phải chọn model multilingual (`bge-m3`).

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
