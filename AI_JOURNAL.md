# NHẬT KÝ LÀM VIỆC VỚI AI (AI JOURNAL)

**Học phần:** Tiền điện tử và Hợp đồng thông minh (ECO2432)  
**Khóa / Ngành:** K58 — Công nghệ Tài chính (Fintech) & Hệ thống Thông tin Quản lý (MIS)  
**Giảng viên phụ trách:** TS. Hà Ngọc Long  
**Quy chuẩn áp dụng:** Mẫu chuẩn Phần B.3 & Phụ lục II — Sổ tay Thực hành ECO2432  
**Mục tiêu liêm chính:** Ghi nhận trung thực các lần tương tác prompt, đánh giá phản hồi, phát hiện lỗi sai kỹ thuật/nghiệp vụ của AI và phương án khắc phục do sinh viên thực hiện.

---

## BẢNG TỔNG HỢP CÁC LẦN TƯƠNG TÁC VÀ LỖI PHÁT HIỆN

| Lần | Bài Lab | Vấn đề nghiệp vụ / Kỹ thuật | Đánh giá | Ai phát hiện |
| :---: | :---: | :--- | :---: | :---: |
| **01** | Lab 1 | Cấu hình bảo mật môi trường và nguy cơ lộ Private Key qua Git | [Không đạt] | **Sinh viên phát hiện** |
| **02** | Lab 2 | Cơ chế tính phí gas EVM khi chuyển toàn bộ số dư (Max Balance) | [Không đạt] | **Sinh viên phát hiện** |
| **03** | Lab 2 | Kiểm tra định dạng địa chỉ ví EIP-55 và xử lý giao dịch gửi nhầm | [Lưu ý] | **Sinh viên phát hiện** |
| **04** | Lab 3 | Bản chất tiêu thụ gas giữa hàm Read Contract (`view`) và Write Contract | [Lưu ý] | **Sinh viên phát hiện** |
| **05** | Lab 3 | Ảo giác tên hàm đóng băng tài khoản và đặc tả chuẩn ERC-20 của USDT | [Không đạt] | **Sinh viên phát hiện** |
| **06** | Lab 3B | Nhầm lẫn trường From/To và bản chất tin nhắn on-chain vụ hack Bybit | [Không đạt] | **Sinh viên phát hiện** |
| **07** | Lab 10 | Ảo tưởng về tính bí mật của biến trạng thái `private` trong Solidity | [Lưu ý] | **Sinh viên phát hiện** |
| **08** | Lab 4 | Lỗi thời `_beforeTokenTransfer` (OZ v4) trên v5 và rủi ro đúc vô trần | [Không đạt] | **Sinh viên phát hiện** |
| **09** | Lab 5 / Lab 8 | Thiết kế quy tắc kinh tế tokenomics, đặc tả dòng tiền và 5 phản biện nhà đầu tư | [Lưu ý] | **Sinh viên phát hiện** |

---

## CHI TIẾT NHẬT KÝ TỪNG PHIÊN LÀM VIỆC

### Lần 1 — Lab 1: Cấu hình môi trường Web3 và rủi ro rò rỉ khóa bí mật

- **Prompt (dán nguyên văn):**
  > "Bạn là trợ lý lập trình Web3 chuyên nghiệp. Hãy viết giúp tôi tệp cấu hình `hardhat.config.js` để kết nối vào mạng thử nghiệm Ethereum Sepolia. Hướng dẫn tôi cách điền Private Key của ví MetaMask và API Key của Alchemy/Infura để có thể chạy lệnh deploy nhanh nhất mà không gặp lỗi."

- **AI trả về (tóm tắt):**
  > AI sinh mã cấu hình `hardhat.config.js` có chứa đoạn mã ghi cứng trực tiếp:  
  > `accounts: ["0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"]`  
  > hoặc gợi ý tạo tệp `.env` nhưng kèm câu lệnh hướng dẫn: `git add . && git commit -m "feat: setup hardhat" && git push origin main` để đẩy toàn bộ dự án lên kho GitHub công khai.

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Nguy cơ bảo mật tối nghiêm trọng).

