### Local port forwarding
```
10.200.12.232 là máy nguồn
10.252.12.233 là máy đích
=> Mục đích Public dịch vụ MYSQL trên máy chủ 10.200.12.233 thông qua máy chủ 10.200.12.232

1) Chạy lệnh ssh -L 3307:10.200.12.232:3306 tunnel-a@10.200.12.232 trên máy chủ 10.200.12.232
2) Kiểm tra Tunnel đã được mở trên 10.200.12.232
[root@~]# netstat -tunpl | grep 3307
tcp        0      0 127.0.0.1:3307          0.0.0.0:*               LISTEN      1369705/sshd: tunne
tcp6       0      0 ::1:3307                :::*                    LISTEN      1369705/sshd: tunne
```

### Remote port forwarding
```
10.200.12.232 là máy đích
10.252.12.233 là máy nguồn
=> Mục đích Public dịch vụ HTTP trên máy chủ 10.200.12.233 thông qua máy chủ 10.200.12.232

1) Trên /etc/ssh/sshd_config của 10.200.12.232 thêm cấu hình GatewayPorts yes
2) Chạy lệnh ssh -R 10.200.12.232:3000:10.200.12.233:80 tunnel-a@10.200.12.232 trên máy chủ 10.200.12.233
3) Kiểm tra Tunnel đã được mở trên 10.200.12.232
[root@~]# netstat -tunpl | grep 3000
tcp        0      0 127.0.0.1:3000          0.0.0.0:*               LISTEN      1369705/sshd: tunne
tcp6       0      0 ::1:3000                :::*                    LISTEN      1369705/sshd: tunne
```

### Dynamic port forwarding
```

```
