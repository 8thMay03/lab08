# Báo Cáo Cá Nhân — Lab Day 08: RAG Pipeline

**Họ và tên:** Nguyễn Quang Minh 
**Vai trò trong nhóm:** Eval Owner  
**Ngày nộp:** 2026-04-13  
**Độ dài yêu cầu:** 500-800 từ

---

## 1. Tôi đã làm gì trong lab này? (100-150 từ)

Trong lab này, tôi đảm nhiệm vai trò Eval Owner, tập trung vào việc thiết kế và phân tích hệ thống đánh giá cho pipeline RAG. Tôi chủ yếu làm việc ở sprint evaluation, nơi tôi xây dựng bộ test questions (bao gồm cả các câu hỏi cross-document và insufficient context) và định nghĩa các metric đánh giá như faithfulness, relevance, context recall và completeness. Tôi cũng implement quy trình A/B testing giữa hai cấu hình retrieval là baseline_dense và variant_hybrid_rerank, đồng thời generate các file kết quả như `ab_comparison.csv` và các scorecard tương ứng. Ngoài ra, tôi phân tích kết quả để xác định điểm mạnh và điểm yếu của từng cấu hình. Công việc của tôi kết nối trực tiếp với Retrieval Owner (ảnh hưởng đến context recall) và phần generation (ảnh hưởng đến faithfulness và completeness), giúp team hiểu rõ hiệu năng thực tế của toàn bộ pipeline.

---

## 2. Điều tôi hiểu rõ hơn sau lab này (100-150 từ)

Sau lab này, tôi hiểu rõ hơn về evaluation loop trong hệ thống RAG và tầm quan trọng của việc tách biệt các loại lỗi khác nhau. Cụ thể, context recall phản ánh chất lượng retrieval, trong khi faithfulness giúp kiểm tra việc model có hallucinate hay không, và completeness đo mức độ đầy đủ của câu trả lời. Tôi nhận ra rằng một hệ thống có thể đạt context recall = 5 nhưng vẫn có completeness thấp nếu model không sử dụng hết thông tin trong context. Ngoài ra, tôi hiểu rõ hơn về hybrid retrieval: việc kết hợp dense và keyword search có thể cải thiện faithfulness (tăng từ 4.30 lên 4.60) nhưng không nhất thiết cải thiện completeness, do vấn đề chính có thể nằm ở bước generation. Điều này cho thấy evaluation cần được nhìn một cách đa chiều thay vì chỉ dựa vào một metric.

---

## 3. Điều tôi ngạc nhiên hoặc gặp khó khăn (100-150 từ)

Điều khiến tôi bất ngờ nhất là hybrid retrieval không cải thiện toàn diện như kỳ vọng. Mặc dù variant_hybrid_rerank có faithfulness cao hơn baseline (4.60 so với 4.30), nhưng relevance và completeness lại không cải thiện rõ rệt. Đặc biệt, ở một số câu hỏi như gq05, variant còn làm giảm relevance do đưa vào context không phù hợp (lấy thông tin về temporary access thay vì admin access chính thức). Ngoài ra, việc kết quả evaluation thay đổi giữa các lần chạy cũng gây khó khăn, có thể do tính nondeterministic của embedding hoặc retrieval order trong vector database. Tôi cũng gặp vấn đề với git khi các file kết quả bị conflict và mất dữ liệu, buộc phải khôi phục lại pipeline và rerun evaluation. Ban đầu tôi giả định retrieval là bottleneck chính, nhưng thực tế cho thấy generation mới là điểm yếu lớn hơn.

---

## 4. Phân tích một câu hỏi trong scorecard (150-200 từ)

**Câu hỏi:** gq05 — "Contractor từ bên ngoài công ty có thể được cấp quyền Admin Access không? Nếu có, cần bao nhiêu ngày và có yêu cầu đặc biệt gì?"

**Phân tích:**

Ở câu hỏi này, baseline_dense có faithfulness thấp (2) nhưng relevance và context recall đều cao (5), trong khi variant_hybrid_rerank cải thiện faithfulness lên 5 nhưng lại giảm relevance (4) và completeness (3). Điều này cho thấy một trade-off rõ ràng giữa hai cấu hình.

Cụ thể, baseline trả lời gần với expected answer hơn về mặt nội dung (có đề cập đến thời gian 5 ngày và training), nhưng lại chứa thông tin không hoàn toàn grounded trong context, dẫn đến điểm faithfulness thấp. Ngược lại, variant chỉ sử dụng thông tin có trong context (quy trình cấp quyền tạm thời trong sự cố P1), nên đạt faithfulness cao, nhưng lại trả lời sai trọng tâm câu hỏi về admin access chính thức.

Lỗi ở đây chủ yếu nằm ở retrieval: hệ thống không phân biệt được ngữ cảnh "admin access" và "temporary access", dẫn đến việc retrieve nhầm chunk. Hybrid retrieval thậm chí còn làm vấn đề tệ hơn do đưa thêm noise vào context. Điều này cho thấy cần cải thiện query understanding hoặc reranking để đảm bảo đúng intent của câu hỏi.

---

## 5. Nếu có thêm thời gian, tôi sẽ làm gì? (50-100 từ)

Nếu có thêm thời gian, tôi sẽ tập trung vào hai cải tiến chính. Thứ nhất, tôi sẽ cải thiện retrieval bằng cách thêm semantic filtering hoặc query rewriting để phân biệt rõ các intent như "admin access" và "temporary access". Thứ hai, tôi sẽ cải thiện prompt để yêu cầu model trả lời đầy đủ hơn, đặc biệt là các câu hỏi cross-document, vì kết quả cho thấy completeness đang là điểm yếu chính (chỉ ~3.40). Ngoài ra, tôi cũng muốn làm cho pipeline deterministic hơn để đảm bảo kết quả evaluation ổn định.

---

*Lưu file này với tên: reports/individual/[ten_ban].md* 
*Ví dụ: reports/individual/nguyen_van_a.md* ""