- **Chỗ sai (mô tả cụ thể, kèm dòng mã):**
  > AI đã vi phạm quy tắc cốt lõi về bảo mật tài sản số được quy định tại mục B.4 của `AGENTS.md`. Việc ghi cứng Private Key trong mã nguồn hoặc hướng dẫn `git add .` khi chưa cấu hình `.gitignore` sẽ dẫn đến việc khóa bí mật bị đẩy lên GitHub. Các bot tự động trên Internet liên tục quét các commit công khai và sẽ rút sạch toàn bộ tài sản trong ví chỉ sau vài giây.

- **Cách sửa (sinh viên đã làm gì):**
  > 1. Sinh viên lập tức tạo tệp `.gitignore` ở thư mục gốc, khai báo rõ ràng các mục nhạy cảm:  
  >    ```gitignore
  >    .env
  >    node_modules/
  >    cache/
  >    artifacts/
  >    ```
  > 2. Sử dụng gói thư viện `dotenv` trong `hardhat.config.js`:
  >    ```javascript
  >    require("dotenv").config();
  >    const SEPOLIA_PRIVATE_KEY = process.env.SEPOLIA_PRIVATE_KEY || "";
  >    ```
  > 3. Tạo tệp `.env.example` làm mẫu cấu hình không chứa dữ liệu thật cho các thành viên trong nhóm.

- **Ai phát hiện:** **Sinh viên phát hiện** (Kiểm tra lại checklist an toàn trước khi commit).

---

### Lần 2 — Lab 2: Cơ chế trừ phí gas của EVM khi thực hiện chuyển tiền

- **Prompt (dán nguyên văn):**
  > "Tôi đang có số dư chính xác là 0.05 Sepolia ETH trong ví MetaMask. Tôi muốn chuyển toàn bộ số tiền này cho bạn học cùng lớp. Nếu tôi nhập số tiền gửi là 0.05 ETH thì giao dịch có thành công không? Hệ thống sẽ trừ phí gas vào đâu? Người nhận sẽ nhận được bao nhiêu tiền?"

- **AI trả về (tóm tắt):**
  > AI trả lời: "Giao dịch sẽ thành công bình thường. Mạng Ethereum sẽ tự động tính toán phí gas và trừ thẳng vào số tiền 0.05 ETH bạn gửi. Người nhận sẽ nhận được số tiền thực tế là 0.05 ETH trừ đi phí gas mạng."

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Sai lệch hoàn toàn bản chất vận hành của EVM).

- **Chỗ sai (mô tả cụ thể):**
  > Trong máy ảo Ethereum (EVM), phí giao dịch (Gas Fee) luôn luôn được thanh toán bằng đồng tiền gốc (ETH) từ số dư ví người gửi (`From`) và được tính độc lập hoàn toàn với giá trị chuyển (`Value`). Công thức kiểm tra số dư tối thiểu của EVM là:  
  > $$\text{Balance} \ge \text{Value} + (\text{Gas Limit} \times \text{Gas Price})$$  
  > EVM tuyệt đối không tự ý khấu trừ phí gas vào tham số `Value` chuyển đi. Do đó, nếu nhập `Value = Balance = 0.05 ETH`, ví người gửi không còn ETH để trả phí gas, dẫn tới giao dịch bị MetaMask chặn ngay tại Client hoặc bị Mempool từ chối với mã lỗi `Insufficient funds for gas * price + value`.

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên tự tay thực nghiệm trên MetaMask: Khi ấn nút "Tối đa" (Max), MetaMask tự động trừ bớt một lượng ETH dự phòng cho gas fee. Khi sinh viên cố tình gõ đủ $0,05$ ETH, MetaMask báo đỏ nút xác nhận. Sinh viên ghi nhận chính xác bài học vào `lab02.md`: Phí gas luôn tính riêng và phải chừa lại số dư để trả gas.

- **Ai phát hiện:** **Sinh viên phát hiện** (Phát hiện khi thực hành kịch bản B của Lab 2).

