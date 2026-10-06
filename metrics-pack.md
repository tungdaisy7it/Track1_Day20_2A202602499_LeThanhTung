# Metrics Pack — AI Personal Assistant for Students

**Họ tên:** Lê Thanh Tùng  
**MHV:** 2A202602499  
**Dự án:** AI Personal Assistant for Students  
**Use case chính:** Quản lý và hoàn thành nhiệm vụ học tập  
**Persona:** Sinh viên năm cuối có nhiều môn học, bài tập và deadline

---

# 00 — Dự án, persona, core job

| Thành phần | Nội dung |
|---|---|
| Dự án | AI Personal Assistant for Students |
| Persona | Sinh viên năm cuối có nhiều môn học, bài tập và deadline cùng lúc |
| Core job | "Tôi muốn biết việc học nào cần làm và hoàn thành đúng hạn mà không phải tự ghi nhớ, tìm lại thông tin hoặc theo dõi quá nhiều nơi." |

### Phạm vi use case

Chỉ phân tích workflow **quản lý và hoàn thành một nhiệm vụ học tập**. Không mở rộng sang toàn bộ chức năng của trợ lý AI.

---

# 01 — Core Action

## 1. Phân biệt bốn khái niệm

| Khái niệm | Câu trả lời |
|---|---|
| Core job | Biết việc học nào cần làm và hoàn thành đúng hạn |
| Core action | **Hoàn thành một nhiệm vụ học tập đã được đưa vào kế hoạch** |
| Core value | Sinh viên biến một nghĩa vụ học tập cụ thể thành công việc đã hoàn thành, giảm nguy cơ bỏ sót deadline |
| Core value event | `task_completed` |

### Vì sao chọn core action này?

`task_completed` gần value hơn các thao tác như mở app, đăng nhập, hỏi AI hoặc xem danh sách nhiệm vụ. Người dùng thực sự nhận được giá trị khi một nhiệm vụ học tập được hoàn tất, chứ không phải chỉ khi hệ thống tạo ra một câu trả lời.

## 2. Core Action Card

| Thành phần | Câu trả lời |
|---|---|
| Target user | Sinh viên sử dụng AI Personal Assistant |
| Core job | Hoàn thành các nhiệm vụ học tập đúng hạn mà không phải tự theo dõi nhiều nguồn |
| Core action | Hoàn thành một nhiệm vụ học tập đã được đưa vào kế hoạch |
| Object | Một study task cụ thể |
| Preconditions | Task đã tồn tại, thuộc về user và ở trạng thái chưa hoàn thành |
| Completion rule | User thực hiện hành động hoàn tất và task chuyển từ `pending/in_progress` sang `completed` |
| Core value | Một nghĩa vụ học tập cụ thể được hoàn thành và trạng thái kế hoạch được cập nhật |
| Evidence of value | Task chuyển trạng thái thành `completed`; task không còn nằm trong nhóm việc cần hoàn thành |
| Candidate event | `task_completed` |

## 3. Tự kiểm 5 tiêu chí

| Tiêu chí | Kết quả | Lý do |
|---|---|---|
| Gần core value | Đạt | Hoàn thành task chính là kết quả mà user cần đạt |
| Có thể lặp lại | Đạt | Mỗi task khác nhau tạo ra một lần completion |
| Có thể quan sát | Đạt | Có trạng thái chuyển từ chưa hoàn thành sang hoàn thành |
| Có ý nghĩa | Đạt | Completion tăng phản ánh số nghĩa vụ học tập thực sự được xử lý |
| Có thể tác động | Đạt | Team có thể cải thiện khả năng hoàn thành bằng ưu tiên task, chia nhỏ việc, nhắc đúng ngữ cảnh và hỗ trợ AI |

**Kết luận Gate 1:** Đạt 5/5. Core action có actor, object và completion rule; không phải thao tác mở app/hỏi AI và cũng không phải output do hệ thống tự tạo.

---

# 02 — Action Nature + cadence

## Action Nature Card

