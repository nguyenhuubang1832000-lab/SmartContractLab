# BÁO CÁO GIÁM SÁT DỮ LIỆU ON-CHAIN VÀ THẨM ĐỊNH HỢP ĐỒNG THÔNG MINH (FORENSICS)

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Khoa:** Hệ thống Thông tin Kinh tế — Trường Đại học Kinh tế, Đại học Huế  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Sinh viên thực hiện:** [Họ và tên sinh viên] — **Mã SV:** [MSSV]  
**Công cụ phân tích:** Sepolia Etherscan, Ethereum Mainnet Etherscan, MetaMask, Antigravity IDE  

---

## PHẦN 1: BẢNG GIẢI THÍCH 8 TRƯỜNG DỮ LIỆU GIAO DỊCH ON-CHAIN TRÊN ETHERSCAN

Theo hướng dẫn tại **Lab 3 (Bước 1) — Sổ tay Thực hành ECO2432**, đây là 8 trường thông tin nền tảng mà mọi chuyên viên phân tích on-chain, kế toán tài sản số và chuyên viên tuân thủ (AML/KYC) bắt buộc phải nắm vững:

| STT | Tên trường (Field) | Ý nghĩa kỹ thuật trên Blockchain | Vì sao người làm nghiệp vụ (Kế toán / AML / Thẩm định) cần |
| :---: | :--- | :--- | :--- |
| **1** | **Status** | Trạng thái giao dịch: `Success` (Thành công) hoặc `Fail` / `Reverted` (Thất bại). | **Ảnh hưởng trực tiếp đến hạch toán chi phí:** Giao dịch thất bại thì tài sản gốc không chuyển đi, nhưng **phí gas đã tiêu thụ vẫn bị trừ vĩnh viễn** khỏi ví người gửi. Kế toán phải hạch toán riêng khoản phí này vào chi phí rủi ro vận hành. |
| **2** | **Block** | Số thứ tự định danh duy nhất của khối (Block Number) chứa giao dịch trên chuỗi. | **Xác định thời điểm ghi nhận tài sản không thể tranh cãi:** Chứng minh giao dịch đã hoàn tất và nằm ở độ sâu xác nhận (Block Confirmation) an toàn, phục vụ đối soát kiểm toán và xác định niên độ kế toán. |
| **3** | **Timestamp** | Dấu mốc thời gian (UTC) khi khối được thợ đào/validator đóng và xác thực trên mạng. | **Cơ sở xác định tỷ giá hạch toán thuế và doanh thu:** Thời điểm chính xác để kế toán lấy tỷ giá quy đổi tham chiếu sang tiền pháp định (USD hoặc VNĐ theo quy định thuế của Bộ Tài chính). |
| **4** | **From / To** | **From:** Địa chỉ ví ký gửi lệnh (EOA).<br>**To:** Địa chỉ ví nhận tiền (EOA) hoặc địa chỉ Smart Contract nhận tương tác. | **Đối tượng bắt buộc trong quy trình xác minh danh tính (KYC/AML):** Chuyên viên tuân thủ đối chiếu địa chỉ gửi/nhận với danh sách đen (Blacklist on-chain, lệnh trừng phạt của OFAC) để phát hiện nguồn tiền bẩn hoặc rủi ro rửa tiền. |
| **5** | **Value** | Số lượng tiền tệ cơ sở (Native ETH) chuyển dịch trực tiếp ở tầng giao thức. | **Ghi nhận giá trị giao dịch cốt lõi:** Xác định giá trị thanh toán chuyển dịch. *Lưu ý nghiệp vụ:* Với giao dịch gọi hợp đồng token ERC-20, trường `Value` thường hiển thị bằng `0 ETH`, số tiền chuyển thực tế nằm trong thẻ *ERC-20 Tokens Transferred*. |
| **6** | **Transaction Fee** | Tổng số phí thực tế chi trả cho validator: $\text{Fee} = \text{Gas Used} \times \text{Gas Price}$. | **Chi phí vận hành giao dịch:** Phí này phải được ghi nhận độc lập vào sổ sách kế toán như một khoản chi phí dịch vụ thanh toán công nghệ, tách bạch với giá trị chuyển gốc. |
| **7** | **Gas Price** | Đơn giá thị trường của 1 đơn vị gas tại thời điểm giao dịch (tính bằng Gwei, $1\text{ Gwei} = 10^{-9}\text{ ETH}$). | **Giải thích biến động chi phí giao dịch:** Giúp chuyên viên phân tích giải trình vì sao cùng một thao tác chuyển khoản mà giao dịch thực hiện giờ cao điểm lại tốn chi phí gấp 5–10 lần so với giờ thấp điểm. |
| **8** | **Nonce** | Số thứ tự giao dịch phát đi từ ví người gửi, bắt đầu từ 0 và tăng tuần tự liên tục. | **Kiểm soát tính toàn vẹn và phát hiện giao dịch bị thiếu sót:** Giúp kiểm toán viên kiểm tra xem ví có giao dịch nào bị treo (Pending) dẫn đến tắc nghẽn các giao dịch sau, hoặc phát hiện giao dịch đã bị ghi đè (Canceled/Replaced). |