---

### Lần 3 — Lab 2: Kiểm tra tổng kiểm địa chỉ EIP-55 và tính bất biến

- **Prompt (dán nguyên văn):**
  > "Nếu tôi vô tình gõ nhầm một ký tự trong địa chỉ ví người nhận khi gửi Sepolia ETH thì mạng blockchain có tự động hoàn tiền lại cho tôi không? Làm thế nào để phần mềm ví phát hiện tôi gõ sai địa chỉ?"

- **AI trả về (tóm tắt):**
  > "Blockchain là hệ thống thông minh, nếu địa chỉ không tồn tại hoặc chưa từng có giao dịch thì mạng lưới sẽ tự động hủy giao dịch và hoàn tiền lại sau 24 giờ. Ví phát hiện gõ sai nhờ gửi truy vấn lên máy chủ ngân hàng trung ương để kiểm tra danh tính chủ tài khoản."

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Bịa đặt cơ chế hoạt động, sai lệch kiến thức nền tảng).

- **Chỗ sai (mô tả cụ thể):**
  > 1. Blockchain không có khái niệm "hoàn tiền tự động sau 24 giờ" và không có máy chủ ngân hàng trung ương nào xác minh danh tính. Khi một giao dịch đã được xác nhận vào block, nó mang tính **bất biến (immutable)** vĩnh viễn. Nếu gửi nhầm vào địa chỉ hợp lệ của người lạ, không ai (kể cả thợ đào hay người sáng lập Ethereum) có thể đảo ngược giao dịch.
  > 2. Cơ chế phát hiện gõ sai địa chỉ là **chuẩn EIP-55 Checksum**: địa chỉ ví Ethereum sử dụng sự kết hợp giữa chữ hoa và chữ thường dựa trên hàm băm Keccak-256. Nếu gõ sai 1 ký tự, mã tổng kiểm không khớp và phần mềm ví từ chối ký lệnh ngay lập tức ở máy người dùng mà không cần gửi bất kỳ dữ liệu nào lên chuỗi.

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên bác bỏ câu trả lời của AI, trích dẫn bài giảng của TS. Hà Ngọc Long về cơ chế mã hóa EIP-55 và viết lại phần giải thích 3 câu về tính bất biến trong tệp `lab02.md`.

- **Ai phát hiện:** **Sinh viên phát hiện**.

---

### Lần 4 — Lab 3: Phân biệt bản chất tiêu hao gas giữa hàm Read (`view`) và hàm Write

- **Prompt (dán nguyên văn):**
  > "Khi tôi mở trình duyệt Etherscan xem hợp đồng thông minh USDT, tại sao tôi bấm gọi hàm `totalSupply()` hoặc `balanceOf()` ở tab Read Contract thì không thấy MetaMask bật lên đòi trả phí gas, trong khi nếu tôi gọi hàm `transfer()` ở tab Write Contract thì lại bắt buộc phải có ví và mất phí gas? Có phải hàm đọc tốn ít gas nên Etherscan trả hộ không?"

- **AI trả về (tóm tắt):**
  > "Đúng vậy, các hàm đọc tốn rất ít tài nguyên tính toán (dưới 2.100 gas) nên hệ thống Etherscan đã hỗ trợ chi trả khoản phí nhỏ này cho người dùng nhằm khuyến khích tra cứu thông tin."

- **Đánh giá:** `[Lưu ý] Phải sửa` (Lý giải sai bản chất kiến trúc EVM).

