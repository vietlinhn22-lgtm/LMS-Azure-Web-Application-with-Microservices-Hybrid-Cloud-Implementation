☁️ LMS Marina: Triển khai Web App Micro-services & Bảo mật trên Azure Hybrid Cloud
📌 Tổng quan Dự án
LMS Marina là dự án chuyển đổi số toàn diện nhằm hiện đại hóa hệ thống giáo dục trực tuyến (E-learning) từ môi trường On-premise nguyên khối sang kiến trúc Microservices trên nền tảng đám mây[cite: 3].

Dự án được thiết kế để giải quyết các vấn đề về nút thắt hiệu suất và tối ưu hóa chi phí vận hành[cite: 3]. Mục tiêu chiến lược bao gồm việc đạt độ sẵn sàng cao (High Availability) với 99.95% SLA, triển khai bảo mật cấp doanh nghiệp theo mô hình Zero Trust, và đảm bảo khả năng phục hồi sau thảm họa (Disaster Recovery) với RPO < 15 phút và RTO < 2 giờ[cite: 3]. Toàn bộ hạ tầng được tự động hóa thông qua Infrastructure as Code (IaC)[cite: 3].

🏗️ Kiến trúc Hybrid Cloud
Hạ tầng vận hành theo mô hình Điện toán đám mây Lai (Hybrid Cloud) kết hợp giữa mạng nội bộ (On-premise) và Microsoft Azure thông qua kết nối VPN Gateway, được phân đoạn theo kiến trúc Hub-Spoke[cite: 3]:

Vùng Azure Cloud (Spoke - Production): Lưu trữ Frontend (React SPA) trên VM Scale Sets và Backend Microservices (Auth, Content, Assignment) trên Azure Kubernetes Service (AKS)[cite: 3]. Dữ liệu được quản lý bởi Azure Database for PostgreSQL, Cosmos DB (MongoDB API) và Blob Storage[cite: 3].

Vùng Kết nối & DMZ (Hub): Đóng vai trò kiểm soát lưu lượng trung tâm với Azure Firewall Premium, Application Gateway tích hợp WAF v2, và Azure Bastion để truy cập quản trị an toàn[cite: 3].

Vùng On-premise (Lõi nội bộ): Chứa hệ thống Active Directory để đồng bộ định danh qua AD Connect và thiết bị NAS Synology đồng bộ dữ liệu lên Azure File Share[cite: 3].

Vùng SIEM/SOC (Giám sát): Trung tâm giám sát và phản ứng sự cố sử dụng Azure Sentinel, Azure Monitor và Log Analytics Workspace[cite: 3].

✨ Triển khai Kỹ thuật & Bảo mật Trọng tâm
1. Bảo mật Cloud & Ứng dụng Web
Bảo vệ Đa lớp (WAF & Firewall): Triển khai Application Gateway với Web Application Firewall (WAF v2) kết hợp Azure Firewall Premium để phân tích, ngăn chặn các luồng traffic độc hại và kiểm soát giao tiếp mạng[cite: 3].

Bảo vệ chống DDoS: Tích hợp Azure DDoS Protection để bảo vệ các tài nguyên mạng (Virtual Network) khỏi các cuộc tấn công cạn kiệt tài nguyên[cite: 3].

Bảo mật Mạng Nội bộ (Private Link): Cấu hình Private Endpoints thông qua Private DNS Zones cho toàn bộ Database, Key Vault và Storage, từ chối hoàn toàn truy cập từ Public Internet[cite: 3].

2. Định danh & Quản lý Truy cập (Zero Trust)
Quản trị Định danh Đám mây: Sử dụng Microsoft Entra ID (Premium P2) tích hợp SSO (Single Sign-On) cho Microsoft 365, bắt buộc xác thực đa yếu tố (MFA) và áp dụng Conditional Access[cite: 3].

Kiểm soát Phân quyền (RBAC): Phân chia vai trò chi tiết (LMS Admin, Teacher, Content Manager, DevOps) để giới hạn đặc quyền (Principle of Least Privilege) trên từng tài nguyên Azure[cite: 3].

Truy cập An toàn không Public IP: Bắt buộc sử dụng Azure Bastion để thiết lập phiên quản trị (SSH/RDP) trên trình duyệt, không mở các cổng quản trị ra bên ngoài[cite: 3].

3. Tự động hóa & Quản lý Khóa (Automation & Secrets)
Quản lý Bí mật Tập trung: Sử dụng Azure Key Vault với tính năng Soft Delete và Purge Protection để lưu trữ an toàn các chuỗi kết nối Database, API Keys, JWT Secret Key và chứng chỉ TLS/SSL[cite: 3].

Tự động hóa Quy trình: Xây dựng các luồng công việc (workflows) bằng Azure Logic Apps để gửi email và báo cáo backup hàng ngày[cite: 3]. Giao tiếp bất đồng bộ giữa các Microservices được xử lý bởi Azure Service Bus[cite: 3].

