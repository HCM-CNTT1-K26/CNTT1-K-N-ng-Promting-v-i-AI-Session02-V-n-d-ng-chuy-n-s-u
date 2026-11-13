# CNTT1-K-N-ng-Promting-v-i-AI-Session02-V-n-d-ng-chuy-n-s-u
PHẦN 1: THIẾT KẾ GIẢI PHÁP (I/O)

Input: Văn bản thô từ khách hàng/nhân viên (có thể thiếu thông tin hoặc sai sót).

Output:

Bảng tóm tắt nhanh cho nhân viên kiểm tra.

Định dạng JSON chuẩn để đẩy dữ liệu (CREATE) vào hệ thống.

Cơ chế bắt lỗi (INVALID) khi thiếu dữ liệu bắt buộc nhằm tránh AI tự bịa (hallucination).

PHẦN 2: PROMPT THỰC THI & KẾT QUẢ
1. Khung Prompt sử dụng:

"Bạn là trợ lý xử lý đơn hàng. Nhiệm vụ: Đọc đoạn văn bản thô, trích xuất thông tin thành Bảng Markdown và Khối JSON (action: CREATE).
Quy tắc bắt buộc: Nếu thiếu Họ tên, SĐT hoặc Sản phẩm, phải ghi MISSING, đặt trạng thái INVALID và liệt kê lỗi trong mảng errors. Tuyệt đối không tự bịa thông tin.
(Đính kèm 2 ví dụ mẫu Few-shot: 1 đơn hợp lệ, 1 đơn thiếu thông tin)
Input cần xử lý: 'Lên đơn cho chị Mai ở Thủ Đức, sđt 0988777666 mua 3 hộp bánh trung thu thập cẩm loại lớn.'"

2. Kết quả trả về:

Trường thông tin	Giá trị
Khách hàng	Chị Mai
Số điện thoại	0988777666
Sản phẩm	Bánh trung thu thập cẩm (Loại lớn)
Số lượng	3
Địa chỉ	Thủ Đức (Thiếu số nhà, tên đường)
Trạng thái	INVALID
JSON
{
  "action": "CREATE",
  "status": "INVALID",
  "order_data": {
    "customer_name": "Chị Mai",
    "phone": "0988777666",
    "items": [
      {
        "product_name": "Bánh trung thu thập cẩm",
        "variant": "Loại lớn",
        "quantity": 3
      }
    ],
    "shipping_address": "Thủ Đức"
  },
  "errors": ["Địa chỉ giao hàng chưa đủ chi tiết (thiếu số nhà, tên đường)"]
}
