# Operating Dashboard — MedSafe AI

> Worksheet nguồn để validator và rubric truy vết evidence. Mọi con số truy về
> `DoQuyDuc_Day25_model.xlsx` (tab `1_Cost_Job`, `2_Pricing`, `4_Channel_Fit`,
> `5_90Day_Plan`) và bộ Eval lâm sàng Day 21–22. Bản trang 1 rút gọn nằm ở
> `one-page-dashboard.md` cùng thư mục.

- Học viên: Đỗ Quý Đức
- Mã học viên: 2A202601628
- Mô hình: B2B
- Cập nhật: 2026-08-28
- North Star: Median time-to-first-value ≤30 ngày từ ký hợp đồng đến 1.000 đơn thẩm định đạt QA

## Chẩn đoán mô hình

MedSafe AI là B2B vì bệnh viện và chuỗi phòng khám ký hợp đồng rồi trả tiền trực tiếp cho MedSafe theo cấu trúc Hybrid 5.200.000 ₫/tháng phí nền cộng 3.500 ₫ cho mỗi đơn thuốc thẩm định hoàn tất; người mở sản phẩm mỗi ngày là dược sĩ lâm sàng làm việc cho chính tổ chức trả tiền đó, chứ không phải một tệp người dùng cuối tách rời. Nhà cung cấp HIS (VNPT Healthcare, Viettel Solutions, Infomed EMR) nhận 20% doanh thu định kỳ nhưng chỉ đóng vai trò kênh phân phối và điểm nhúng kỹ thuật, không phải bên mua và không sở hữu quan hệ hợp đồng. Handbook §3.3 ghi rõ biến thể giới thiệu hoặc phân phối không tạo thành B2B2C mà phải dùng bảng B2B, nên MedSafe chốt B2B và giữ rủi ro kênh partner làm một đèn Leading riêng thay vì đổi cả loại mô hình. Bệnh nhân là người chịu tác động cuối cùng nhưng MedSafe không có bề mặt sản phẩm nào chạm tới họ, và điều đó cũng là một dữ kiện phải ghi ra.

| Dữ liệu đầu vào | Trạng thái | Nằm ở đâu hoặc cần gì để đo | Ngày có số |
|---|---|---|---|
| Value Metric và Cost/Job Day 25 | Đo được | `DoQuyDuc_Day25_model.xlsx` tab `1_Cost_Job` và `2_Pricing`: COGS trực tiếp 170,57 USD/tháng chia 8.400 đơn hoàn tất bằng 0,0203 USD/đơn; giá bán 0,1346 USD/đơn | 2026-08-28 |
| Containment và chất lượng lâm sàng Day 21–22 | Đo được | Bộ Eval 1.500 ca đối chiếu Hội đồng Dược lâm sàng: Precision 98,2%, Recall 96,5%, Containment 84,0%, False Positive 2,8% | 2026-08-20 |
| CAC và CAC payback kênh partner-led | Trong 2 tuần | Tab `4_Channel_Fit` mới có ước lượng 2.500 USD/khách chưa có hoá đơn thật; cần cộng chi phí kỹ sư tích hợp, pre-sales và phí trả trước cho đối tác HIS trên 3 hợp đồng đầu | 2026-09-11 |
| Unit economics dài hạn Day 24 (LTV, NPV, runway) | Chưa biết cách đo cho sản phẩm này | File `DoQuyDuc_Day24.xlsx` dựng cho SmartStudy AI (B2C sinh viên), không dùng lại được cho hợp đồng bệnh viện; phải dựng lại mô hình 36 tháng theo chu kỳ mua sắm y tế | 2026-10-31 |

## Kiểm kê đèn ứng viên

