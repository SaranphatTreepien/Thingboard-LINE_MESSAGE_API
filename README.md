# ThingsBoard Temperature Alert via LINE Messaging API 🚨🌡️



## 📌 Overview

โปรเจกต์นี้เป็นตัวอย่างการตั้งค่า **ThingsBoard** ให้ส่งการแจ้งเตือนเมื่อค่าอุณหภูมิ (temperature) จากอุปกรณ์ IoT เกิน 30°C ผ่าน **LINE Messaging API** โดยใช้ Rule Chain ของ ThingsBoard

---

## 🚀 Features

- ตรวจสอบค่า temperature จากอุปกรณ์
- แจ้งเตือนผ่าน LINE เมื่ออุณหภูมิสูงเกิน 30°C
- ใช้ Script Transformation Node ใน ThingsBoard เพื่อเตรียมข้อความ
- ส่งข้อความผ่าน REST API Call Node ไปยัง LINE Messaging API

---
## ⚙️ Setup Guide
### 1. สร้าง LINE Channel Access Token

1. เข้าสู่ [LINE Developers Console](https://developers.line.biz/console/)
2. สร้าง Provider และ Channel ใหม่ (Messaging API)
3. คัดลอก **Channel Access Token** สำหรับใช้งานใน ThingsBoard
4. https://www.youtube.com/watch?v=C-1tuVlnEo0
---
### 2. สร้าง Rule Chain ใน ThingsBoard
![image](https://github.com/user-attachments/assets/94d12895-98af-4f79-8d58-051bc28cf1c4)
![image](https://github.com/user-attachments/assets/041b7c0e-8ff8-4b5a-8d5f-a858b35d5cb3)
![image](https://github.com/user-attachments/assets/c51210e4-1939-443a-8d06-41b29d17e451)


#### 2.1 เพิ่ม Script Transformation Node
- ใช้โค้ดจากไฟล์ [`Script.js`](./Script.js) โดยคัดลอกโค้ดนี้ไปวางใน Script Node ของ ThingsBoard

#### 2.2 เพิ่ม REST API Call Node

- ตั้งค่าดังนี้:

| ค่า           | รายละเอียด                              |
| ------------- | ------------------------------------- |
| URL           | `https://api.line.me/v2/bot/message/push` |
| Method        | POST                                  |
| Headers       | `Content-Type: application/json`<br>`Authorization: Bearer {LINE_CHANNEL_ACCESS_TOKEN}` |
| Request Body  | ใช้ค่าจาก `${msg}` (ค่าที่ได้จาก Script Node) **ไม่ต้อง Setbody ใน REST API |
---
![image](https://github.com/user-attachments/assets/3985a061-8949-497f-8196-9e986bebd9ca)
![image](https://github.com/user-attachments/assets/9b4d2f89-718a-4a5f-953a-54735a7c39df)
### 3. ทดสอบระบบ
- ส่ง Attributes,Telemetry ข้อมูล temperature จากอุปกรณ์ผ่าน ThingsBoard
- หากค่า temperature > 30 จะได้รับข้อความแจ้งเตือนใน LINE

> 🚨 **หมายเหตุ:**  
> อย่าลืมเปลี่ยนค่า `USER_ID` และ `LINE_CHANNEL_ACCESS_TOKEN` ให้ถูกต้องก่อนใช้งานจริง

### ขอบคุณที่เนื้อหานี้ประโยชน์ต่อผู้ที่มีความสนใจ 🎉
