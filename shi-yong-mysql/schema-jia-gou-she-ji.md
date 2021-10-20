# Schema 架構設計

## 實用工具

{% embed url="https://my.vertabelo.com/" %}

> 設計後可以自動產生 schema

![](<../.gitbook/assets/截圖 2021-04-12 上午11.57.32.png>)

範例：

```sql
-- Created by Vertabelo (http://vertabelo.com)
-- Last modification date: 2021-04-14 03:15:49.561

-- tables
-- Table: block_user
CREATE TABLE block_user (
    id serial  NOT NULL,
    user_account_id int  NOT NULL,
    user_account_id_blocked int  NOT NULL,
    CONSTRAINT block_user_ak_1 UNIQUE (user_account_id, user_account_id_blocked) NOT DEFERRABLE  INITIALLY IMMEDIATE,
    CONSTRAINT block_user_pk PRIMARY KEY (id)
);

-- Table: conversation
CREATE TABLE conversation (
    id serial  NOT NULL,
    user_account_id int  NOT NULL,
    time_started timestamp  NOT NULL,
    time_closed timestamp  NULL,
    CONSTRAINT conversation_pk PRIMARY KEY (id)
);

-- Table: grade
CREATE TABLE grade (
    id serial  NOT NULL,
    user_account_id_given int  NOT NULL,
    user_account_id_received int  NOT NULL,
    grade int  NOT NULL,
    CONSTRAINT grade_ak_1 UNIQUE (user_account_id_given, user_account_id_received) NOT DEFERRABLE  INITIALLY IMMEDIATE,
    CONSTRAINT grade_pk PRIMARY KEY (id)
);

-- Table: message
CREATE TABLE message (
    id serial  NOT NULL,
    conversation_id int  NOT NULL,
    user_account_id int  NOT NULL,
    participant_id int  NOT NULL,
    message_text text  NOT NULL,
    time_sent timestamp  NOT NULL,
    time_edit timestamp  NOT NULL,
    CONSTRAINT message_ak_1 UNIQUE (conversation_id, user_account_id) NOT DEFERRABLE  INITIALLY IMMEDIATE,
    CONSTRAINT message_pk PRIMARY KEY (id)
);

-- Table: participant
CREATE TABLE participant (
    id serial  NOT NULL,
    conversation_id int  NOT NULL,
    user_account_id int  NOT NULL,
    time_joined timestamp  NOT NULL,
    time_left timestamp  NULL,
    CONSTRAINT participant_ak_1 UNIQUE (conversation_id, user_account_id) NOT DEFERRABLE  INITIALLY IMMEDIATE,
    CONSTRAINT participant_pk PRIMARY KEY (id)
);

-- Table: user_account
CREATE TABLE user_account (
    id serial  NOT NULL,
    first_name varchar(64)  NOT NULL,
    last_name varchar(64)  NOT NULL,
    nickname varchar(64)  NOT NULL,
    age varchar(32)  NULL,
    details text  NULL,
    email varchar(128)  NOT NULL,
    last_login_time timestamp  NOT NULL,
    create_time timestamp  NOT NULL,
    popularity decimal(5,2)  NULL,
    interested_in_relation varchar(32)  NULL,
    interested_in_gender varchar(32)  NULL,
    tag varchar(256)  NULL,
    point int  NOT NULL,
    membership_type varchar(32)  NOT NULL,
    account_status varchar(32)  NOT NULL,
    CONSTRAINT user_account_ak_1 UNIQUE (email) NOT DEFERRABLE  INITIALLY IMMEDIATE,
    CONSTRAINT user_account_pk PRIMARY KEY (id)
);

-- Table: user_photo
CREATE TABLE user_photo (
    id serial  NOT NULL,
    user_account_id int  NOT NULL,
    link text  NOT NULL,
    details text  NULL,
    time_added timestamp  NOT NULL,
    active bool  NOT NULL,
    type varchar(32)  NOT NULL,
    size int  NOT NULL,
    CONSTRAINT user_photo_pk PRIMARY KEY (id)
);

-- foreign keys
-- Reference: block_user_user_account (table: block_user)
ALTER TABLE block_user ADD CONSTRAINT block_user_user_account
    FOREIGN KEY (user_account_id)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: block_user_user_account_blocked (table: block_user)
ALTER TABLE block_user ADD CONSTRAINT block_user_user_account_blocked
    FOREIGN KEY (user_account_id_blocked)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: grade_given_user_account (table: grade)
ALTER TABLE grade ADD CONSTRAINT grade_given_user_account
    FOREIGN KEY (user_account_id_given)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: grade_recieved_user_account (table: grade)
ALTER TABLE grade ADD CONSTRAINT grade_recieved_user_account
    FOREIGN KEY (user_account_id_received)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: message_participant (table: message)
ALTER TABLE message ADD CONSTRAINT message_participant
    FOREIGN KEY (participant_id)
    REFERENCES participant (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: participant_conversation (table: participant)
ALTER TABLE participant ADD CONSTRAINT participant_conversation
    FOREIGN KEY (conversation_id)
    REFERENCES conversation (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: participant_user_account (table: participant)
ALTER TABLE participant ADD CONSTRAINT participant_user_account
    FOREIGN KEY (user_account_id)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: thread_user_account (table: conversation)
ALTER TABLE conversation ADD CONSTRAINT thread_user_account
    FOREIGN KEY (user_account_id)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- Reference: user_photo_user_account (table: user_photo)
ALTER TABLE user_photo ADD CONSTRAINT user_photo_user_account
    FOREIGN KEY (user_account_id)
    REFERENCES user_account (id)  
    NOT DEFERRABLE 
    INITIALLY IMMEDIATE
;

-- End of file.


```

## Messaging APP

> User: 存入 conversation Id List（參與的對話列表）
>
> Messages: 存入每個對話的細節以及 conversation Id
>
> Conversations: Key 為 conversation Id，存入該對話最後的傳輸人與時間與最後傳輸的訊息內容
>
> 使用：對話列表為查詢 User conversation Id List 對應出的 Conversations Table 內容

> ，點進去則查詢 conversation Id 對應的所有 messages

{% embed url="https://stackoverflow.com/questions/6033062/messaging-system-database-schema" %}

{% embed url="https://stackoverflow.com/questions/6420264/creating-a-threaded-private-messaging-system-like-facebook-and-gmail" %}

[https://stackoverflow.com/questions/6541302/thread-messaging-system-database-schema-design](https://stackoverflow.com/questions/6541302/thread-messaging-system-database-schema-design)