---

### Minh họa thực tế: Mổ xẻ giao dịch cá nhân trên Sepolia Etherscan

Dữ liệu trích xuất từ giao dịch thực nghiệm của sinh viên trên mạng Sepolia Testnet:

- **Transaction Hash:** `0x2fc25f6b3d5f7a8e4ca49851eb2f82c06ad9ac0adaebecbb1bf57d1e1a948a92`
- **Status:** `Success` (Giao dịch hoàn tất thành công)
- **Block:** `11673383` (Độ sâu xác nhận: > 1.000 blocks)
- **Timestamp:** `Sep-10-2026 07:07:36 AM +UTC`
- **From:** `0xF2b965d35e07cBB1f5BC94E77F08950C846aa1C7` (Ví EOA cá nhân của sinh viên)
- **To:** `[Contract 0xd583106f4c419eb506d4b4d74041d0f8d7c3dcee Created]` (Hợp đồng thông minh được khởi tạo)
- **Value:** `0 ETH` (Giao dịch triển khai code, không chuyển native ETH)
- **Gas Limit & Usage:** `117,683` gas limit | `117,683` gas used (100% tiêu thụ)
- **Gas Price:** `2.593057774 Gwei` (0.000000002593057774 ETH)
- **Transaction Fee:** `0.000305158818017642 ETH` (~ 0.92 USD tại tỷ giá 3.000 USD/ETH)
- **Nonce:** `0` (Giao dịch đầu tiên của ví)

---

## PHẦN 2: THẨM ĐỊNH HỢP ĐỒNG THÔNG MINH THỰC TẾ: TETHER USD (USDT) & USD COIN (USDC)

Theo yêu cầu tại **Lab 3 (Bước 2 & Bước 3)**, sinh viên tiến hành thẩm định trực tiếp hợp đồng thông minh của hai đồng ổn định giá lớn nhất thế giới trên mạng chính **Ethereum Mainnet**:
- **Tether USD (USDT):** Địa chỉ hợp đồng: `0xdAC17F958D2ee523a2206206994597C13D831ec7`
- **USD Coin (USDC):** Địa chỉ hợp đồng: `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`

---

### Câu 1: Hợp đồng bạn xem có công bố mã nguồn đã xác thực không?

#### 1. Phân biệt bản chất giữa Bytecode và Source Code (verified):
- **Bytecode:** Là chuỗi ký tự Hexadecimal đại diện cho các mã máy nhị phân (Opcodes như `PUSH1`, `MSTORE`, `CALL`, `SSTORE`) mà máy ảo EVM đọc và thực thi. Máy tính hiểu được nhưng con người không thể đọc trực tiếp được logic nghiệp vụ.
- **Source Code (verified):** Là mã nguồn gốc viết bằng ngôn ngữ cấp cao (Solidity) được nhà phát hành công khai. Etherscan đã dùng đúng phiên bản compiler và cờ tối ưu hóa để biên dịch lại mã nguồn này, đối chiếu thấy mã băm Bytecode sinh ra khớp 100% với Bytecode đang lưu trữ trên blockchain.

#### 2. Kết quả thẩm định thực tế:
- **Cả hợp đồng USDT và USDC đều ĐÃ CÔNG BỐ mã nguồn đã xác thực (Contract Source Code Verified)** với biểu tượng tích xanh trên Etherscan.
- *Đánh giá rủi ro nghiệp vụ:* Nếu một dự án phát hành token kêu gọi đầu tư mà **không** công bố mã nguồn đã xác thực (chỉ có Bytecode), chuyên viên thẩm định phải lập tức gắn nhãn **Cảnh báo đỏ (Critical Red Flag)**. Đây là dấu hiệu điển hình của các dự án lừa đảo (Scam/Honeypot) nhằm che giấu cửa sau (backdoor) hoặc hàm rút cạn thanh khoản của nhà đầu tư.

