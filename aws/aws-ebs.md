# AWS EBS

EBS 為掛載在 EC2 上的儲存體，具有以下特性。

1. **持久性存儲**：EBS卷是持久性的，數據不會在EC2實例停止或終止時丟失。您可以將EBS卷附加到不同的EC2實例上，以便在不同實例之間共享數據。
2. **高性能**：EBS卷可以提供高性能存儲，適用於各種工作負載，包括數據庫、文件存儲和應用程序數據。
3. **多種卷類型**：EBS提供多種卷類型，包括通用用途（gp2）、硬盤（sc1、st1）、提供I/O優化（io1）等。可以根據應用程序的需求選擇適當的卷類型。
4. **快照**：EBS卷支持創建快照，這是卷的點對點備份。可以隨時創建卷的快照，以便在需要時還原數據。
5. **自動備份**：通過Amazon EBS快照，可以自動備份數據，以防止數據丟失。這對於應對意外數據損壞或刪除非常有用。
6. **可擴展性**：可以根據需要輕鬆擴展EBS卷的大小，而無需停機。
7. **安全性**：EBS卷的數據在傳輸和靜止時都受到加密保護，提供安全的存儲解決方案。
8. **區域和可用區冗余**：EBS數據會自動在多個可用區進行複製，以提供高可用性和容錯性。

## 硬碟種類

1. **General Purpose SSD (gp3、gp2)：** 通用用途的SSD硬碟，適用於大多數應用程序，提供良好的性能和成本效益。gp3是gp2的後繼者，提供更高的性能和更低的成本。
2. **Provisioned IOPS SSD (io2、io1)：** 針對需要高I/O性能的應用程序而設計的SSD硬碟。io2是io1的後繼者，提供更高的IOPS和更低的成本。
3. **Throughput Optimized HDD (st1)：** 高吞吐量儲存硬碟，適用於大數據、數據倉庫和大型文件流的應用程序。
4. **Cold HDD (sc1)：** 低成本的冷儲存硬碟，適用於需要長期保留數據但不需要頻繁訪問的應用程序。
5. **Magnetic (standard)：** 傳統的磁碟硬碟，提供基本的存儲性能，適用於低成本需求的應用程序。

## 掛載路徑

1. `/dev/xvdf`：
   * 這是在較早的 EC2 實例類型上使用的命名約定。
   * `xvd` 前綴表示區塊設備。
   * `f` 代表字母標識符，可以是 `a`、`b`、`c` 等，每個字母代表不同的區塊設備。
2. `/dev/nvme0n1p1`：
   * 這是在較新的 EC2 實例類型上使用的命名約定，特別是針對 NVMe SSD 設備。
   * `nvme` 前綴表示 NVMe（非揮發性記憶體擴充）設備。
   * `0n1` 代表第一個 NVMe 設備。
   * `p1` 代表第一個分區。

在新一代的 EC2 實例上，通常會使用 `/dev/nvme0n1p1` 這種命名約定來表示 EBS 卷的分區，而不再使用 `xvd` 前綴。因此，如果使用較新的 EC2 實例，可能會看到像 `/dev/nvme0n1p1` 這樣的路徑來表示 EBS 卷上的分區。

## 擴容 EBS 後需要到 Server 分配空間

進入 Server 執行以下兩指令即可。

```
sudo growpart /dev/nvme0n1 1
sudo resize2fs /dev/nvme0n1p1
```

之後使用 `df -h` 查看

[https://docs.aws.amazon.com/zh\_tw/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html](https://docs.aws.amazon.com/zh\_tw/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html)