| Thành phần | Câu trả lời |
|---|---|
| Actor | Sinh viên / user account |
| Intent | Xử lý một nghĩa vụ học tập đang cần hoàn thành |
| Trigger | Chủ yếu do deadline, lịch học, kế hoạch cá nhân hoặc task phát sinh |
| Effort | Phụ thuộc loại task; có thể từ vài phút đến nhiều giờ |
| Value timing | Value xuất hiện khi task thực sự hoàn thành; một số task có value tích lũy trong quá trình thực hiện |
| State | Task giữ trạng thái, deadline, priority, completion time và lịch sử xử lý |
| Dependency | Có thể phụ thuộc lịch học, deadline, tài liệu, giảng viên hoặc thành viên nhóm |
| Repeat condition | Xuất hiện khi có task học tập mới hoặc task tiếp theo đến thời điểm cần xử lý |

## Kết luận cadence

> Đối với **sinh viên sử dụng AI Personal Assistant**, core action **hoàn thành một nhiệm vụ học tập đã được đưa vào kế hoạch** thường xuất hiện **theo nhịp phát sinh và deadline của nhiệm vụ học tập, thường tập trung theo ngày/tuần nhưng không cố định**, vì **nhu cầu học tập đến từ lịch học, bài tập, deadline và workload thực tế chứ không phải từ việc mở ứng dụng**. Do đó, nhịp đo phù hợp là **theo tuần ở cấp user-cohort, đồng thời quan sát completion theo từng task và deadline**.

### Dạng hành vi

**Workflow có tính tiến trình, kết hợp event-response.**

Không chọn daily làm bản chất bắt buộc. Daily có thể là một pattern quan sát được ở một số sinh viên, nhưng không phải điều kiện để định nghĩa value.

**Kết luận Gate 2:** Đạt. Cadence được suy ra từ nguồn phát sinh task và deadline, không phải từ dashboard.

---

# 03 — Metric System

## 1. Activation

### Start event

`first_task_created`

User bắt đầu journey khi tạo hoặc đưa nhiệm vụ học tập đầu tiên vào hệ thống.

### Activation event

`task_completed`

User được xem là activated khi hoàn thành **ít nhất 1 task** đã được tạo trong hệ thống.

### Time window

**Trong vòng 7 ngày kể từ `first_task_created`.**

Lý do: khoảng thời gian này đủ để một sinh viên trải qua ít nhất một vòng lập kế hoạch → thực hiện → hoàn thành task, nhưng không quá dài để mất ý nghĩa của first value.

### Activation metric

> **Activation Rate = số user hoàn thành ≥1 task trong 7 ngày / số user tạo task đầu tiên**

Activation ở đây không phải hoàn thành onboarding, đăng nhập hay gửi câu hỏi cho AI.

---

## 2. Engagement

Chọn hai góc đo:

### Frequency

**Số task được hoàn thành trên mỗi active user trong một tuần.**

Vì core action có thể lặp lại khi sinh viên xử lý nhiều nhiệm vụ học tập.

### Depth

**Tỷ lệ task đã bắt đầu được hoàn thành.**

Công thức khái niệm:

`completed_tasks / started_tasks`

Depth giúp phân biệt việc user tạo nhiều task với việc thực sự xử lý chúng.

---

## 3. North Star Metric

### NSM

> **Số task học tập được hoàn thành đúng hạn, mỗi task chỉ được tính khi đạt trạng thái `completed`, theo mỗi active student mỗi tuần.**

Cấu trúc:

- **Unit of value:** một task học tập được hoàn thành
- **Quality threshold:** task hoàn thành trước hoặc tại deadline
- **Frequency:** mỗi active student mỗi tuần

### Vì sao không dùng số lần hỏi AI?

Vì nhiều câu hỏi không đồng nghĩa với nhiều giá trị. Một user có thể hỏi AI nhiều lần nhưng vẫn không hoàn thành nhiệm vụ. NSM cần phản ánh outcome gần với core value hơn.

---

## 4. Leading indicators

### Leading indicator 1 — `task_started_rate`

**Tỷ lệ task được chuyển sang `in_progress`.**

Vì: nếu task được bắt đầu đúng lúc thì xác suất hoàn thành sẽ cao hơn so với task chỉ được tạo rồi bỏ đó.

### Leading indicator 2 — `task_plan_completion_rate`

**Tỷ lệ task có đủ thông tin cần thiết để thực hiện: deadline, priority và next step.**

