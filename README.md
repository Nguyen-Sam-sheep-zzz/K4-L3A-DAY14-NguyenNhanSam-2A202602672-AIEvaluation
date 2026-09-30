# K4 — Level 3A, Ngày 14: Đánh giá và so sánh chất lượng trợ lý AI (225 phút)

**AICB-P1 · Giai đoạn 1 · Ngày 14 trong 15 · K4**

Đây là bài **đánh giá trợ lý AI**. Bộ chấm điểm nằm trong `template.py`;
`golden_dataset.json` có 20 câu hỏi chuẩn; `domain_assistant.py` là trợ lý
RAG trả lời theo tài liệu hỗ trợ khách hàng OrbitTech. Kết quả chạy thật đã
được lưu để phân tích.

> `domain_assistant.py` **trả lời câu hỏi**; `template.py` **chấm câu trả lời**.
> Hai phần độc lập để trợ lý không nhìn thấy đáp án chuẩn khi đang trả lời.

### Xem nhanh kết quả bài làm

```text
10 tài liệu nguồn → 20 câu hỏi chuẩn → 20 câu trả lời RAG thật → 5 chỉ số
→ 3 ca điểm thấp nhất → phân tích 5 lần hỏi “Tại sao?”
```

Đã kiểm tra: **42 test đạt**, bộ 20 câu **PASS** và dùng đủ **10/10** tài liệu.
Kết quả benchmark đã lưu: **16/20 câu đạt theo công thức trùng từ (80%)**.
Đây không phải tỷ lệ đúng về nghĩa; báo cáo giải thích các ca máy chấm nhầm.
Đọc [Hướng dẫn demo tiếng Việt](HUONG_DAN_DEMO_VI.md) để chạy và thuyết trình.

---

## ⚠️ Bài Làm Cá Nhân

**Đây là bài tập cá nhân. Mỗi học viên nộp một repository của riêng mình.**

Tài liệu chính thức của bài lab:

- [SUBMISSION.md](SUBMISSION.md) — cấu trúc bài nộp, tên kho mã và nơi nộp
- [RUBRIC.md](RUBRIC.md) — tiêu chí chấm, bằng chứng và điều kiện mất điểm
- [CHECKPOINTS.md](CHECKPOINTS.md) — sản phẩm, kiến thức và cách tự kiểm tra từng checkpoint
- [RULES.md](RULES.md) — quy định làm bài, dùng AI, hợp tác và bảo mật

### Quy chuẩn đặt tên kho mã

| Vai trò | Tên chuẩn |
|---|---|
| Kho mã đề bài | `K4-L3A-AI-Evaluation` |
| Kho mã học viên nộp | `K4-L3A-DAY14-<HoVaTen>-<MSSV>-AIEvaluation` |
| Ví dụ | `K4-L3A-DAY14-NguyenVanAn-L3A202600280-AIEvaluation` |

> ⚠️ **Đặt sai tên repo = trừ 5 điểm** theo quy định trong [RUBRIC.md](RUBRIC.md).

Bài lab là **bài làm cá nhân**. **Mỗi cá nhân phải tự nộp link repo của mình lên LMS / Codelab** (không nộp hộ, không dùng chung repository).  
Hạn nộp mặc định: **23h59 ngày lab (GMT+7)**; coach có thể gia hạn tối đa ≤48h.

---

## Yêu cầu và cách chạy nhanh trên Windows PowerShell

**Yêu cầu:** Python 3.11 trở lên. Kiểm thử và xem bảng kết quả đã lưu không
cần gọi API. Chỉ cần API key khi sinh câu trả lời mới hoặc bấm nút hỏi trực
tiếp trong giao diện.

```powershell
.venv\Scripts\python.exe -m pytest tests/ -q
.venv\Scripts\python.exe validate_golden_dataset.py
.venv\Scripts\python.exe -m streamlit run app.py
```

Trong giao diện, chọn E01/H03/A01 để xem câu hỏi, đáp án tham khảo và kết quả
đã lưu bằng tiếng Việt. Bản trả lời và đoạn tài liệu gốc vẫn là tiếng Anh để
đối chiếu với benchmark.

