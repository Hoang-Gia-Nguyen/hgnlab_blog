+++
title = "Lộ Trình Self-Hosting: Từng Bước Để Trở Thành Dân Chuyên"
date = 2025-10-13
description = "Lộ trình chi tiết, dẫn bạn từ bước đầu tiên chạy Docker container cho tới khi tự tay quản lý một hệ sinh thái self-host tự động, ổn định và đáng tin cậy."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["self-hosting", "hobby"]

[extra]
cover.image = "images/self-hosting-road-map-cover.jpg"
cover.alt = "Self-Hosting Road Map Banner"

+++

| **Cấp Độ**                                     | **Mục Tiêu**                               | **Cần Tập Trung**                                                                                                                                                                              | **Cột Mốc**                                                |
| ---------------------------------------------- | ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| **1️⃣ Nhập Môn — Chạy Dịch Vụ Đầu Tiên**       | Làm cho nó chạy được                       | • Học Docker cơ bản (`docker run`, `docker compose`)<br>• Chạy 1 ứng dụng container (Jellyfin, Vaultwarden, AdGuardHome)<br>• Dùng named volume để lưu dữ liệu<br>• Khởi động lại và xem nó có tự phục hồi không | Một dịch vụ ổn định bạn dùng hàng ngày                     |
| **2️⃣ Tay Vững — Truy Cập Dễ Dàng**             | Truy cập dịch vụ dễ dàng và an toàn         | • Thêm reverse proxy (Nginx Proxy Manager, Caddy)<br>• Bật HTTPS bằng Let’s Encrypt hoặc Cloudflare Tunnel<br>• Cài đặt domain nội bộ hoặc DNS của Cloudflare<br>• Hiểu về port và firewall     | Truy cập công khai hoặc qua mạng LAN, bảo mật bằng SSL     |
| **3️⃣ Trung Cấp — Duy Trì Hoạt Động**            | Giảm thiểu mất dữ liệu hay sập server           | • Tự động backup (dùng cron hoặc script)<br>• Chạy thử quy trình backup - recover<br>• Thêm Watchtower hoặc Renovate để tự cập nhật<br>• Dùng uptime-kuma hoặc healthchecks.io để theo dõi trạng thái    | Có thể phá và sửa lại hệ thống trong vòng 1 giờ            |
| **4️⃣ Nâng Cao — Tự Động & Mở Rộng**           | Coi homelab như một hệ thống production     | • Dùng Ansible, Terraform, hoặc script để tự động cài đặt<br>• Tách biệt secrets khỏi file cấu hình<br>• Thêm metrics/logs (Prometheus, Grafana, Loki)<br>• Lưu backup ở nơi khác (R2, Backblaze,...) | Triển khai lại toàn bộ chỉ bằng một dòng lệnh              |
| **5️⃣ Chuyên Nghiệp — Thiết Kế Để Tin Cậy**    | Xây dựng hệ sinh thái, không phải đồ chơi   | • Hosting đa node (Nomad, Kubernetes, Proxmox)<br>• Truy cập zero-trust (WireGuard, Tailscale, Cloudflare Access)<br>• Tự build Docker image<br>• Dùng CI/CD tự host để tự động deploy | Hệ thống có thể tái tạo, an toàn và có thể giám sát được    |
