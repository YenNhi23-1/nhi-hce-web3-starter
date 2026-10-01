# 🎓 ECO2432 — Web3 & Smart Contract Starter

> **Trường Đại học Kinh tế — Đại học Huế (HCE)**  
> **Khoa:** Hệ thống Thông tin Kinh tế   
> **Học phần:** ECO2432 — Tiền điện tử & hợp đồng thông minh  
> **Giảng viên hướng dẫn:** TS. Hà Ngọc Long  
> **Sinh viên thực hiện:** Phan Thị Yến Nhi  
> **Mã số sinh viên (MSSV):** `23K4300014`  
> **Repository:** (https://github.com/YenNhi23-1/nhi-hce-web3-starter)

---

## 📌 Giới thiệu chung

Kho lưu trữ **`nhi-hce-web3-starter`** được sử dụng xuyên suốt 15 bài thực hành trong học phần Tiền điện tử & hợp đồng thông minh. Dự án cung cấp môi trường lập trình, biên dịch, kiểm thử hợp đồng thông minh trên nền tảng máy ảo Ethereum (EVM) và thực hành điều tra dòng tiền On-chain (Blockchain Forensics).

### 🛠️ Bộ công cụ & Môi trường sử dụng
- **IDE:** Antigravity IDE / VS Code
- **Cầu nối cục bộ:** Daemon `remixd` kết nối trực tiếp mã nguồn máy với Remix Web IDE
- **Nền tảng kiểm thử & triển khai:** Remix VM (Cancun) và Ethereum Sepolia Testnet
- **Ví Web3:** MetaMask (chỉ dùng tài khoản thử nghiệm Testnet)

---

## 📂 Cấu trúc thư mục dự án

```text
nhi-hce-web3-starter/
├── contracts/                     # Toàn bộ mã nguồn Smart Contract (.sol)
│   ├── 01_Basics/                 # Hợp đồng nhập môn (HelloWorld, Counter, SimpleStorage)
│   ├── lab04/                     # Lab 4: Hợp đồng ba token ClubTokens.sol
│   └── training/                  # Hợp đồng mẫu Lab 9, 10, 11, 13 (TimeLockVault, VaultBuggy,...)
├── web/                           # Giao diện Web3 Frontend
│   └── index.html                 # Giao diện mẫu cho Lab 15
├── scripts/                       # Các kịch bản phân tích dữ liệu on-chain (Python / JS / PowerShell)
├── trade.md / trace.md            # Báo cáo điều tra Lab 3B: Follow the Money (vụ hack Bybit 401.346 ETH)
├── prompt_templates.md            # Khung mẫu câu lệnh AI Prompt Engineering
├── GEMINI.md                      # Quy chuẩn lập trình sạch (Clean Code) & Tối ưu Gas
├── HUONG-DAN.md                   # Sổ tay hướng dẫn chi tiết từng bài thực hành
├── package.json                   # Cấu hình Node.js & gói công cụ remixd
└── README.md                      # Tài liệu tổng quan dự án
