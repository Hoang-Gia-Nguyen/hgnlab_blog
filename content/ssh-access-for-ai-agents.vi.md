+++
title = "SSH Access cho AI Agents và Automation Tools — Hướng dẫn an toàn cấp tốc"
date = 2026-06-09
draft = false
description = "Hướng dẫn thực tế để cấp quyền SSH an toàn cho AI coding agents và công cụ tự động hoá, dùng user riêng và key tạm thời."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["ssh", "security", "ai-agents", "automation", "devops"]

[extra]
#cover.image = "images/ssh-access-for-ai-agents-cover.png"
#cover.alt = "Minh hoạ AI agent kết nối tới server qua SSH"
+++

Bạn vừa dựng xong một homelab, và giờ muốn cho AI agents (như Codex CLI, Cursor, hay GitHub Copilot) hoặc các công cụ tự động hoá (Ansible, CI/CD runners) truy cập vào server để làm việc. Nhưng đưa luôn SSH key cá nhân cho chúng? Đó là rủi ro không đáng có.

Bài này mình sẽ chỉ bạn một mẫu hình bảo mật nhanh, lặp lại được: **user riêng + key tạm thời + quyền hạn chế**. Hãy áp dụng các bước này mỗi lần bạn cấp quyền truy cập cho một công cụ hay agent mới.

## Vấn đề là gì?

Rủi ro không nằm ở chỗ bạn có tin tưởng agent hay không — mà là **bán kính sát thương (blast radius)**. Nếu phiên làm việc của AI agent bị xâm phạm, hoặc CI pipeline làm lộ thông tin đăng nhập, kẻ tấn công có thể:

- Xoá sạch toàn bộ server
- Đánh cắp dữ liệu nhạy cảm (config, mật khẩu, database)
- Dùng server của bạn làm bàn đạp tấn công các hệ thống khác

SSH key cá nhân của bạn cho phép truy cập toàn bộ mọi thứ mà user của bạn có thể làm. Một user riêng với quyền hạn chế sẽ khoanh vùng thiệt hại chỉ trong một phạm vi kiểm soát được.

## Bước 1: Tạo một User Riêng

Đặt tên dễ nhận biết *cái gì* đang kết nối. Đừng bao giờ dùng tài khoản cá nhân.

```bash
# Tạo user, có thư mục home
sudo useradd -m -s /bin/bash ai-agent

# Khoá mật khẩu — chỉ cho phép đăng nhập bằng key
sudo passwd -l ai-agent
```

Tuỳ chọn `-m` sẽ tạo thư mục home tại `/home/ai-agent/`, nơi bạn sẽ đặt SSH authorized keys.

> **Mẹo:** Nếu muốn chặt chẽ hơn, dùng `-s /usr/sbin/nologin` để chặn hoàn toàn quyền shell — agent chỉ chạy được các lệnh chỉ định trong `authorized_keys` qua tuỳ chọn `command=`.

## Bước 2: Giới Hạn Những Gì User Này Được Làm

### Cách A: Khoá vào một lệnh cụ thể

Sửa file `~/.ssh/authorized_keys` của user, thêm `command=` vào đầu public key:

```bash
# Key này chỉ chạy được ~/allowed-scripts/deploy.sh
command="/home/ai-agent/allowed-scripts/deploy.sh",no-port-forwarding,no-X11-forwarding,no-agent-forwarding ssh-ed25519 AAAAC3...
```

Tạo thư mục riêng cho agent, chỉ cấp quyền ghi ở nơi cần thiết:

```bash
sudo mkdir -p /home/ai-agent/allowed-scripts
sudo chown -R ai-agent:ai-agent /home/ai-agent
sudo chmod 755 /home/ai-agent
sudo chmod 750 /home/ai-agent/.ssh
```

### Cách B: Giới hạn bằng `Match` blocks trong `sshd_config`

```bash
sudo tee -a /etc/ssh/sshd_config << 'EOF'

# Giới hạn user ai-agent
Match User ai-agent
    AllowTcpForwarding no
    X11Forwarding no
    PermitTTY no
    ForceCommand /usr/sbin/nologin
EOF

sudo systemctl restart sshd
```

## Bước 3: Tạo Cặp Key SSH Tạm Thời

Tạo một cặp key **riêng biệt** cho từng agent hoặc công cụ. Đừng bao giờ dùng lại key giữa các công cụ.

```bash
# Tạo key Ed25519 — nhanh và an toàn hơn RSA
ssh-keygen -t ed25519 -f ~/temp-ssh-keys/ai-agent-key -C "ai-agent-temp-20260609" -N ""
```

Lệnh này tạo ra hai file:

- `~/temp-ssh-keys/ai-agent-key` — **private key** (đưa cho agent)
- `~/temp-ssh-keys/ai-agent-key.pub` — public key (để lên server)

> **Tại sao Ed25519?** Đây là chuẩn hiện đại. Key nhỏ hơn, xác thực nhanh hơn, bảo mật tuyệt vời. Chỉ dùng RSA (`-t rsa -b 4096`) nếu server của bạn quá cũ không hỗ trợ Ed25519.

## Bước 4: Copy Public Key Lên Máy Đích

```bash
# Copy public key vào authorized_keys của user vừa tạo
sudo mkdir -p /home/ai-agent/.ssh
cat ~/temp-ssh-keys/ai-agent-key.pub | sudo tee -a /home/ai-agent/.ssh/authorized_keys
sudo chown -R ai-agent:ai-agent /home/ai-agent/.ssh
sudo chmod 700 /home/ai-agent/.ssh
sudo chmod 600 /home/ai-agent/.ssh/authorized_keys
```