- **Chỗ sai (mô tả cụ thể):**
  > Etherscan hoàn toàn không "trả hộ phí". Bản chất kỹ thuật là:
  > - Các hàm trong tab *Read Contract* được định nghĩa với từ khóa `view` hoặc `pure`. Những hàm này chỉ thực hiện truy vấn dữ liệu từ bộ nhớ trạng thái (State Storage) sẵn có của node RPC cục bộ thông qua phương thức `eth_call`. Không có trạng thái nào bị thay đổi, không cần tạo khối mới, không cần các validator đồng thuận nên **phí gas bằng 0 tuyệt đối**.
  > - Các hàm trong tab *Write Contract* làm biến đổi trạng thái (thay đổi số dư trong mapping storage), đòi hỏi phải tạo ra một Transaction đóng gói chữ ký số, truyền qua Mempool và cần toàn mạng lưới cập nhật sổ cái. Do đó, bắt buộc phải trả phí gas cho validator.

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên đính chính lại định nghĩa trong `forensics.md`: Phân biệt rạch ròi giữa Off-chain State Query (miễn phí) và On-chain State Mutation (tính phí gas).

- **Ai phát hiện:** **Sinh viên phát hiện** (Dựa vào nội dung bài giảng Session 02 và Lab 3).

---

### Lần 5 — Lab 3: Ảo giác tên hàm đóng băng tài khoản và tiêu chuẩn ERC-20 của USDT

- **Prompt (dán nguyên văn — áp dụng mẫu Phụ lục II.1):**
  > "Bạn là chuyên viên thẩm định rủi ro tài sản số. Dưới đây là mã nguồn hợp đồng thông minh TetherToken (USDT) tại địa chỉ `0xdAC17F958D2ee523a2206206994597C13D831ec7`. Hãy liệt kê mọi quyền đặc biệt mà chủ sở hữu hợp đồng có thể thực hiện, và với mỗi quyền nêu rõ: tên hàm và rủi ro cho người nắm giữ token. Chỉ trả lời dựa trên mã nguồn thực tế. Nếu không tìm thấy, nói là không tìm thấy."

- **AI trả về (tóm tắt):**
  > AI liệt kê:
  > 1. Hàm `freezeAccount(address target, bool freeze)`: Dùng để đóng băng ví người dùng.
  > 2. Hàm `mint(address to, uint256 amount)`: In thêm token vô hạn.
  > 3. AI bổ sung bình luận: "Chức năng đóng băng tài khoản `freezeAccount` là quy định chuẩn có sẵn trong mọi token ERC-20 để tuân thủ luật phòng chống rửa tiền quốc tế."

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Ảo giác tên hàm và sai lệch kiến thức chuẩn công nghệ).

- **Chỗ sai (mô tả cụ thể, kèm dòng mã):**
  > 1. Chuẩn ERC-20 tiêu chuẩn (EIP-20) hoàn toàn **KHÔNG có bất kỳ hàm đóng băng tài khoản nào**. Chuẩn ERC-20 chỉ gồm 6 hàm cơ bản: `totalSupply`, `balanceOf`, `transfer`, `transferFrom`, `approve`, `allowance` và 2 sự kiện: `Transfer`, `Approval`.
  > 2. AI tự bịa ra tên hàm `freezeAccount`. Trong mã nguồn thực tế của USDT (`TetherToken.sol`), cơ chế quản trị danh sách đen được xây dựng thông qua hợp đồng kế thừa `BlackList` với các hàm có tên chính xác là:
  >    - `addBlackList(address _evilUser)` (Dòng 249)
  >    - `removeBlackList(address _clearedUser)` (Dòng 253)
  >    - `destroyBlackFunds(address _blackListedUser)` (Dòng 257) — Tịch thu và tiêu hủy toàn bộ số dư USDT của ví bị chặn.

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên trực tiếp vào tab `Contract -> Code` của USDT trên Etherscan, bấm `Ctrl + F` tìm từ khóa "black" và "freeze". Sau đó trích xuất nguyên văn định nghĩa hàm từ mã nguồn và bổ sung vào bảng phân tích rủi ro trong `forensics.md`.

- **Ai phát hiện:** **Sinh viên phát hiện** (Thẩm định trực tiếp mã nguồn trên Etherscan).

---

### Lần 6 — Lab 3B: Phân tích vụ hack Bybit 1,5 tỷ USD và thông điệp trên chuỗi

