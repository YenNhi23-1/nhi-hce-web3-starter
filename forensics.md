

### Bảng 10 trường giao dịch chi tiết

| STT | Tên trường | Giá trị trong giao dịch (Lab 2) | Ý nghĩa kỹ thuật | Vì sao người làm nghiệp vụ (Tuân thủ / Kế toán on-chain) cần? |
|:---:|:---|:---|:---|:---|
| 1 | **Status** | **Success** (Thành công ✅) | Trạng thái thực thi giao dịch trên EVM (thành công hoặc thất bại / revert). | Giao dịch thất bại **vẫn bị trừ phí gas**. Kế toán bắt buộc phải ghi nhận chi phí này dù việc chuyển tiền không thành công. |
| 2 | **Block** | *Số block chứa giao dịch* | Số thứ tự của khối trên chuỗi chứa giao dịch này và số xác nhận (confirmations). | Xác định chính xác thời điểm giao dịch được đóng băng vĩnh viễn trên sổ cái blockchain. |
| 3 | **Timestamp** | *Thời điểm xác nhận khối* | Dấu thời gian (UTC) lúc khối được các validator xác thực và đóng gói. | Cung cấp mốc thời gian để kế toán ghi nhận doanh thu, chi phí hoặc quy đổi tỷ giá hối đoái tại thời điểm phát sinh. |
| 4 | **From / To** | • **From:** *Địa chỉ ví người gửi*<br>• **To:** *Địa chỉ ví người nhận* | Địa chỉ ví khởi tạo giao dịch (`From`) và ví tiếp nhận hoặc hợp đồng thông minh (`To`). | Nghiệp vụ KYC/AML và truy vết danh tính. Giúp xác minh nguồn gốc tiền và đối chiếu với danh sách các thực thể bị trừng phạt. |
| 5 | **Value** | **0.01 SepoliaETH** | Lượng native cryptocurrency (ETH) thực sự được chuyển giao giữa hai địa chỉ. | Giá trị tài sản thanh toán, làm căn cứ định khoản tài sản số. |
| 6 | **Transaction Fee** | **0.000054547984092 ETH** | Tổng chi phí mạng lưới thực tế mà người gửi phải chi trả cho validator. | Chi phí hoạt động nghiệp vụ (OpEx) cần được hạch toán riêng rẽ với giá trị chuyển tiền. |
| 7 | **Gas Price** | **~2.5975 Gwei** *(0.00000000259752 ETH)* | Đơn giá cho mỗi đơn vị gas mà người gửi chấp nhận chi trả tại thời điểm giao dịch. | Giải thích nguyên nhân biến động chi phí: cùng một lệnh chuyển tiền nhưng lúc mạng nghẽn thì đơn giá gas tăng cao. |
| 8 | **Gas Limit** | **21,000** *(Mức tiêu chuẩn cho chuyển ETH)* | Số đơn vị gas tối đa mà ví người gửi cấp phép cho máy ảo EVM sử dụng. | Quản trị rủi ro: Nếu đặt quá thấp, giao dịch thất bại vì lỗi `Out of Gas` nhưng vẫn mất sạch phí; nếu đặt quá cao có thể mất nhiều tiền khi contract gặp lỗi. |
| 9 | **Gas Used** | **21,000 (100%)** | Lượng gas thực tế mà EVM đã tiêu thụ để xử lý xong giao dịch. | Công thức: `Transaction Fee = Gas Used × Gas Price`. Khi một giao dịch bị lỗi mà `Gas Used == Gas Limit`, đó là dấu hiệu cạn kiệt gas (`Out of Gas`). |
| 10 | **Nonce** | *Số thứ tự giao dịch (VD: 0 hoặc 1)* | Số đếm giao dịch tăng tuần tự của ví người gửi (bắt đầu từ 0). | Chống tấn công phát lại (Replay Attack), giúp phát hiện giao dịch bị bỏ sót hoặc thay thế giao dịch đang bị nghẽn (speed up/cancel). |

---

## Bước 2 & 3: Đọc hợp đồng thật trên Ethereum Mainnet

Bài thực hành phân tích trực tiếp hợp đồng **USDT (Tether USD)** và đối chiếu với **USDC (USD Coin)**:

| Thông số | USDT (Tether USD) | USDC (USD Coin) |
|:---|:---|:---|
| **Contract Address** | `0xdAC17F958D2ee523a2206206994597C13D831ec7` | `0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48` |
| **Kiểu kiến trúc** | Direct Contract (Hợp đồng nguyên khối) | Transparent Proxy (chuẩn EIP-1967) |

---

### Trả lời chi tiết 3 câu hỏi của Lab 3:

### Câu 1: Hợp đồng bạn xem có công bố mã nguồn đã xác thực không?
- **Với USDT (`0xdAC17F958D2ee523a2206206994597C13D831ec7`):**
  - **Có.** Mã nguồn của hợp đồng đã được công bố và xác thực hoàn toàn trên Etherscan (*Contract Source Code Verified - Exact Match*).
  - Tên hợp đồng là `TetherToken`, được viết bằng Solidity phiên bản `v0.4.18`.
