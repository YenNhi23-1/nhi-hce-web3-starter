# Báo cáo thẩm định rủi ro hợp đồng token - Lab 04

## Bảng kết luận thẩm định

Dựa trên mã nguồn tệp `contracts/lab04/ClubTokens.sol`:

| Hợp đồng | Kết luận | Tên hàm | Số dòng | Rủi ro cho người nắm giữ |
| :--- | :--- | :--- | :--- | :--- |
| **ClubTokenA** | Không có quyền đặc biệt | Không tìm thấy | Dòng 7 - 11 | Không tìm thấy quyền đặc biệt nào của chủ sở hữu sau khi triển khai. Tổng cung cố định 1,000,000 CTA (được mint ở dòng 9). Không ai có thể mint thêm hay chặn chuyển token. |
| **ClubTokenB** | Rủi ro cao: In token vô hạn (Lạm phát / Pha loãng) | `mint(address to, uint256 amount)` | Dòng 18 - 20 | Chủ sở hữu (`onlyOwner`) có thể đúc token không giới hạn số lượng bất cứ lúc nào. Người nắm giữ đối mặt với nguy cơ token bị pha loãng giá trị nghiêm trọng hoặc chủ sở hữu tự đúc rồi xả hàng (dump/rug pull). |
| **ClubTokenC** | Rủi ro cao: Đóng băng tài khoản (Blacklist / Censor) | `setRestricted(address user, bool status)` | Dòng 30 - 32 | Chủ sở hữu (`onlyOwner`) có thể đơn phương đưa bất kỳ ví nào vào danh sách hạn chế (`restricted[user] = true`). Khi bị hạn chế, hàm `_update` (dòng 34 - 37) chặn chuyển tiền (`require(!restricted[from])`), làm người nắm giữ bị phong tỏa tài sản hoàn toàn và không thể bán/chuyển token. |

---

## Chi tiết phân tích từng hợp đồng

### 1. Hợp đồng `ClubTokenA` (Dòng 7 - 11)
- **Kế thừa:** `ERC20` (OpenZeppelin). Không kế thừa `Ownable`.
- **Phân quyền:** Không có cơ chế quản trị hay phân quyền chủ sở hữu sau hàm khởi tạo.
- **Hành vi:** Toàn bộ 1,000,000 CTA được cấp phát một lần duy nhất cho `msg.sender` trong `constructor` (dòng 9).
- **Kết luận:** An toàn về mặt đặc quyền can thiệp (không có backdoor/admin key).

### 2. Hợp đồng `ClubTokenB` (Dòng 13 - 21)
- **Kế thừa:** `ERC20, Ownable`.
- **Hàm đặc biệt:** `mint(address to, uint256 amount)` (dòng 18 - 20) với modifier `onlyOwner`.
- **Cơ chế rủi ro:** 
  - Không có ngưỡng trần tổng cung (`cap`).
  - Chủ sở hữu có toàn quyền gọi hàm `mint` để tạo ra lượng token tùy ý cho bất kỳ địa chỉ nào.
- **Hàm kế thừa từ `Ownable`:** `transferOwnership` và `renounceOwnership`.

### 3. Hợp đồng `ClubTokenC` (Dòng 23 - 38)
- **Kế thừa:** `ERC20, Ownable`.
- **Biến trạng thái:** `mapping(address => bool) public restricted;` (dòng 24).
- **Hàm đặc biệt:** `setRestricted(address user, bool status)` (dòng 30 - 32) với modifier `onlyOwner`.
- **Hàm can thiệp luồng chuyển token:** `_update(address from, address to, uint256 value)` (dòng 34 - 37).
- **Cơ chế rủi ro:** 
  - Dòng 35 có câu lệnh `require(!restricted[from], "Dia chi bi han che");`.
  - Nếu chủ sở hữu gọi `setRestricted(victim, true)`, địa chỉ `victim` sẽ vĩnh viễn không thể thực hiện giao dịch chuyển token (kể cả bán trên sàn DEX).