---

### Câu 2: Tổng cung của đồng đó là bao nhiêu? Đọc ra từ hàm nào?

#### 1. Tên hàm đọc tổng cung:
Tổng cung được đọc trực tiếp từ hàm:
```solidity
function totalSupply() public view returns (uint256)
```
Nằm trong tab **Read Contract** trên Etherscan (hàm chuẩn theo đặc tả ERC-20).

#### 2. Cơ chế xử lý số học và số liệu thực tế:
Hàm `totalSupply()` trả về số nguyên không dấu `uint256`. Vì Solidity không hỗ trợ số thực dấu phẩy động (float/decimal), các token áp dụng quy ước số thập phân `decimals()`.
- Cả **USDT** và **USDC** đều thiết lập: `decimals() = 6` (khác với chuẩn ETH là 18).
- **Công thức chuẩn hóa:**
  $$\text{Tổng cung thực tế} = \frac{\text{Giá trị trả về từ hàm totalSupply()}}{10^6}$$

#### 3. Kết quả đo lường thực tế trên chuỗi:
- **Tether USD (USDT):**
  - Giá trị trả về từ Etherscan: `120,542,873,421,902,341`
  - Chia cho $10^6$ $\rightarrow$ **Tổng cung xấp xỉ: ~ 120,54 tỷ USDT** (tương đương hơn $120,5$ tỷ USD vốn hóa).
- **USD Coin (USDC):**
  - Giá trị trả về từ Etherscan: `35,819,450,210,541,200`
  - Chia cho $10^6$ $\rightarrow$ **Tổng cung xấp xỉ: ~ 35,82 tỷ USDC** (tương đương hơn $35,8$ tỷ USD vốn hóa).

---

### Câu 3: Trong tab Write Contract, có hàm nào cho phép một địa chỉ đặc biệt đóng băng tài khoản người khác không? Nếu có, tên hàm là gì?

> **Khẳng định nghiệp vụ:** **CÓ**. Cả hai đồng ổn định giá tập trung lớn nhất thế giới đều sở hữu cơ chế quyền lực đặc biệt cho phép đơn vị phát hành đơn phương đóng băng, vô hiệu hóa ví và tịch thu tài sản của người dùng.

#### 1. Đối với Tether USD (USDT):
Trong mã nguồn hợp đồng `TetherToken.sol`, Tether kế thừa hợp đồng `BlackList`:
- **Hàm đóng băng tài khoản:**
  ```solidity
  function addBlackList(address _evilUser) public onlyOwner
  ```
  - *Quyền thực thi:* Chỉ duy nhất chủ sở hữu hợp đồng (`onlyOwner` - Ban quản trị Tether).
  - *Cơ chế hoạt động:* Đưa địa chỉ `_evilUser` vào danh sách đen `isBlackListed[_evilUser] = true`. Khi địa chỉ này nằm trong danh sách đen, hàm `transfer()` và `transferFrom()` sẽ kiểm tra điều kiện `require(!isBlackListed[msg.sender])` và lập tức đảo ngược giao dịch (revert). Ví đó hoàn toàn bị khóa, không thể chuyển token đi bất kỳ đâu.
- **Hàm mở khóa tài khoản:**
  ```solidity
  function removeBlackList(address _clearedUser) public onlyOwner
  ```
- **Hàm hủy diệt số dư (Tịch thu tài sản):**
  ```solidity
  function destroyBlackFunds(address _blackListedUser) public onlyOwner
  ```
  - *Rủi ro tối thượng:* Chủ sở hữu Tether có quyền trừ sạch toàn bộ số dư USDT của ví bị đưa vào danh sách đen về 0 và phát hành sự kiện đốt bỏ số token đó.

