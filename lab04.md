# BÁO CÁO THỰC HÀNH LAB 4: THẨM ĐỊNH RỦI RO HỢP ĐỒNG THÔNG MINH

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Khoa:** Hệ thống Thông tin Kinh tế — Trường Đại học Kinh tế, Đại học Huế  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Sinh viên thực hiện:** [Họ và tên sinh viên] — **Mã SV:** [MSSV]  
**Vị trí công việc mô phỏng:** Chuyên viên thẩm định rủi ro dự án tài sản số (Smart Contract Risk Analyst)  

---

## 1. BẢNG KẾT LUẬN THẨM ĐỊNH RỦI RO 3 HỢP ĐỒNG MẪU (A, B, C)

Theo quy định tại **Lab 4 (Bước 3) — Sổ tay Thực hành ECO2432**, kết luận thẩm định bắt buộc phải trích dẫn chính xác tên hàm và số dòng cụ thể làm bằng chứng:

| Hợp đồng | Kết luận | Tên hàm liên quan | Số dòng trích dẫn | Rủi ro cụ thể cho người nắm giữ token |
| :---: | :---: | :--- | :---: | :--- |
| **Hợp đồng A**<br>*(ClubTokenA)* | **Sạch**<br>*(Không rủi ro)* | `constructor()` | Dòng 7–9 | **Không có rủi ro bất thường.** Hợp đồng kế thừa ERC-20 thuần túy, đúc cố định 1.000.000 token trong hàm khởi tạo. Không có hàm `mint()` bổ sung, không có phân quyền quản trị (`Ownable`), không ai có thể can thiệp hay chặn chuyển token. Đây là hợp đồng đối chứng chuẩn. |
| **Hợp đồng B**<br>*(ClubTokenB)* | **Rủi ro Cao**<br>*(Pha loãng vô hạn)* | `mint(address to, uint256 amount)` | Dòng 14–16 | **Rủi ro pha loãng giá trị (Hyper-inflation / Rug-pull):** Hàm `mint` chỉ có ràng buộc `onlyOwner` mà **hoàn toàn không có giới hạn trần tổng cung (Cap)**. Chủ sở hữu có thể tự ý đúc thêm hàng tỷ token bất kỳ lúc nào để xả lên thị trường, làm bốc hơi toàn bộ giá trị tài sản của người nắm giữ. |
| **Hợp đồng C**<br>*(ClubTokenC)* | **Rủi ro Cực kỳ Cao**<br>*(Bẫy khóa thanh khoản)* | `setRestricted(address user, bool status)`<br>kết hợp `_update()` | Dòng 14–16<br>&<br>Dòng 18–21 | **Rủi ro bị giam vốn vĩnh viễn (Honeypot Scam):** Chủ sở hữu có quyền đơn phương đánh dấu bất kỳ ví nào vào danh sách `restricted[user] = true`. Tại hàm `_update`, lệnh `require(!restricted[from])` sẽ chặn chiều bán ra. Người dùng mua token vào được nhưng bị khóa vĩnh viễn không thể chuyển hoặc bán lại. Không có cơ chế khiếu nại, không có mốc thời hạn mở khóa, và không có sự kiện (`event`) công khai để theo dõi. |

---

## 2. ĐỐI CHIẾU PHƯƠNG PHÁP: ĐỌC THỦ CÔNG VS TRỢ LÝ AI

Theo quy trình bắt buộc tại Bước 1 và Bước 2 của Lab 4 (Sinh viên phải tự đọc thủ công 15 phút trước khi mở công cụ AI):

### 2.1. Đọc thủ công sinh viên tìm ra gì?
- **Ở Hợp đồng B:** Nhận thấy ngay hàm `mint` có modifier `onlyOwner` nhưng không hề kiểm tra `totalSupply() + amount` với bất kỳ con số tối đa nào. So sánh với các nguyên lý kinh tế học, đây là "máy in tiền" vô hạn.
- **Ở Hợp đồng C:** Phát hiện biến mapping `restricted` và điều kiện kiểm tra tại hàm `_update`. Nhận thấy đây chính là cấu trúc kinh điển của mã độc **Honeypot** thường thấy trong các vụ lừa đảo trên sàn phi tập trung (DEX): Cho phép mua (Buy) nhưng cấm bán (Sell).
- **Lỗ hổng quản trị:** Hợp đồng C không hề có cơ chế Time-lock, không có Multi-sig, và không phát ra sự kiện (`event`) khi đưa một ví vào danh sách đen.

