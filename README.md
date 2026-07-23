# Credit Risk Model Stability

**Tác giả:** Bùi Hồng Phúc

Đây là dự án giải quyết bài toán dự đoán Rủi ro tín dụng và độ ổn định của mô hình (Credit Risk Model Stability). Dự án tập trung mạnh vào việc xử lý dữ liệu và thiết kế đặc trưng (Feature Engineering) dựa trên những hiểu biết thực tế (domain knowledge) về rủi ro tài chính và tín dụng.

Thư viện xử lý dữ liệu chính được sử dụng là **Polars** nhằm tối ưu hóa tốc độ và giảm thiểu lượng bộ nhớ RAM tiêu thụ.

## Nội dung chính của dự án

Dự án được cấu trúc theo các bước chính sau:

1. **Đọc và Tối ưu dữ liệu (Load & Optimize Data):**
   - Đọc dữ liệu từ các file định dạng Parquet.
   - Ép kiểu dữ liệu (Downcasting) và thiết lập dtypes chuẩn để tiết kiệm RAM.

2. **Thiết kế Đặc trưng (Feature Engineering):**
   Trích xuất đặc trưng đa dạng và bám sát vào hành vi tài chính của khách hàng:
   - **Gánh nặng nợ (Debt Burden & Affordability):** Tính tỷ lệ Nợ trên Thu nhập (DTI), Tỷ lệ nợ quá hạn so với tổng dư nợ.
   - **Xu hướng hành vi thanh toán (Payment Behavior Deterioration):** Phân tích xu hướng số ngày quá hạn (DPD) 3 tháng gần nhất so với 24 tháng, đỉnh trễ hạn trong 9 tháng qua.
   - **Mức độ sử dụng tín dụng (Credit Utilization & Exposure):** Tỷ lệ sử dụng hạn mức, số hợp đồng đang active.
   - **Tín hiệu gian lận (Application Fraud & Behavioral Signals):** Số lượng hồ sơ dùng chung điện thoại/email, tần suất vay trong 30 ngày.
   - **Mức độ ổn định thu nhập (Employment & Income Stability):** Đánh giá qua lịch sử đóng thuế, biến động thu nhập (income volatility).
   - **Lịch sử tín dụng (RFM):** Tần suất quá hạn, tổng số tiền quá hạn, nợ xấu nghiêm trọng (>31 ngày, >90 ngày).
   - **Chất lượng tài sản đảm bảo (Collateral & LTV):** Tổng giá trị tài sản đảm bảo, tỷ lệ Khoản vay / Giá trị tài sản (LTV).
   - **Rủi ro theo thời gian (Temporal Risk):** Hành vi nộp hồ sơ (cuối tháng, đầu tháng, cuối tuần).

3. **Huấn luyện Mô hình (Model Training):**
   Sử dụng ba thuật toán Gradient Boosting phổ biến nhất để huấn luyện:
   - **LightGBM**
   - **XGBoost**
   - **CatBoost**

4. **Đánh giá và Kết hợp (Evaluation & Ensembling):**
   - Mô hình được đánh giá thông qua hai thước đo: **AUC Score** và **Gini Stability Metric** (đo lường độ ổn định của mô hình qua thời gian).
   - Tối ưu hóa trọng số để kết hợp (Weighted Ensemble) cả 3 mô hình (LGBM, XGBoost, CatBoost) nhằm đạt được sự cân bằng tốt nhất giữa hiệu suất (AUC) và tính ổn định.

## Kết quả đạt được (Ensemble model)

- **Best Stability (Validation):** 0.6009
- **Best AUC (Validation):** 0.8223
- **Trọng số tốt nhất (Weights):** LightGBM (0.05), XGBoost (0.60), CatBoost (0.35)

## Các công cụ và thư viện sử dụng
- Ngôn ngữ: `Python`
- Xử lý dữ liệu: `Polars`, `Pandas`, `Numpy`
- Machine Learning: `LightGBM`, `XGBoost`, `CatBoost`, `Scikit-learn`