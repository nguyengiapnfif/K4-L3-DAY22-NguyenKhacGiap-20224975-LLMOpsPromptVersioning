# Evidence — Day 22 LLMOps Prompt Versioning

## So sánh V1 và V2 (RAGAS, 50 câu hỏi)

| Chỉ số | V1 (ngắn gọn, 2–4 câu) | V2 (có cấu trúc, 3–5 câu) |
|---|---|---|
| faithfulness | 0.9569 | 0.9386 |
| answer_relevancy | 0.9161 | 0.8899 |
| context_recall | 1.0000 | 1.0000 |
| context_precision | 0.9417 | 0.9417 |

Cả hai phiên bản đạt faithfulness ≥ 0.8. V1 nhỉnh hơn ở faithfulness và answer_relevancy.

**Nhận xét:**
- `context_recall` và `context_precision` giống nhau vì cả hai dùng chung retriever (FAISS, k=3, chunk 500/50). Phần truy xuất không phụ thuộc prompt, nên khác biệt chỉ đến từ cách viết câu trả lời.
- V2 yêu cầu câu trả lời dài hơn, có mở đầu và chi tiết. Câu dài hơn có nhiều nhận định hơn để RAGAS kiểm tra, nên khả năng có một claim không bám context cao hơn, dẫn đến faithfulness thấp hơn một chút. V1 ngắn nên bám sát trọng tâm câu hỏi, answer_relevancy cao hơn.
- Chênh lệch nhỏ (khoảng 0.02–0.03) trên 50 mẫu, nên chưa đủ để kết luận chắc V1 tốt hơn.

## Ghi chú chạy

- LLM cho nhiệm vụ 1 và 2: `openai/gpt-6-luna` qua OpenRouter.
- Nhiệm vụ 3 (RAGAS) chạy bằng `openai/gpt-4o-mini` qua OpenRouter, vì tài khoản mới bị giới hạn 20 request/phút với `gpt-6-luna`.
- Routing A/B dùng MD5 của `request_id` (`req-0000` đến `req-0049`): V1 = 19 câu, V2 = 31 câu.
- `evidence/02_prompt_hub.png` chụp Prompt Hub với 2 prompt đặt tên riêng.
