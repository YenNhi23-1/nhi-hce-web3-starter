# BÁO CÁO LAB 07: TÍNH CHI PHÍ VẬN HÀNH THỰC TẾ TRÊN BLOCKCHAIN

> **Học phần:** ECO2432 - Công nghệ Web3 và Tài sản số  
> **Chủ đề:** Phân tích hiệu quả kinh tế và tính khả thi mô hình tích điểm CLB sinh viên  

---

## 1. Công thức và thông số tính toán

- **Công thức tính phí 1 giao dịch:**
  $$\text{Phí (ETH)} = \text{Lượng gas} \times \text{Đơn giá gas (Gwei)} \times 10^{-9}$$
  $$\text{Phí (USD)} = \text{Phí (ETH)} \times \text{Giá ETH (USD)}$$
- **Thông số đầu vào (Bài toán CLB):**
  - Số lượng giao dịch hàng tháng: **1.000 lượt** cộng điểm.
  - Lượng gas tham khảo cho 1 thao tác ghi/cộng điểm (chuẩn ERC-20 / ghi biến trạng thái): **~50.000 gas**.
  - Đơn giá gas trên Ethereum (Layer 1): **20 Gwei**.
  - Tỷ giá ETH: **3.000 USD / ETH**.
  - Hệ số tiết kiệm của mạng Layer 2: Rẻ hơn **100 lần**.

---

## 2. Bảng tính chi phí vận hành (Bài toán Bước 2)

| Chỉ tiêu tính toán | Mạng Ethereum (Layer 1) | Mạng Layer 2 (Arbitrum / Optimism / Base) | Chênh lệch |
| :--- | :---: | :---: | :---: |
| **Lượng gas / giao dịch** | 50.000 gas | 50.000 gas | Không đổi |
| **Đơn giá gas tương đương** | 20 Gwei | 0.2 Gwei | Rẻ hơn 100 lần |
| **Phí cho 1 giao dịch (ETH)** | 0,001 ETH | 0,00001 ETH | Giảm 99% |
| **Phí cho 1 giao dịch (USD)** | **3,00 USD** | **0,03 USD** | Giảm 99% |
| **Phí quy đổi VND / giao dịch** *(tỷ giá 25.000đ)* | ~75.000 VNĐ | ~750 VNĐ | Giảm từ 75k xuống 750đ |
| **Tổng chi phí 1 tháng (1.000 lượt - USD)** | **3.000 USD** | **30 USD** | Tiết kiệm 2.970 USD/tháng |
| **Tổng chi phí 1 tháng (VND)** | **~75.000.000 VNĐ** | **~750.000 VNĐ** | Phù hợp quỹ sinh viên |

---

## 3. Trả lời chi tiết các câu hỏi bài toán

### a. Chi phí một tháng trên Ethereum (Layer 1) là bao nhiêu USD?
- Phí 1 lượt cộng điểm: $50.000 \times 20 \times 10^{-9} \times 3.000 = 3,00\text{ USD}$.
- Tổng chi phí 1 tháng cho 1.000 lượt: 
  $$1.000 \times 3,00\text{ USD} = \mathbf{3.000\text{ USD/tháng}}\quad (\approx 75.000.000\text{ VNĐ/tháng})$$

### b. Chi phí trên mạng Layer 2 là bao nhiêu?
- Nhờ giải pháp Rollup (Layer 2 gom nhiều giao dịch đưa về L1), chi phí rẻ hơn 100 lần:
  $$\text{Chi phí L2} = \frac{3.000\text{ USD}}{100} = \mathbf{30\text{ USD/tháng}}\quad (\approx 750.000\text{ VNĐ/tháng})$$
  *(Mỗi lượt tích điểm chỉ tốn khoảng **0,03 USD** $\approx$ 750 VNĐ).*

### c. Ai trả khoản phí này — Câu lạc bộ hay sinh viên? Sinh viên có chấp nhận không?
- **Nếu sinh viên trả:** 
  - Trên **Layer 1**: Hoàn toàn **không chấp nhận**. Mỗi lần được cộng 1 điểm thưởng mà phải tự trả 75.000 VNĐ tiền gas (trong khi một ly trà sữa chỉ 25.000 – 30.000 VNĐ) là phi lý về mặt kinh tế học hành vi.
  - Trên **Layer 2**: Phí 750 VNĐ/giao dịch tuy thấp nhưng đòi hỏi sinh viên phải có ví Web3 chứa sẵn ETH trên L2 để trả gas, tạo ra rào cản gia nhập (friction) quá lớn.
- **Giải pháp tối ưu:** **Câu lạc bộ (đơn vị phát hành) nên là bên chi trả**.
  - Áp dụng cơ chế **Gas Sponsorship / Paymaster (ERC-4337 - Account Abstraction)**: CLB nạp trước 30 USD/tháng vào hợp đồng Paymaster để bảo trợ toàn bộ phí gas cho sinh viên. Sinh viên chỉ cần quét mã QR/ký giao dịch mà không tốn phí và không cần sở hữu ETH.

### d. Kết luận tính khả thi
- **Trên Ethereum Layer 1:** **HOÀN TOÀN BẤT KHẢ THI**. Chi phí vận hành (75 triệu VNĐ/tháng) vượt xa ngân sách của bất kỳ CLB sinh viên nào, và chi phí giao dịch lớn hơn nhiều lần giá trị kinh tế của điểm thưởng.
- **Trên Layer 2:** **HOÀN TOÀN KHẢ THI**. Mức ngân sách ~750.000 VNĐ/tháng nằm trọn vẹn trong hạn mức tài trợ hoạt động của CLB hoặc trích từ quỹ hội viên.

---

## 4. Mở rộng: Phân tích tính khả thi đồ án nhóm (Bước 3)

### Ý tưởng đồ án: "Hệ sinh thái Chứng chỉ & Điểm rèn luyện số (SBT/NFT) cho trường Đại học"

- **Quy mô dự kiến:**
  - 5.000 sinh viên toàn trường.
  - Mỗi học kỳ có 10.000 lượt cấp chứng nhận hoạt động ngoại khóa/điểm danh sự kiện (trung bình ~2.000 lượt/tháng).
  - Thao tác: Đúc token Soulbound Token (SBT) ghi nhận thành tích (tiêu thụ ~80.000 gas/lượt).

- **Bảng so sánh chi phí đồ án:**

| Hạng mục | Ethereum L1 | Layer 2 (Base / Arbitrum) | Đánh giá |
| :--- | :---: | :---: | :--- |
| Gas tiêu thụ / chứng chỉ | 80.000 gas | 80.000 gas | Chuẩn ERC-721/SBT mint |
| Chi phí 1 chứng chỉ | 4,80 USD (~120.000đ) | 0,048 USD (~1.200đ) | L2 rẻ hơn 100 lần |
| Chi phí 1 tháng (2.000 lượt) | **9.600 USD (~240 triệu đ)** | **96 USD (~2,4 triệu đ)** | Chênh lệch 100 lần |
| Khả năng chi trả của Nhà trường | Không khả thi | Khả thi cao | 2,4 tr/tháng rẻ hơn chi phí in phôi giấy |

- **Kết luận đồ án:** 
  Đồ án chỉ có mô hình kinh tế bền vững khi triển khai trên **Layer 2 (hoặc Layer 3 / Appchain)**, kết hợp hợp đồng thông minh tài trợ gas để Nhà trường chi trả tập trung, mang lại trải nghiệm hoàn toàn miễn phí cho sinh viên.
