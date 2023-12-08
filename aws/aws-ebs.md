# AWS EBS

EBS 為掛載在 EC2 上的儲存體，具有以下特性。

1. **持久性存儲**：EBS卷是持久性的，數據不會在EC2實例停止或終止時丟失。您可以將EBS卷附加到不同的EC2實例上，以便在不同實例之間共享數據。
2. **高性能**：EBS卷可以提供高性能存儲，適用於各種工作負載，包括數據庫、文件存儲和應用程序數據。
3. **多種卷類型**：EBS提供多種卷類型，包括通用用途（gp2）、硬盤（sc1、st1）、提供I/O優化（io1）等。您可以根據應用程序的需求選擇適當的卷類型。
4. **快照**：EBS卷支持創建快照，這是卷的點對點備份。您可以隨時創建卷的快照，以便在需要時還原數據。
5. **自動備份**：通過Amazon EBS快照，您可以自動備份數據，以防止數據丟失。這對於應對意外數據損壞或刪除非常有用。
6. **可擴展性**：您可以根據需要輕鬆擴展EBS卷的大小，而無需停機。
7. **安全性**：EBS卷的數據在傳輸和靜止時都受到加密保護，提供安全的存儲解決方案。
8. **區域和可用區冗余**：EBS數據會自動在多個可用區進行複製，以提供高可用性和容錯性。

## 擴容 EBS 後需要到 Server 分配空間

```
sudo growpart /dev/nvme0n1 1
sudo resize2fs /dev/nvme0n1p1
```

[https://docs.aws.amazon.com/zh\_tw/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html](https://docs.aws.amazon.com/zh\_tw/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html)