**Nếu cài mới từ đầu**, làm theo `guide_lab.md` và `requirements.txt`. Đoạn
lệnh dưới đây là hướng dẫn khởi tạo môi trường của đề gốc, không phải việc
cần chạy lại khi `.venv` đã hoạt động:

```powershell
python --version
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
Copy-Item .env.example .env                              # chỉ để gọi API
```

Chi tiết hướng dẫn theo hệ điều hành và xử lý lỗi: xem [`guide_lab.md`](guide_lab.md).

---

## Mục tiêu

Sau bài thực hành này, học viên có thể:

1. Xây dựng quy trình đánh giá tự động cho trợ lý AI trên 20 câu hỏi.
2. Tính năm chỉ số theo ý tưởng của RAGAS: ba chỉ số câu trả lời và hai chỉ số tìm tài liệu.
3. Thiết kế tiêu chí cho model chấm theo thang 1–5 và kiểm soát thiên lệch.
4. Xây dựng bộ câu hỏi chuẩn theo các mức độ khó.
5. Gom nhóm lỗi và phân tích nguyên nhân bằng 5 lần hỏi “Tại sao?”.
6. Dùng kết quả đánh giá làm điều kiện kiểm tra trước khi phát hành.

---

## Luồng từ câu hỏi đến báo cáo

```text
data/technology_store/*.md
             │
             ├── viết 20 câu chuẩn ──> golden_dataset.json
             │                               │
             └── Trợ lý RAG <── câu hỏi
                       │
                       ├── tìm đoạn tài liệu
                       └── tạo câu trả lời
                                  │
                                  v
                     artifacts/actual_answers.json
                                  │
                    evaluate_answers.py
                                  │
                 template.py (bộ chấm điểm)
                                  │
                                  v
                  artifacts/benchmark_results.json
                                  │
                     exercises.md + reflection.md
```

Khi sinh câu trả lời, `domain_assistant.py` chỉ đọc mã và câu hỏi; nó **không
đọc đáp án chuẩn hoặc bằng chứng chuẩn**. Nhờ đó kết quả không bị lộ đáp án.

---

## Cấu trúc repo

```text
.
├── SUBMISSION.md                # quy định nộp bài, tên repo, deliverables, checklist
├── RUBRIC.md                    # bảng điểm 100, bằng chứng, deductions, bonus
├── CHECKPOINTS.md               # lộ trình CP0–CP5, sản phẩm, cách tự kiểm tra
├── RULES.md                     # quy định cá nhân, AI, hợp tác, bảo mật, deadline
├── README.md                    # tổng quan bài và cách chạy nhanh
├── guide_lab.md                 # hướng dẫn chi tiết từ đầu đến cuối của đề
├── exercises.md                 # bài tập và bảng kết quả
├── reflection.md                # phân tích lỗi, 5 lần hỏi Tại sao và kiểm tra sau sửa
├── template.py                  # bộ chấm điểm đã hoàn thiện
├── solution/
│   └── solution.py              # bản đồng bộ logic chấm điểm để nộp bài
├── domain_assistant.py          # trợ lý RAG OrbitTech được đánh giá
├── evaluate_answers.py          # đọc câu trả lời đã lưu và gọi bộ chấm điểm
├── validate_golden_dataset.py   # kiểm tra cấu trúc và trích dẫn bộ câu hỏi
├── golden_dataset.json          # 20 câu hỏi và đáp án chuẩn gốc
├── demo_vi.py                   # nhãn và diễn giải tiếng Việt cho giao diện
├── HUONG_DAN_DEMO_VI.md         # lệnh chạy và lời thoại thuyết trình
├── data/technology_store/       # corpus tài liệu nguồn của OrbitTech Store
├── tests/                       # bộ unit tests kiểm tra evaluation core
├── requirements.txt
└── .env.example
```

Khi chạy benchmark, các script sẽ tạo thư mục `artifacts/` chứa `actual_answers.json` và `benchmark_results.json` để phục vụ phân tích.

---

## Tổng quan Tasks

### Giao diện demo Streamlit

Sau khi cài dependencies và cấu hình `.env`, chạy trong PowerShell:

```powershell
.venv\Scripts\python.exe -m streamlit run app.py
```