Vì: task có trạng thái kế hoạch rõ ràng làm giảm friction trước khi bắt đầu và có thể dự báo khả năng chuyển sang thực hiện.

### Leading indicator 3 — `overdue_task_rate`

**Tỷ lệ task vượt deadline mà chưa hoàn thành.**

Vì: overdue tăng là tín hiệu sớm cho thấy user đang gặp vấn đề trước khi completion/retention giảm.

---

## 5. Counter-metric

### `task_overdue_rate`

Core action tăng nhưng **tỷ lệ task quá hạn không được tăng**.

Lý do: hệ thống có thể làm completion count tăng bằng cách khuyến khích user hoàn thành các task nhỏ/dễ trước, trong khi task quan trọng vẫn bị trễ. Counter-metric giúp phát hiện việc tối ưu số lượng completion nhưng làm giảm chất lượng kết quả học tập.

**Metric bổ sung để theo dõi chất lượng AI:** `ai_suggestion_ignore_rate`.

Nếu hệ thống đưa ra nhiều gợi ý nhưng user liên tục bỏ qua, việc tăng tương tác AI không nên được xem là thành công.

**Kết luận Gate 3:** Đạt. Activation có start event + activation event + window; NSM có unit/value + quality threshold + frequency; có leading indicators và counter-metric.

---

# 04 — Retention Definition

## Retention chính

| Thành phần | Định nghĩa |
|---|---|
| Unit | Student/user account |
| Cohort entry | User phát sinh `first_task_created` |
| Return event | User thực hiện lại `task_completed` |
| Window | Weekly |
| Threshold | ≥ 1 task hoàn thành trong tuần retention |
| Segment | User đã được activation bằng cách hoàn thành ≥1 task trong 7 ngày đầu |

### Định nghĩa hoàn chỉnh

> **Weekly student retention** là tỷ lệ user trong một cohort bắt đầu bằng `first_task_created`, đã activation trong 7 ngày đầu, sau đó thực hiện ít nhất một `task_completed` trong một tuần retention được xác định trước.

### Vì sao không dùng D7 retention?

D7 có thể dùng như một mốc phân tích, nhưng không phải retention definition chính vì core action có cadence gắn với task/deadline và không nhất thiết xuất hiện đúng mỗi 7 ngày. Weekly retention phù hợp hơn để quan sát hành vi lặp lại theo workload học tập.

### Ba mốc so sánh

1. **Natural cycle:** nhịp task/deadline thực tế của sinh viên.
2. **Cohort segment:** so sánh các cohort sinh viên có mức workload tương đồng.
3. **Category benchmark:** benchmark của các sản phẩm productivity/student task management nếu có dữ liệu đáng tin cậy.

Không đặt một ngưỡng retention cứng kiểu "30% là tốt" nếu chưa có benchmark phù hợp.

---

# 05 — Product Loop

## Loại loop chính

**Progress loop kết hợp event-response loop.**

## Chu kỳ 1

```text
Task/deadline phát sinh
        ↓
User mở kế hoạch và chọn task cần xử lý
        ↓
Core action: hoàn thành task
        ↓
Immediate value: task được đánh dấu completed + kế hoạch được cập nhật
        ↓
Saved state: lịch sử completion + trạng thái workload được lưu
        ↓
Task tiếp theo đến thời điểm cần xử lý
```

## Chu kỳ 2

```text
Task tiếp theo đến thời điểm cần xử lý
        ↓
User xem priority/next step
        ↓
Core action: hoàn thành task tiếp theo
        ↓
Immediate value: workload giảm + tiến độ được cập nhật
        ↓
Saved state: completion history + progress được tích lũy
        ↓
Nhiệm vụ tiếp theo
```

### Reason to return nếu bỏ notification

User quay lại vì **còn nhiệm vụ học tập cần xử lý và deadline/workload tiếp tục thay đổi**. Notification chỉ là nurture giúp user nhận biết trigger sớm hơn, không phải nguồn tạo ra nhu cầu.

## Metric hypothesis

> **Nếu loop này hoạt động, weekly student retention sẽ tăng và số task hoàn thành đúng hạn trên mỗi active student mỗi tuần sẽ tăng trong 4–8 tuần, vì mỗi lần hoàn thành task tạo ra tiến độ được lưu lại và làm rõ nhiệm vụ tiếp theo cần xử lý.**