| Đèn ứng viên từ handbook | Tầng | Trạng thái | Bằng chứng hiện có hoặc kế hoạch đo |
|---|---|---|---|
| Time-to-first-value (TTFV) | L | 🔧 | Có mốc kickoff trong hợp đồng nhưng chưa gắn event mốc 1.000 đơn đạt QA vào Langfuse; backfill 3 pilot Infomed trước 2026-09-30 |
| Pipeline coverage | L | ❌ | Chưa có CRM chuẩn hoá stage và amount cho pipeline bệnh viện; chưa đặt lịch dựng vì 90 ngày đầu chỉ chạy 3 pilot đã có |
| % deal chết ở khâu security/procurement | L | 🔧 | Đã có Risk và Security Whitepaper (Day 24) nhưng chưa gắn closed-lost reason bắt buộc; thêm trường lý do trước 2026-10-15 |
| POC → paid | O | 🔧 | 3 pilot Infomed đang chạy, chưa pilot nào kết thúc; cohort đầu tiên chốt kết quả 2026-09-30 |
| Sales cycle (ngày) | O | ❌ | Chưa có deal nào đi trọn từ qualified opportunity đến chữ ký; chỉ đo được từ hợp đồng thứ 3 trở đi |
| Usage depth trong tài khoản | O | 🔧 | Event log đã ghi lượt mở widget nhưng chưa gắn user_id dược sĩ, chưa tách khỏi lượt gọi API hệ thống; instrument trước 2026-09-18 |
| Chi phí triển khai ÷ ACV | O | ❌ | Chưa có timesheet kỹ sư tích hợp gắn theo contract ID; bắt đầu ghi giờ từ hợp đồng đầu tiên |
| Tập trung doanh thu | O | ✅ | Billing export theo từng bệnh viện đã có sẵn trong hệ thống hoá đơn; hiện 100% doanh thu pilot đến từ 1 bệnh viện nên đèn này đang đỏ theo định nghĩa |
| NRR | G | ❌ | Chưa có bệnh viện nào đủ 12 tháng hợp đồng; sớm nhất có cohort đầu vào 2027-08-31 |
| Gross Margin | G | ✅ | Tab `2_Pricing` cho GM 84,9% trên đơn giá usage và 73,7% full-loaded, ghép được với hoá đơn token và log QA |
| CAC payback | G | 🔧 | Ước lượng 2,5 tháng dựa trên CAC giả định 2.500 USD; chốt lại bằng chi phí thật của 3 hợp đồng đầu trước 2026-10-15 |

## Đèn báo sớm

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| L-01 | Time-to-first-value | Số ngày từ ngày ký hợp đồng đến ngày bệnh viện đạt 1.000 đơn thuốc thẩm định hoàn tất và được dược sĩ tiếp nhận qua QA; lấy median theo cohort bệnh viện go-live trong quý | Mỗi khách · Product Lead | Chưa có số, 3 pilot đang chạy | ≤30 ngày | 31–60 ngày | Trên 60 ngày | [TB] Handbook §3.2 không có benchmark công bố cho TTFV nên MedSafe tự đặt baseline: đo 2 cohort go-live rồi chốt mốc sau 6 bệnh viện vào 2026-11-30; ranh giới 60 ngày lấy theo quan sát của handbook rằng khách chưa thấy giá trị trong 60 ngày đầu gần như không tái ký | 2026-08-28 | POC-to-paid và NRR năm đầu | R-01 |
| L-02 | Tỷ lệ cảnh báo bị dược sĩ bỏ qua | Số cảnh báo dược sĩ bấm bỏ qua mà không ghi nhận chia tổng số cảnh báo hiển thị trong kỳ; đo trên widget trong màn hình duyệt đơn của HIS, không đo trên log model | Tuần · Clinical Quality Lead | 2,8% trên 1.500 ca Eval | ≤3,5% | 3,6–6,0% | Trên 6,0% | [TB] Ngưỡng 3,5% lấy từ chính KPI pilot tháng 1 của kế hoạch 90 ngày Day 25 và mức False Positive 2,8% của bộ Eval 1.500 ca; chốt baseline production sau 3 bệnh viện và 30.000 cảnh báo thật vào 2026-10-31 | 2026-08-28 | Usage depth và NRR | R-02 |
| L-03 | Tỷ lệ bệnh viện partner-sourced go-live trong 30 ngày | Số bệnh viện do đối tác HIS giới thiệu đã chạy đơn thật trong 30 ngày kể từ ngày giới thiệu chia tổng số bệnh viện được giới thiệu; không tính tài khoản sandbox | Tuần · Kỹ sư Tích hợp | Chưa có số, 0 partner ký chính thức | ≥60% | 40–59% | Dưới 40% | [TB] Day 25 đặt 100% ngân sách GTM vào kênh partner-led (scorecard 29/30) nhưng chưa có một partner nào ký phân phối; đo 2 chu kỳ 30 ngày kể từ hợp đồng Infomed rồi chốt baseline vào 2026-11-30 | 2026-08-28 | POC-to-paid và MRR quý tới | R-03 |

