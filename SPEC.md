# BẢN ĐẶC TẢ YÊU CẦU NGHIỆP VỤ (SPECIFICATION) — LAB 1 ĐẾN LAB 3

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Đơn vị đào tạo:** Trường Đại học Kinh tế — Khoa Hệ thống Thông tin Kinh tế, Đại học Huế  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Quy chuẩn áp dụng:** Cấu trúc đặc tả 6 phần chuẩn theo Sổ tay Thực hành (Lab Manual ECO2432)  

---

## MỤC LỤC TỔNG HỢP
1. [QUY ƯỚC CHUNG VÀ TẦM QUAN TRỌNG CỦA ĐẶC TẢ](#quy-ước-chung)
2. [LAB 1: ĐẶC TẢ THIẾT LẬP MÔI TRƯỜNG PHÁT TRIỂN & QUẢN TRỊ DANH TÍNH ON-CHAIN](#lab-1)
3. [LAB 2: ĐẶC TẢ VÍ KHÔNG LƯU KÝ & CƠ CHẾ GIAO DỊCH P2P TRÊN ETHEREUM](#lab-2)
4. [LAB 3: ĐẶC TẢ GIÁM SÁT DỮ LIỆU ON-CHAIN & THẨM ĐỊNH HỢP ĐỒNG THÔNG MINH](#lab-3)

---

<a name="quy-ước-chung"></a>
## QUY ƯỚC CHUNG VÀ NGUYÊN TẮC THIẾT KẾ ĐẶC TẢ

Theo chỉ dẫn tại **Phần B — Sổ tay Thực hành ECO2432**, bản đặc tả `SPEC.md` là sản phẩm bắt buộc phải được hoàn thiện **trước khi gõ bất kỳ câu lệnh prompt nào cho công cụ AI**.
- **Không có `SPEC.md`, bài Lab không được nghiệm thu**, kể cả khi mã nguồn chạy đúng.
- Bản đặc tả là ranh giới pháp lý và logic kỹ thuật giữa người làm phân tích nghiệp vụ (BA) / chuyên viên thẩm định với đội ngũ phát triển (hoặc trợ lý AI).
- Mọi quy tắc nghiệp vụ trong bản đặc tả phải là các câu khẳng định kiểm chứng được (verifiable assertions).
- Đáp ứng chuẩn mực đánh giá **Mức 3 (Giỏi)**: Phân tích đầy đủ các trường hợp ngoại lệ, bẫy rủi ro bảo mật và bổ sung tối thiểu 2 quy tắc nghiệp vụ thực tiễn ngoài đề bài.

---

<a name="lab-1"></a>
## LAB 1: ĐẶC TẢ THIẾT LẬP MÔI TRƯỜNG PHÁT TRIỂN & QUẢN TRỊ DANH TÍNH ON-CHAIN

```
Tên bài: Chuẩn bị môi trường làm việc và Quản trị danh tính số
Hình thức: Cá nhân · Thời lượng: 75 phút
Mục tiêu nghề nghiệp: Chuẩn bị hạ tầng sạch cho chuyên viên phân tích on-chain, kế toán tài sản số và chuyên viên tuân thủ AML.
```

### 1. Mục đích
Hệ thống hóa quy trình thiết lập môi trường lập trình Web3 có AI hỗ trợ (Google Antigravity / Gemini Code Assist), khởi tạo ví không lưu ký (MetaMask) trên mạng thử nghiệm Sepolia, thiết lập kho lưu trữ mã nguồn GitHub cá nhân tuân thủ tuyệt đối quy ước bảo mật `AGENTS.md` nhằm bảo vệ khóa bí mật và quản trị an toàn danh tính số cho sinh viên ngành Fintech/MIS/TMĐT.

### 2. Đầu vào
- **Tài khoản định danh:** Tài khoản Google sinh viên dùng để kích hoạt công cụ trợ lý AI Antigravity.
- **Tiện ích mở rộng:** Trình cài đặt tiện ích ví MetaMask (phiên bản v11 trở lên) trên nền tảng Chrome/Edge.
- **Mã nguồn cơ sở:** Kho mã nguồn mẫu `hce-web3-starter` được cung cấp từ giảng viên trên GitHub.
- **Nguồn cấp tài nguyên:** Địa chỉ vòi cấp phát công khai Google Cloud Web3 Faucet Sepolia hoặc địa chỉ ví kho bạc (Treasury Wallet) của giảng viên.
- **Bảng dữ liệu định danh lớp học:** Bảng tính Google Sheet trực tuyến do giảng viên cung cấp để quản lý ánh xạ: `[Họ và tên] <-> [Mã SV] <-> [Địa chỉ ví công khai Sepolia]`.

### 3. Quy tắc nghiệp vụ (Business Rules)
- **R1 (Bảo mật cụm từ khôi phục):** Chuỗi 12 từ khôi phục (Secret Recovery Phrase / Mnemonic) bắt buộc phải được ghi chép thủ công bằng tay lên giấy vật lý và cất giữ ngoại tuyến. Nghiêm cấm tuyệt đối mọi hành vi lưu trữ số: không chụp ảnh màn hình, không lưu trữ trên ghi chú đám mây, không gửi qua email/tin nhắn mạng xã hội.
- **R2 (Nguyên tắc phân định môi trường):** Cấu hình ví MetaMask phải bật chế độ mạng thử nghiệm (`Show test networks`) và chuyển mạng hoạt động mặc định sang **Sepolia Testnet**. Tuyệt đối không sử dụng ví chứa tài sản thật (Mainnet) cho các tác vụ lập trình hay tương tác thử nghiệm.
- **R3 (Quy ước ràng buộc AI thường trực):** Tệp `AGENTS.md` bắt buộc phải đặt tại thư mục gốc của repository, chứa đầy đủ các chỉ dẫn ràng buộc phiên bản (Solidity `^0.8.20`, OpenZeppelin v5.x) và quy chuẩn viết mã an toàn (Checks-Effects-Interactions, Custom Errors, cấm `tx.origin`). Mọi thành viên phải thêm ít nhất 01 quy tắc cá nhân vào cuối tệp.
- **R4 (Ngăn chặn rò rỉ khóa bí mật - Private Key):** Kho lưu trữ Git bắt buộc phải kích hoạt tệp `.gitignore` để loại trừ triệt để tệp `.env`, thư mục `node_modules/`, `artifacts/`, `cache/`. Nghiêm cấm đưa bất kỳ Private Key hoặc API Key nào lên kho mã nguồn công khai (GitHub public repo).
- **R5 (Định danh công khai duy nhất - Bổ sung ngoài đề bài):** Mỗi sinh viên chỉ được phép liên kết một địa chỉ ví công khai duy nhất cho toàn bộ quá trình học tập. Địa chỉ ví nộp lên bảng tính lớp học phải tuân theo định dạng chuẩn EIP-55 (có phân biệt chữ hoa/chữ thường để kiểm tra checksum).
- **R6 (Chuẩn hóa thông điệp kiểm soát phiên bản - Bổ sung ngoài đề bài):** Bản ghi thay đổi đầu tiên (First Commit) phải tuân thủ chuẩn Conventional Commits với cấu trúc: `chore: thiet lap moi truong lam viec` nhằm phục vụ việc kiểm toán lịch sử phát triển dự án.

### 4. Đầu ra
- Môi trường Antigravity IDE hoạt động trôi chảy, nhận diện và thực thi phản hồi prompt kiểm tra ban đầu về bản chất khác biệt giữa Blockchain và Cơ sở dữ liệu truyền thống.
- Ví MetaMask đã kích hoạt trên mạng Sepolia, sở hữu số dư thử nghiệm khả dụng từ $0,05$ đến $0,5$ Sepolia ETH.
- Bản sao chép (Fork) kho mã nguồn cá nhân trên GitHub có chứa tệp `AGENTS.md` đã được cá nhân hóa và được lưu vết qua Commit Hash công khai.
- Thông tin định danh ví của sinh viên được ghi nhận thành công trên Bảng tính lớp học mà không bị trùng lặp với bất kỳ sinh viên nào khác.

### 5. Trường hợp ngoại lệ (Edge Cases & Exception Handling)
- **E1 (Cổng cấp phát Faucet từ chối):** Nếu cổng cấp phát Google Cloud Faucet chặn yêu cầu do ví mới không có số dư trên Ethereum Mainnet (cơ chế chống spam Sybil), hệ thống chuyển sang quy trình dự phòng: Sinh viên gửi địa chỉ ví công khai cho Giảng viên để nhận ETH phân phối trực tiếp từ Ví kho bạc.
- **E2 (Nguy cơ rò rỉ Private Key do commit nhầm `.env`):** Nếu sinh viên lỡ thêm tệp `.env` vào trạng thái theo dõi của Git (`git status` báo staged), sinh viên phải lập tức thực thi `git reset HEAD .env`, kiểm tra lại cú pháp `.gitignore` và không được thực hiện `git push` cho đến khi tệp nhạy cảm đã bị loại bỏ hoàn toàn.
- **E3 (Thiết bị không tương thích Antigravity):** Trường hợp máy tính cá nhân cấu hình thấp không thể khởi chạy Antigravity IDE, giải pháp dự phòng tiêu chuẩn được kích hoạt: Chuyển sang sử dụng Remix Ethereum IDE trên nền web kết hợp trợ lý Gemini Code Assist trong VS Code hoặc giao diện Gemini Web.
- **E4 (Trùng lặp địa chỉ ví giữa các sinh viên):** Nếu hệ thống đối soát bảng tính phát hiện hai sinh viên có cùng một địa chỉ ví, hệ thống đánh dấu vi phạm liêm chính học thuật và yêu cầu tạo lại ví mới độc lập từ đầu dưới sự giám sát của giảng viên.

### 6. Ngoài phạm vi (Out of Scope)
- Không triển khai các hợp đồng thông minh phức tạp hoặc viết mã Solidity trong buổi này.
- Không sử dụng mạng chính (Ethereum Mainnet) và không giao dịch bằng tiền thật.
- Không cấu hình các node RPC cục bộ tự vận hành (Geth/Besu) hay các mạng Layer 2.

---

<a name="lab-2"></a>
## LAB 2: ĐẶC TẢ VÍ KHÔNG LƯU KÝ & CƠ CHẾ GIAO DỊCH P2P TRÊN ETHEREUM

```
Tên bài: Ví và Giao dịch đầu tiên
Hình thức: Cặp đôi · Thời lượng: 75 phút
Mục tiêu nghề nghiệp: Trang bị kiến thức mổ xẻ cấu trúc giao dịch on-chain, cơ chế tính phí gas của EVM và hiểu sâu sắc nguyên nhân thất bại của giao dịch phục vụ nghiệp vụ kế toán và đối soát thanh toán.
```

### 1. Mục đích
Mô tả chi tiết quy trình thực thi một giao dịch chuyển tài sản số (Sepolia ETH) ngang hàng (P2P) thành công giữa hai thực thể độc lập và thực hiện các kịch bản cố tình gây lỗi giao dịch có chủ đích nhằm kiểm chứng cơ chế vận hành của mã kiểm tra địa chỉ EIP-55 và nguyên lý trích xuất phí gas độc lập của máy ảo Ethereum (EVM).

### 2. Đầu vào
- **Địa chỉ ví gửi (From):** Ví EOA của sinh viên thực hiện lệnh chuyển, số dư khả dụng tối thiểu $> 0,02$ Sepolia ETH.
- **Địa chỉ ví nhận (To):** Ví EOA của bạn học cùng lớp, dạng chuỗi Hex 42 ký tự bắt đầu bằng tiền tố `0x`.
- **Số lượng chuyển (Value):** Cố định $0,01$ Sepolia ETH cho phiên giao dịch thành công chuẩn.
- **Thông số phí mạng (Gas parameters):** Gas Limit tiêu chuẩn cho chuyển ETH đơn thuần ($21.000$ gas units), Max Fee per Gas và Priority Fee (Tip) do thị trường Sepolia quy định tại thời điểm gửi.
- **Dữ liệu kịch bản lỗi có chủ đích:**
  - *Kịch bản A (Sai địa chỉ nhận):* Địa chỉ To bị thay đổi 01 ký tự bất kỳ làm sai lệch tổng kiểm checksum.
  - *Kịch bản B (Không đủ phí gas):* Cố tình đặt lệnh chuyển toàn bộ số dư hiện có (`Value = Balance`), không chừa lại ETH để trả phí gas.

### 3. Quy tắc nghiệp vụ (Business Rules)
- **R1 (Điều kiện tiên quyết về khả năng thanh toán):** Một giao dịch chỉ đủ điều kiện phát tán lên mạng nếu số dư ví gửi thỏa mãn: $\text{Số dư} \ge \text{Số tiền chuyển} + (\text{Gas Limit} \times \text{Max Fee per Gas})$.
- **R2 (Cơ chế khấu trừ gas fee độc lập):** Phí gas giao dịch luôn luôn được trả bằng đồng tiền cơ sở của mạng lưới (Native ETH) và bị trừ riêng vào số dư của ví gửi. Tuyệt đối không được trừ phí gas vào số tiền người nhận được hưởng (`Value` chuyển đi bao nhiêu thì người nhận nhận đủ bấy nhiêu).
- **R3 (Tính tuần tự của chỉ số Nonce):** Mỗi giao dịch gửi đi thành công từ ví sẽ làm tăng chỉ số Nonce của ví đó lên đúng 1 đơn vị theo cấp số cộng. Không một giao dịch nào có thể được xử lý nếu tồn tại khoảng trống Nonce (Nonce Gap).
- **R4 (Kiểm tra mã tổng kiểm EIP-55):** Giao diện ví MetaMask phải tự động thẩm tra địa chỉ nhận theo chuẩn EIP-55. Nếu địa chỉ bị sai lệch cấu trúc viết hoa/viết thường tương ứng với mã băm Keccak-256, giao diện phải lập tức chặn hành động gửi tiền của người dùng.
- **R5 (Quy tắc xác nhận trạng thái bất biến):** Giao dịch chuyển từ trạng thái `Pending` trong Mempool sang `Confirmed` khi được đóng vào block hợp lệ. Khi đã có ít nhất 1 xác nhận trên block, kết quả giao dịch mang tính bất biến và không có bất kỳ cơ quan, cá nhân nào có thể đảo ngược hay hủy bỏ.
- **R6 (Bổ sung ngoài đề bài - Xử lý thất bại tầng EVM):** Trong trường hợp giao dịch đã được phát tán vào block nhưng bị Revert/Thất bại do hết gas (Out of Gas), toàn bộ lượng gas đã tiêu hao vẫn bị trừ vĩnh viễn khỏi ví người gửi để trả công cho Validator; số tiền gốc `Value` được hoàn lại ví người gửi.
- **R7 (Bổ sung ngoài đề bài - Kiểm soát trượt phí Gas EIP-1559):** Hệ thống giao dịch phải tuân thủ chuẩn EIP-1559, gồm Base Fee (bị đốt cháy - Burned) và Priority Fee (thưởng cho Validator). Khi mạng biến động cao, người dùng phải biết cách điều chỉnh Priority Fee để tránh giao dịch bị treo.

### 4. Đầu ra
- Mã băm giao dịch (Transaction Hash) của giao dịch thành công chuyển $0,01$ ETH, hiển thị trạng thái `Success` trên Sepolia Etherscan.
- Bằng chứng thực nghiệm giao dịch thất bại (thông báo lỗi từ MetaMask đối với Kịch bản A hoặc Transaction Hash trạng thái `Fail` / Lỗi Mempool đối với Kịch bản B).
- Tệp tài liệu `lab02.md` hoàn chỉnh gồm bảng đối chiếu 5 trường dữ liệu giữa 2 giao dịch và đoạn văn 3 câu giải thích bản chất tính bất biến của công nghệ Blockchain.

### 5. Trường hợp ngoại lệ (Edge Cases & Exception Handling)
- **E1 (Người dùng cố tình bấm nút "Max Balance"):** MetaMask tính toán tự động trừ hao phí gas ước tính. Nếu sinh viên can thiệp thô buộc gửi toàn bộ số dư không chừa gas, MetaMask lập tức khóa nút bấm và cảnh báo "Insufficient funds for gas * price + value".
- **E2 (Chuyển tiền vào địa chỉ hợp đồng không có hàm nhận):** Nếu người dùng gửi nhầm ETH vào một địa chỉ Smart Contract mà contract đó không khai báo hàm `receive()` hoặc `fallback() payable`, giao dịch sẽ bị từ chối ngay lập tức và EVM hoàn lại tiền (revert), người gửi mất toàn bộ phí gas đã tiêu thụ.
- **E3 (Giao dịch bị nghẽn do mạng đột ngột tăng Gas Price):** Nếu giao dịch nằm ở trạng thái `Pending` quá 5 phút, sinh viên phải thực hiện thao tác "Speed Up" trên MetaMask (tạo giao dịch thay thế có cùng Nonce nhưng tăng phí gas lên tối thiểu 10%).

### 6. Ngoài phạm vi (Out of Scope)
- Không thực hiện hoàn tiền giao dịch đã được xác nhận (vì Blockchain không có cơ chế Chargeback).
- Không thực hiện chuyển các loại token chuẩn ERC-20, ERC-721 trong bài thực hành này.
- Không can thiệp sửa đổi dữ liệu khối của các node trên mạng lưới.

---

<a name="lab-3"></a>
## LAB 3: ĐẶC TẢ GIÁM SÁT DỮ LIỆU ON-CHAIN & THẨM ĐỊNH HỢP ĐỒNG THÔNG MINH

```
Tên bài: Đọc giao dịch và Hợp đồng trên Etherscan
Hình thức: Cá nhân · Thời lượng: 75 phút
Mục tiêu nghề nghiệp: Chuẩn bị năng lực nền tảng cho chuyên viên phân tích dữ liệu on-chain, chuyên viên thẩm định rủi ro dự án số và kiểm toán viên tuân thủ AML/CFT.
```

### 1. Mục đích
Đặc tả phương pháp giải mã và phân tích toàn diện 8 trường dữ liệu cốt lõi của một giao dịch on-chain trên trình duyệt khối Etherscan; đồng thời thiết lập quy trình nghiệp vụ thẩm định hợp đồng thông minh thực tế (Smart Contract Due Diligence) đối với các đồng ổn định giá hàng đầu thị trường (USDT/USDC) nhằm nhận diện cơ chế quản trị tập trung và chức năng đóng băng tài khoản.

### 2. Đầu vào
- **Mã băm giao dịch (TxHash):** Mã băm giao dịch cá nhân thực hiện từ Lab 2 (hoặc giao dịch triển khai hợp đồng thông minh trên mạng Sepolia).
- **Trình duyệt khối:** Trình khám phá mạng thử nghiệm `https://sepolia.etherscan.io` và mạng chính `https://etherscan.io`.
- **Hợp đồng thông minh mục tiêu thẩm định:**
  - Hợp đồng Tether USD (USDT) trên Ethereum Mainnet: `0xdAC17F958D2ee523a2206206994597C13D831ec7`.
  - Hợp đồng USD Coin (USDC) trên Ethereum Mainnet: `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`.
- **Giao diện phân tích Etherscan:** Các phân vùng: Transaction Overview, Contract Tab (Bytecode vs Verified Code), Read Contract Tab, Write Contract Tab.

### 3. Quy tắc nghiệp vụ (Business Rules)
- **R1 (Quy chuẩn phân tích 8 trường dữ liệu on-chain):** Báo cáo phân tích bắt buộc phải giải thích chi tiết ý nghĩa kỹ thuật và giá trị nghiệp vụ kế toán/tuân thủ cho 8 trường thông tin:
  1. `Status`: Trạng thái thành công hay thất bại (giao dịch thất bại vẫn phải hạch toán chi phí gas tiêu hao).
  2. `Block`: Thứ tự khối (xác định niên độ kế toán và thời điểm ghi nhận tài sản không thể tranh cãi).
  3. `Timestamp`: Dấu thời gian Unix (mốc xác định tỷ giá hạch toán doanh thu/chi phí theo luật thuế).
  4. `From / To`: Địa chỉ ví gửi và ví nhận (cơ sở rà soát danh tính KYC và đối soát danh sách trừng phạt).
  5. `Value`: Số lượng tài sản cơ sở chuyển dịch trực tiếp ở tầng giao thức.
  6. `Transaction Fee`: Phí giao dịch thực tế chi trả ($\text{Gas Used} \times \text{Gas Price}$), hạch toán vào chi phí tài chính.
  7. `Gas Price`: Đơn giá thị trường của mỗi đơn vị gas (Gwei), phản ánh mức độ ưu tiên và tình trạng tắc nghẽn mạng.
  8. `Nonce`: Chỉ số thứ tự giao dịch của ví gửi, phục vụ phát hiện giao dịch bị thiếu sót hoặc thay thế có chủ đích.
- **R2 (Phân biệt cấp độ mã nguồn hợp đồng):**
  - *Bytecode:* Chuỗi hex máy thực thi opcodes, không thể thẩm định logic nghiệp vụ bằng mắt thường.
  - *Source Code (Verified):* Mã nguồn ngôn ngữ bậc cao (Solidity) được công khai và đối chiếu khớp 100% với bytecode đã triển khai. Một dự án không công bố Verified Source Code bị xếp vào diện **Rủi ro mức độ Cực kỳ Nghiêm trọng (Red Flag)**.
- **R3 (Quy tắc tương tác Read vs Write):**
  - Các hàm trong tab *Read Contract* (như `totalSupply`, `balanceOf`) là hàm đọc trạng thái (`view`/`pure`), thực thi qua node cục bộ, hoàn toàn miễn phí gas và không yêu cầu kết nối ví.
  - Các hàm trong tab *Write Contract* (như `transfer`, `addBlackList`) là hàm làm biến đổi trạng thái sổ cái, bắt buộc phải trả phí gas và phải có chữ ký số xác thực từ ví có thẩm quyền.
- **R4 (Chuẩn hóa số học tổng cung Token):** Dữ liệu trả về từ hàm `totalSupply()` ở dạng số nguyên lớn (BigNumber). Người làm nghiệp vụ bắt buộc phải chia cho $10^{\text{decimals}}$ (USDT/USDC có `decimals = 6`) để tính ra tổng cung lưu hành thực tế.
- **R5 (Nhận diện cơ chế đóng băng tài khoản):** Thẩm định viên phải xác định cụ thể tên hàm và phân quyền của cơ chế khóa tài khoản:
  - Với USDT: Hàm `addBlackList(address _evilUser)` và `destroyBlackFunds(address _blackListedUser)`.
  - Với USDC: Kế thừa từ `Blacklistable.sol`, gồm hàm `blacklist(address _account)`.
- **R6 (Bổ sung ngoài đề bài - Nhận diện mô hình Proxy có thể nâng cấp):** Thẩm định viên phải kiểm tra xem hợp đồng stablecoin có áp dụng mẫu thiết kế Proxy (Upgradeability) hay không. Với kiến trúc Proxy, bên phát hành có thể đơn phương thay đổi toàn bộ logic hợp đồng mà người dùng không thể can thiệp.
- **R7 (Bổ sung ngoài đề bài - Cơ chế phát hiện rửa tiền qua Internal Transactions):** Nếu trường `Value = 0 ETH` nhưng tài sản di chuyển là token hoặc thông qua contract trung gian, thẩm định viên bắt buộc phải kiểm tra tab "ERC-20 Token Txns" và "Internal Transactions" để truy vết dòng tiền thực tế.

### 4. Đầu ra
- Tệp tài liệu `forensics.md` hoàn chỉnh theo đúng chuẩn mực của Sổ tay Thực hành ECO2432, bao gồm:
  - Bảng giải thích chi tiết 8 trường dữ liệu giao dịch on-chain kèm giá trị ứng dụng thực tiễn cho kế toán và AML.
  - Số liệu thực tế trích xuất từ 01 giao dịch thực tế trên mạng Sepolia Etherscan.
  - Kết quả thẩm định hợp đồng USDT/USDC trên Ethereum Mainnet trả lời chính xác 3 câu hỏi cốt lõi: Xác thực mã nguồn, Tổng cung lưu hành, và Cơ chế đóng băng tài sản.
  - Bài luận phân tích đánh đổi giữa tính phi tập trung lý tưởng và yêu cầu tuân thủ pháp lý tài chính thực tế.

### 5. Trường hợp ngoại lệ (Edge Cases & Exception Handling)
- **E1 (Hợp đồng thông minh chưa được xác minh mã nguồn - Unverified):** Etherscan chỉ hiển thị Disassembled Bytecode. Báo cáo thẩm định phải đưa ra khuyến nghị: Tạm đình chỉ mọi giao dịch tương tác và gửi cảnh báo rủi ro cao cho ban quản lý rủi ro.
- **E2 (Đơn vị tính toán Decimals sai lệch):** Một số token sử dụng 18 chữ số thập phân, nhưng USDT/USDC sử dụng 6 chữ số thập phân. Nếu thẩm định viên chia nhầm cho $10^{18}$, tổng cung sẽ bị tính sai lệch một triệu lần ($10^{12}$ lần).
- **E3 (Địa chỉ mục tiêu bị đóng băng khi đang chuyển tiền):** Nếu một địa chỉ nằm trong danh sách đen cố gắng thực hiện giao dịch chuyển USDT/USDC, lệnh chuyển sẽ bị Revert ngay lập tức với thông báo lỗi do hợp đồng ném ra; giao dịch thất bại và người gửi mất phí gas.

### 6. Ngoài phạm vi (Out of Scope)
- Không thực hiện gọi các hàm ghi (Write) trực tiếp trên Ethereum Mainnet để tránh phát sinh chi phí tiền thật.
- Không phân tích các hợp đồng tài chính phái sinh phức tạp hoặc thuật toán thanh lý vay nợ tự động trong phạm vi Lab này.
- Không dịch ngược toàn bộ các thư viện bytecode chưa xác thực của bên thứ ba.
