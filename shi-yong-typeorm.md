# 使用TypeORM

可以用來操作 SQL, NoSQL，且整合 typescript 十分方便。

## Table 取名

預設會用 entity 檔案內的小寫 class name 來取名，或是可以用 @Entity("TheDeposit")，table 名稱及為 TheDeposit。

```javascript
@Entity("TheDeposit")
export class Deposit {

}
```

## 主鍵

使用 @PrimaryGeneratedColumn()

````javascript
@Entity()
export class Deposit {
  @PrimaryGeneratedColumn()
  id: number;

```
````

## 使用外鍵 (Foreign key)

使用如下即可對應到 Wallet entity 內的主鍵 Id，並且在 deposit entity 內的欄位稱作為 walletId

```javascript
import { Wallet } from './wallet.entity';

@Entity()
export class Deposit {
  .....

  @ManyToOne(() => Wallet)
  @JoinColumn({ name: 'walletId' })
  wallet: Wallet;
  .....
```