- **Với USDC (`0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48`):**
  - **Có.** Cả hợp đồng Proxy (`FiatTokenProxy`) và hợp đồng logic thực thi (`FiatTokenV2_2`) đều được công bố và xác thực mã nguồn đầy đủ trên Etherscan.
- **Ý nghĩa:** Việc xác thực mã nguồn (verified) giúp người dùng và chuyên viên đối chiếu 1:1 mã nguồn Solidity con người đọc được với Bytecode máy thực thi. Nếu hợp đồng chưa được xác thực, việc tương tác tiềm ẩn rủi ro lừa đảo rất lớn (honeypot, backdoor).

---

### Câu 2: Tổng cung của đồng đó là bao nhiêu? Đọc ra từ hàm nào?
- **Đọc từ hàm:** Hàm **`totalSupply()`** trong tab **Read Contract** (với USDT) hoặc tab **Read as Proxy** (với USDC).
- **Đặc điểm:** Đây là hàm view/read-only, hoàn toàn **không tốn phí gas** và **không cần kết nối ví** để truy vấn.
- **Quy đổi tổng cung thực tế:**
  - Cả USDT và USDC đều quy định độ dài phần thập phân là **6 decimals** (`decimals = 6`).
  - Giá trị nguyên thô do hàm `totalSupply()` trả về là một chuỗi số rất lớn (Ví dụ: `73,450,000,000,000,000`).
  - Quy đổi ra giá trị token thực tế:
    $$\text{Tổng cung} = \frac{\text{Giá trị trả về}}{10^6} \approx 73.450.000.000 \text{ USDT}$$
  - Tổng cung USDT lưu hành trên Ethereum Mainnet hiện tại đạt khoảng **~73,45 tỷ USD**.

---

### Câu 3: Trong tab Write Contract (với USDC: Write as Proxy), có hàm nào cho phép một địa chỉ đặc biệt đóng băng tài khoản người khác không? Nếu có, tên hàm là gì?

- **Trả lời: CÓ.** Cả hai đồng ổn định giá lớn nhất thế giới đều tích hợp sẵn cơ chế đóng băng (Blacklist/Freeze) tài khoản người dùng:

#### 1. Đối với USDT (Tether) — Tab *Write Contract*:
- **Tên hàm đóng băng:** **`addBlackList(address _evilUser)`**
- **Hàm gỡ đóng băng:** **`removeBlackList(address _clearedUser)`**
- **Hàm tiêu hủy tiền bị đóng băng:** **`destroyBlackFunds(address _blackListedUser)`**
- **Cơ chế hoạt động:** 
  - Chỉ có địa chỉ ví của chủ sở hữu hợp đồng (`onlyOwner` - công ty Tether) mới có quyền thực thi.
  - Khi địa chỉ ví bị gán `isBlackListed[_maker] = true`, toàn bộ các lệnh gọi hàm `transfer()` và `transferFrom()` liên quan tới ví đó sẽ lập tức bị hủy bỏ (`throw`/revert).

#### 2. Đối với USDC (Circle) — Tab *Write as Proxy*:
- **Tên hàm đóng băng:** **`blacklist(address _account)`**
- **Hàm gỡ đóng băng:** **`unBlacklist(address _account)`**
- **Cơ chế hoạt động:**
  - Chỉ có địa chỉ được cấp quyền đặc biệt `blacklister` (`onlyBlacklister` - do Circle quản lý) mới được phép gọi hàm này.
  - Sau khi bị đưa vào danh sách đen, biến `_blacklisted[account]` chuyển thành `true`, khiến tài khoản bị tê liệt hoàn toàn mọi thao tác nạp, rút hay phê duyệt thanh toán.

---

### Mở rộng thảo luận: Mức độ phi tập trung thực tế (Decentralization vs Compliance)

Điểm then chốt ở Câu 3 mở ra một góc nhìn quan trọng về bản chất của tiền mã hóa:
1. **Tính chất tập trung của Stablecoin pháp định:** USDT và USDC là các đồng tiền neo giá USD do các công ty pháp nhân ngoài đời thực phát hành (Tether Ltd. và Circle). Do đó, họ bắt buộc phải tuân theo các quy định phòng chống rửa tiền (AML/CFT) và lệnh đóng băng tài sản từ các cơ quan như OFAC (Bộ Tài chính Mỹ) hay lệnh của tòa án quốc tế.
2. **Sự đánh đổi:**
   - **Mặt tích cực:** Có thể can thiệp khẩn cấp để đóng băng tài sản do hacker hoặc kẻ lừa đảo đánh cắp, bảo vệ người dùng và thị trường tài chính.
   - **Mặt hạn chế:** Trái ngược với bản chất phi tập trung tuyệt đối của Bitcoin hoặc native ETH (không một ai có quyền đóng băng hay cấm đoán ví cá nhân), USDT và USDC tồn tại **rủi ro kiểm duyệt (censorship risk)** vì quyền lực tối cao nằm trong tay tổ chức phát hành nắm giữ private key quản trị.
