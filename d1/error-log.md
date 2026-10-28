# CCA-F Error Log

## 2026-10-27 · Baseline: 12 câu mẫu Exam Guide (trang 27–33)

Điểm: 7/12 (58%)

| Domain | Câu | Đúng | Tỷ lệ |
|---|---|---|---|
| D1 Agentic Architecture & Orchestration (27%) | 1, 7 | 1/2 | 50% |
| D2 Tool Design & MCP Integration (18%) | 2, 9 | 0/2 | 0% |
| D3 Claude Code Configuration & Workflows (20%) | 4, 5, 6, 10 | 3/4 | 75% |
| D4 Prompt Engineering & Structured Output (20%) | 11, 12 | 2/2 | 100% |
| D5 Context Management & Reliability (15%) | 3, 8 | 1/2 | 50% |

Ước tính theo tỷ trọng đề: khoảng 56%. Mẫu chỉ có 12 câu, mỗi domain 2–4 câu, nên chỉ xem là tín hiệu ban đầu.

| Câu | Domain / Task | Chọn | Đáp án | Vì sao sai | Pattern bẫy |
|---|---|---|---|---|---|
| Q1 | D1 · 1.4 Enforcement | B: thêm vào system prompt "bắt buộc gọi get_customer" | A: chặn bằng code, lookup_order/process_refund chỉ chạy khi đã có customer ID xác minh | Prompt chỉ làm LLM tuân thủ theo xác suất; lỗi gây thiệt hại tiền (hoàn tiền sai) cần đảm bảo chắc chắn | Logic nghiệp vụ quan trọng + hậu quả tài chính → enforcement bằng code, không dùng prompt/few-shot |
| Q2 | D2 · 2.1 Tool description | C: thêm routing layer chọn tool theo keyword | B: viết lại tool description (input, ví dụ, edge case, khi nào dùng tool nào) | Gốc lỗi là mô tả tool quá sơ sài; routing layer là over-engineering và bỏ qua khả năng hiểu ngôn ngữ của LLM | "First step" + gốc lỗi rõ ràng → sửa gốc rẻ nhất; loại các đáp án xây thêm hạ tầng |
| Q8 | D5 · 5.3 Error propagation | B: retry + exponential backoff, cuối cùng trả "search unavailable" chung chung | A: trả error context có cấu trúc (loại lỗi, query, kết quả dở dang, hướng thay thế) | Thông báo lỗi chung chung che mất ngữ cảnh, coordinator không đủ thông tin để quyết định phục hồi | Câu hỏi nói "intelligent recovery" → chọn đáp án đưa nhiều ngữ cảnh lên coordinator; loại nuốt lỗi, che lỗi, dừng toàn bộ |
| Q9 | D2 · 2.3 Phân bổ tool | D: web search agent cache trước ngữ cảnh | A: cho synthesis agent một tool verify_fact giới hạn cho 85% ca đơn giản, ca khó vẫn đi qua coordinator | Cache đón trước là đoán mò, không chắc trúng thứ cần kiểm | Có số liệu chia ca (85/15) → giải pháp phục vụ đúng ca phổ biến + least privilege; loại cách "đoán trước" và cách cấp toàn quyền |
| Q10 | D3 · 3.6 CI/CD | B: đặt biến CLAUDE_HEADLESS=true | A: dùng cờ -p (--print) | CLAUDE_HEADLESS không tồn tại; -p là cách chính thức chạy non-interactive | Đáp án nhiễu hay bịa tính năng nghe hợp lý (CLAUDE_HEADLESS, --batch, .claude/config.json) → phải thuộc cờ/thư mục thật |

## 2026-10-27 · Diagnostic Test (claudecertificationguide.com)

Điểm: 8/10 (80%), mỗi domain 2 câu

| Domain | Đúng |
|---|---|
| D1 Agentic Architecture & Orchestration | 2/2 |
| D2 Tool Design & MCP Integration | 2/2 |
| D3 Claude Code Configuration & Workflows | 2/2 |
| D4 Prompt Engineering & Structured Output | 1/2 |
| D5 Context Management & Reliability | 1/2 |

