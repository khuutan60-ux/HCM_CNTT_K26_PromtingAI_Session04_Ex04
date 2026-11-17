# Phân tích Trade-off các giải pháp ra lệnh cho Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)

Bài tập thuộc môn **Kỹ Năng Prompting với AI** ([Chi tiết môn học](https://lms-student.rikkei.edu.vn/courses)) - Buổi 4: Kỹ thuật Prompting Nâng cao (Logic & Chuỗi).

---

## 1. Mục tiêu bài tập
Rèn luyện tư duy phản biện và khả năng đánh đổi (trade-off) khi thiết kế prompt thông qua việc đề xuất 2 giải pháp viết prompt khác nhau cho cùng một bài toán tự động hóa nghiệp vụ, sau đó so sánh và lựa chọn phương án tối ưu nhất.

## 2. Bối cảnh & Yêu cầu bài toán
* **Bối cảnh:** Xây dựng hệ thống AI tự động hóa quy trình chăm sóc khách hàng và xử lý đơn hàng tại cửa hàng.
* **Yêu cầu:** Ứng dụng tối ưu hóa các kỹ thuật **Chain-of-Thought (CoT)**, **Prompt Chaining** và **Xử lý đứt gãy/lỗi dữ liệu (Validation & Fallback)**.


## Bước 1: Đề xuất 2 Prompt mẫu

### Giải pháp 1: Prompt Đơn Giản, Ngắn Gọn
 **Đặc điểm:** Định hướng trực diện, yêu cầu AI suy luận ngắn gọn và phản hồi ngay mà không thiết lập nhiều ràng buộc cấu trúc hay quy trình bước nhỏ.

Bạn là trợ lý bán hàng. Hãy đọc tin nhắn khách hàng: "Cho mình đặt 1 cái Quần Jean Slimfit size 32, màu đen nhé." 
Kho hàng hiện có: Quần Jean Slimfit chỉ còn size 30 và 31.
Hãy dùng suy nghĩ từng bước (Chain-of-Thought) để tư vấn và viết tin nhắn phản hồi lại cho khách hàng.
Giải pháp 2: Prompt Chi Tiết, Nhiều Ràng Buộc
Đặc điểm: Chia rõ thành các bước (Prompt Chaining), thiết lập điều kiện rẽ nhánh xử lý lỗi dữ liệu biên (chặn lỗi khi khách chọn sai size/hết hàng) và định dạng đầu ra rõ ràng.

Bạn là Trợ lý AI cao cấp chuyên tự động hóa quy trình bán hàng. Hãy thực hiện quy trình sau theo đúng thứ tự:

### DỮ LIỆU KHO HÀNG:
- Quần Jean Slimfit (Mã: QJS02) - Giá: 550.000đ - Size còn lại: 30, 31 (Hết size 32).

### QUY TRÌNH XỬ LÝ:
1. **Kiểm tra lỗi dữ liệu (Validation Check):** Kiểm tra xem size khách chọn có tồn tại trong kho không. Nếu hết size hoặc thông tin bị sai lệch, BẮT BUỘC dừng việc chốt đơn và chuyển sang bước xử lý lỗi.
2. **Suy luận logic (Chain-of-Thought):** Phân tích phương án thay thế tốt nhất cho khách hàng (gợi ý size gần nhất hoặc sản phẩm tương đương).
3. **Prompt Chaining Output:** Viết nội dung tin nhắn phản hồi thân thiện để thương lượng lại với khách hàng.

### ĐẦU VÀO TỪ KHÁCH HÀNG:
"Cho mình đặt 1 cái Quần Jean Slimfit size 32, màu đen nhé."

### ĐỊNH DẠNG ĐẦU RA:
- Trình bày lần lượt kết quả qua các mục: [1. Kiểm tra lỗi], [2. Suy luận CoT], [3. Tin nhắn phản hồi].
Bước 2: Lập bảng so sánh Trade-off
Tiêu chí	Giải pháp 1 (Đơn giản)	Giải pháp 2 (Chi tiết)
Độ dễ hiểu	Cao (Ngắn gọn, dễ viết nhanh)	Trung bình (Cần tư duy cấu trúc các bước rõ ràng)
Tốn token	Ít (Tiết kiệm chi phí gọi API/token)	Nhiều hơn (Do chứa nhiều ràng buộc hệ thống và văn bản hướng dẫn)
Chặn lỗi biên	Trung bình (Dễ bị AI bỏ sót các trường hợp ngoại lệ, tự ý bịa dữ liệu giả định)	Cực tốt (Có điều kiện rẽ nhánh và quy tắc xử lý lỗi chặt chẽ)
Bước 3: Lựa chọn và Chạy thử
1. Lựa chọn phương án tối ưu
Phương án được chọn: Giải pháp 2 (Chi tiết).

Lý do lựa chọn: Trong các bài toán nghiệp vụ thực tế (bán hàng, tài chính, vận hành), việc kiểm soát tính chính xác của dữ liệu (tránh việc AI tự ý đồng ý bán sản phẩm đã hết hàng) quan trọng hơn việc tiết kiệm một lượng nhỏ token. Giải pháp 2 giúp siết chặt hành vi của AI thông qua các điều kiện ràng buộc rõ ràng.

2. Kết quả chạy thử (Thực thi Giải pháp 2)
[1. Kiểm tra lỗi]:

Sản phẩm yêu cầu: Quần Jean Slimfit (size 32).

Đối chiếu kho: Kho chỉ còn size 30 và 31, hết size 32.

Kết luận: Phát hiện lỗi lệch dữ liệu tồn kho. Kích hoạt quy tắc xử lý đứt gãy: Không chốt đơn trực tiếp mà phải thông báo cho khách hàng và đề xuất phương án thay thế.

[2. Suy luận CoT]:

Khách hàng muốn size 32 nhưng không đáp ứng được. AI cần giữ chân khách bằng cách thông báo lịch sự, minh bạch tình trạng kho, đồng thời gợi ý chuyển sang size 31 hoặc tư vấn dòng sản phẩm khác.

[3. Tin nhắn phản hồi]:

"Dạ em chào anh/chị! Cảm ơn anh/chị đã quan tâm đến sản phẩm của cửa hàng. Hiện tại mẫu Quần Jean Slimfit size 32 bên em vừa hết hàng tạm thời (kho chỉ còn size 30 và 31). Anh/chị có muốn thử đổi sang size 31 (form ôm gọn) hoặc ghé xem các mẫu quần khác tương tự không ạ? Em gửi mẫu qua cho mình xem ngay nhé!"