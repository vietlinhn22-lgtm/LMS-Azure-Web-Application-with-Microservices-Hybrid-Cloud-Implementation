# 🛡️ LMS Marina: Triển khai Web App Micro-services & Bảo mật trên Azure Hybrid Cloud

![Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)

## 📌 Tổng quan Dự án
**LMS Marina** là dự án chuyển đổi số toàn diện nhằm hiện đại hóa hệ thống giáo dục trực tuyến (E-learning) từ môi trường On-premise nguyên khối sang kiến trúc Microservices trên nền tảng đám mây. 

Dự án được thiết kế để giải quyết các vấn đề về nút thắt hiệu suất và tối ưu hóa chi phí vận hành. Mục tiêu chiến lược bao gồm việc đạt độ sẵn sàng cao (High Availability) với 99.95% SLA, triển khai bảo mật cấp doanh nghiệp theo mô hình Zero Trust, và đảm bảo khả năng phục hồi sau thảm họa (Disaster Recovery) với RPO < 15 phút và RTO < 2 giờ. Toàn bộ hạ tầng được tự động hóa thông qua Infrastructure as Code (IaC).

## 🏗️ Kiến trúc Hybrid Cloud
Hạ tầng vận hành theo mô hình Điện toán đám mây Lai (Hybrid Cloud) kết hợp giữa mạng nội bộ (On-premise) và Microsoft Azure thông qua kết nối VPN Gateway, được phân đoạn theo kiến trúc Hub-Spoke:
*   **Vùng Azure Cloud (Spoke - Production):** Lưu trữ Frontend (React SPA) trên VM Scale Sets và Backend Microservices (Auth, Content, Assignment) trên Azure Kubernetes Service (AKS). Dữ liệu được quản lý bởi Azure Database for PostgreSQL, Cosmos DB (MongoDB API) và Blob Storage.
*   **Vùng Kết nối & DMZ (Hub):** Đóng vai trò kiểm soát lưu lượng trung tâm với Azure Firewall Premium, Application Gateway tích hợp WAF v2, và Azure Bastion để truy cập quản trị an toàn.
*   **Vùng On-premise (Lõi nội bộ):** Chứa hệ thống Active Directory để đồng bộ định danh qua AD Connect và thiết bị NAS Synology đồng bộ dữ liệu lên Azure File Share.
*   **Vùng SIEM/SOC (Giám sát):** Trung tâm giám sát và phản ứng sự cố sử dụng Azure Sentinel, Azure Monitor và Log Analytics Workspace.

## ✨ Triển khai Kỹ thuật & Bảo mật Trọng tâm (Blue Team)

### 1. Bảo mật Cloud & Ứng dụng Web
*   **Bảo vệ Đa lớp (WAF & Firewall):** Triển khai Application Gateway với Web Application Firewall (WAF v2) kết hợp Azure Firewall Premium để phân tích, ngăn chặn các luồng traffic độc hại và kiểm soát giao tiếp mạng.
*   **Bảo vệ chống DDoS:** Tích hợp Azure DDoS Protection để bảo vệ các tài nguyên mạng (Virtual Network) khỏi các cuộc tấn công cạn kiệt tài nguyên.
*   **Bảo mật Mạng Nội bộ (Private Link):** Cấu hình Private Endpoints thông qua Private DNS Zones cho toàn bộ Database, Key Vault và Storage, từ chối hoàn toàn truy cập từ Public Internet.

### 2. Định danh & Quản lý Truy cập (Zero Trust)
*   **Quản trị Định danh Đám mây:** Sử dụng Microsoft Entra ID (Premium P2) tích hợp SSO (Single Sign-On) cho Microsoft 365, bắt buộc xác thực đa yếu tố (MFA) và áp dụng Conditional Access.
*   **Kiểm soát Phân quyền (RBAC):** Phân chia vai trò chi tiết (LMS Admin, Teacher, Content Manager, DevOps) để giới hạn đặc quyền (Principle of Least Privilege) trên từng tài nguyên Azure.
*   **Truy cập An toàn không Public IP:** Bắt buộc sử dụng Azure Bastion để thiết lập phiên quản trị (SSH/RDP) trên trình duyệt, không mở các cổng quản trị ra bên ngoài.

### 3. Tự động hóa & Quản lý Khóa (Automation & Secrets)
*   **Quản lý Bí mật Tập trung:** Sử dụng Azure Key Vault với tính năng Soft Delete và Purge Protection để lưu trữ an toàn các chuỗi kết nối Database, API Keys, JWT Secret Key và chứng chỉ TLS/SSL.
*   **Tự động hóa Quy trình:** Xây dựng các luồng công việc (workflows) bằng Azure Logic Apps để gửi email và báo cáo backup hàng ngày. Giao tiếp bất đồng bộ giữa các Microservices được xử lý bởi Azure Service Bus.
*   **Quản trị Hạ tầng bằng Code (IaC):** Sử dụng Terraform để module hóa và triển khai tự động toàn bộ tài nguyên mạng, bảo mật và lưu trữ.

### 4. Phục hồi sau thảm họa & Tính liên tục (DR/BC)
*   **Disaster Recovery (DR):** Thiết lập cơ chế sao chép (replication) sang vùng dự phòng (Secondary Region) bằng Azure Site Recovery.
*   **Sao lưu Tự động:** Triển khai Azure Backup cho VM, Database và File Share với các lịch trình snapshot hàng giờ và hàng ngày.
*   **Kiểm soát Tuân thủ (Governance):** Triển khai Azure Policy và Azure Blueprint để thực thi các tiêu chuẩn như mã hóa Storage Accounts và từ chối Public Endpoints.