### 2.2. Trợ lý AI tìm thêm được gì?
- AI chỉ ra rằng việc Hợp đồng B thiếu trần tổng cung vi phạm các chuẩn mực kiểm toán bảo mật của CertiK và OpenZeppelin.
- AI phân tích được chi phí gas: Hàm `_update` của Hợp đồng C tốn thêm khoảng $2.100$ gas cho mỗi giao dịch chuyển tiền thông thường do phải đọc biến `restricted[from]` từ Storage (Cold SLOAD).

### 2.3. Chỗ AI nói sai / Ảo giác (Hallucination) cần lưu ý
- Khi prompt không có câu lệnh cấm chặt chẽ (`Negative Constraint`), AI có xu hướng phỏng đoán tên hàm thành `freezeAccount()` hoặc `blacklist()` (do ảnh hưởng từ mã nguồn USDT/USDC) thay vì trích xuất đúng tên hàm thực tế là `setRestricted()` trong Hợp đồng C.
- AI ban đầu không trích dẫn chính xác số dòng mà chỉ đưa ra nhận xét chung chung: *"Hợp đồng B có thể mint thêm"*. Sinh viên phải áp dụng mẫu prompt tại Phụ lục II.1 để buộc AI trích dẫn số dòng cụ thể.

---

## 3. CÂU HỎI MỞ RỘNG VỀ QUẢN TRỊ KINH TẾ (PHỤ LỤC I)

> **Câu hỏi:** *"Nếu Hợp đồng C bổ sung sự kiện `event Restricted(address indexed user, bool status)` thì rủi ro có giảm đi không? Giảm ở điểm nào?"*

### Trả lời phân tích chuyên sâu:
1. **Về mặt kỹ thuật và quyền lực:** Rủi ro **KHÔNG HỀ GIẢM**. Chủ sở hữu hợp đồng vẫn nắm giữ toàn quyền khóa tài khoản của bất kỳ ai mà không cần xin phép hay thông báo trước. Tài sản của người dùng vẫn có thể bị đóng băng bất cứ lúc nào.
2. **Về mặt giám sát thị trường (Market Surveillance):** Rủi ro được **GIẢM THIỂU Ở KHẢ NĂNG PHÁT HIỆN VÀ GIÁM SÁT**:
   - Khi có sự kiện `event`, mọi thao tác khóa ví sẽ được ghi công khai vĩnh viễn vào nhật ký giao dịch (Transaction Logs/Receipts) của khối.
   - Các hệ thống cảnh báo tự động của sàn giao dịch (như Chainalysis, Etherscan Watchlist, GoPlus Security) có thể phát hiện ngay lập tức hành vi bất thường khi chủ dự án bắt đầu khóa ví của các nhà đầu tư lớn.
   - **Bài học quản trị:** *Minh bạch hóa (Transparency) không loại bỏ được sự lạm quyền của người quản trị, nhưng nó làm cho hành vi lạm quyền bị cả cộng đồng nhìn thấy.*

---

## 4. BẢNG TỔNG HỢP CÁC HÀM KIỂM THỬ TRÊN HỢP ĐỒNG MYTOKEN.SOL (LAB 4 - NÂNG CAO)

