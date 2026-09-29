LAB 7: TÍNH CHI PHÍ VẬN HÀNH THỰC TẾ (GAS COST & FEASIBILITY REPORT)

## 1. Thông số đầu vào & Bài toán
- **Thao tác**: Ghi một biến mới vào bộ nhớ lâu dài (Thẻ tích điểm CLB).
- **Lượng gas tiêu thụ tham khảo**: 20,000 gas / giao dịch.
- **Tần suất**: 1,000 lượt cộng điểm / tháng.
- **Tổng lượng gas tiêu thụ / tháng**: 1,000 x 20,000 = 20,000,000 gas.
- **Đơn giá gas giả định**: 20 Gwei (20 x 10^-9 ETH).
- **Giá ETH giả định**: $3,000 USD.

---

## 2. Bảng tính chi phí vận hành

| Tiêu chí | Mạng Layer 1 (Ethereum Mainnet) | Mạng Layer 2 (Arbitrum / Optimism / Base) |
| :--- | :--- | :--- |
| **Phí 1 giao dịch (ETH)** | 0.0004 ETH | 0.000004 ETH |
| **Phí 1 giao dịch (USD)** | $1.2 USD (~30,000 VNĐ) | $0.012 USD (~300 VNĐ) |
| **Tổng chi phí 1 tháng (1,000 lượt)** | **$1,200 USD** (~0.4 ETH) | **$12 USD** (~0.004 ETH) |

---

## 3. Trả lời các câu hỏi & Đánh giá tính khả thi

- **a. Chi phí một tháng trên Layer 1 là bao nhiêu USD?**
  - Chi phí 1 tháng trên Layer 1 là **$1,200 USD / tháng** (tương đương 0.4 ETH).
- **b. Chi phí trên mạng Layer 2 (rẻ hơn 100 lần) là bao nhiêu?**
  - Chi phí 1 tháng trên Layer 2 chỉ còn **$12 USD / tháng** (tương đương 0.004 ETH).
- **c. Ai trả khoản này - Câu lạc bộ hay sinh viên? Sinh viên có chấp nhận không?**
  - Khoản phí này có thể do CLB tài trợ hoặc sinh viên tự trả.
  - Trường hợp sinh viên tự trả: Trên Layer 1, sinh viên **chắc chắn KHÔNG chấp nhận** vì việc tốn $1.2 USD (~30,000 VNĐ) cho mỗi lần cộng điểm là quá vô lý và đắt đỏ. Trên Layer 2, mức phí $0.012 USD (~300 VNĐ) là mức chấp nhận được.
- **d. Kết luận: Mô hình này khả thi trên mạng nào?**
  - **Mạng Layer 1 (Ethereum)**: **KHÔNG KHẢ THI** về mặt mô hình kinh doanh do chi phí vận hành cực kỳ đắt đỏ ($1,200/tháng).
  - **Mạng Layer 2**: **RẤT KHẢ THI** vì chi phí vận hành rất rẻ ($12/tháng), hoàn toàn nằm trong quỹ hoạt động của CLB.

---

## 4. Mở rộng ứng dụng cho ý tưởng đồ án nhóm
- **Ý tưởng**: Ký quỹ mua bán đồ cũ KTX (Escrow).
- **Ước tính giao dịch**: 100 giao dịch/tháng.
- **Loại thao tác**: Tạo hợp đồng & Khóa tiền (~100,000 gas/tx).
- **Chi phí L1**: $100 \times (100,000 \times 20 \times 10^{-9} \times 3,000) = $600 USD/tháng (Không khả thi).
- **Chi phí L2**: $600 \div 100 = $6 USD/tháng (Rất khả thi khi triển khai thực tế).
