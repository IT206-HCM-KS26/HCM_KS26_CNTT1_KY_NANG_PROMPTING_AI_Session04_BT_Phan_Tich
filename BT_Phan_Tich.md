# BÀI TẬP SESSION 04 [Phân tích] So sánh Trade-off các giải pháp ra lệnh cho Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)
## 1. Đề xuất 2 prompt mẫu
### Viết rõ 2 prompt tương ứng với 2 phương án giải quyết.

-> **"Bối cảnh:** Tối ưu phân tích đánh giá của khách hàng, và báo cáo cho doanh nghiệp thông qua Chain-of-Thought, Prompt Chaining, xử lý đứt gãy khi thông tin đầu vào bị lỗi hoặc thiếu thông tin."

- **Prompt 1:** "Hãy đọc danh sách phản hồi của khách hàng dưới đây. Đầu tiên, phân tích từng bước để phân loại cảm xúc (Tích cực/Tiêu cực) và tìm ra vấn đề chính. Sau đó, tiếp tục với kết quả đó để đề xuất 3 hành động cải thiện cho cửa hàng. Nếu dữ liệu bị lỗi hoặc trống thì báo lại cho tôi."

- **Prompt 2:** "Bạn là một chuyên gia Phân tích Dữ liệu về những phản hồi của Khách hàng."

   - Bước1: Kiểm tra dữ liệu đầu vào. Nếu dữ liệu trống, hoặc chứa ký tự không có nghĩa: DỪNG LẠI NGAY, không suy diễn và trả về đúng chuỗi: ERROR_DATA: Dữ liệu không hợp lệ, cần bổ sung và sửa lại.

   - Bước 2: Với phản hồi hợp lệ, hãy phân tích theo cấu trúc: Từ khóa chính -> Cảm xúc (Tích cực/Tiêu cực/Trung lập) -> Nguyên nhân gốc rễ.

   - Bước 3: Dựa vào duy nhất kết quả ở Bước 2, tổng hợp báo cáo. 

   **Ràng buộc:** Không tự bịa thêm thông tin ngoài dữ liệu đã cung cấp. Trình bày ngắn gọn khoảng 200 từ đến 300 từ.

## 2. Lập bảng so sánh Trade-off
### Lập bảng so sánh tiêu chí.

| Tiêu chí | Giải pháp 1 (Đơn giản) | Giải pháp 2 (Chi tiết) |
| :--- | :--- | :--- |
| Độ dễ hiểu | **Cao:** Viết nhanh, ngắn gọn, ngôn ngữ đơn giản. | **Trung bình:** Mất thời gian thiết kế cấu trúc, chia bước. |
| Tốn Token | **Ít tốn Token hơn** | **Nhiều hơn:** Gấp mấy lần ở giải pháp 1. | 
| Chặn lỗi biên | **Trung bình:** AI dễ tự ý bịa ra nhận xét, chỉ báo lỗi chung chung. | **Cực tốt:** Có chặn ngắt chuỗi ngay khi đầu vào lỗi, để trách bị gãy chuỗi giữa các bước. |

## 3. Lựa chọn và chạy thử 
### Chọn phương án tối ưu nhất, giải thích lý do chọn và chạy thử lấy kết quả.

- **Phương án chọn:** Giải pháp 2.
- **Lý do:** Vì giải pháp 2 chúng ta áp dụng cấu trúc Prompting Nâng cao sẽ giảm thiểu được rủi ro là khi sai một bước nào đó sẽ dẫn đến sai toàn bộ các bước ở phía sau. Việc tốn nhiều Token ở giải pháp 2 là xứng đáng để có thể đổi lại những dữ liệu chỉnh chu, chính xác, không nói chung chung từ AI. Ép định dạng ngắt lỗi an toàn khi gặp lỗi.

- **Kết quả chạy thử:**

**Dữ liệu nạp vào:** "1. Giao hàng quá chậm, đồ ăn nguội ngắt. 2. Nhân viên tư vấn nhiệt tình nhưng app hay bị văng. 3. Shipper giao trễ 40 phút làm hỏng bữa trưa."

**AI trả về**

>**Bước 1:** Kiểm tra dữ liệu đầu vào
Dữ liệu đầu vào hợp lệ, bao gồm 3 phản hồi rõ nghĩa từ khách hàng.

>**Bước 2:** Phân tích từng phản hồi
>
>->Phản hồi 1: "Giao hàng quá chậm, đồ ăn nguội ngắt."
>
>Từ khóa chính: Giao hàng chậm, đồ ăn nguội.
>
>Cảm xúc: Tiêu cực.
>
>Nguyên nhân gốc rễ: Thời gian giao hàng kéo dài làm đồ ăn mất nhiệt, giảm chất lượng.
>
>->Phản hồi 2: "Nhân viên tư vấn nhiệt tình nhưng app hay bị văng."
>
>Từ khóa chính: Tư vấn nhiệt tình, app bị văng.
>
>Cảm xúc: Trung lập (kết hợp giữa khen ngợi và phàn nàn).
>
>Nguyên nhân gốc rễ: Nhân sự hỗ trợ tốt nhưng hệ thống ứng dụng gặp lỗi kỹ thuật gây gián đoạn trải nghiệm.
>
>->Phản hồi 3: "Shipper giao trễ 40 phút làm hỏng bữa trưa."
>
>Từ khóa chính: Shipper giao trễ 40 phút, hỏng bữa trưa.
>
>Cảm xúc: Tiêu cực.
>
>Nguyên nhân gốc rễ: Tài xế giao hàng trễ làm ảnh hưởng trực tiếp đến thời gian dùng bữa của khách.

>Bước 3: Báo cáo tổng hợp
>
>->Dựa trên phân tích 3 phản hồi thu thập được, bức tranh trải nghiệm khách hàng hiện ghi nhận:
>
>->Phân bổ cảm xúc: Có 2/3 phản hồi mang sắc thái Tiêu cực và 1/3 phản hồi ở mức Trung lập. Không có phản hồi tích cực hoàn toàn.
>
>->Điểm mạnh ghi nhận: Chất lượng phục vụ của đội ngũ nhân viên tư vấn được đánh giá cao nhờ sự nhiệt tình.
>
>Các vấn đề cốt lõi cần lưu ý:
>
> - Khâu giao nhận (Chiếm 2/3 phản hồi): Tình trạng giao hàng chậm (cụ thể trễ 40 phút) là nguyên nhân chính gây bức xúc, dẫn đến hệ quả làm đồ ăn bị nguội và hỏng bữa trưa của khách.
>
> - Lỗi kỹ thuật nền tảng: Ứng dụng thường xuyên bị văng trong quá trình sử dụng, làm giảm trải nghiệm chung bất chấp sự hỗ trợ tốt từ nhân viên.


**Dữ liệu đầu vào bị lỗi (Kiểm tra bẫy đứt gãy)**

- Dữ liệu nạp vào: "@@@ ### 12345 [null]"

**AI trả về kết quả**

ERROR_DATA: Dữ liệu không hợp lệ, cần bổ sung và sửa lại.