### Giả thuyết phụ

> Nếu hệ thống giảm friction trước khi bắt đầu task, `task_started_rate` sẽ tăng và sau đó `task_completion_rate` cũng tăng, trong khi `task_overdue_rate` không tăng.

**Kết luận Gate 4:** Loop có hai chu kỳ, reason to return đến từ nhu cầu học tập tự nhiên và metric hypothesis trỏ trực tiếp về NSM/leading metrics.

---

# 06 — Tracking nhanh

## Core events

| Tên event | Ý nghĩa | Thời điểm ghi nhận | Metric sử dụng |
|---|---|---|---|
| `first_task_created` | User tạo task học tập đầu tiên | Khi task đầu tiên được lưu thành công | Activation, Retention cohort |
| `task_created` | Một task học tập được tạo thành công | Sau khi server xác nhận task được tạo | Engagement, funnel |
| `task_started` | User thực sự bắt đầu xử lý task | Khi trạng thái chuyển từ `pending` sang `in_progress` | Leading indicator `task_started_rate` |
| `task_completed` | Task thực sự chuyển sang completed | Khi server xác nhận trạng thái `in_progress/pending` → `completed` | Core action, NSM, retention |
| `task_overdue` | Task đã vượt deadline nhưng chưa completed | Khi hệ thống xác định task quá deadline | Counter-metric, leading indicator |
| `next_step_saved` | User lưu next step cụ thể cho task | Khi next step được lưu thành công | Leading indicator `task_plan_completion_rate` |

## Event → Metric mapping

```text
first_task_created
    → Activation cohort

task_created
    → Engagement / task creation funnel

task_started
    → task_started_rate

task_completed
    → Core action
    → NSM
    → Weekly retention

task_overdue
    → Counter-metric
    → overdue_task_rate

next_step_saved
    → task_plan_completion_rate
```

## Acceptance criteria

### AC1 — Event chỉ xuất hiện khi hành vi thật sự hoàn tất

Với mỗi cặp `user_id` và `task_id`, hệ thống chỉ ghi `task_completed` khi trạng thái task thực sự chuyển từ chưa hoàn thành sang `completed`. Việc mở task, bấm vào màn hình hoặc tải lại trang không được tạo `task_completed`.

### AC2 — Không ghi trùng completion

Với cùng một `user_id` và `task_id`, một lần chuyển trạng thái sang `completed` chỉ tạo tối đa một `task_completed` hợp lệ. Reload, retry request hoặc autosave không được tạo thêm completion event cho cùng lần chuyển trạng thái.

### Identity tối thiểu cần có

- `user_id`
- `task_id`
- `event_id`
- `event_timestamp`
- `event_name`

### Properties quan trọng

- `task_status_before`
- `task_status_after`
- `deadline`
- `priority`
- `source`
- `is_overdue`

**Kết luận Gate 5:** Bảy câu tự soi đều đạt; event đại diện cho hành vi đã xảy ra và mỗi event đều map về metric.

---

# Revision

Không thay đổi core action sau khi hoàn thiện các phase.

Lý do giữ lựa chọn:

> Core action `task_completed` vừa quan sát được, có completion rule rõ ràng, có thể lặp lại theo task và phản ánh outcome gần core value hơn các thao tác như mở app, đăng nhập, hỏi AI hoặc xem output AI.

---

# Final consistency check

| Chuỗi | Kết quả |
|---|---|
| Core job → Core action | Đúng |
| Core action → Nature | Đúng |
| Nature → Cadence | Đúng |
| Core action → Activation | Đúng |
| Core action → NSM | Đúng |
| Cadence → Retention | Đúng |
| NSM → Loop hypothesis | Đúng |
| Metric → Tracking event | Đúng |
| Event → Acceptance criteria | Đúng |

## Chuỗi cuối cùng

```text
Sinh viên có nhiệm vụ học tập
        ↓
Đưa task vào kế hoạch
        ↓
Bắt đầu xử lý
        ↓
HOÀN THÀNH TASK  ← CORE ACTION / CORE VALUE EVENT
        ↓
Tiến độ được lưu + workload giảm
        ↓
Task/deadline tiếp theo xuất hiện
        ↓
Lặp lại
```