Chọn một mã câu chuẩn để xem kết quả đã lưu và năm chỉ số bằng tiếng Việt mà
không gọi API. Nút **Hỏi trợ lý trực tiếp** sẽ gọi API trong `.env`; câu trả
lời mới có thể khác lần benchmark đã lưu. Với câu hỏi tự nhập, ba chỉ số cần
đáp án chuẩn sẽ để trống, tránh hiển thị điểm giả.

- **Việc 1 — Cấu trúc dữ liệu:** Hoàn thiện `QAPair`, `EvalResult` và phương thức `overall_score()`.
- **Việc 2 — Năm chỉ số:** Tính ba chỉ số câu trả lời (`faithfulness`, `relevance`, `completeness`) và hai chỉ số tìm tài liệu (`context_recall`, `context_precision`).
- **Việc 3 — Model chấm điểm:** Dùng `score_response()` chấm theo tiêu chí và `detect_bias()` tìm thiên lệch.
- **Việc 4 — Chạy 20 câu:** Dùng `BenchmarkRunner` tổng hợp báo cáo và phát hiện điểm giảm hơn 0.05.
- **Việc 5 — Phân tích lỗi:** Dùng `FailureAnalyzer` phân loại, tìm nguyên nhân và lập bảng hành động cải tiến.
- **Việc 6 — Bộ câu hỏi và lần chạy thật:** Viết 20 câu chuẩn, chạy RAG, chấm kết quả và hoàn thiện `reflection.md`.

Chi tiết từng task và checkpoints xem tại [`CHECKPOINTS.md`](CHECKPOINTS.md) và [`guide_lab.md`](guide_lab.md).

---

## Thời gian làm bài

Buổi học diễn ra từ **14:15 đến 18:00**. Hoàn thành bài lab trước **17:00**; thời gian 17:00–18:00 dành cho demo và Q&A.

| Thời gian | Checkpoint | Hoạt động |
|---|---|---|
| 14:15–14:30 | **CP0** Setup | Tạo môi trường, baseline tests (42 failed), cấu hình `.env` |
| 14:30–14:45 | **CP1** Task 1 | Hoàn thành Data Models và `overall_score` (3 passed) |
| 14:45–15:20 | **CP2** Tasks 2–3 | Hoàn thành RAGAS metrics và LLMJudge (21 passed) |
| 15:20–15:40 | **CP3** Tasks 4–5 | BenchmarkRunner, FailureAnalyzer (full suite 41 passed, 1 skipped) |
| 15:40–16:35 | **CP4** Part 3 | Golden Dataset 20 QA, chạy RAG, benchmark thật và rubric |
| 16:35–17:00 | **CP5** Part 4 | Failure analysis, 5 Whys trong `reflection.md`, copy `solution/solution.py` |
| 17:00–18:00 | Wrap-up | Demo, review và Q&A |

---

## Đánh giá & Tiêu chí chấm điểm

| Tiêu chí | Điểm |
|---|---:|
| Core coding hoàn chỉnh, toàn bộ required tests pass | 50 |
| Golden dataset 20 QA đúng schema, stratification và evidence | 15 |
| LLM-as-a-Judge rubric design rõ ràng, domain-specific | 10 |
| Benchmark, 5 Whys, failure analysis và improvement log | 15 |
| Chất lượng code, type hints và regression strategy | 10 |
| **Tổng điểm bắt buộc** | **100** |

Điểm thưởng (Bonus):

| Tiêu chí Bonus | Điểm |
|---|---:|
| Exercise 3.4 — So sánh hai evaluation frameworks | +5 |
| Exercise 3.5 — Reranking và phân tích retrieval metrics | +5 |
| **Tổng bonus tối đa** | **+10** |

> Tổng bonus của bài lab tối đa **10 điểm** (Exercise 3.4 +5, Exercise 3.5 +5). Đây là điểm sản phẩm lab, không phải điểm giơ tay / pitching.

Chi tiết tiêu chí chấm điểm, bằng chứng và các trường hợp trừ điểm xem tại [RUBRIC.md](RUBRIC.md).  
Hướng dẫn nộp bài và checklist trước khi nộp xem tại [SUBMISSION.md](SUBMISSION.md).