## Đèn vận hành

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| O-01 | Usage depth trong khoa dược | Số dược sĩ được cấp quyền có ít nhất 1 phiên duyệt đơn qua widget trong tuần làm việc chia tổng số dược sĩ được cấp quyền của bệnh viện đó | Tuần · Customer Success | Chưa có số, chưa gắn user_id | ≥60% | 30–59% | Dưới 30% | [TB] Handbook §3.2 để usage depth là baseline tự đặt; MedSafe đo 4 tuần liên tiếp tại 3 bệnh viện pilot rồi chốt mốc vào 2026-10-31, ranh giới 30% lấy theo quy ước handbook coi đó là tín hiệu churn sớm | 2026-08-28 | NRR và tỷ lệ gia hạn năm đầu | R-02 |
| O-02 | Chi phí AI trên mỗi đơn thẩm định hoàn tất | Tổng chi phí token LLM cộng chi phí retry trong kỳ chia số đơn thuốc thẩm định hoàn tất trong kỳ; không gộp hạ tầng và QA nội bộ để đèn này chỉ phản ánh phần inference | Tuần · FinOps | 0,0042 USD tương đương 110 ₫ | ≤0,0080 USD | 0,0081–0,0250 USD | Trên 0,0250 USD | [MH] MH-01 cho trần 0,0378 USD/đơn để giữ gross margin usage ở 60%; đỏ đặt ở 0,0250 USD tức khoảng 66% trần, để khi containment rơi từ 84% xuống 60% thì phần chi phí cố định phân bổ tăng lên vẫn chưa kéo margin xuống dưới 60% trước lúc đèn đổi màu | 2026-08-28 | Gross margin sau rev-share | R-04 |
| O-03 | POC chuyển thành hợp đồng trả phí | Số pilot ký hợp đồng trả phí chia số pilot kết thúc trong kỳ; chỉ tính pilot đã chạy đủ 10.000 đơn thật | Quý · Business Development | Chưa có số, 0/3 pilot kết thúc | ≥50% | 35–49% | Dưới 35% | [BM] ICONIQ State of Go-to-Market 2026 đo POC hoặc free-trial chuyển thành paid khoảng 50% năm 2026 so với 36% năm 2025, https://www.iconiq.com/growth/reports/state-of-go-to-market-2026 — đây là mẫu phần mềm B2B nói chung chứ không riêng y tế Việt Nam, nên dùng 50% làm mốc xanh và hạ đỏ về 35% đúng bằng mức 2025 | 2026-08-28 | MRR quý tới và CAC payback | R-03 |

