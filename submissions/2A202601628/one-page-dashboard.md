# Operating Dashboard — MedSafe AI

> Bản rút gọn để xuất trang 1 PDF. Mọi giá trị khớp worksheet nguồn
> `operating-dashboard.md`; hai phép tính `[MH]` nằm ở phụ lục trang 2.

**Model:** B2B · **Cập nhật:** 2026-08-28 · **Owner phiên họp:** Product Lead (Đỗ Quý Đức)

**Chẩn đoán:** Bệnh viện ký hợp đồng và trả tiền (5.200.000 ₫/tháng + 3.500 ₫/đơn hoàn tất) · dược sĩ lâm sàng của chính bệnh viện đó là người dùng hằng ngày · đối tác HIS chỉ là kênh phân phối ăn 20% rev-share, không phải bên mua · MedSafe không có bề mặt nào chạm bệnh nhân.

**North Star:** Median time-to-first-value · hiện tại chưa có số (3 pilot đang chạy) · mục tiêu ≤30 ngày · trạng thái 🔴

## Cây đèn 3 tầng

| Tầng · ID | Metric và định nghĩa ngắn | Hiện tại · 🟢 / 🟡 / 🔴 · Nguồn | Nhịp · Owner | Báo trước cho · Luật |
|---|---|---|---|---|
| L · L-01 | **Time-to-first-value** — ngày từ ký đến 1.000 đơn đạt QA, median theo cohort | chưa có số · ≤30 / 31–60 / >60 ngày · [TB] | Mỗi khách · Product Lead | POC-to-paid, NRR · R-01 |
| L · L-02 | **Cảnh báo bị dược sĩ bỏ qua** — alert dismiss ÷ alert hiển thị, đo trên widget HIS | 2,8% (Eval 1.500 ca) · ≤3,5 / 3,6–6,0 / >6,0% · [TB] | Tuần · Clinical Quality Lead | Usage depth, NRR · R-02 |
| L · L-03 | **Bệnh viện partner-sourced go-live ≤30 ngày** ÷ số bệnh viện được giới thiệu | chưa có số · ≥60 / 40–59 / <40% · [TB] | Tuần · Kỹ sư Tích hợp | POC-to-paid, MRR · R-03 |
| O · O-01 | **Usage depth** — dược sĩ có ≥1 phiên duyệt đơn/tuần ÷ dược sĩ được cấp quyền | chưa có số · ≥60 / 30–59 / <30% · [TB] | Tuần · Customer Success | NRR, gia hạn · R-02 |
| O · O-02 | **Chi phí AI/đơn hoàn tất** — (token + retry) ÷ đơn hoàn tất | 0,0042 USD (110 ₫) · ≤0,0080 / 0,0081–0,0250 / >0,0250 USD · [MH] MH-01 | Tuần · FinOps | Gross margin · R-04 |
| O · O-03 | **POC → paid** — pilot ký trả phí ÷ pilot kết thúc (đủ 10.000 đơn) | 0/3 chưa kết thúc · ≥50 / 35–49 / <35% · [BM] ICONIQ GTM 2026 | Quý · Business Development | MRR, payback · R-03 |
| G · G-01 | **Tập trung doanh thu** — doanh thu bệnh viện lớn nhất ÷ tổng doanh thu | 100% (1 pilot) · <20 / 20–30 / >30% · [TB] | Tháng · Finance | Rủi ro doanh thu · R-05 |
| G · G-02 | **GM sau rev-share 20% HIS** — (DT − COGS − rev-share) ÷ DT, theo từng bệnh viện | 65,7% (mô hình) · ≥65 / 55–64,9 / <55% · [MH] MH-02 | Quý · Finance | Runway, payback · R-05 |

## Luật quyết định