- **Prompt (dán nguyên văn):**
  > "Dùng dữ liệu từ Etherscan phân tích giao dịch gốc của vụ tấn công sàn Bybit lúc 14:16 UTC ngày 21/02/2025. Cho biết ai là người gửi (From), ai là người nhận (To), tại sao trường Value hiển thị bằng 0 ETH mà ví hacker vẫn nhận được 401.346 ETH? Giải mã thông điệp gửi cho hacker tại giao dịch 0xf0f7c2a84e29300089fc1049982675c68efcfc8a18557193f113f9881d014f55."

- **AI trả về (tóm tắt):**
  > 1. AI xác định: Trường `From` là ví Bybit Cold Wallet (`0x1db9...`) tự gửi tiền cho hacker do nhân viên bấm nhầm lệnh chuyển.
  > 2. Giá trị hiển thị 0 ETH là do lỗi giao diện hiển thị của Etherscan khi số tiền quá lớn.
  > 3. Về tin nhắn on-chain: AI cho rằng sàn Bybit đã gửi thông điệp đề nghị trả 10% tiền thưởng (Bounty) nếu hacker trả lại 90% số tiền đã lấy.

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Sai nghiêm trọng cả 3 luận điểm).

- **Chỗ sai (mô tả cụ thể):**
  > 1. Nhầm lẫn trường `From` và `To`: Người ký giao dịch (`From`) là ví EOA của hacker (`0x0fa0...`), còn `To` là ví đa chữ ký Gnosis Safe của Bybit (`0x1db9...`). Bybit không hề tự bấm lệnh gửi tiền.
  > 2. `Value = 0` không phải lỗi hiển thị. Lệnh gọi từ EOA mang `msg.value = 0`, nhưng thông qua `DELEGATECALL` vào hợp đồng Safe đã bị tráo implementation độc hại, kích hoạt hàm rút tiền nội bộ (**Internal Transaction**), chuyển $401.346,7688$ ETH từ số dư lưu trữ của ví Safe sang ví hacker (`0x4766...`).
  > 3. Giao dịch `0xf0f7c2a8...` chứa chuỗi dữ liệu Hex Input. Khi giải mã bằng UTF-8, nội dung thực sự là:  
  >    `"This is FBI. You could be arrested. We are monitoring at you over kitchen window. 100 ETH to this address and we are goi..."`  
  >    Đây là tin nhắn của một đối tượng nặc danh giả mạo cơ quan điều tra FBI nhằm tống tiền hacker, hoàn toàn không phải thư thương lượng Bounty của sàn Bybit!

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên tự tra cứu mã băm giao dịch trên Etherscan, bấm mở tab "Internal Txns" để chứng minh dòng tiền $401.346$ ETH đi qua Internal Call; đồng thời dùng công cụ giải mã Hex-to-Text để trích xuất nguyên văn tin nhắn tống tiền mạo danh FBI, ghi lại trọn vẹn vào báo cáo `trace.md`.

- **Ai phát hiện:** **Sinh viên phát hiện**.

---

### Lần 7 — Lab 10: Hiểu lầm về từ khóa `private` trong hợp đồng thông minh

- **Prompt (dán nguyên văn — áp dụng mẫu Phụ lục II.4):**
  > "Bạn là kiểm toán viên hợp đồng thông minh. Rà soát hợp đồng `VaultBuggy.sol` dưới đây. Trong hợp đồng có biến `uint256 private emergencyPin`. Liệu việc đặt từ khóa `private` có bảo vệ được mã PIN này không bị người ngoài đọc trộm trên blockchain không?"

- **AI trả về (tóm tắt):**
  > "Có. Từ khóa `private` trong ngôn ngữ Solidity bảo đảm tính đóng gói (encapsulation). Biến `emergencyPin` chỉ có thể được truy xuất nội bộ bên trong hợp đồng `VaultBuggy`, hoàn toàn vô hình và được mã hóa an toàn đối với các người dùng và hợp đồng bên ngoài."

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Lỗ hổng nhận thức căn bản về kiến trúc Blockchain).