### Lệnh một dòng cho server từ xa

Nếu bạn đang setup từ máy khác:

```bash
# Từ máy local của bạn
ssh-copy-id -i ~/temp-ssh-keys/ai-agent-key.pub ai-agent@your-server-ip
```

Hoặc làm thủ công:

```bash
# Copy public key và nối vào file authorized_keys từ xa
cat ~/temp-ssh-keys/ai-agent-key.pub | ssh admin@your-server-ip "sudo tee -a /home/ai-agent/.ssh/authorized_keys"
```

## Bước 5: Kiểm Tra Kết Nối

Trước khi đưa key cho bất kỳ agent nào, hãy xác nhận mọi thứ hoạt động:

```bash
ssh -i ~/temp-ssh-keys/ai-agent-key ai-agent@your-server-ip
```

Nếu bạn đã giới hạn user, bạn sẽ thấy thông báo lệnh bị từ chối hoặc shell bị chặn — đó là hành vi mong muốn. Agent vẫn có thể chạy các lệnh được chỉ định.

## Bước 6: Đưa Private Key Cho Agent

Giờ bạn có thể đưa **đường dẫn file private key** cho AI agent hoặc công cụ tự động hoá của mình.

**Với AI coding agents** (Codex CLI, Cursor, Claude Code):

```bash
# Ví dụ: yêu cầu agent dùng key này
codex --ssh-key ~/temp-ssh-keys/ai-agent-key
```

**Với Ansible:**

```ini
# ansible.cfg hoặc inventory
[defaults]
private_key_file = ~/temp-ssh-keys/ai-agent-key
```

**Với CI/CD runners** (GitHub Actions, GitLab CI):

```bash
# Lưu private key dưới dạng CI secret (ví dụ: SSH_PRIVATE_KEY)
# Sau đó trong pipeline:
- name: Setup SSH
  run: |
    mkdir -p ~/.ssh
    echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/ci-key
    chmod 600 ~/.ssh/ci-key
```

## Bảo Vệ Private Key

Private key là **vật bất ly thân**. Hãy coi nó như mật khẩu:

- **Không bao giờ dán nội dung key vào chat prompt.** Dù bạn có tin tưởng nhà cung cấp LLM, bạn vẫn đang gửi thông tin đăng nhập ra ngoài.
- **Yêu cầu agent không đọc hoặc in key ra màn hình.** Hầu hết AI coding tools đều chấp nhận flag `--ssh-key` và tự xử lý file nội bộ. Nếu agent hỏi nội dung key, hãy bảo nó dùng đường dẫn file.
- **Set quyền hạn chế:**

  ```bash
  chmod 600 ~/temp-ssh-keys/ai-agent-key
  ```

- **Xoay vòng key thường xuyên.** Tạo cặp key mới cho mỗi phiên làm việc hoặc mỗi lần chạy CI. Với các lệnh một dòng ở trên, bạn chỉ mất 30 giây.
- **Thu hồi khi không dùng nữa.** Xoá key khỏi `authorized_keys` hoặc xoá luôn user sau khi hoàn thành:

  ```bash
  # Xoá key cụ thể
  sudo sed -i '/ai-agent-temp-20260609/d' /home/ai-agent/.ssh/authorized_keys

  # Hoặc xoá luôn user
  sudo userdel -r ai-agent
  ```

## Tham Khảo Nhanh — Toàn Bộ Các Bước Trong Một Block

```bash
# === Trên máy đích ===

# 1. Tạo user
sudo useradd -m -s /bin/bash ai-agent
sudo passwd -l ai-agent

# 2. Tạo thư mục SSH
sudo mkdir -p /home/ai-agent/.ssh

# 3. Thêm public key (thay bằng public key thật của bạn)
echo "ssh-ed25519 AAAAC3..." | sudo tee /home/ai-agent/.ssh/authorized_keys

# 4. Sửa quyền
sudo chown -R ai-agent:ai-agent /home/ai-agent/.ssh
sudo chmod 700 /home/ai-agent/.ssh
sudo chmod 600 /home/ai-agent/.ssh/authorized_keys

# === Trên máy local của bạn ===

# 5. Tạo key tạm thời
ssh-keygen -t ed25519 -f ~/temp-ssh-keys/ai-agent-key -C "ai-agent-$(date +%Y%m%d)" -N ""

# 6. Kiểm tra
ssh -i ~/temp-ssh-keys/ai-agent-key ai-agent@your-server

# 7. Đưa key cho agent
#    Agent dùng: ~/temp-ssh-keys/ai-agent-key
```

## Tóm Lại

- **Luôn dùng user riêng** — không bao giờ dùng tài khoản cá nhân.
- **Tạo một cặp key cho mỗi công cụ** — không bao giờ dùng lại key.
- **Giới hạn những gì user được làm** — `command=` restrictions hoặc `Match` blocks.
- **Bảo vệ private key** — chỉ dùng đường dẫn file, không bao giờ dán nội dung.
- **Xoay vòng và thu hồi** — key là tạm thời, không phải vĩnh viễn.

Mẫu hình này mất năm phút để thiết lập và cứu bạn khỏi cả một thế giới đau đầu nếu có sự cố xảy ra. Server của bạn vẫn an toàn, AI agents có quyền truy cập chúng cần.