Quản trị Hạ tầng bằng Code (IaC): Sử dụng Terraform để module hóa và triển khai tự động toàn bộ tài nguyên mạng, bảo mật và lưu trữ[cite: 3].

4. Phục hồi sau thảm họa & Tính liên tục (DR/BC)
Disaster Recovery (DR): Thiết lập cơ chế sao chép (replication) sang vùng dự phòng (Secondary Region) bằng Azure Site Recovery[cite: 3].

Sao lưu Tự động: Triển khai Azure Backup cho VM, Database và File Share với các lịch trình snapshot hàng giờ và hàng ngày[cite: 3].

Kiểm soát Tuân thủ (Governance): Triển khai Azure Policy và Azure Blueprint để thực thi các tiêu chuẩn như mã hóa Storage Accounts và từ chối Public Endpoints[cite: 3].

🔎 Giám sát số (Cloud Monitoring & SIEM)
Để đảm bảo khả năng theo dõi liên tục, một quy trình giám sát hệ thống phân tán được triển khai bằng các công cụ nội tại của Azure[cite: 3]:

Giám sát Hạ tầng & Log: Sử dụng Log Analytics Workspace để tổng hợp Azure Activity Logs, NSG Flow Logs, Application logs (từ AKS, VMs) và Database audit logs[cite: 3].

Theo dõi Ứng dụng (APM): Tích hợp Application Insights để giám sát thời gian phản hồi API (Response time) và tỷ lệ lỗi (Error rate) của hệ thống[cite: 3].

SOC & Phân tích chuyên sâu: Ứng dụng ngôn ngữ Kusto (KQL) trong Azure Sentinel để truy vết các lượt đăng nhập thất bại và phát hiện các truy vấn Database chậm (Slow queries)[cite: 3].

🛠️ Công nghệ sử dụng
Cloud & Hạ tầng: Microsoft Azure, Terraform, Azure Kubernetes Service (AKS), Virtual Machine Scale Sets, VPN Gateway[cite: 3].

Bảo mật & Identity: Azure Firewall Premium, Application Gateway WAF v2, Microsoft Defender, Azure Key Vault, Microsoft Entra ID (Azure AD), Azure Bastion[cite: 3].

Database & Storage: Azure Database for PostgreSQL Flexible Server, Azure Cosmos DB (MongoDB API), Azure Blob Storage, Azure File Share, NAS Synology[cite: 3].

Monitoring & Automation: Azure Monitor, Log Analytics, Azure Sentinel, Azure Logic Apps, Azure Service Bus[cite: 3].

Web & Container: Docker, Nginx, FastAPI (Python), React (SPA)[cite: 3].

👨‍💻 Nhóm 4 (Tác giả)
Nguyễn Quang Huy (JK-ENR-HA-11646) - Lãnh đạo Kỹ thuật & IaC/Compute[cite: 3].

Phạm Minh Hoàng (JK-ENR-HA-11622) - Hybrid Cloud, Identity & Bảo mật[cite: 3].

Lê Phát Hoàng Phúc (JK-ENR-HA-11621) - Database, Lưu Trữ & DR/HA[cite: 3].

Nguyễn Việt Linh (JK-ENR-HA-11617) - Hybrid, Networking, Identity & Bảo mật Nâng cao[cite: 3].

Quách Thành Tân (JK-ENR-HA-11618) - Giám Sát, Automation & Integration[cite: 3].

📸 DEMO Triển khai & Vận hành
1. Triển khai Hạ tầng Mạng & Bảo mật với Terraform
(Liên kết video/tài liệu demo VNet, NSG, Firewall, Load Balancer)

2. Tích hợp Hybrid Identity & SSO (On-premise to Entra ID)
(Liên kết video/tài liệu demo AD Connect, MFA, Conditional Access)

3. Khởi chạy Microservices Web App trên AKS & VMSS
(Liên kết video/tài liệu demo quá trình deploy Docker containers)

4. Kiểm thử Cân bằng tải & WAF Application Gateway
(Liên kết video/tài liệu demo thử tải và chặn request độc hại)

5. Diễn tập Phục hồi Thảm họa (Azure Site Recovery)
(Liên kết video/tài liệu demo khôi phục hệ thống từ bản sao lưu)

📸 REPORT Báo cáo Quản trị & Vận hành
1. [📄 Báo cáo: Giám sát toàn diện Hệ thống Microservices với Azure Monitor]
2. [📄 Báo cáo: Ứng phó sự cố bảo mật & Phân tích Log với KQL]
3. [📄 Báo cáo: Áp dụng Chính sách Quản trị (Azure Policy & Blueprint)]
4. [📄 Báo cáo: Phân tích Tối ưu hóa & Quản lý Chi phí Hàng tháng]
