# How to setup Uptime Kuma + WAHA

## Giới thiệu ngắn

**Uptime Kuma**\
Một hệ thống monitor giúp theo dõi trạng thái dịch vụ/app và hỗ trợ gửi
thông báo.

**WAHA**\
Dự án mã nguồn mở cho phép tương tác WhatsApp thông qua API bằng cách mô
phỏng một server duy trì socket.

**Tóm tắt cổng chạy** - **Kuma** chạy ở:
**$HOST:3000** - **WAHA** chạy ở: **$HOST:3001**

------------------------------------------------------------------------

## Bước 1: Tạo thư mục lưu volume dữ liệu

Thư mục đặt tùy ý, chỉ cần dễ nhớ:

``` bash
mkdir -p /root/khainam/uptime-kuma-data
mkdir -p /root/khainam/waha_sessions
```

------------------------------------------------------------------------

## Bước 2: Start container và map volume

### Uptime Kuma (tự restart khi server down)

``` bash
docker run -d   --name uptime-kuma   -p 3000:3001   -v /root/khainam/uptime-kuma-data   --restart unless-stopped   louislam/uptime-kuma:latest
```

------------------------------------------------------------------------

### WAHA (tự restart)

1.  Pull image:

``` bash
docker pull devlikeapro/waha
```

2.  Run container:

``` bash
docker run -it   -v /root/khainam/waha_sessions:/app/.sessions   -p 3001:3000   --restart unless-stopped   --name waha   devlikeapro/waha
```

WAHA sẽ sinh một **password** mỗi lần start lại.\
Dữ liệu session được lưu trong volume nên không bị mất.

------------------------------------------------------------------------

## Bước 3: Thiết lập WhatsApp trên WAHA

1.  Truy cập **http://\$HOST:3001**
2.  Nhập mã **authorize**
3.  Mở **\$HOST/api/screenshot** để xem mã QR
4.  Scan QR để login WhatsApp
5.  Quay lại giao diện → xem được chats

------------------------------------------------------------------------

## API cần biết

-   Lấy group ID, chat ID từ các API liệt kê trong WAHA.
-   Gửi tin nhắn bằng endpoint:

```{=html}
<!-- -->
```
    $HOST/api/sendText