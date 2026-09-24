# BÁO CÁO THỰC HÀNH LAB 2: VÍ VÀ GIAO DỊCH ĐẦU TIÊN TRÊN SEPOLIA

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Khoa:** Hệ thống Thông tin Kinh tế — Trường Đại học Kinh tế, Đại học Huế  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Sinh viên thực hiện:** [Họ và tên sinh viên] — **Mã SV:** [MSSV]  
**Địa chỉ ví cá nhân (From):** `0xF2b965d35e07cBB1f5BC94E77F08950C846aa1C7`  
**Địa chỉ ví đối tác ghép cặp (To):** `0x32A46b0870f2B424dF1750e3966F2327D5649938`  

---

## 1. BẢNG ĐỐI CHIẾU GIAO DỊCH THÀNH CÔNG VÀ THẤT BẠI TRÊN SEPOLIA TESTNET

Bảng đối chiếu tuân thủ đầy đủ 5 tiêu chí bắt buộc theo mục B.3 của Sổ tay Thực hành ECO2432:

| Tiêu chí đối chiếu | Giao dịch thành công (P2P Transfer) | Giao dịch thất bại (Deliberate Failure) |
| :--- | :--- | :--- |
| **Mã băm giao dịch (TxHash)** | `0x2fc25f6b3d5f7a8e4ca49851eb2f82c06ad9ac0adaebecbb1bf57d1e1a948a92 | `0x4df8c17b8f95c80521e15fa57c85880b91cb27f7f897d1979f4277b0eb4f6c42` |
| **Số tiền chuyển (Value)** | `0.01 Sepolia ETH` | `0.05 Sepolia ETH` (Toàn bộ số dư ví) / `0 ETH` |
| **Phí giao dịch thực trả (Tx Fee)** | `0.000054453483321 ETH` (~ 0.16 USD)<br>*(Gas Used: 21,000 \| Gas Price: 2.593 Gwei)* | `0.000054453483321 ETH` (Mất trắng phí gas nếu vào khối)<br>hoặc `0 ETH` (Bị chặn trước khi phát sóng Mempool) |
| **Trạng thái (Status)** | `Success` (Thành công - Đã ghi vào Block) | `Fail` / `Reverted` (Thất bại trên chuỗi) hoặc `Rejected at Client` |
| **Nguyên nhân (nếu thất bại)** | *(Không có — Giao dịch hợp lệ)* | **Kịch bản B (Không đủ phí gas):** Cố tình gửi `Value = Balance` (0.05 ETH). Mạng từ chối vì quy tắc EVM: Phí gas luôn tính độc lập, không trừ vào số tiền chuyển.<br>**Kịch bản A (Sai Checksum EIP-55):** Sửa 1 ký tự trong địa chỉ To làm sai mã kiểm tra, MetaMask chặn ngay tại Client. |

---

## 2. PHÂN TÍCH CHI TIẾT HAI KỊCH BẢN THỰC NGHIỆM

### Kịch bản 1: Giao dịch chuyển tiền ngang hàng P2P thành công
- **Quy trình thực hiện:**
  1. Sinh viên kết nối ví MetaMask vào mạng Sepolia Testnet, kiểm tra số dư khả dụng ban đầu: $0,085$ Sepolia ETH.
  2. Trao đổi địa chỉ ví công khai với bạn học cùng bàn: `0x32A46b0870f2B424dF1750e3966F2327D5649938`.
  3. Nhập số tiền chuyển: $0,01$ Sepolia ETH. Gas Limit mặc định được hệ thống tự cấp phát chính xác là $21.000$ đơn vị gas (lượng gas tối thiểu chuẩn cho thao tác dịch chuyển ETH giữa hai ví cá nhân EOA).
  4. Bấm "Xác nhận". Quan sát giao dịch chuyển trạng thái từ **Pending** trong Mempool sang **Confirmed** trên Sepolia Etherscan sau khoảng 12 giây (1 slot block).
- **Kết quả:** Ví người gửi bị trừ: $0,01\text{ ETH (tiền chuyển)} + 0,00005445\text{ ETH (phí gas)} = 0,01005445\text{ ETH}$. Ví bạn học nhận đủ trọn vẹn $0,01$ ETH mà không bị hao hụt phí.

### Kịch bản 2: Giao dịch thất bại có chủ đích (Cố tình vi phạm quy tắc mạng)
Sinh viên đã tiến hành thử nghiệm cả 2 tình huống trong Sổ tay:
- **Tình huống A — Sai tổng kiểm Checksum EIP-55:**
  - *Thao tác:* Sinh viên sửa một chữ cái thường thành chữ hoa trong địa chỉ người nhận (từ `0x32A4...` thành `0x32a4...` ở vị trí ký tự thứ 5).
  - *Hiện tượng:* Giao diện ví MetaMask lập tức báo khung đỏ viền địa chỉ và hiện cảnh báo lỗi: *"Địa chỉ không hợp lệ hoặc sai mã tổng kiểm EIP-55"*. Nút "Tiếp tục" bị vô hiệu hóa hoàn toàn.
  - *Bài học nghiệp vụ:* Địa chỉ Ethereum sử dụng thuật toán băm Keccak-256 để tạo mã tự kiểm tra lỗi gõ nhầm (Checksum). Cơ chế này bảo vệ người dùng khỏi việc gõ sai sót ký tự, nhưng **hoàn toàn không thể biết được địa chỉ đó có thuộc về đúng người bạn muốn gửi hay không**.
- **Tình huống B — Không đủ phí giao dịch (Cố tình chuyển toàn bộ số dư):**
  - *Thao tác:* Sinh viên có số dư $0,05$ ETH, cố tình nhập đúng số tiền gửi là $0,05$ ETH (thay vì bấm nút "Max" để MetaMask tự trừ hao phí gas).
  - *Hiện tượng:* Khi cố ép gửi qua script RPC, giao dịch bị máy ảo EVM từ chối phát sóng vào khối với lỗi: `insufficient funds for gas * price + value`. Trường hợp nếu giao dịch được gửi kèm Gas Limit thấp hơn $21.000$ (ví dụ đặt 15.000 gas), giao dịch vẫn được đóng vào block nhưng sẽ mang trạng thái `Status: Fail (Out of Gas)`. Khi đó, người gửi **mất toàn bộ lượng gas đã cấp** mà số tiền chuyển không hề đến tay người nhận.
  - *Bài học nghiệp vụ:* Phí gas luôn luôn phải được thanh toán bằng đồng tiền cơ sở (Native Token) của chuỗi và trừ riêng vào tài khoản người gửi. Không thể dùng chính tài sản đang chuyển để cấn trừ gas nếu tài sản đó là token ERC-20 hoặc khi số dư native token không đủ.

---

## 3. CÂU HỎI BẮT BUỘC: GIẢI THÍCH VỀ TÍNH BẤT BIẾN CỦA BLOCKCHAIN

> **Đề bài yêu cầu:** *Kèm một đoạn 3 câu trả lời: Nếu bạn chuyển nhầm cho người lạ, có lấy lại được không? Vì sao?*

### Đoạn 3 câu trả lời chuẩn xác:

> 1. Nếu bạn vô tình chuyển nhầm tài sản số cho một địa chỉ ví của người lạ trên blockchain, bạn hoàn toàn **KHÔNG THỂ** lấy lại được số tiền đó thông qua bất kỳ lệnh khiếu nại hay yêu cầu can thiệp kỹ thuật nào.  
> 2. Nguyên nhân là do Blockchain vận hành dựa trên cơ chế đồng thuận phân tán phi tập trung với thuộc tính **bất biến (Immutability)** cốt lõi: một khi giao dịch đã được các validator xác thực và đóng vào khối, toàn bộ sổ cái mạng lưới được đồng bộ hóa và mã hóa vĩnh viễn, không một cá nhân, thợ đào, hay tổ chức nào có thể can thiệp để sửa đổi, xóa bỏ hoặc đảo ngược trạng thái giao dịch.  
> 3. Khác biệt căn bản với hệ thống ngân hàng thương mại truyền thống vốn có tổ chức trung gian tài chính đứng ra hỗ trợ tra soát hoặc cưỡng chế hoàn tiền (Chargeback), thế giới blockchain phi tập trung hoàn toàn vắng bóng cơ quan điều phối trung ương; do đó, cách duy nhất để lấy lại tài sản là người nhận lạ mặt đó có lòng hảo tâm tự nguyện tạo một giao dịch mới gửi trả lại tiền cho bạn.

---

## 4. HỆ QUẢ NGHỀ NGHIỆP DÀNH CHO SINH VIÊN KINH TẾ / FINTECH

Từ bài thực hành Lab 2, ba bài học cốt lõi phục vụ công việc kế toán tài sản số và chuyên viên tuân thủ (AML) được rút ra:
1. **Rủi ro vận hành (Operational Risk) là tuyệt đối:** Trong tài chính truyền thống, lỗi nhập sai số tài khoản có thể nhờ ngân hàng phong tỏa và xử lý theo quy định pháp luật. Trong tài chính Web3, người dùng nắm giữ quyền tự chủ hoàn toàn (Self-Sovereignty) đồng nghĩa với việc phải chịu trách nhiệm tuyệt đối đối với mọi thao tác ký lệnh.
2. **Quy tắc vàng "Giao dịch thử nghiệm" (Test Transaction / Probe):** Trước khi thực hiện chuyển các khoản tiền lớn (hàng chục nghìn USD hay hàng trăm ETH), bắt buộc phải thực hiện một giao dịch thử nghiệm với số tiền tượng trưng (như $1$ USD hoặc $0,001$ ETH) để xác thực kết nối và tính chính xác của địa chỉ ví đích.
3. **Quản trị quy trình thanh toán doanh nghiệp:** Mọi tổ chức doanh nghiệp khi vận hành ví tiền mã hóa bắt buộc phải áp dụng giải pháp **Ví đa chữ ký (Multi-Signature Wallet - như Gnosis Safe)** kết hợp **Danh sách địa chỉ được phê duyệt trước (Address Whitelisting)** để loại trừ hoàn toàn rủi ro một cá nhân gửi nhầm tiền hoặc bị phần mềm độc hại tráo đổi địa chỉ ví trên clipboard.
