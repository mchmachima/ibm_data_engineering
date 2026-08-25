# A Brief Introduction to Networking

`Tags: Networking, Linux, IP-address, URL`

| Field            | Value                                                           |
| ---------------- | --------------------------------------------------------------- |
| **Certificate**  | IBM Data Engineering Professional Certificate                   |
| **Course**       | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**       | M02 Introduction to Linux Commands                              |
| **Lesson**       | L02 Working with Text files, Networking and Archiving Commands  |
| **Date studied** | 2026-08-25                                                      |

---

## Table of Contents

- [Overview](#overview)
- [Computer Networks](#computer-networks)
- [Hosts, Clients, and Servers](#hosts-clients-and-servers)
- [Packets and Pings](#packets-and-pings)
- [URLs and IP Addresses](#urls-and-ip-addresses)
- [Key Terms & Glossary](#-key-terms--glossary)
- [My Questions & Gaps](#-my-questions--gaps)
- [Resources](#-resources)

---

## Overview

บทความ (optional reading) นี้ปูพื้นฐานแนวคิดเรื่อง computer networking ก่อนที่จะไปเรียนคำสั่งด้าน networking ในวิดีโอถัดไป ครอบคลุมความหมายของ network, node, host/client/server, packet และ ping รวมถึงความหมายของ IP address และ URL

---

## Computer Networks

Computer network คือกลุ่มของคอมพิวเตอร์ที่สามารถสื่อสารกันและแชร์ทรัพยากร (resource) ที่ network node ต่าง ๆ จัดเตรียมไว้ ตัวอย่างเช่น LAN, WAN และ Internet ซึ่งเป็นเครือข่ายของเครือข่ายขนาดใหญ่

- **Network resource** คือ object ใด ๆ (เช่น ไฟล์หรือเอกสาร) ที่สามารถระบุตัวตนได้ด้วยชื่อและ address ที่ไม่ซ้ำกัน
- **Network node** คือ device ใด ๆ ที่เป็นส่วนหนึ่งของ network เช่น modem, network switch, hub, wifi hotspot ไม่จำเป็นต้องเป็นคอมพิวเตอร์

---

## Hosts, Clients, and Servers

- **Host** คือ node ชนิดพิเศษที่ทำหน้าที่เป็นได้ทั้ง server หรือ client บน network
- **Server** คือ host ที่รับ connection จาก client host และตอบสนอง resource request ที่ client ร้องขอ โดย host หนึ่งตัวสามารถทำหน้าที่ได้ทั้งสองบทบาทพร้อมกัน

---

## Packets and Pings

**Network packet** คือชุดข้อมูล (chunk) ที่ถูกจัดรูปแบบเพื่อส่งผ่าน network โดยแต่ละ packet ประกอบด้วยข้อมูล 2 ส่วน:

| ส่วนของ Packet      | ความหมาย                                                                      |
| ------------------- | ----------------------------------------------------------------------------- |
| Control information | ข้อมูลเกี่ยวกับวิธีและปลายทางในการส่ง payload เช่น source/destination address |
| Payload             | เนื้อหาข้อความที่ต้องการส่งจริง                                               |

คำสั่ง `ping` ทำงานโดยส่ง packet ชนิด "echo request" ไปยัง host แล้วรอการตอบกลับ (response) เพื่อทดสอบและ troubleshoot การเชื่อมต่อ

---

## URLs and IP Addresses

- **IP (Internet Protocol)** กำหนดรูปแบบของข้อมูลที่ถูกส่งผ่าน internet หรือ local network
- **IP address** คือรหัสที่ใช้ระบุตัวตนของ host แต่ละตัวบน network แบบไม่ซ้ำกัน ใช้สร้าง connection และแลกเปลี่ยน packet กับ host นั้นได้ โดย IP packet จะบรรจุ IP address ของทั้ง source และ destination host ไว้ในตัวมันเอง
- **URL (Uniform Resource Locator)** หรือที่รู้จักกันในชื่อ web address ใช้ระบุตัวตนของ web resource แบบไม่ซ้ำกันและเข้าถึง resource นั้นได้ โดยทั่วไปมักชี้ไปยัง web page แต่ก็ใช้กับงานอื่นได้ เช่น ส่งไฟล์ ส่งอีเมล หรือเข้าถึงฐานข้อมูล

ตัวอย่าง URL: `https://en.wikipedia.org/wiki/URL` ประกอบด้วย protocol (`https`), hostname (`en.wikipedia.org`), และ file name (`/wiki/URL`)

---

## 📖 Key Terms & Glossary

| Term (ศัพท์)                   | คำอธิบาย (ภาษาไทย)                                                                      |
| ------------------------------ | --------------------------------------------------------------------------------------- |
| Computer network               | กลุ่มของคอมพิวเตอร์ที่สื่อสารกันและแชร์ resource ที่ network node จัดเตรียมไว้          |
| Network resource               | object ใด ๆ ที่ระบุตัวตนได้ด้วยชื่อและ address ไม่ซ้ำกัน เช่น ไฟล์หรือเอกสาร            |
| Network node                   | device ใด ๆ ที่เป็นส่วนหนึ่งของ network เช่น modem, switch, hub                         |
| Host                           | node ชนิดพิเศษที่ทำหน้าที่เป็นได้ทั้ง server หรือ client บน network                     |
| Server                         | host ที่รับ connection จาก client และตอบสนอง resource request                           |
| Client                         | host ที่ส่ง request ไปยัง server เพื่อขอ resource                                       |
| Network packet                 | ชุดข้อมูลที่ถูกจัดรูปแบบเพื่อส่งผ่าน network ประกอบด้วย control information และ payload |
| ping                           | คำสั่งที่ส่ง echo request packet ไปยัง host แล้วรอ response เพื่อทดสอบการเชื่อมต่อ      |
| IP (Internet Protocol)         | มาตรฐานที่กำหนดรูปแบบของข้อมูลที่ส่งผ่าน internet หรือ local network                    |
| IP address                     | รหัสระบุตัวตนของ host บน network แบบไม่ซ้ำกัน                                           |
| URL (Uniform Resource Locator) | web address ที่ระบุตัวตนของ web resource และใช้เข้าถึง resource นั้น                    |

---

## ❓ My Questions & Gaps

- [x] IPv4 กับ IPv6 ต่างกันอย่างไร และเหตุใดจึงต้องมี IPv6
  - **คำตอบ:** IPv4 ใช้ address ขนาด 32 บิต (เช่น `192.168.1.1`) จึงรองรับ address ได้ประมาณ 4.3 พันล้านหมายเลขเท่านั้น ส่วน IPv6 ใช้ address ขนาด 128 บิต (เช่น `2001:0db8:85a3::8a2e:0370:7334`) ทำให้รองรับจำนวน address ได้มากขึ้นมหาศาล IPv6 ถูกสร้างขึ้นเพราะจำนวน device ที่ต่อ internet เพิ่มขึ้นเรื่อย ๆ จน address แบบ IPv4 เริ่มไม่พอใช้งาน (IPv4 address exhaustion) นอกจากนี้ IPv6 ยังออกแบบมาให้มี overhead ในการประมวลผล header ที่มีประสิทธิภาพกว่าด้วย
- [x] port number มีบทบาทอย่างไรเพิ่มเติมจาก IP address ในการระบุ service บน host เดียวกัน
  - **คำตอบ:** IP address ใช้ระบุตัวตนของ host บน network แต่ host หนึ่งตัวสามารถรัน service หรือ application หลายตัวพร้อมกันได้ (เช่น web server, mail server) port number คือหมายเลข (0-65535) ที่ใช้ระบุว่า packet นั้นควรถูกส่งไปยัง service ใดบน host นั้น เช่น port 80 สำหรับ HTTP หรือ port 443 สำหรับ HTTPS ดังนั้นการเชื่อมต่อจริงจึงระบุด้วยคู่ IP address + port number ไม่ใช่ IP address เพียงอย่างเดียว

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมนอกเหนือจากเนื้อหาในบทความนี้