- **Chỗ sai (mô tả cụ thể):**
  > Blockchain là một sổ cái phi tập trung hoàn toàn công khai, mọi dữ liệu lưu trữ tại các ô nhớ (Storage Slots) của hợp đồng đều được sao chép đến tất cả các node trong mạng.
  > - Từ khóa `private` trong Solidity **CHỈ** có tác dụng ở cấp độ trình biên dịch (Compiler-level access control): ngăn không cho các hợp đồng khác gọi trực tiếp biến đó thông qua interface.
  > - Bất kỳ ai cũng có thể đọc được giá trị của biến `private` trực tiếp từ dữ liệu khối thông qua hàm RPC tiêu chuẩn của Ethereum: `eth_getStorageAt(contractAddress, slotPosition)`. Biến `private` hoàn toàn không có tính năng bí mật hay mã hóa dữ liệu!

- **Cách sửa (sinh viên đã làm gì):**
  > Sinh viên thực hiện kiểm chứng thực nghiệm theo hướng dẫn của TS. Hà Ngọc Long:
  > 1. Triển khai hợp đồng với tham số `_pin = 123456` (dạng hex là `0x01e240`).
  > 2. Mở Console trình duyệt gõ lệnh:
  >    ```javascript
  >    await window.ethereum.request({
  >      method: "eth_getStorageAt",
  >      params: ["0x<dia_chi_hop_dong>", "0x2", "latest"]
  >    });
  >    ```
  > 3. Kết quả trả về đúng giá trị mã PIN `0x000000000000000000000000000000000000000000000000000000000001e240`. Chứng minh AI đã nhầm lẫn nghiêm trọng giữa khái niệm private của lập trình hướng đối tượng (OOP) truyền thống và tính công khai của Blockchain Storage.

- **Ai phát hiện:** **Sinh viên phát hiện**.

---

### Lần 8 — Lab 4: Xây dựng hợp đồng MyToken.sol và lỗi thời phiên bản OpenZeppelin v5

- **Prompt (dán nguyên văn — áp dụng mẫu Phụ lục II.2):**
  > "Đọc tệp SPEC.md trong dự án và viết mã hợp đồng Solidity MyToken.sol kế thừa OpenZeppelin ERC20, ERC20Burnable, Ownable. Yêu cầu có hàm mint, burn và chức năng đóng băng tài khoản (Blacklist). Tuân thủ mọi quy ước trong AGENTS.md. Trước khi viết mã, hãy tóm tắt lại cách bạn hiểu yêu cầu."

- **AI trả về (tóm tắt):**
  > AI sinh mã hợp đồng có sử dụng hàm hook:
  > ```solidity
  > function _beforeTokenTransfer(address from, address to, uint256 amount) internal override {
  >     require(!isFrozen[from], "From account is frozen");
  >     super._beforeTokenTransfer(from, to, amount);
  > }
  > ```
  > Đồng thời hàm `mint()` cho phép `onlyOwner` đúc số lượng tùy ý không có giới hạn trần tổng cung.

- **Đánh giá:** `[Không đạt] Sai, bỏ` (Lỗi biên dịch do phiên bản thư viện lỗi thời và rủi ro kinh tế).

- **Chỗ sai (mô tả cụ thể):**
  > 1. Thư viện OpenZeppelin v5.x đã **loại bỏ hoàn toàn hàm `_beforeTokenTransfer`** và thay thế bằng cơ chế `_update(address from, address to, uint256 value)`. Khi biên dịch bằng Hardhat, trình biên dịch báo lỗi nghiêm trọng: `DeclarationError: Function has override specified but does not override any base class function`. Đây là bẫy phổ biến của AI do dữ liệu huấn luyện chủ yếu dựa trên OpenZeppelin v4 (đúng như TS. Hà Ngọc Long đã cảnh báo tại trang 30 của Sổ tay).
  > 2. Hàm `mint()` không có trần giới hạn tổng cung (`MAX_SUPPLY`), lặp lại đúng rủi ro kinh tế của Hợp đồng B trong Phụ lục I.2 (chủ sở hữu có thể tự do pha loãng vô hạn làm mất giá trị token của nhà đầu tư).

