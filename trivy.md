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

  2.2. Tối ưu hóa giai đoạn Shift - Left: Tích hợp cả hai vào quy trình CI (ví dụ: Gitlab CI hoặc GitHub Actions) giúp đội ngũ Dev phát hiện sớm các vấn đề ngay từ khi push code hoặc build image giảm thiểu rủi ro khi đưa lên môi trường Production