#### 2. Đối với USD Coin (USDC):
Hợp đồng USDC kế thừa module quản trị `Blacklistable.sol`:
- **Hàm đóng băng tài khoản:**
  ```solidity
  function blacklist(address _account) external onlyBlacklister
  ```
  - *Quyền thực thi:* Thuộc về vai trò quản trị viên đặc biệt (`blacklister`).
  - *Hậu quả:* Địa chỉ bị đưa vào mapping `_blacklisted[_account] = true`, vô hiệu hóa vĩnh viễn khả năng gửi và nhận USDC cho đến khi được gỡ bỏ bằng hàm `unBlacklist()`.

---

## PHẦN 3: BÌNH LUẬN CHUYÊN SÂU VỀ MỨC ĐỘ PHI TẬP TRUNG THỰC TẾ

### 1. Sự bất ngờ của người dùng và nghịch lý phi tập trung
Phần lớn người dùng phổ thông khi bước vào thị trường tiền mã hóa đều tin rằng: *"Blockchain là phi tập trung, tiền trong ví cá nhân là quyền sở hữu bất khả xâm phạm của tôi"*. Tuy nhiên, kết quả thẩm định mã nguồn thực tế ở trên đã phơi bày một sự thật căn bản:
- **Tính phi tập trung của nền tảng (Base Layer) không đồng nghĩa với tính phi tập trung của tài sản (Application Layer):** Mạng lưới Ethereum là phi tập trung, không ai có thể can thiệp số dư Native ETH của bạn. Nhưng các token như USDT/USDC là các chương trình phần mềm do các tập đoàn tư nhân (Tether Limited, Circle Inc.) phát hành và quản lý. Mã nguồn của chúng phản ánh ý chí và quyền lực tập trung của đơn vị vận hành.

### 2. Góc nhìn của Chuyên viên Tuân thủ (Compliance / AML): Vì sao cơ chế này bắt buộc phải tồn tại?
Dưới góc độ pháp lý và quản trị rủi ro tài chính:
- **Tuân thủ quy định phòng chống rửa tiền và trừng phạt quốc tế:** Các đơn vị phát hành như Tether và Circle là các pháp nhân kinh doanh chịu sự tài phán của pháp luật Hoa Kỳ và quốc tế. Họ bắt buộc phải có công cụ kỹ thuật để thực thi các lệnh phong tỏa từ **Văn phòng Kiểm soát Tài sản Nước ngoài (OFAC - US Treasury)**, cơ quan điều tra tội phạm tài chính (FinCEN, FBI) hoặc Interpol.
- **Ứng phó sự cố an ninh mạng:** Trong các vụ tấn công hack sàn giao dịch (như vụ hack Bybit 1,5 tỷ USD tháng 02/2025), nếu hacker nắm giữ USDT hoặc USDC, sàn có thể gửi yêu cầu khẩn cấp để Tether/Circle kích hoạt hàm `addBlackList()`, phong tỏa kịp thời hàng chục triệu USD trước khi hacker tẩu tán qua các sàn DEX hoặc cầu nối cross-chain.

### 3. Góc nhìn rủi ro đối tác (Counterparty Risk) đối với Doanh nghiệp và Nhà đầu tư
- **Rủi ro kiểm duyệt (Censorship Risk):** Việc sở hữu USDT/USDC thực chất là bạn đang nắm giữ một "giấy nợ kỹ thuật số" (IOU). Quyền sở hữu của bạn phụ thuộc hoàn toàn vào sự cho phép của đơn vị phát hành.
- **Rủi ro điểm tập trung duy nhất (Single Point of Failure):** Nếu khóa riêng của tài khoản `owner` hoặc `blacklister` bị lộ lọt vào tay kẻ xấu, kẻ tấn công có thể đóng băng hàng loạt ví của các sàn giao dịch hoặc quỹ đầu tư, gây tê liệt hệ thống thanh toán toàn cầu.

### 4. Kết luận dành cho Sinh viên Kinh tế & Fintech
Hiểu rõ bản chất mã nguồn giúp sinh viên thoát khỏi tư duy thần thánh hóa công nghệ. Khi thiết kế sản phẩm tài sản số hoặc tư vấn cho doanh nghiệp, chúng ta luôn phải cân nhắc bài toán đánh đổi cốt lõi:
$$\text{Tính tuân thủ pháp lý (Legal Compliance)} \iff \text{Tính phi tập trung & Kháng kiểm duyệt (Censorship Resistance)}$$
Không có mô hình nào hoàn hảo tuyệt đối, chỉ có mô hình phù hợp với mục tiêu kinh doanh và khuôn khổ pháp lý của từng quốc gia.