# Báo Cáo Cá Nhân — Lab Day 08: RAG Pipeline

**Họ và tên:** Nguyễn Quang Minh 
**Vai trò trong nhóm:** Eval Owner  
**Ngày nộp:** 2026-04-13  
**Độ dài yêu cầu:** 500-800 từ

---

## 1. Tôi đã làm gì trong lab này? (100-150 từ)

Trong lab này, tôi đảm nhiệm vai trò Eval Owner, tập trung vào việc thiết kế và phân tích hệ thống đánh giá cho pipeline RAG. Tôi chủ yếu làm việc ở sprint evaluation, nơi tôi xây dựng bộ test questions và định nghĩa các metric đánh giá bao gồm faithfulness, relevance, context recall và completeness. Tôi cũng implement quy trình so sánh giữa hai cấu hình retrieval là baseline_dense và variant_hybrid_rerank, đồng thời generate các file kết quả như `ab_comparison.csv` và các scorecard tương ứng. Ngoài ra, tôi phân tích kết quả để xác định điểm mạnh và điểm yếu của từng cấu hình. Công việc của tôi kết nối trực tiếp với Retrieval Owner (ảnh hưởng đến recall) và Generation (ảnh hưởng đến faithfulness và completeness), giúp team hiểu rõ hiệu năng của toàn bộ pipeline.

---

## 2. Điều tôi hiểu rõ hơn sau lab này (100-150 từ)

Sau lab này, tôi hiểu rõ hơn về evaluation loop trong hệ thống RAG và tầm quan trọng của việc sử dụng nhiều metric khác nhau thay vì chỉ dựa vào accuracy. Cụ thể, tôi nhận ra rằng faithfulness giúp phát hiện hallucination, context recall phản ánh chất lượng retrieval, trong khi completeness đánh giá mức độ đầy đủ của câu trả lời. Tôi cũng hiểu rằng retrieval tốt không đồng nghĩa với câu trả lời tốt, vì generation có thể bỏ sót thông tin quan trọng từ context. Ngoài ra, tôi học được rằng hybrid retrieval (kết hợp dense và keyword search) không phải lúc nào cũng outperform baseline, đặc biệt trong các dataset nhỏ hoặc câu hỏi đơn giản. Việc đánh giá cần dựa trên nhiều lần chạy và phân tích chi tiết từng trường hợp lỗi.

---

## 3. Điều tôi ngạc nhiên hoặc gặp khó khăn (100-150 từ)

Điều khiến tôi bất ngờ nhất là kết quả evaluation không ổn định giữa các lần chạy dù không thay đổi code. Ở lần chạy đầu tiên, variant_hybrid_rerank outperform baseline, nhưng ở các lần sau thì baseline lại có kết quả tốt hơn. Sau khi phân tích, tôi nhận ra nguyên nhân có thể đến từ tính nondeterministic của embedding, thứ tự retrieval hoặc caching trong vector database (ví dụ chroma_db). Ngoài ra, việc mất file kết quả do git conflict và untracked files cũng gây khó khăn lớn, khiến tôi phải tìm cách khôi phục dữ liệu. Ban đầu tôi giả định rằng hybrid retrieval luôn tốt hơn, nhưng thực tế cho thấy nó có thể tạo thêm noise nếu reranking không đủ hiệu quả.

---

## 4. Phân tích một câu hỏi trong scorecard (150-200 từ)

**Câu hỏi:** q07 — "Approval Matrix để cấp quyền hệ thống là tài liệu nào?"

**Phân tích:**

Ở câu hỏi này, cả baseline_dense và variant_hybrid_rerank đều đạt điểm cao ở faithfulness, relevance và context recall (đều là 5), nhưng completeness chỉ đạt 2. Điều này cho thấy hệ thống retrieval đã hoạt động đúng và cung cấp đủ context liên quan. Tuy nhiên, lỗi nằm ở bước generation: câu trả lời chỉ đề cập đến tên cũ của tài liệu ("Approval Matrix for System Access") mà không đề cập đến tên mới ("Access Control SOP") như trong expected answer.

Variant không cải thiện được vấn đề này vì lỗi không nằm ở retrieval mà ở việc model không tổng hợp đầy đủ thông tin từ context. Có thể chunk chứa thông tin rename không được ưu tiên hoặc prompt chưa đủ mạnh để yêu cầu model cung cấp thông tin đầy đủ. Trường hợp này cho thấy rằng context recall cao không đảm bảo completeness cao. Để cải thiện, cần điều chỉnh prompt hoặc cải thiện chiến lược chunking để đảm bảo thông tin quan trọng không bị bỏ sót.

---

## 5. Nếu có thêm thời gian, tôi sẽ làm gì? (50-100 từ)

Nếu có thêm thời gian, tôi sẽ thực hiện hai cải tiến chính. Thứ nhất, tôi sẽ làm cho quá trình evaluation trở nên deterministic hơn bằng cách cố định random seed và reset vector database mỗi lần chạy, nhằm đảm bảo kết quả ổn định. Thứ hai, tôi sẽ cải thiện prompt để yêu cầu model trả lời đầy đủ hơn, ví dụ như yêu cầu đề cập đến các tên tài liệu đã được cập nhật. Ngoài ra, tôi cũng muốn thử nghiệm các phương pháp reranking mạnh hơn để giảm noise từ hybrid retrieval.

---

*Lưu file này với tên: reports/individual/[ten_ban].md* 
*Ví dụ: reports/individual/nguyen_van_a.md* ""