## 🔎 Giám sát số (Cloud Monitoring & SIEM)
Để đảm bảo khả năng theo dõi liên tục, một quy trình giám sát hệ thống phân tán được triển khai bằng các công cụ nội tại của Azure:
*   **Giám sát Hạ tầng & Log:** Sử dụng Log Analytics Workspace để tổng hợp Azure Activity Logs, NSG Flow Logs, Application logs (từ AKS, VMs) và Database audit logs.
*   **Theo dõi Ứng dụng (APM):** Tích hợp Application Insights để giám sát thời gian phản hồi API (Response time) và tỷ lệ lỗi (Error rate) của hệ thống.
*   **SOC & Phân tích chuyên sâu:** Ứng dụng ngôn ngữ Kusto (KQL) trong Azure Sentinel để truy vết các lượt đăng nhập thất bại và phát hiện các truy vấn Database chậm (Slow queries). 

## 🛠️ Công nghệ sử dụng
*   **Cloud & Hạ tầng:** Microsoft Azure, Terraform, Azure Kubernetes Service (AKS), Virtual Machine Scale Sets, VPN Gateway.
*   **Bảo mật & Identity:** Azure Firewall Premium, Application Gateway WAF v2, Microsoft Defender, Azure Key Vault, Microsoft Entra ID (Azure AD), Azure Bastion.
*   **Database & Storage:** Azure Database for PostgreSQL Flexible Server, Azure Cosmos DB (MongoDB API), Azure Blob Storage, Azure File Share, NAS Synology.
*   **Monitoring & Automation:** Azure Monitor, Log Analytics, Azure Sentinel, Azure Logic Apps, Azure Service Bus.
*   **Web & Container:** Docker, Nginx, FastAPI (Python), React (SPA).

## 👨‍💻 Nhóm 4 (Tác giả)
*   **Nguyễn Quang Huy** (JK-ENR-HA-11646) - Lãnh đạo Kỹ thuật & IaC/Compute.
*   **Nguyễn Việt Linh** (JK-ENR-HA-11617) - Hybrid, Networking, Identity & Bảo mật Nâng cao.
*   **Phạm Minh Hoàng (JK-ENR-HA-11622) - Hybrid Cloud, Identity & Bảo mật
*   **Lê Phát Hoàng Phúc (JK-ENR-HA-11621) - Database, Lưu Trữ & DR/HA
*   **Quách Thành Tân (JK-ENR-HA-11618) - Giám Sát, Automation & Integration

## 📸 DEMO Triển khai & Vận hành

### 1. Kiến trúc Lai & Định danh (Hybrid Identity):
* [▶️ Cấu hình On-premise](https://drive.google.com/file/d/1OZqZr0sfPMeQLRa-msmlLrOJWCxHXc5l/view)
* [▶️ Cấu hình EntraID và Share Subcription](https://drive.google.com/file/d/1nPfFf5_lpIp-bBrU45ef6H5-nQKUfLHq/view)
* [▶️ Cấu hình ADConnect cho On-premise](https://drive.google.com/file/d/1JhRBV_zWhuhfs7sruMaO0XCIcNS68xqc/view)
* [▶️ Cấu hình Azure Arc cho Window Server 2022](https://drive.google.com/file/d/1AhxS-0j70D5G-Rd6itvxzp5uMA4tRQ-K/view)

### 2. Tự động hóa Hạ tầng (IaC):
* [▶️ Đẩy Web app on-premise bằng code Azure PowerShell](https://drive.google.com/file/d/1mIVi8JzkFhn2YHw7vHZLQ-S4GGInjH4y/view?usp=drive_link)
* [▶️ Deploy Project with Terraform Code](https://drive.google.com/file/d/1MNBmyB9IjuC_W2KIy5-FBt2wAp59_g3U/view)

### 3. Mạng & Bảo mật (Networking & Security):
* [▶️ Cấu hình VPN Point-to-Site](https://drive.google.com/file/d/1aB-vDksp57wfDit_a1qYRrGWzgYikjk1/view?usp=drive_link)
* [▶️ Cấu hình Azure Firewall](https://drive.google.com/file/d/1gafP1bmujVLnQmoFFSveRi98keMX0U4C/view?usp=drive_link)

### 4. SOC & Giám sát Hệ thống (Monitoring):
* [▶️ Azure Monitor](https://drive.google.com/file/d/1qBK0FUfqfiZ6iIMcPINQ3fk8tzfGc04y/view?usp=drive_link)
* [▶️ Azure Log Analytics](https://drive.google.com/file/d/1YoKLJepEZv-j4wdso5ye0tGHJzuows3d/view?usp=drive_link)

### 5. Tích hợp Dịch vụ & Automation:
* [▶️ Cấu hình Custom Domain](https://drive.google.com/file/d/1okyrOvy8maqOUJrgn74J5v3a2nD-pXq9/view?usp=drive_link)
* [▶️ Cấu hình Microsoft 365 & Teams](https://drive.google.com/file/d/14J3PIaZchrglB7zj-0kMmu2mzlwEktTx/view?usp=drive_link)
* [▶️ Azure Service Bus](https://drive.google.com/file/d/1m_W-EneYWTDTAvrSPszKnd15GdE0IuuO/view?usp=drive_link)
* [▶️ Azure Function App](https://drive.google.com/file/d/1O-MOwqccY-6TSM1LjW0e0Xr8bBhLzRCL/view?usp=drive_link)

## 📸 TÀI LIỆU THAM KHẢO
[📖 Đọc trực tiếp: TÀI LIỆU THIẾT KẾ ĐỒ ÁN (TLTK_Group4.pdf)](./TLTK_Group4.pdf)

