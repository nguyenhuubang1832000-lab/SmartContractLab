# Lab 6: TimeLockVault - Gas Metrics Report

## 1. Môi trường thử nghiệm
- **IDE**: Remix IDE
- **Compiler**: Solidity ^0.8.20
- **EVM Version**: Cancun (Remix VM)
- **Lock Duration**: 120 seconds

## 2. Kết quả đo đạc Gas Consumed
| Thao tác / Hàm | Execution Cost (Gas) | Transaction Cost (Gas) | Trạng thái |
| :--- | :--- | :--- | :--- |
| **Deployment** | 330,241 | 409,931 | Success |
| **deposit()** | 1,798 | 22,862 | Success (Value: 1 ETH) |
| **withdraw()** | 8,958 | 30,022 | Success (Post-lock duration) |

## 3. Nhận xét & Đánh giá
- Hàm `withdraw()` có phí `execution cost` là **8,958 gas**, cao hơn hàm `deposit()` (**1,798 gas**) do phải thực hiện kiểm tra ràng buộc thời gian (`require(block.timestamp >= unlockTime)`) và chuyển khoản ETH ra khỏi hợp đồng qua `payable(owner).call`.
- Tính năng khóa thời gian hoạt động chính xác: Thao tác `withdraw()` trước 120 giây bị từ chối (revert), và thực hiện thành công sau khi hết thời gian khóa.