| Câu | Domain / Task | Chọn | Đáp án | Vì sao sai | Pattern bẫy |
|---|---|---|---|---|---|
| Diag Q07 | D4 · 4.1 Explicit criteria | A: thêm "only report high-confidence documentation issues" vào system prompt | C: tạm tắt category documentation mismatch, đồng thời viết lại prompt của nó với tiêu chí rõ ràng và ví dụ code | Đề đã nói cách này từng thử và không hiệu quả; yêu cầu "high-confidence" mơ hồ không cải thiện precision. Thêm lượt kiểm tra (B) bị loại vì không có ngân sách; hạ thành ghi chú thu gọn (D) bị loại vì dev vẫn mất tin khi thấy kết quả sai | Mỗi ràng buộc trong đề loại một đáp án ("đã thử" → A, "không thêm model call" → B, "thấy là mất tin dù đã thu gọn" → D). Một category nhiễu kéo tụt niềm tin vào toàn bộ → tắt tạm rồi sửa tiêu chí |
| Diag Q10 | D5 · 5.2 Escalation | C: sentiment analysis, escalate khi khách bực | D: thêm tiêu chí escalate rõ ràng + few-shot vào system prompt | Sentiment không tương quan với độ phức tạp của ca: ca đơn giản mà khách bực sẽ bị escalate, ca phức tạp mà khách bình tĩnh bị bỏ sót | Gần giống Q3 của Exam Guide (làm đúng ở bài đó). Bản này thêm ràng buộc để loại đáp án: "không có dữ liệu ticket" → loại classifier, "agent tự tin cao ở cả hai loại ca" → loại self-confidence. Còn lại sentiment (sai vấn đề) và tiêu chí + few-shot (đúng gốc) |

## Baseline gộp (22 câu)

| Domain (tỷ trọng) | Exam Guide | Diagnostic | Gộp |
|---|---|---|---|
| D1 Agentic Architecture (27%) | 1/2 | 2/2 | 3/4 (75%) |
| D2 Tool Design & MCP (18%) | 0/2 | 2/2 | 2/4 (50%) |
| D3 Claude Code Config (20%) | 3/4 | 2/2 | 5/6 (83%) |
| D4 Prompt Engineering (20%) | 2/2 | 1/2 | 3/4 (75%) |
| D5 Context & Reliability (15%) | 1/2 | 1/2 | 2/4 (50%) |
| Tổng | 7/12 | 8/10 | 15/22 (68%) |

Ưu tiên ôn: D5 (sai ở cả hai bài), sau đó D2 và D4.

### Pattern chung rút ra

1. Proportionality: câu hỏi có chữ "first step" hoặc "most effective" thì ưu tiên sửa đúng gốc với chi phí thấp nhất (Q2, Q3). Classifier, routing layer, ML model riêng thường là bẫy over-engineering.
2. Ngoại lệ của (1): khi sai sót gây hậu quả nghiêm trọng (tiền, bảo mật), cần đảm bảo chắc chắn bằng code, prompt không đủ (Q1).
3. Multi-agent: lỗi và thông tin phải đi lên coordinator kèm đủ ngữ cảnh (Q8); subagent chỉ được cấp đúng tool cần dùng (Q9).
4. Claude Code: học thuộc cờ và đường dẫn thật (-p, .claude/commands/, .claude/rules/, ~/.claude/), vì đề hay cài tính năng bịa nghe rất hợp lý (Q10).
5. Đọc ràng buộc trong đề như luật loại trừ: "đã thử và không hiệu quả", "không có ngân sách", "không có dữ liệu", "tự tin cao ở cả hai loại" thường loại thẳng một đáp án. Gạch các đáp án bị loại trước rồi mới chọn (Diag Q07, Diag Q10).
6. Escalation: quyết định dựa trên độ phức tạp của ca và chính sách công ty. Sentiment và self-confidence đều là bẫy (Exam Guide Q3, Diag Q10).
