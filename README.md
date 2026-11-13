
### BÀI LÀM: THIẾT KẾ GIẢI PHÁP PROMPTING

**Bài toán:** Tự động trích xuất đơn hàng từ tin nhắn chat, kiểm tra lỗi thiếu dữ liệu/sai SĐT và ép định dạng JSON kèm bảng.

---

### Bước 1: Thiết kế giải pháp (I/O)

* **Input:** Tin nhắn thô của khách (ví dụ có lỗi thiếu SĐT, thiếu size).
* **Output:**
1. Cõi JSON chuẩn hóa cho hệ thống.
2. Bảng Markdown (`Table`) tóm tắt cho nhân viên kiểm tra.



---

### Bước 2: Viết Prompt & Chạy thử

**Mẫu Prompt hoàn chỉnh (áp dụng CREATE + Few-shot + Bẫy lỗi):**


[Role & Context] Bạn là AI bóc tách đơn hàng cho shop thời trang.
[Few-shot Examples] (Dùng 2 ví dụ mẫu chuẩn hóa Input -> Output JSON).
[Action & Target] Đọc tin nhắn sau và trích xuất: "shop ơi lấy mình 2 áo hoodie đen size XL với 1 quần jogger xám nhé, giao về 45/12 đường số 8 Linh Trung Thủ Đức. sđt 091234567, tên Huy. giao trước 5h chiều nay"
[Constraints & Error Handling] 
1. Nếu thiếu thông tin (tên, sđt, địa chỉ, size), bắt buộc điền "TRONG".
2. Kiểm tra SĐT, nếu không đủ 10 số phải báo lỗi vào "canh_bao_loi" và đổi trang_thai thành "CAN_KIEM_TRA".
3. Đầu ra bắt buộc gồm 1 khối JSON và 1 bảng Table.



**Kết quả AI phản hồi:**

json

{

  "trang_thai": "CAN_KIEM_TRA",
  
  "khach_hang": { "ten": "Huy", "sdt": "091234567", "dia_chi": "45/12 đường số 8, Linh Trung, Thủ Đức" },
  
  "san_pham": [

  { "ten_sp": "Áo hoodie", "mau_sac": "Đen", "size": "XL", "so_luong": 2 },
  
  { "ten_sp": "Quần jogger", "mau_sac": "Xám", "size": "TRONG", "so_luong": 1 }
  
  ],
  
  "ghi_chu_giao": "Giao trước 5h chiều nay",
  
  "canh_bao_loi": ["SĐT thiếu 1 số (9 chữ số)", "Quần jogger chưa có size"]

}



| Hạng mục | Dữ liệu trích xuất | Tình trạng lỗi |
| --- | --- | --- |
| **Khách hàng** | Huy | Đầy đủ |
| **SĐT** | 091234567 | **Lỗi (Thiếu 1 số)** |
| **Địa chỉ** | 45/12 đường số 8, Linh Trung, Thủ Đức | Đầy đủ |
| **Sản phẩm 1** | Áo hoodie - Đen - Size XL (SL: 2) | Hợp lệ |
| **Sản phẩm 2** | Quần jogger - Xám - Size: `TRONG` (SL: 1) | **Lỗi (Thiếu size)** |
