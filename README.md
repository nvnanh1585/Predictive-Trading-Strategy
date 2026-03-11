# Predictive Trading Pipeline for HNX Stocks

## Giới thiệu

Dự án xây dựng **pipeline dự báo lợi suất cổ phiếu và tạo tín hiệu giao dịch** cho các cổ phiếu niêm yết trên **sàn HNX**.

Mô hình được xây dựng để dự báo lợi suất trong **nhiều horizon (15, 90, 180 ngày)** bằng cách sử dụng dữ liệu tài chính, chỉ báo kỹ thuật và các đặc trưng được xây dựng từ dữ liệu.

Các mô hình học máy được huấn luyện và so sánh, trong đó **XGBoost cho hiệu quả tốt nhất ở horizon ngắn hạn**.
Backtest chỉ được thực hiện cho **horizon 15 ngày**.

---

# 1. Thu thập dữ liệu

### Phạm vi dữ liệu

* Thời gian: **01/01/2023 – 30/06/2025**
* Thị trường: **toàn bộ cổ phiếu niêm yết trên HNX**
* Nguồn dữ liệu: **FiinQuant**

### Dữ liệu thị trường

* `open`
* `high`
* `low`
* `close`
* `volume`

### Dữ liệu kỹ thuật (Technical Indicators)

Các chỉ báo được xây dựng từ dữ liệu giá:

**Moving Average**

* `sma10`
* `sma20`
* `sma50`
* `sma200`

Biến dẫn xuất:

* `sma10_above_sma20`
* `distance_sma20`
* `volume_spike20`

**MACD**

* `macd_signal`
* `macd_diff`

**Ichimoku**

* `senkou_span_a`
* `senkou_span_b`
* `signal_ichimoku`

**RSI**

* `rsi`
* `rsi_ob`
* `rsi_os`

**Bollinger Bands**

* `bollinger_hband`
* `bollinger_lband`
* `bb_position`

Các chỉ báo khác:

* `atr`
* `obv`
* `vwap`

---

### Dữ liệu cơ bản (Fundamental Data)

Từ báo cáo tài chính quý:

* `EBITMargin`
* `ROA`
* `ROE`
* `ROIC`
* `EPS`
* `P/E`
* `P/B`
* Tăng trưởng doanh thu

---

# 2. Xử lý dữ liệu và xây dựng biến

Quy trình xử lý gồm:

* Chuẩn hóa và xử lý dữ liệu
* Xây dựng **biến mục tiêu (target)** cho bài toán dự báo lợi suất
* Áp dụng **clustering** để tạo thêm đặc trưng cho mô hình

---

# 3. Bài toán dự báo

Bài toán được xây dựng dưới dạng **phân loại** nhằm dự báo khả năng lợi suất cổ phiếu **vượt một ngưỡng xác định**.

Các horizon dự báo:

* **15 ngày (ngắn hạn)**
* **90 ngày (trung hạn)**
* **180 ngày (dài hạn)**

Các mô hình học máy được huấn luyện và so sánh giữa các horizon.

---

# 4. Backtest chiến lược giao dịch

Backtest chỉ được thực hiện cho **horizon 15 ngày**.

Chiến lược dựa trên **tín hiệu dự báo của mô hình**.

### Kết quả backtest

Số giao dịch: **107**

| Metric                 | Giá trị     |
| ---------------------- | ----------- |
| Average return / trade | **5.01%**   |
| Win rate               | **57.9%**   |
| Cumulative return      | **~2396%**  |
| Maximum drawdown       | **−51.8%**  |
| Benchmark VN-Index     | **+210.7%** |

---

# 5. Phân tích hiệu suất

Các biểu đồ và phân tích được thực hiện gồm:

* Equity Curve
* Drawdown
* Win/Loss Ratio
* Distribution of per-trade returns
* Per-trade return over time
* Equity curves across cost scenarios
* Bootstrap distribution of cumulative returns
* Walk-forward cumulative return
* Equity with stop loss & take profit

---

# 6. Phân tích khả năng ứng phó với biến động thị trường

Báo cáo đánh giá:

* Khả năng thích nghi của chiến lược trước **các cú sốc thông tin**
* Độ linh hoạt của mô hình khi thị trường biến động
