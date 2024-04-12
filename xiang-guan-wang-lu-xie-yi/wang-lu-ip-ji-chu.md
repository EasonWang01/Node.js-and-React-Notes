# 網路 IP 基礎

## CIDR 計算

CIDR 為 IP 位置表示法，例如 10.0.16.0/20 後面的 /20 代表著該 ip range 使用前面的 20 bitss 當作 host，host 固定不變，後面的才會配給區域網路 IP。所以 10.0.16.0/20 代表的意思為，可分配的 IP 範圍為：10.0.16.0 至 10.0.31.255。

計算方式為：10.0.16.0 轉為 二進位：00001010.00000000.0001

由於 ip 位置總共為 32 bits，然後 CIDR /20 表示後面 12 bits 都是可分配的 bits，所以把後面都變成 1：00001010.00000000.00011111.11111111 及為 10.0.31.255