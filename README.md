# ThingsBoard Temperature Alert via LINE Messaging API 🚨🌡️

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![ThingsBoard](https://img.shields.io/badge/Platform-ThingsBoard-green.svg)](https://thingsboard.io/)
[![LINE Messaging API](https://img.shields.io/badge/API-LINE%20Messaging-blue.svg)](https://developers.line.biz/en/services/messaging-api/)

---

## 📌 Overview

โปรเจกต์นี้เป็นตัวอย่างการตั้งค่า **ThingsBoard** ให้ส่งการแจ้งเตือนเมื่อค่าอุณหภูมิ (temperature) จากอุปกรณ์ IoT เกิน 30°C ผ่าน **LINE Messaging API** โดยใช้ Rule Chain ของ ThingsBoard

---

## 🚀 Features

- ตรวจสอบค่า temperature จากอุปกรณ์
- แจ้งเตือนผ่าน LINE เมื่ออุณหภูมิสูงเกิน 30°C
- ใช้ Script Transformation Node ใน ThingsBoard เพื่อเตรียมข้อความ
- ส่งข้อความผ่าน REST API Call Node ไปยัง LINE Messaging API

---

## 🛠️ Prerequisites

- ThingsBoard (Community หรือ Professional Edition) ที่ติดตั้งและใช้งานได้
- LINE Official Account พร้อมเปิดใช้งาน Messaging API
- Channel Access Token ของ LINE Messaging API
- USER_ID หรือ groupId ของ LINE ที่ต้องการส่งข้อความ

---

## ⚙️ Setup Guide

### 1. สร้าง LINE Channel Access Token

1. เข้าสู่ [LINE Developers Console](https://developers.line.biz/console/)
2. สร้าง Provider และ Channel ใหม่ (Messaging API)
3. คัดลอก **Channel Access Token** สำหรับใช้งานใน ThingsBoard

---

### 2. สร้าง Rule Chain ใน ThingsBoard

#### 2.1 เพิ่ม Script Transformation Node

- ใส่โค้ดนี้ใน Script Node เพื่อเช็คค่า temperature และสร้างข้อความแจ้งเตือน

if (msg.temperature > 30) {
var newMsg = {};
newMsg.to = "USER_ID"; // แทนที่ด้วย LINE userId หรือ groupId
newMsg.messages = [{
"type": "text",
"text": "⚠️ แจ้งเตือน! อุณหภูมิสูงเกิน 30°C (" + msg.temperature + "°C)"
}];
return {msg: newMsg, metadata: metadata, msgType: msgType};
} else {
return null; // หยุด flow หากไม่เกิน 30
}

text

#### 2.2 เพิ่ม REST API Call Node

- ตั้งค่าดังนี้:

| ค่า           | รายละเอียด                              |
| ------------- | ------------------------------------- |
| URL           | `https://api.line.me/v2/bot/message/push` |
| Method        | POST                                  |
| Headers       | `Content-Type: application/json`<br>`Authorization: Bearer {LINE_CHANNEL_ACCESS_TOKEN}` |
| Request Body  | ใช้ค่าจาก `${msg}` (ค่าที่ได้จาก Script Node) |

---

### 3. ทดสอบระบบ

- ส่ง Telemetry ข้อมูล temperature จากอุปกรณ์ผ่าน ThingsBoard
- หากค่า temperature > 30 จะได้รับข้อความแจ้งเตือนใน LINE

---

## 📚 References

- [ThingsBoard Rule Engine Documentation](https://thingsboard.io/docs/user-guide/rule-engine/)
- [LINE Messaging API Documentation](https://developers.line.biz/en/reference/messaging-api/)
- [LINE Official Account Manager](https://manager.line.biz/)

---

## 📝 License

โปรเจกต์นี้ใช้สัญญาอนุญาตแบบ MIT License - ดูรายละเอียดในไฟล์ [LICENSE](LICENSE)

---

## 🙏 Contact

หากมีข้อสงสัยหรือแนะนำ สามารถติดต่อได้ที่:

- Email: your.email@example.com
- GitHub: [your-github-profile](https://github.com/your-github-profile)

---

> **หมายเหตุ:**  
> อย่าลืมเปลี่ยนค่า `USER_ID` และ `LINE_CHANNEL_ACCESS_TOKEN` ให้ถูกต้องก่อนใช้งานจริง

---

### ขอบคุณที่ใช้โปรเจกต์นี้! 🎉
