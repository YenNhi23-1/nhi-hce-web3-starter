# Lab 02 — Ví và Giao Dịch Đầu Tiên

## Bảng đối chiếu

| Trường | Giao dịch thành công | Giao dịch thất bại |
|---|---|---|
| Mã băm giao dịch | 0x9f9833db97b1e35a789c6b8eb77f2f71f8f2043db397569e711672ddd10286c3b | Không có (bị từ chối trước khi gửi) |
| Số tiền chuyển | 0.01 SepoliaETH | 0.01 SepoliaETH (dự định) |
| Phí giao dịch thực trả | 0.000054547984092 ETH | 0 ETH |
| Trạng thái | ✅ Success — Đã xác nhận | ❌ Thất bại — Bị từ chối |
| Nguyên nhân (nếu thất bại) | — | Địa chỉ không hợp lệ: sửa một ký tự trong địa chỉ người nhận khiến checksum sai, MetaMask từ chối ngay lập tức |

## Câu hỏi: Nếu chuyển nhầm cho người lạ, có lấy lại được không? Vì sao?

Không thể lấy lại được nếu chuyển nhầm cho người lạ trên blockchain.
Lý do là vì mọi giao dịch trên blockchain đều là bất biến — một khi đã được xác nhận vào khối, không ai có thể xóa hay hoàn tác, kể cả người gửi.
Cách duy nhất để lấy lại là nhờ người nhận tự nguyện gửi trả, nhưng blockchain không có cơ chế bắt buộc điều đó.
