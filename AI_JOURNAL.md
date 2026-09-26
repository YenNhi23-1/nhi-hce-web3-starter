# NHẬT KÝ LÀM VIỆC VỚI AI - Lab 04 (Thẩm định rủi ro hợp đồng token)

## Lần 1

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
