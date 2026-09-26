# BẢN ĐẶC TẢ YÊU CẦU NGHIỆP VỤ (SPECIFICATION) — LAB 1 ĐẾN LAB 5

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Đơn vị đào tạo:** Trường Đại học Kinh tế — Khoa Hệ thống Thông tin Kinh tế, Đại học Huế  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Quy chuẩn áp dụng:** Cấu trúc đặc tả 6 phần chuẩn theo Sổ tay Thực hành (Lab Manual ECO2432)  

---

## MỤC LỤC TỔNG HỢP
1. [QUY ƯỚC CHUNG VÀ NGUYÊN TẮC THIẾT KẾ ĐẶC TẢ](#quy-ước-chung)
2. [LAB 1: ĐẶC TẢ THIẾT LẬP MÔI TRƯỜNG PHÁT TRIỂN & QUẢN TRỊ DANH TÍNH ON-CHAIN](#lab-1)
3. [LAB 2: ĐẶC TẢ VÍ KHÔNG LƯU KÝ & CƠ CHẾ GIAO DỊCH P2P TRÊN ETHEREUM](#lab-2)
4. [LAB 3: ĐẶC TẢ GIÁM SÁT DỮ LIỆU ON-CHAIN & THẨM ĐỊNH HỢP ĐỒNG THÔNG MINH](#lab-3)
5. [LAB 4: ĐẶC TẢ HỢP ĐỒNG THÔNG MINH MYTOKEN (ERC-20, MINT, BURN & BLACKLIST)](#lab-4)
6. [LAB 5: ĐẶC TẢ CÔNG CỤ PHÂN TÍCH DÒNG TIỀN ON-CHAIN & MÔ HÌNH KINH TẾ DÒNG TIỀN TOKEN](#lab-5)

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
- **Số tiền chuyển (Value):** Cố định $0,01$ Sepolia ETH cho phiên giao dịch thành công chuẩn.
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
- Tệp tài liệu `forensics.md` hoàn chỉnh gồm:
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

---

<a name="lab-4"></a>
## LAB 4: ĐẶC TẢ HỢP ĐỒNG THÔNG MINH MYTOKEN (ERC-20, MINT, BURN & BLACKLIST)

```
Tên bài: Thiết kế & Xây dựng Token ERC-20 chuẩn OpenZeppelin tích hợp Blacklist
Hình thức: Nhóm / Cá nhân · Thời lượng: 75 phút
Mục tiêu nghề nghiệp: Xây dựng hợp đồng tài sản số đáp ứng chuẩn mực phát hành tiền mã hóa, kiểm soát rủi ro kinh tế và tuân thủ phòng chống tội phạm tài chính (AML/CFT).
```

### 1. Mục đích
Thiết kế và triển khai hợp đồng thông minh tài sản số `MyToken` (ký hiệu `MTK`) tuân thủ nghiêm ngặt tiêu chuẩn OpenZeppelin Contracts v5.x trên nền Solidity `^0.8.20`, tích hợp trần phát hành để ngăn chặn lạm phát vô hạn (bài học Lab 4, Phụ lục I.2), cơ chế đốt token giảm phát, và hệ thống đóng băng tài khoản (Blacklist/Freeze) nhằm trang bị năng lực quản trị rủi ro tuân thủ quy chuẩn pháp lý.

### 2. Đầu vào
- **Tham số nhận diện:**
  - Tên token: `"MyToken"`
  - Ký hiệu: `"MTK"`
  - Số chữ số thập phân (`decimals`): `18`
- **Số lượng đúc ban đầu (Initial Supply):** $1.000.000\text{ MTK}$ ($1.000.000 \times 10^{18}$ wei) được đúc trực tiếp vào ví người triển khai (`msg.sender`) trong hàm khởi tạo `constructor`.
- **Hằng số giới hạn trần tổng cung (Max Supply Cap):** $10.000.000\text{ MTK}$ ($10.000.000 \times 10^{18}$ wei).
- **Tham số hàm gọi quản trị:**
  - `mint(address to, uint256 amount)`: Đúc thêm token đến ví chỉ định.
  - `freezeAccount(address account)` / `addToBlacklist(address account)`: Đưa ví vào danh sách đen.
  - `unfreezeAccount(address account)` / `removeFromBlacklist(address account)`: Mở khóa ví.
  - `burn(uint256 amount)`: Người dùng tự hủy token từ số dư của chính mình.

### 3. Quy tắc nghiệp vụ (Business Rules)
- **R1 (Quyền lực đúc token có kiểm soát):** Chỉ duy nhất chủ sở hữu hợp đồng (`onlyOwner`) mới có thẩm quyền kích hoạt hàm `mint()`. Mọi nỗ lực gọi hàm từ ví không có quyền quản trị phải bị đảo ngược (revert) ngay lập tức.
- **R2 (Bảo vệ giá trị chống lạm phát vô hạn):** Tổng cung lưu thông sau khi đúc không được phép vượt quá giới hạn trần `MAX_SUPPLY` ($10.000.000\text{ MTK}$). Đây là bài học thực tiễn từ Hợp đồng B (Phụ lục I.2) nhằm bảo vệ quyền lợi người nắm giữ token khỏi rủi ro bị chủ dự án pha loãng tài sản vô tội vạ.
- **R3 (Cơ chế giảm phát tài sản - Burning):** Người nắm giữ token có quyền tự nguyện đốt bỏ một phần hoặc toàn bộ số dư của mình thông qua hàm `burn(amount)` (kế thừa `ERC20Burnable`), làm giảm trực tiếp cả số dư cá nhân và tổng cung lưu hành toàn mạng lưới.
- **R4 (Cơ chế đóng băng tài khoản tập trung - Blacklist):** Chỉ chủ sở hữu (`onlyOwner`) mới có quyền thực thi `freezeAccount()` hoặc `unfreezeAccount()`. Một tài khoản khi bị đưa vào trạng thái đóng băng (`isFrozen[account] == true`) sẽ bị chặn hoàn toàn cả **chiều gửi** (không thể chuyển đi, không thể burn) và **chiều nhận** (không thể nhận chuyển khoản, không thể mint tới).
- **R5 (Bảo vệ địa chỉ chủ sở hữu):** Hợp đồng nghiêm cấm hành vi tự đóng băng chính ví của Chủ sở hữu (`CannotFreezeOwner`), ngăn chặn nguy cơ hợp đồng rơi vào trạng thái bế tắc quản trị (Deadlock).
- **R6 (Điểm chặn duy nhất tầng EVM v5):** Toàn bộ các logic kiểm tra danh sách đen phải được thực thi tại hàm hook `_update(address from, address to, uint256 value)` nội bộ của OpenZeppelin v5, bảo đảm không có bất kỳ ngóc ngách nào (kể cả `transferFrom` hay `burn`) có thể lách qua cơ chế kiểm soát.
- **R7 (Bổ sung ngoài đề bài - Phát sự kiện kiểm toán):** Mọi thao tác quản trị nhạy cảm (`mint`, `freezeAccount`, `unfreezeAccount`) bắt buộc phải phát ra sự kiện on-chain tương ứng (`TokensMinted`, `AccountFrozen`, `AccountUnfrozen`) với các trường đánh chỉ mục (`indexed`) để các hệ thống giám sát AML ngoài chuỗi có thể lập chỉ mục theo thời gian thực.
- **R8 (Bổ sung ngoài đề bài - Tối ưu hóa phí gas bằng Custom Errors):** Toàn bộ các điều kiện chặn lỗi phải sử dụng mã lỗi tùy biến (`revert ZeroAddressNotAllowed()`, `revert AccountIsFrozen()`) thay vì chuỗi văn bản dài trong `require`, giúp giảm kích thước bytecode triển khai và tiết kiệm gas giao dịch cho người dùng.

### 4. Đầu ra
- Tệp mã nguồn [MyToken.sol](file:///c:/SmartContractLab/contracts/MyToken.sol) hoàn chỉnh, biên dịch thành công 100% với trình biên dịch Solidity `0.8.20`.
- Bộ kiểm thử tự động [MyToken.test.js](file:///c:/SmartContractLab/test/MyToken.test.js) vượt qua toàn bộ 17 ca kiểm nghiệm (Passing 17/17 tests).
- Bảng ánh xạ trạng thái và sự kiện on-chain sẵn sàng tích hợp với giao diện Web3 DApp.

### 5. Trường hợp ngoại lệ (Edge Cases & Exception Handling)
- **E1 (Đúc token vượt trần):** Nếu chủ sở hữu cố gắng đúc số lượng token làm $\text{totalSupply} + \text{amount} > \text{MAX\_SUPPLY}$, hợp đồng kích hoạt lỗi `MaxSupplyExceeded(attemptedTotal, maxSupplyLimit)` và hủy bỏ giao dịch.
- **E2 (Giao dịch liên quan đến ví bị đóng băng):** Nếu ví A gửi tiền cho ví B mà một trong hai ví (hoặc cả hai) nằm trong danh sách đen, giao dịch bị Revert lập tức với lỗi `AccountIsFrozen(account)`. Tiền không di chuyển, người gửi chịu phí gas đã tiêu thụ.
- **E3 (Đóng băng tài khoản đã bị đóng băng trước đó):** Kích hoạt lỗi `AccountAlreadyFrozen(account)` nhằm tránh phát sinh sự kiện trùng lặp lãng phí gas.
- **E4 (Mở khóa tài khoản chưa từng bị đóng băng):** Kích hoạt lỗi `AccountNotFrozen(account)`.
- **E5 (Tương tác với địa chỉ rỗng):** Bất kỳ tham số nào nhận vào `address(0)` (cho `to` khi mint hoặc `account` khi freeze) đều kích hoạt lỗi `ZeroAddressNotAllowed()`.

### 6. Ngoài phạm vi (Out of Scope)
- Không tích hợp cơ chế thu phí trên mỗi giao dịch (Fee-on-transfer) trong phiên bản này.
- Không cấu hình mô hình bỏ phiếu quản trị phi tập trung (Governance / DAO).
- Không hỗ trợ tính năng cấp phép ngoài chuỗi (EIP-2612 Permit).

---

<a name="lab-5"></a>
## LAB 5: ĐẶC TẢ CÔNG CỤ PHÂN TÍCH DÒNG TIỀN ON-CHAIN & MÔ HÌNH KINH TẾ DÒNG TIỀN TOKEN

```
Tên bài: Viết đặc tả cho công cụ phân tích dòng tiền
Hình thức: Nhóm 2 người · Thời lượng: 75 phút · KHÔNG VIẾT MÃ NGUỒN TRONG BUỔI NÀY
Mục tiêu nghề nghiệp: Chuẩn bị năng lực cho Chuyên viên phân tích nghiệp vụ (BA) sản phẩm tài sản số và Chuyên viên phân tích dữ liệu on-chain tại ngân hàng/công ty Fintech. Viết đặc tả đủ rõ để đội kỹ thuật hoặc công cụ AI làm ra đúng thứ mình muốn mà không cần hỏi lại.
```

### 1. Mục đích
Xây dựng một công cụ phân tích dữ liệu on-chain chuyên sâu, tự động tiếp nhận một địa chỉ ví Ethereum mục tiêu (dạng EOA hoặc Contract), truy xuất và bóc tách toàn bộ lịch sử biến động số dư trong chu kỳ 90 ngày gần nhất thông qua Etherscan API; từ đó kết xuất báo cáo dòng tiền vào/ra, tính toán số dư khả dụng lũy kế theo thời gian thực, trực quan hóa biểu đồ tài chính và đối soát với quy tắc kinh tế token trong tệp [tokenomics.md](file:///c:/SmartContractLab/tokenomics.md).

### 2. Đầu vào
- **Địa chỉ ví mục tiêu:** Chuỗi định dạng Hex 42 ký tự bắt đầu bằng tiền tố `0x`, tuân thủ tổng kiểm chuẩn EIP-55 (ví dụ: ví của cá nhân, ví Quỹ kho bạc `classFund` hoặc ví cá voi).
- **Khóa xác thực API (API Credentials):** Khóa `ETHERSCAN_API_KEY` đọc trực tiếp và bảo mật từ biến môi trường của hệ điều hành / tệp cấu hình `.env` (nghiêm cấm ghi cứng trong mã nguồn theo quy ước `AGENTS.md`).
- **Khung thời gian phân tích (Time Window):** Số ngày cần phân tích tính từ thời điểm hiện tại ngược về quá khứ, giá trị mặc định là **90 ngày** ($\Delta t = 90 \times 86.400\text{ giây}$).
- **Địa chỉ hợp đồng token MTK (Tùy chọn):** Địa chỉ triển khai của hợp đồng [contracts/MyToken.sol](file:///c:/SmartContractLab/contracts/MyToken.sol) để trích xuất dòng tiền token ERC-20 và đối soát phí chuyển nhượng.
- **Tham số mạng:** Mặc định mạng Sepolia Testnet (`api-sepolia.etherscan.io`) hoặc Ethereum Mainnet (`api.etherscan.io`).

### 3. Quy tắc nghiệp vụ (Business Rules)
- **R1 (Quy chuẩn nhận diện Dòng tiền vào - Cash Inflow):** Mọi giao dịch hợp lệ có trường `to` trùng khớp với địa chỉ ví đang xét được hạch toán là **Dòng tiền vào**. Số tiền vào được cộng trực tiếp vào số dư lũy kế tại mốc thời gian đó:  
  $$\Delta \text{Balance} = +\text{Value}$$
- **R2 (Quy chuẩn nhận diện Dòng tiền ra - Cash Outflow):** Mọi giao dịch có trường `from` trùng khớp với địa chỉ ví đang xét được hạch toán là **Dòng tiền ra**.
- **R3 (Nguyên tắc khấu trừ kép cho giao dịch đi ra):** Đối với mọi giao dịch đi ra thành công, số tiền thực tế bị trừ khỏi ví phải bao gồm cả giá trị chuyển dịch và chi phí tiêu hao mạng lưới (Transaction Gas Fee):  
  $$\text{Dòng tiền ra thực tế} = \text{Value chuyển đi} + (\text{Gas Used} \times \text{Gas Price})$$
- **R4 (Nguyên lý kế toán giao dịch thất bại):** Giao dịch có trạng thái thất bại (`Status == 0` / `isError == 1` / `Fail`) không làm chuyển dịch số tiền gốc `Value`, nhưng **toàn bộ phí gas đã tiêu thụ vẫn bị mạng lưới khấu trừ vĩnh viễn** khỏi số dư của ví gửi. Khoản phí này bắt buộc phải được hạch toán độc lập vào Dòng tiền ra:  
  $$\text{Dòng tiền ra} = 0 + (\text{Gas Used} \times \text{Gas Price})$$
- **R5 (Chuẩn hóa đơn vị đo lường BigNumber):** Mọi dữ liệu số dư và giá trị giao dịch trích xuất từ Etherscan API ở đơn vị cơ sở `wei` bắt buộc phải được chuyển đổi sang đơn vị `ETH` bằng cách chia cho $10^{18}$ trước khi thực hiện các phép cộng trừ kế toán và hiển thị cho người dùng.
- **R6 (Tuần tự hóa thời gian tuyệt đối):** Toàn bộ danh sách giao dịch sau khi làm sạch phải được sắp xếp theo trật tự thời gian tăng dần (Chronological Order: từ giao dịch cũ nhất đến mới nhất) dựa trên chỉ số khối (`blockNumber`) và dấu thời gian (`timeStamp`) trước khi tính toán số dư lũy kế (Cumulative Running Balance).
- **R7 (Bổ sung ngoài đề bài - Đối soát dòng tiền kép Token MTK & Phí giao dịch 1%):** Khi người dùng kích hoạt cờ phân tích Token ERC-20, hệ thống gọi thêm endpoint `tokentx`. Với mỗi lệnh chuyển MTK, công cụ tự động bóc tách:
  - Số lượng token thực nhận của ví đích: $\text{Received} = \text{Amount} \times (1 - \text{feeBps}/10.000) = \text{Amount} \times 99\%$.
  - Số lượng token trích nạp Quỹ Kho bạc `classFund`: $\text{Fee} = \text{Amount} \times 1\%$.
- **R8 (Bổ sung ngoài đề bài - Nhận diện mẫu hình dòng tiền bất thường AML):** Tích hợp thuật toán cảnh báo đỏ (Red Flags) cho chuyên viên tuân thủ:
  - Cảnh báo **Layering / Smurfing:** Khi ví thực hiện $\ge 5$ giao dịch giá trị giống hệt nhau trong vòng dưới 10 phút (mẫu hình vụ hack Bybit Lab 3B).
  - Cảnh báo **Wash Trading:** Khi xuất hiện chuỗi giao dịch chuyển tiền vòng tròn A $\rightarrow$ B $\rightarrow$ A trong cùng một ngày.

### 4. Đầu ra
- **Bảng nhật ký dòng tiền chi tiết (Cashflow Ledger Table):** Kết xuất bảng dữ liệu gồm 7 trường chuẩn:
  1. `Dấu thời gian (UTC)`: Ngày giờ phát sinh giao dịch chuẩn ISO-8601.
  2. `Mã băm giao dịch (TxHash)`: Định danh rút gọn kèm liên kết trực tiếp tới Etherscan.
  3. `Phân loại luồng tiền`: Dán nhãn `VÀO (Inflow)`, `RA (Outflow)`, hoặc `THẤT BẠI (Failed)`.
  4. `Số tiền chuyển (Value)`: Đơn vị ETH / MTK đã chuẩn hóa.
  5. `Phí giao dịch thực trả (Tx Fee)`: Đơn vị ETH.
  6. `Ví đối tác (Counterparty)`: Địa chỉ gửi (nếu tiền vào) hoặc địa chỉ nhận (nếu tiền ra).
  7. `Số dư lũy kế (Cumulative Balance)`: Số dư khả dụng của ví ngay sau thời điểm giao dịch được xác nhận.
- **Biểu đồ biến thiên số dư theo thời gian (Interactive Balance Chart):**
  - Trục hoành ($X$): Trục thời gian trải dài từ ngày $T-90$ đến ngày $T$.
  - Trục tung ($Y$): Số dư lũy kế (ETH / MTK).
  - Điểm đánh dấu (Markers): Các mốc đột biến có dòng tiền lớn $> 10\%$ số dư trung bình.
- **Bảng tóm tắt chỉ số tài chính tổng hợp (Executive Summary Metrics):**
  - *Tổng tiền vào trong kỳ (Total Inflow):* $\sum \text{Inflow}$.
  - *Tổng tiền ra trong kỳ (Total Outflow):* $\sum \text{Outflow}$ (bao gồm cả giá trị chuyển và tổng phí gas).
  - *Tổng phí mạng đã nộp cho EVM (Total Gas Burned):* $\sum \text{Gas Fee}$.
  - *Số dư ròng biến động (Net Cashflow):* $\text{Total Inflow} - \text{Total Outflow}$.
  - *Số dư cuối kỳ thực tế (Closing Balance):* Đối chiếu khớp $100\%$ với kết quả trả về từ hàm `eth_getBalance`.

### 5. Trường hợp ngoại lệ (Edge Cases & Exception Handling)
- **E1 (Ví không phát sinh giao dịch trong kỳ - Zero Activity):** Nếu Etherscan API trả về danh sách giao dịch rỗng (`result = []` hoặc `message = "No transactions found"`), công cụ hiển thị thông báo nghiệp vụ thân thiện: *"Ví không có giao dịch trong chu kỳ 90 ngày được chọn"*, không ném ngoại lệ dừng chương trình; đồng thời tự động gọi phương thức `eth_getBalance` để hiển thị số dư tĩnh hiện tại của ví.
- **E2 (Khóa API không hợp lệ hoặc vượt ngưỡng giới hạn tần suất - Rate Limit / Invalid Key):** Nếu API trả về `status = "0"` kèm thông báo lỗi (như *"Invalid API Key"* hoặc *"Max rate limit reached"*), hệ thống bắt lỗi (try/catch), in cảnh báo hướng dẫn người dùng kiểm tra lại biến môi trường `.env`, kích hoạt cơ chế tự động chờ thử lại sau 1 giây (Exponential Backoff), tuyệt đối không để lộ chuỗi khóa API ra log màn hình.
- **E3 (Ví có số lượng giao dịch khổng lồ vượt ngưỡng trang - Pagination > 10.000 Txns):** Etherscan giới hạn tối đa $10.000$ giao dịch cho mỗi lần gọi API đơn lẻ. Khi gặp các ví hoạt động với tần suất cao (sàn giao dịch hoặc bot), hệ thống phải kích hoạt thuật toán phân trang tự động: chia nhỏ khoảng khối (`startblock` đến `endblock`) và lặp truy vấn để thu thập đầy đủ $100\%$ dữ liệu lịch sử mà không bỏ sót bất kỳ dòng tiền nào.
- **E4 (Dòng tiền ẩn qua Giao dịch nội bộ - Internal Transactions):** Các giao dịch rút tiền từ hợp đồng thông minh hoặc ví đa chữ ký (như ví Gnosis Safe trong vụ hack Bybit) có trường `Value = 0` ở tầng giao dịch gốc. Hệ thống tự động kích hoạt truy vấn song song endpoint `txlistinternal` để đối soát, bảo đảm số dư lũy kế không bị sai lệch so với thực tế sổ cái on-chain.
- **E5 (Giao dịch tự chuyển cho chính mình - Self-Transfer):** Khi `from == to == targetWallet`, số tiền chuyển không làm thay đổi số dư tài sản gốc, nhưng giao dịch vẫn tiêu tốn phí gas. Hệ thống nhận diện và chỉ hạch toán khoản phí gas vào dòng tiền ra, gán nhãn nghiệp vụ là *"Self-transfer / Gas Burn"*.

### 6. Ngoài phạm vi (Out of Scope)
- Không xây dựng mô hình máy học (AI/ML) để dự báo xu hướng giá tương lai của đồng tiền số.
- Không tự động thực thi các lệnh đảo ngược giao dịch, phong tỏa số dư hay can thiệp vào máy ảo EVM (vì tính bất biến của Blockchain).
- Không quy đổi tự động ra tiền pháp định (VND hoặc USD) theo tỷ giá thời gian thực trong phiên bản này, nhằm tránh rủi ro sai lệch do phụ thuộc vào nhà cung cấp dữ liệu Oracle bên thứ ba.
- Không xử lý các mạng lưới Blockchain không tương thích chuẩn EVM (như Bitcoin UTXO, Solana hay Tron) trong phạm vi học phần này.