## Đèn kết quả

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| G-01 | Tập trung doanh thu | Doanh thu tháng từ bệnh viện lớn nhất chia tổng doanh thu tháng | Tháng · Finance | 100% từ 1 bệnh viện pilot | Dưới 20% | 20–30% | Trên 30% | [TB] Handbook §3.2 để tập trung doanh thu là baseline tự đặt; MedSafe giữ nguyên quy ước 20/30% của bảng B2B làm mốc khởi đầu và chốt lại sau khi có 8 bệnh viện trả phí vào 2026-11-30, vì với 8 khách thì một khách rời đi đã là 12,5% doanh thu | 2026-08-28 | Rủi ro doanh thu và runway | R-05 |
| G-02 | Gross margin sau rev-share cho đối tác HIS | Doanh thu trừ COGS trực tiếp trừ 20% rev-share trả đối tác HIS, chia doanh thu; tính theo từng bệnh viện, không lấy trung bình toàn danh mục | Quý · Finance | 65,7% theo mô hình, chưa có hoá đơn | ≥65% | 55–64,9% | Dưới 55% | [MH] MH-02 cho thấy con số 84,9% của Day 25 là gross margin trước rev-share; sau khi trả 20% cho đối tác HIS chỉ còn 65,7%, và nếu cộng cả overhead R&D và support thì rơi về 49,0%, tức dưới mức dự báo 53% của ICONIQ cho AI-native năm 2026 nên sàn đỏ đặt ở 55% | 2026-08-28 | Runway và CAC payback | R-05 |

## Luật quyết định

| ID | NẾU | TRONG | VÀ | THÌ | KHÔNG THÌ | Luật dừng? |
|---|---|---|---|---|---|---|
| R-01 | Median time-to-first-value vượt 60 ngày | 2 cohort go-live liên tiếp | Mỗi cohort có ít nhất 3 bệnh viện đã ký hợp đồng | Đóng băng ký hợp đồng bệnh viện mới trong 30 ngày và cắt phạm vi pilot xuống đúng một khoa dược ngoại trú với một luồng đơn cho tới khi median về dưới 30 ngày | Không tuyển thêm sales và không mở rộng pilot ra toàn viện để bù tiến độ chậm | CÓ |
| R-02 | Tỷ lệ cảnh báo bị dược sĩ bỏ qua vượt 6,0% | 3 tuần liên tiếp | Có ít nhất 3.000 cảnh báo được hiển thị trong kỳ | Khoá phát hành rule mới, chuyển toàn bộ cảnh báo mức trung bình sang chế độ im lặng và chạy lại bộ Eval 1.500 ca trước khi bật lại | Không mở rộng thêm bệnh viện mới trong lúc tỷ lệ báo động giả còn trên ngưỡng đỏ | KHÔNG |
| R-03 | Tỷ lệ bệnh viện partner-sourced go-live trong 30 ngày dưới 40% | 60 ngày | Đã có ít nhất 5 bệnh viện được đối tác HIS giới thiệu | Dừng ký thêm đối tác HIS mới, dồn toàn bộ kỹ sư tích hợp vào kích hoạt các bệnh viện đã được giới thiệu và đàm phán lại điều khoản minimum commitment với Infomed | Không dùng số đối tác HIS đã ký làm chỉ số tăng trưởng trong bất kỳ báo cáo nào cho giảng viên hay nhà đầu tư | CÓ |
| R-04 | Chi phí AI trên mỗi đơn hoàn tất vượt 0,0250 USD | 2 tuần liên tiếp | Có ít nhất 5.000 đơn hoàn tất trong kỳ | Giới hạn context đưa vào model, bật lại prompt caching cho toàn bộ Dược thư Master và đàm phán lại quota với nhà cung cấp model trước kỳ billing kế tiếp | Không hạ tỷ lệ QA nội bộ 3% xuống để làm cost per job trông thấp hơn | KHÔNG |
| R-05 | Gross margin sau rev-share dưới 55% | 2 quý liên tiếp | Có ít nhất 6 bệnh viện trả phí trong kỳ | Đàm phán lại tỷ lệ chia doanh thu với đối tác HIS xuống dưới 15% hoặc chuyển phần đơn vượt 1.000 đơn/tháng sang cơ chế pass-through chi phí token cho bệnh viện | Không bù bằng cách ký thêm bệnh viện ở nguyên mức giá 3.500 ₫ mỗi đơn | KHÔNG |

