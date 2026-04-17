# Giới thiệu về Cilium
Cilium là một nền tảng mạng (networking) và bảo mật (security) mã nguồn mở dành cho Kubernetes, được xây dựng trên công nghệ eBPF (extended Berkeley Packet Filter) của Linux. Thay vì sử dụng các cơ chế truyền thống như iptables, Cilium tận dụng eBPF để xử lý lưu lượng mạng trực tiếp trong kernel, giúp đạt hiệu năng cao, độ trễ thấp và khả năng quan sát (observability) mạnh mẽ.

Cilium không chỉ thay thế kube-proxy mà còn cung cấp các tính năng nâng cao như:

- Load Balancing hiệu quả ở Layer 3–7
- Network Policy dựa trên identity (không chỉ IP)
- Observability chi tiết thông qua Hubble
- Security theo mô hình Zero Trust

Ngoài ra, Cilium còn tích hợp với Envoy Proxy để hỗ trợ các use case Layer 7 như HTTP routing, metrics, và custom load balancing.

Với khả năng mở rộng tốt và phù hợp cho môi trường production, Cilium đang trở thành lựa chọn phổ biến trong các hệ thống cloud-native hiện đại.