- **Cách sửa (sinh viên đã làm gì):**
  > 1. Sinh viên đối chiếu với tài liệu OpenZeppelin Contracts v5 chính thức và tệp `AGENTS.md`, chuyển sang override hàm hook `_update(address from, address to, uint256 value)` để chặn cả chiều gửi (`from != address(0) && isFrozen[from]`) và chiều nhận (`to != address(0) && isFrozen[to]`).
  > 2. Khai báo hằng số `uint256 public constant MAX_SUPPLY = 10_000_000 * 10 ** 18;` và bổ sung điều kiện kiểm tra `if (totalSupply() + amount > MAX_SUPPLY) revert MaxSupplyExceeded(...)` vào hàm `mint()`.
  > 3. Thay thế toàn bộ `require` bằng Custom Errors để tối ưu hóa gas.

- **Ai phát hiện:** **Sinh viên phát hiện** (Kiểm tra lỗi biên dịch và đối chiếu chuẩn thiết kế tokenomics).

---

### Lần 9 — Lab 5 & Lab 8: Đặc tả dòng tiền on-chain, thiết kế kinh tế token và phản biện nhà đầu tư thận trọng

- **Prompt (dán nguyên văn — kết hợp mẫu Phụ lục II.6 & yêu cầu Lab 5):**
  > "Dựa trên yêu cầu của LAB 5 — VIẾT ĐẶC TẢ CHO CÔNG CỤ PHÂN TÍCH DÒNG TIỀN trong Sổ tay thực hành:
  > 1. Hãy tạo tệp `tokenomics.md` mô tả đặc tả dòng tiền và quy tắc kinh tế token cho dự án của tôi theo đúng cấu trúc tiêu chuẩn.
  > 2. Đóng vai là một nhà đầu tư thận trọng, hãy tự phản biện dự án bằng cách đưa ra 5 điểm yếu nghiêm trọng nhất (xếp theo mức độ rủi ro giảm dần) và mô tả tình huống cụ thể người dùng bị thiệt hại đối với tệp `tokenomics.md` vừa tạo.
  > 3. Thêm mục 'Phản biện và trả lời' vào cuối tệp `tokenomics.md` để trả lời đầy đủ từng điểm trong 5 phản biện trên (nêu rõ căn cứ chấp nhận rủi ro hoặc phương án xử lý).
  > 4. Cập nhật các câu lệnh và tiến trình công việc này vào tệp `AI_JOURNAL.md` và `SPEC.md`."

- **AI trả về (tóm tắt):**
  > AI phác thảo bản `tokenomics.md` ban đầu. Tuy nhiên, các luận điểm phản biện mang tính "tiếp thị bề nổi" (cho rằng dự án thiếu marketing, cộng đồng chưa đông, giao diện chưa bắt mắt). Ngoài ra, ở phần dòng tiền, AI xem nhẹ bản chất khấu trừ gas của EVM (chỉ tính dòng tiền ra là số token/ETH chuyển đi mà quên cộng phí gas), không đề cập đến việc giao dịch thất bại vẫn làm hao hụt số dư gas, và cho rằng trần 2% nắm giữ trong mã nguồn là đã ngăn chặn triệt để cá mập thao túng.

- **Đánh giá:** `[Lưu ý] Phải sửa` (Thiếu góc nhìn tài chính thực tế và bẫy nhận thức về tính ẩn danh Blockchain).