## Cổng gác 90 ngày

| Ngày | Metric gác cổng | Ngưỡng | Bằng chứng vật lý | Nếu đạt | Nếu trượt |
|---:|---|---|---|---|---|
| 30 | Số bệnh viện pilot có Clinical Pilot Report được Hội đồng Dược lâm sàng ký nghiệm thu | Ít nhất 2 trong 3 bệnh viện pilot, mỗi báo cáo dựa trên tối thiểu 10.000 đơn thật | Biên bản nghiệm thu có chữ ký Trưởng khoa Dược kèm export audit log Langfuse theo mã đơn | GO | FIX |
| 60 | Median time-to-first-value trên các bệnh viện đã go-live | Không quá 30 ngày trên ít nhất 3 bệnh viện | Event log kickoff và mốc 1.000 đơn đạt QA, export riêng theo từng contract ID | GO | PIVOT |
| 90 | Gross margin sau rev-share 20% cho đối tác HIS | Từ 55% trở lên trên ít nhất 6 bệnh viện trả phí và 40.000 đơn hoàn tất | Billing export ghép hoá đơn token, hoá đơn rev-share của Infomed và QA report nội bộ | GO | KILL |

## Kill criteria

Dừng hẳn hướng partner-led qua HIS vào 2026-11-26 (ngày 90 kể từ 2026-08-28) nếu cả hai điều sau cùng đúng: gross margin sau rev-share vẫn dưới 55% sau một vòng đàm phán lại tỷ lệ chia, và chưa có đủ 4 bệnh viện trả phí đạt time-to-first-value dưới 45 ngày. Khi đó giả định gốc rằng đối tác HIS rút ngắn được đường vào bệnh viện đã sai, MedSafe cắt toàn bộ chi phí kênh và chuyển sang bán trực tiếp cho bệnh viện tư, hoặc dừng dự án nếu 3 tháng bán trực tiếp không ký được hợp đồng nào.

## Chưa đo được

| Đèn hoặc giả định | Cần gì để đo | Ai chịu trách nhiệm | Ngày có số |
|---|---|---|---|
| Time-to-first-value — chưa bệnh viện nào đi trọn từ ngày ký đến mốc 1.000 đơn đạt QA | Gắn event kickoff và event mốc 1.000 đơn đạt QA vào Langfuse, backfill thủ công cho 3 pilot Infomed đang chạy | Product Lead (Đỗ Quý Đức) | 2026-09-30 |
| CAC thực tế của kênh partner-led — Day 25 mới có ước lượng 2.500 USD chưa có hoá đơn | Cộng chi phí kỹ sư tích hợp theo giờ, chi phí pre-sales và phí trả trước cho đối tác HIS, gắn theo contract ID của 3 hợp đồng đầu | Lead Business Development | 2026-10-15 |
| Usage depth — event log chưa tách lượt dược sĩ mở widget khỏi lượt gọi API hệ thống | Gắn user_id dược sĩ vào event mở widget trong HIS và dựng bảng weekly active theo từng tài khoản bệnh viện | Kỹ sư Tích hợp | 2026-09-18 |
| Unit economics dài hạn (LTV, CAC payback, NPV, runway) — file Day 24 dựng cho sản phẩm SmartStudy AI, không dùng lại được | Dựng lại mô hình 36 tháng theo hợp đồng bệnh viện, chu kỳ mua sắm y tế và tỷ lệ gia hạn thực tế thay vì churn tiêu dùng | Đỗ Quý Đức | 2026-10-31 |

