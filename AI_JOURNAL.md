# NHẬT KÝ LÀM VIỆC VỚI AI - ECO2432

---

## Lab 04: Thẩm định rủi ro hợp đồng token

### Lần 1

**Prompt:**
```text
Bạn là chuyên viên thẩm định rủi ro tài sản số.
Dưới đây là mã nguồn một hợp đồng token. Hãy liệt kê mọi quyền đặc biệt mà
chủ sở hữu hợp đồng có thể thực hiện, và với mỗi quyền, nêu rõ:
- Tên hàm và số dòng
- Người nắm giữ token chịu rủi ro gì
Chỉ trả lời dựa trên mã nguồn tôi cung cấp. Nếu không tìm thấy, nói là không tìm thấy.

[Mã nguồn contracts/lab04/ClubTokens.sol]
```

**AI trả về:**
- Nhận diện đúng 3 hợp đồng trong file `ClubTokens.sol`: `ClubTokenA`, `ClubTokenB`, `ClubTokenC`.
- Xác định `ClubTokenA` không có quyền đặc biệt nào của chủ sở hữu sau khi khởi tạo (dòng 7 - 11).
- Xác định `ClubTokenB` có hàm `mint` (dòng 18 - 20) với quyền `onlyOwner`, gây nguy cơ lạm phát / pha loãng vô hạn.
- Xác định `ClubTokenC` có hàm `setRestricted` (dòng 30 - 32) với quyền `onlyOwner`, kết hợp `_update` (dòng 34 - 37) để chặn địa chỉ chuyển tiền (đóng băng/blacklist tài khoản).
- Liệt kê chính xác tên hàm, số dòng và rủi ro thực tế cho nhà đầu tư.

**Đánh giá:** Dùng được.

**So sánh kết quả giữa đọc thủ công và AI:**
1. **Đọc thủ công tìm ra gì:**
   - Dễ dàng thấy modifier `onlyOwner` ở `ClubTokenB` (hàm `mint`) và `ClubTokenC` (hàm `setRestricted`).
   - Nhận biết `ClubTokenA` không kế thừa `Ownable` nên không có hàm quản trị riêng.
2. **AI tìm thêm được gì:**
   - Phân tích sâu tác động thực tế của hàm `_update` ở `ClubTokenC`: giải thích chi tiết rằng `require(!restricted[from])` chỉ chặn chiều gửi (`from`), nghĩa là ví bị hạn chế vẫn có thể nhận token vào nhưng không thể bán hoặc chuyển ra.
   - Nhắc nhở thêm về các hàm thừa kế mặc định từ OpenZeppelin `Ownable` (`transferOwnership`, `renounceOwnership`).
3. **AI có nói sai chỗ nào không:**
   - Không bịa thêm hàm (tuân thủ tốt ràng buộc *"Nếu không tìm thấy, nói là không tìm thấy"*).
   - Số dòng được trích dẫn khớp hoàn toàn với tệp `ClubTokens.sol`.

**Chỗ sai:** Không có sai sót logic hoặc sai lệch số dòng.

**Cách sửa:** Không cần sửa mã nguồn, bổ sung bảng tổng hợp chuẩn hóa vào `lab04.md`.

**Ai phát hiện:** Sinh viên đối chiếu và xác nhận mã nguồn.

---

## Lab 06: Sinh mã bằng AI và kiểm tra kết quả (Phân tích dòng tiền ví)

### Lần 1

**Prompt:**
```text
Đọc tệp SPEC.md trong dự án và viết chương trình Python thực hiện đúng đặc tả đó.
Tuân thủ các quy ước trong AGENTS.md.
Trước khi viết mã, tóm tắt lại cách bạn hiểu yêu cầu để tôi xác nhận.
```

**AI trả về:**
Tóm tắt cách hiểu yêu cầu và sinh mã chương trình phân tích ví ban đầu. Khi chạy đối chiếu với danh mục 6 điểm kiểm tra bắt buộc, sinh viên phát hiện các lỗi bên dưới.

**Đánh giá:** Phải sửa.

---

### Ghi nhận lỗi 1: Bỏ qua phí gas của giao dịch đi ra bị thất bại (Điểm kiểm tra số 4)

- **Mô tả lỗi:** Trong phiên bản mã ban đầu, khi phân loại giao dịch đi ra (`from == address`), AI viết điều kiện:
  ```python
  if tx.get("isError") == "0":
      so_tien = value + fee
      so_du -= so_tien
  else:
      continue  # Bo qua vi giao dich loi
  ```
  AI đã bỏ qua hoàn toàn giao dịch khi `isError == "1"`.
- **Dòng mã sai:** Khối xử lý nhánh `else` của `isError` trong hàm tính toán.
- **Hậu quả:** Trên mạng Ethereum, dù giao dịch thất bại (revert), thợ đào/validator vẫn thu phí mạng (`gasUsed * gasPrice`). Việc bỏ qua khiến số dư lũy kế bị lệch cao hơn thực tế (thất thoát phí gas không được ghi nhận).
- **Cách sửa:** Sinh viên sửa lại: Khi `isError == "1"`, không trừ `value` nhưng vẫn phải trừ `fee = (gasUsed * gasPrice) / 10**18` vào dòng tiền ra.
- **Ai phát hiện:** Sinh viên phát hiện (đối chiếu quy tắc R4 trong `SPEC.md`).

---

### Ghi nhận lỗi 2: Thiếu cơ chế phân trang khi số lượng giao dịch lớn (Điểm kiểm tra số 3)

- **Mô tả lỗi:** Khi gọi API Etherscan, AI chỉ gửi một request đơn giản:
  ```python
  params = {"module": "account", "action": "txlist", "address": address, "apikey": api_key}
  ```
  không có tham số `page` và vòng lặp `while True` để kiểm tra phân trang.
- **Dòng mã sai:** Hàm gọi API lấy dữ liệu giao dịch.
- **Hậu quả:** Etherscan chỉ trả về tối đa 10.000 bản ghi trên 1 trang mặc định. Nếu ví có tần suất giao dịch cao trong 90 ngày, dữ liệu trả về bị thiếu các trang tiếp theo, dẫn đến biểu đồ và số dư tổng hợp bị sai lệch nghiêm trọng.
- **Cách sửa:** Sinh viên viết thêm vòng lặp phân trang (`page += 1`, `offset=1000`), liên tục nạp kết quả cho đến khi độ dài danh sách trả về nhỏ hơn `offset` hoặc hết giao dịch trong 90 ngày.
- **Ai phát hiện:** Sinh viên phát hiện (đối chiếu trường hợp ngoại lệ E4 trong `SPEC.md`).