- **Chỗ sai (mô tả cụ thể):**
  > 1. **Mắc bẫy "Quy tắc kỹ thuật thay thế định danh" (Câu hỏi E5 trang 55):** AI khẳng định quy định `maxHolding = 2%` trong hàm `_update` đã bảo vệ an toàn cho nhà đầu tư khỏi cá voi. Thực tế, kẻ thao túng dễ dàng thực hiện tấn công mạo danh (Sybil Attack) bằng cách phân tán tiền ra 25-30 ví EOA mới để âm thầm thâu tóm 40-50% tổng cung.
  > 2. **Bỏ qua rủi ro tương thích DeFi (Fee-on-Transfer Composability):** AI đề xuất cơ chế trích 1% phí chuyển nhượng về ví Quỹ nhưng không cảnh báo rằng điều này sẽ làm gãy tính tương thích với Uniswap V2 Router chuẩn và các hợp đồng ký quỹ `SimpleEscrow.sol`, gây lỗi thiếu số dư và làm kẹt vĩnh viễn tiền của người dùng.
  > 3. **Nhầm lẫn kế toán dòng tiền on-chain (Lab 5):** AI không đưa quy tắc hạch toán kép: Với giao dịch ra, $\text{Dòng tiền ra} = \text{Value} + \text{Gas Fee}$; và giao dịch `Fail` vẫn bị trừ phí gas mạng.

- **Cách sửa (sinh viên đã làm gì):**
  > 1. Sinh viên áp dụng kỷ luật prompt theo Phụ lục II.6: Ép AI vào vai nhà đầu tư thận trọng, tập trung bóc trần 5 điểm yếu nghiêm trọng nhất xếp theo mức độ giảm dần từ Thảm họa (Critical) đến Trung bình (Low-Medium) kèm tình huống thiệt hại cụ thể.
  > 2. Sinh viên trực tiếp đưa ra các giải pháp công nghệ vững chắc trong mục 5.2 của [tokenomics.md](file:///c:/SmartContractLab/tokenomics.md):
  >    - Chuyển giao Admin Key sang Gnosis Safe Multi-sig ($3/5$) và OpenZeppelin TimelockController hoãn thi hành 48 giờ.
  >    - Áp dụng hợp đồng `TokenVesting.sol` khóa tuyến tính token sáng lập (Cliff 6 tháng, Vesting 18 tháng).
  >    - Kết hợp định danh on-chain qua [contracts/StudentRegistry.sol](file:///c:/SmartContractLab/contracts/StudentRegistry.sol) và Soulbound Token (SBT) để vô hiệu hóa tấn công Sybil.
  >    - Thiết lập danh sách `isFeeExempt` cho các địa chỉ DEX Router/Pair để bảo toàn tính tương thích DeFi.
  >    - Xây dựng mô hình Mua lại & Đốt (Buyback-and-Burn) từ doanh thu dịch vụ thật (Real Yield) để neo giá trị kinh tế.
  > 3. Hoàn thiện toàn diện mục Lab 5 trong [SPEC.md](file:///c:/SmartContractLab/SPEC.md) tuân thủ đúng chuẩn 6 phần và các quy tắc R1–R8 của TS. Long.

- **Ai phát hiện:** **Sinh viên phát hiện** (Dựa trên kiến thức thẩm định rủi ro Lab 4, câu hỏi tình huống E5 và bài toán dòng tiền Lab 5).

---

## BÀI HỌC KINH NGHIỆM VÀ QUY TẮC PHỐI HỢP VỚI AI

1. **AI chỉ biết những gì đã có, không biết những gì vừa cập nhật:** Điển hình là AI thường sinh mã theo OpenZeppelin v4 (vẫn dùng hàm `_beforeTokenTransfer` đã bị xóa bỏ trên bản v5, thay vì hàm `_update`). Sinh viên luôn phải mở tài liệu chính thức (Official Docs) để kiểm chứng.
2. **Nguyên tắc "Prompt có ranh giới cấm" (Negative Constraints):** Khi ra lệnh cho AI, câu quan trọng nhất luôn là: *"Chỉ trả lời dựa trên mã nguồn tôi cung cấp. Nếu không tìm thấy, hãy nói rõ là không tìm thấy, tuyệt đối không được suy đoán hoặc bịa đặt nội dung"*.
3. **Sinh viên là người chịu trách nhiệm cuối cùng:** AI là công cụ gia tăng tốc độ, nhưng năng lực thẩm định nghiệp vụ, rà soát tính hợp lý của số liệu kế toán và đánh giá rủi ro pháp trị là tài sản độc quyền của sinh viên ngành Kinh tế / Fintech.