Nhằm khắc phục toàn bộ các lỗ hổng đã chỉ ra ở Hợp đồng B và C, sinh viên đã xây dựng và kiểm thử thành công hợp đồng chuẩn sản xuất [contracts/MyToken.sol](file:///c:/SmartContractLab/contracts/MyToken.sol) với 17 ca kiểm thử tự động tại [test/MyToken.test.js](file:///c:/SmartContractLab/test/MyToken.test.js):

| Nhóm chức năng | Tên ca kiểm thử (Test Case) | Điều kiện kiểm tra & Kỳ vọng | Kết quả |
| :--- | :--- | :--- | :---: |
| **1. Khởi tạo hợp đồng (Deployment)** | `Phải thiết lập đúng tên và ký hiệu` | Tên là `"MyToken"`, ký hiệu `"MTK"`, `decimals = 18`. | ✅ Đạt |
| | `Chủ sở hữu phải là người deploy` | `owner() == deployer.address`. | ✅ Đạt |
| | `Đúc đủ 1.000.000 MTK ban đầu` | Số dư Owner = $1.000.000 \times 10^{18}$ wei. | ✅ Đạt |
| | `Hằng số MAX_SUPPLY là 10.000.000 MTK` | Kiểm tra trần tổng cung chống lạm phát vô hạn. | ✅ Đạt |
| **2. Chuyển tiền & Đốt (Transfer & Burn)** | `Chuyển token thành công` | Chuyển 500 MTK, phát ra sự kiện `Transfer`. | ✅ Đạt |
| | `Chặn chuyển tiền nếu thiếu số dư` | Revert với Custom Error `ERC20InsufficientBalance`. | ✅ Đạt |
| | `Đốt token (Burn) giảm tổng cung` | Đốt 10.000 MTK, tổng cung và số dư giảm tương ứng. | ✅ Đạt |
| **3. Phân quyền Đúc (Mint & Supply Cap)** | `Owner đúc thêm token hợp lệ` | Đúc 20.000 MTK, phát ra sự kiện `TokensMinted`. | ✅ Đạt |
| | `Người ngoài KHÔNG ĐƯỢC phép mint` | Revert với lỗi `OwnableUnauthorizedAccount`. | ✅ Đạt |
| | `Chặn đúc vượt trần MAX_SUPPLY` | Đúc quá 10.000.000 MTK $\rightarrow$ Revert với `MaxSupplyExceeded`. | ✅ Đạt |
| | `Chặn đúc tới địa chỉ rỗng address(0)` | Revert với lỗi `ZeroAddressNotAllowed`. | ✅ Đạt |
| **4. Cơ chế Đóng băng (Blacklist/Freeze)** | `Owner đóng băng ví thành công` | Đặt `isFrozen = true`, phát sự kiện `AccountFrozen`. | ✅ Đạt |
| | `Người ngoài KHÔNG THỂ đóng băng ví` | Revert với lỗi `OwnableUnauthorizedAccount`. | ✅ Đạt |
| | `Không thể tự đóng băng ví Owner` | Revert với lỗi `CannotFreezeOwner` (chống Deadlock). | ✅ Đạt |
| | `Ví bị đóng băng KHÔNG THỂ chuyển đi` | Revert với `AccountIsFrozen` khi gọi `transfer()`. | ✅ Đạt |
| | `Ví khác KHÔNG THỂ chuyển vào ví bị đóng băng` | Revert với `AccountIsFrozen` khi nhận token. | ✅ Đạt |
| | `Owner mở khóa (Unfreeze) tài khoản` | Mở khóa thành công, tài khoản giao dịch bình thường. | ✅ Đạt |

---

## 5. KẾT LUẬN VÀ BÀI HỌC THỰC TIỄN

1. **Quy tắc vàng khi thẩm định dự án tài sản số:** Không bao giờ tin tưởng vào lời hứa trong Whitepaper hay giao diện website bóng bẩy. Chuyên viên thẩm định chỉ tin vào những gì được viết rõ ràng trong mã nguồn đã xác thực (`Verified Source Code`) trên Etherscan.
2. **Nhận thức về rủi ro tập trung:** Mọi quyền lực quản trị (`onlyOwner`, `admin`, `blacklist`, `mint`) đều là một "con dao hai lưỡi". Đối với tổ chức phát hành (như Tether/Circle), đó là công cụ tuân thủ pháp lý; nhưng đối với nhà đầu tư cá nhân, đó là rủi ro đối tác (Counterparty Risk) bắt buộc phải được chiết khấu vào định giá tài sản.
3. **Tiêu chuẩn thiết kế an toàn:** Một hợp đồng token an toàn cho cộng đồng bắt buộc phải có: (1) Trần tổng cung bất biến (`MAX_SUPPLY`), (2) Cơ chế phát sự kiện minh bạch cho mọi thao tác quản trị, và (3) Sử dụng các thư viện đã được kiểm toán toàn cầu như OpenZeppelin v5.