| ID | NẾU · TRONG · VÀ | THÌ | KHÔNG THÌ | Dừng? |
|---|---|---|---|---|
| R-01 | TTFV >60 ngày · 2 cohort go-live liên tiếp · ≥3 bệnh viện đã ký/cohort | Đóng băng ký hợp đồng mới 30 ngày; cắt pilot xuống 1 khoa dược ngoại trú, 1 luồng đơn, tới khi median <30 ngày | Không tuyển thêm sales, không mở pilot ra toàn viện để bù tiến độ | **CÓ** |
| R-02 | Cảnh báo bị bỏ qua >6,0% · 3 tuần liên tiếp · ≥3.000 cảnh báo hiển thị | Khoá phát hành rule mới; chuyển cảnh báo mức trung bình sang chế độ im lặng; chạy lại Eval 1.500 ca trước khi bật lại | Không mở rộng bệnh viện mới khi tỷ lệ báo động giả còn trên ngưỡng đỏ | KHÔNG |
| R-03 | Partner-sourced go-live <40% · 60 ngày · ≥5 bệnh viện được giới thiệu | Dừng ký đối tác HIS mới; dồn kỹ sư tích hợp vào kích hoạt bệnh viện đã giới thiệu; đàm phán lại minimum commitment với Infomed | Không dùng "số partner đã ký" làm chỉ số tăng trưởng trong bất kỳ báo cáo nào | **CÓ** |
| R-04 | Chi phí AI/đơn >0,0250 USD · 2 tuần liên tiếp · ≥5.000 đơn hoàn tất | Giới hạn context vào model; bật lại prompt caching toàn bộ Dược thư Master; đàm phán lại quota trước kỳ billing kế tiếp | Không hạ tỷ lệ QA nội bộ 3% để làm cost/job trông thấp hơn | KHÔNG |
| R-05 | GM sau rev-share <55% · 2 quý liên tiếp · ≥6 bệnh viện trả phí | Đàm phán lại rev-share xuống <15%, hoặc chuyển phần đơn vượt 1.000/tháng sang pass-through token cho bệnh viện | Không bù bằng cách ký thêm bệnh viện ở nguyên giá 3.500 ₫/đơn | KHÔNG |

## Cổng 90 ngày

| Ngày | Một metric · ngưỡng | Evidence | Đạt / Trượt |
|---:|---|---|---|
| 30 | Clinical Pilot Report được Hội đồng Dược lâm sàng ký nghiệm thu · ≥2/3 bệnh viện pilot, mỗi báo cáo ≥10.000 đơn thật | Biên bản nghiệm thu có chữ ký Trưởng khoa Dược + export audit log Langfuse theo mã đơn | GO / **FIX** |
| 60 | Median time-to-first-value · ≤30 ngày trên ≥3 bệnh viện | Event log kickoff và mốc 1.000 đơn đạt QA, export riêng theo contract ID | GO / **PIVOT** |
| 90 | Gross margin sau rev-share 20% HIS · ≥55% trên ≥6 bệnh viện trả phí và 40.000 đơn hoàn tất | Billing export ghép hoá đơn token, hoá đơn rev-share Infomed, QA report nội bộ | GO / **KILL** |

**Kill criteria:** Dừng hẳn hướng partner-led qua HIS vào **2026-11-26** (ngày 90) nếu **cả hai** cùng đúng: GM sau rev-share vẫn <55% sau một vòng đàm phán lại tỷ lệ chia, **và** chưa đủ 4 bệnh viện trả phí đạt TTFV <45 ngày. Khi đó giả định gốc "đối tác HIS rút ngắn đường vào bệnh viện" đã sai → cắt toàn bộ chi phí kênh, chuyển sang bán trực tiếp bệnh viện tư, hoặc dừng dự án nếu 3 tháng bán trực tiếp không ký được hợp đồng nào.

**Chưa đo được:** TTFV (chưa bệnh viện nào đi trọn từ ký đến 1.000 đơn QA) · cần gắn event kickoff + mốc QA vào Langfuse, backfill 3 pilot · owner Product Lead · có số 2026-09-30 — CAC thật kênh partner-led (Day 25 mới có ước lượng 2.500 USD) · owner Lead BD · 2026-10-15 — Usage depth (chưa tách user_id dược sĩ khỏi API hệ thống) · owner Kỹ sư Tích hợp · 2026-09-18 — Unit economics dài hạn (file Day 24 dựng cho SmartStudy AI, không dùng lại được) · owner Đỗ Quý Đức · 2026-10-31.
