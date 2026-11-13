Trivy là một công cụ scan bảo mật mã nguồn mở (open-source vulnerability scanner) do Aqua Security phát triển. Nó chuyên dùng để tìm kiếm lỗ hổng bảo mật (CVE), lỗi cấu hình IaC/Kubernetes, lộ lọt thông tin nhạy cảm (Secrets) và kiểm tra license trong container images, file systems, git repositories hoặc các tệp cấu hình.

Việc kết nợp Trivy với SonarQube (Sonarqube) trong CI/CD pipeline, đây là phương pháp tiếp cận tiêu chuẩn trong bảo mật ứng dụng hiện tại (DevSecOps).

1. Phân vai trò giữa Trivy và SonarQube

|Tiêu chuẩn|Trivy|SonarQube|
|---|---|---|
|Mục đích chính|Scan hạ tầng, dependency, Infrastructure & Supply Chain|Phân tích chất lượng mã nguồn và lỗi logic/bảo mật tĩnh trong code (SAST - Static Application Security Testing)|
|Đối tượng Scan|Container images, OS packages, library bên thư ba (npm, pip, maven ...), file cấu hình (Dockerfile, Terraform, K8s manifests), secrets|Source code tự viết của lập trình viên (Java, Python, Go ...)|
|Điểm mạnh|Phát hiện rất nhanh các lỗ hổng CVE trong hệ điều hành, thư viện lỗi và file cấu hình triển khai|Phát hiện sâu các lỗ hổng logic nghiệp vụ trong code (như SQL Injection, XSS, tràn bộ nhớ, lỗi thiêt kế)|

2. Lợi ích khi két hợp cả hai trong CI/CD Pipeline

2.1. Bảo mật toàn diện (Defense in Depth)

* SonarQube đảm bảo mã do lập trình viên viết ra sạch sẽ, không có logic hay lỗ hổng cú pháp bảo mật (SAST)
* Trivy bảo đảm các thư viện bên thứ ba (dependencies) và môi trường đóng gói (container image, file config) không chứa lổ hỗng CEV hay để lộ password/token.

 2.2. Tối ưu hóa giai đoạn Shift - Left

Tích hợp cả hai vào quy trình CI (ví dụ: Gitlab CI hoặc GitHub Actions) giúp đội ngũ Dev phát hiện sớm các vấn đề ngay từ khi push code hoặc build image giảm thiểu rủi ro khi đưa lên môi trường Production

3. So sánh chi tiết giữa Trivy Open Source và Trivy Commercial/Enterprise

|Tiêu chí so sánh|Trivy Open Source|Trivy Commercial/Enterprise|
|---|---|---|
|Mô hình & Bản chất|Công cụ CLI mã nguồn mở bhoanf toàn miễn phí, triển khai cục bộ|Nền tảng bảo mật đám mây thương mại (CNAPP) có tính phí qua Aqua Security|
|Tính năng scan|Scan CEV, Config, Secret, License, SBOM cho Containerm Filesystem, Git repository|Bao gồm các tính năng cốt lõi kèm theo các công cụ phân tích nâng cao độc quyền|
|Cơ sở dữ liệu|Sử dụng public vulnerability feed, có thể bị rate limiting khi scan mô hình lớn|Có SLA cập nhật nhanh hơn và không bị giới hạn|
|Phân tích độ rủi ro & Khả năng khai thác|Chỉ liệt kê thô danh sách CVE|Có tính năng Reachability Analysis và đánh giá khả năng khai thác thực tế|
|Kiến trúc & Khả năng mở rộng|Chạy đơn lẻ từng tác vụ (Single scan), phụ thuộc vào năng lực CI/CD runner hoặc máy trạm|Kiến trúc cấp độ doanh nghiệp độ sẵn sàng cao (High Availabilit), hỗ trợ scan tập trung, đồng thời quy mô lớn với ít ảnh hưởng tài nguyên cụm|
|Quản lý & UI Dashboard|Chủ yếu xuất kết quả dạng terminal (CLI), JSON hoặc dùng Trivy Operator xuất ra CRDs trong Kubernetes|Có giao diện Dashboard tập trung quản lý toàn bộ, phân quyền cho người dùng, báo cáo điều hành và tích hợp quản lý sự cố (Incident Management)|
|Bảo mật Runtime & Supply Chain|Tập trung chủ yếu ở giai đoạn Build-time và CI/CD (Shift-left)|Mở rộng bảo mật thời gian chạy (Runtime Security), phát hiện phầm mềm độc hại (Malware detection)|
|Hỗ trợ kỹ thuật (Support)|Dựa vào cộng đồng (Community-driven), Github Issues, không có cam kết SLA|Hỗ trợ chuyên nghiệp từ đội ngũ chuyên gia của hãng có cam kết SLA cụ thể|

* Dùng bản miễn phí khi bạn là cá nhân, team nhỏ, hoặc doanh nghiệp muốn tích hợp nhanh việc quét lỗ hổng và kiểm tra code/container vào CI/CD pipeline
* Chuyển lên Bản phí khi hệ hệ thống lớn (enterprise), cần quản lý tập trung nhiều cluster, tuân thủ các tiêu chuẩn khắt khe (SOC 2, PCI-DSS, HIPAA), cần giảm thiểu cảnh báo giả (false positive)