## Phụ lục ngưỡng suy từ mô hình

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | Trần chi phí AI trên mỗi đơn thẩm định hoàn tất | Giá bán 0,1346 USD/đơn hoàn tất; gross margin usage mục tiêu 60% (ngưỡng sống còn Day 25 tab `2_Pricing`); chi phí biến đổi phi-AI gồm hạ tầng 60,10 USD/tháng và QA nội bộ 75,00 USD/tháng; mẫu số 8.400 đơn hoàn tất/tháng ở containment 84% | 0,1346 × (1 − 0,60) = 0,05384 và (60,10 + 75,00) ÷ 8.400 = 0,01608 nên trần AI = 0,05384 − 0,01608 = 0,03776 USD/đơn | Trần tuyệt đối 0,0378 USD/đơn. Đèn O-02 đặt đỏ ở 0,0250 USD tức 66% trần, chừa buffer cho kịch bản containment rơi về 60%: khi đó mẫu số còn 6.000 đơn, chi phí phi-AI phân bổ lên 135,10 ÷ 6.000 = 0,02252 USD/đơn và trần AI tụt còn 0,05384 − 0,02252 = 0,0313 USD/đơn, vẫn cao hơn mức đỏ. Baseline hiện tại 0,0042 USD/đơn nên xanh đặt ở 0,0080 USD, khoảng 1,9 lần baseline |
| MH-02 | Gross margin sau rev-share 20% cho đối tác HIS | Doanh thu tháng một bệnh viện tiêu chuẩn: phí nền 200 USD cộng 7.400 đơn vượt quota 1.000 đơn nhân 0,1346 USD; COGS trực tiếp 170,57 USD/tháng từ tab `1_Cost_Job`; overhead R&D và support 200,00 USD/tháng; tỷ lệ rev-share 20% doanh thu theo tab `4_Channel_Fit` | Doanh thu = 200 + 7.400 × 0,1346 = 1.196,04 USD; rev-share = 0,20 × 1.196,04 = 239,21 USD; GM sau rev-share = (1.196,04 − 170,57 − 239,21) ÷ 1.196,04 = 786,26 ÷ 1.196,04 = 0,657; GM full-loaded sau rev-share = (1.196,04 − 370,57 − 239,21) ÷ 1.196,04 = 0,490 | 65,7% là mức mô hình hiện tại, thấp hơn 84,9% của Day 25 đúng 19,2 điểm phần trăm vì Day 24–25 chưa trừ rev-share. Đèn G-02 đặt xanh từ 65%, đỏ dưới 55%: dưới 55% thì bản full-loaded rơi xuống khoảng 38% và không còn nuôi nổi chi phí kênh, nên đó là điểm phải đàm phán lại tỷ lệ chia theo luật R-05 |

## Ghi nhận AI critique

| Phản biện | Chấp nhận hay bác bỏ | Thay đổi đã thực hiện | Lý do |
|---|---|---|---|
| Gross margin 84,9% trong Day 25 là con số trước rev-share, dashboard đang khoe một biên không tồn tại | Chấp nhận | Thay đèn Gross Margin bằng Gross margin sau rev-share và dựng MH-02 để tính lại còn 65,7%, full-loaded 49,0% | Đối tác HIS lấy 20% doanh thu định kỳ là chi phí chắc chắn xảy ra, không phải rủi ro giả định |
| Nên dùng benchmark NRR trung vị 101% của Benchmarkit làm đèn kết quả cho đủ bộ | Bác bỏ | Giữ Tập trung doanh thu làm đèn kết quả thứ hai, đưa NRR vào phần chưa đo được | MedSafe chưa có bệnh viện nào đủ 12 tháng hợp đồng nên NRR sớm nhất có số vào 2027-08-31, đặt đèn bây giờ chỉ là trang trí |
| Với 100% GTM đặt vào một kênh partner, thiếu đèn nào bắt được việc partner ký rồi không đẩy | Chấp nhận | Thêm L-03 đo tỷ lệ bệnh viện partner-sourced go-live trong 30 ngày và luật dừng R-03 cấm dùng số partner đã ký làm chỉ số tăng trưởng | Day 25 chốt partner-led 29/30 điểm nhưng chưa partner nào ký, đây là giả định lớn nhất chưa có đèn canh |
