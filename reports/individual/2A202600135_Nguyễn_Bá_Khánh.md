# Báo Cáo Cá Nhân — Lab Day 08: RAG Pipeline

**Họ và tên:** Nguyễn Bá Khánh  
**Vai trò trong nhóm:**  Preprocess, Chunk
**Ngày nộp:** 13/04/2026  
**Độ dài yêu cầu:** 500–800 từ

---

1. Tôi đã làm gì trong lab này? (100-150 từ)

-Tôi chủ yếu tham gia ở sprint đầu tiên, xây dựng và hoàn thiện phần indexing cho pipeline.

- Tôi thiết kế và implement toàn bộ quá trình tiền xử lý và chunking tài liệu trong index.py, quyết định cách chia đoạn, độ dài chunk, và các bước làm sạch dữ liệu.

- Dữ liệu tôi xử lý và chunk là đầu vào cho các thành viên khác thực hiện truy xuất (retrieval) và sinh câu trả lời (generation). Nếu chunking không hợp lý, các bước sau sẽ bị ảnh hưởng về độ chính xác.
2. Điều tôi hiểu rõ hơn sau lab này (100-150 từ)
- Sau lab này, tôi hiểu rõ hơn về concept chunking. Trước đây, tôi nghĩ chỉ cần chia nhỏ văn bản là đủ, nhưng thực tế, cách chia chunk ảnh hưởng rất lớn đến hiệu quả truy xuất thông tin. Nếu chunk quá dài, hệ thống có thể bỏ sót thông tin quan trọng; nếu quá ngắn, ngữ cảnh sẽ bị mất, dẫn đến câu trả lời không đầy đủ. Tôi cũng nhận ra việc tiền xử lý dữ liệu (loại bỏ ký tự thừa, chuẩn hóa văn bản) là rất quan trọng để đảm bảo các chunk nhất quán và dễ dàng cho mô hình xử lý. Nhờ đó, tôi hiểu sâu hơn về vai trò của preprocessing và chunking trong pipeline RAG.


3. Điều tôi ngạc nhiên hoặc gặp khó khăn (100-150 từ)
- Khó khăn lớn nhất tôi gặp phải là khi chunking tài liệu, một số đoạn bị tách không hợp lý do ký tự xuống dòng hoặc dấu câu đặc biệt. Ban đầu, tôi nghĩ chỉ cần split theo số lượng từ hoặc ký tự, nhưng thực tế, cần phải xử lý các trường hợp đặc biệt như tiêu đề, danh sách, hoặc đoạn hội thoại. Có lần, pipeline trả về kết quả không liên quan vì chunk chứa thông tin rời rạc. Tôi đã mất khá nhiều thời gian debug, thử nhiều cách chia khác nhau và cuối cùng chọn phương án kết hợp split theo đoạn và kiểm soát độ dài. Điều này giúp cải thiện chất lượng truy xuất rõ rệt.
4. Phân tích một câu hỏi trong scorecard (150-200 từ)
- Câu hỏi: "Chính sách hoàn tiền áp dụng trong trường hợp nào?"

- Baseline trả lời chưa chính xác, điểm chỉ đạt mức trung bình vì câu trả lời không nêu rõ các trường hợp cụ thể mà chỉ nói chung chung về hoàn tiền. Lỗi chủ yếu nằm ở bước chunking và retrieval: chunk chứa thông tin về hoàn tiền bị chia tách, dẫn đến retrieval không lấy được đầy đủ ngữ cảnh. Ở phiên bản variant, sau khi cải thiện chunking và bổ sung một số rule cho retrieval, câu trả lời đã đầy đủ hơn, liệt kê rõ các trường hợp áp dụng hoàn tiền, điểm số tăng lên đáng kể. Điều này cho thấy việc chunking hợp lý và retrieval tốt sẽ giúp mô hình generation trả lời sát với yêu cầu hơn.

5. Nếu có thêm thời gian, tôi sẽ làm gì? (50-100 từ)

- Tôi sẽ thử các phương pháp chunking động, ví dụ như chia theo đoạn văn hoặc tiêu đề thay vì cố định số từ, vì kết quả eval cho thấy một số chunk hiện tại vẫn bị mất ngữ cảnh. Ngoài ra, tôi muốn thử tích hợp thêm các bước preprocessing nâng cao như loại bỏ stopword hoặc chuẩn hóa unicode để tăng chất lượng dữ liệu đầu vào cho pipeline."


---

*Lưu file này với tên: `reports/individual/[ten_ban].md`*
*Ví dụ: `reports/individual/nguyen_van_a.md`*