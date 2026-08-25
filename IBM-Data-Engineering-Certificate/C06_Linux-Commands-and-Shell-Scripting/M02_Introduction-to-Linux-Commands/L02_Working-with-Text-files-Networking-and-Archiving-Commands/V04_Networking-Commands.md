# Networking Commands

`Tags: Networking, Linux, Shell, curl, wget`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M02 Introduction to Linux Commands        |
| **Lesson**        | L02 Working with Text files, Networking and Archiving Commands |
| **Date studied**  | 2026-08-25                                |

---

## Table of Contents

- [Overview](#overview)
- [Checking Identity with hostname](#checking-identity-with-hostname)
- [Interface Details with ip](#interface-details-with-ip)
- [Testing Connectivity with ping](#testing-connectivity-with-ping)
- [Transferring Data with curl](#transferring-data-with-curl)
- [Retrieving Files with wget](#retrieving-files-with-wget)
- [Key Terms & Glossary](#-key-terms--glossary)
- [My Questions & Gaps](#-my-questions--gaps)
- [Resources](#-resources)

---

## Overview

วิดีโอนี้สอนคำสั่งด้าน networking ใน Linux ได้แก่ `hostname`, `ip`, `ping`, `curl`, และ `wget` ซึ่งใช้ตรวจสอบการตั้งค่า network ของเครื่อง ทดสอบความเสถียรของการเชื่อมต่อไปยัง URL และดึงข้อมูล/ไฟล์จาก URL มาเก็บไว้ในเครื่อง

---

## Checking Identity with hostname

`hostname` ใช้ดูหรือตั้งค่า hostname และข้อมูลอื่นที่ระบุตัวตนของเครื่องแบบไม่ซ้ำกัน หากเครื่องมี local domain ตั้งไว้ output จะมี suffix `.local` ต่อท้าย

| Option | ความหมาย |
| --- | --- |
| (ไม่มี option) | แสดง hostname ของเครื่อง |
| `-s` | แสดง hostname แบบตัด domain suffix ออก |
| `-i` | แสดง IP address ของ hostname |

```bash
# แสดง hostname ของเครื่อง (เช่น mylinuxmachine.local)
hostname

# แสดง hostname แบบไม่มี domain suffix
hostname -s

# แสดง IP address ของ hostname
hostname -i
```

---

## Interface Details with ip

`ip` เป็นคำสั่งสำหรับตั้งค่าและแสดงข้อมูล network interface ของเครื่อง ใช้ `ip a` เพื่อดูรายละเอียดของทุก interface เช่น IP address, MAC address และข้อมูลเฉพาะของแต่ละ interface หรือระบุ device เจาะจงเพื่อดูรายละเอียด เช่น จำนวน packet ที่รับ/ส่ง, error, dropped packet

```bash
# แสดงรายละเอียดของทุก communication interface
ip a

# แสดงรายละเอียดของ interface ชื่อ eth0 เท่านั้น
ip address show eth0
```

---

## Testing Connectivity with ping

`ping` ทดสอบการเชื่อมต่อไปยัง host หรือ IP address โดยส่ง ICMP (Internet Control Message Protocol) echo request แล้วพิมพ์ผลลัพธ์ของแต่ละ response ต่อเนื่องไปเรื่อย ๆ จนกว่าจะกด Ctrl+C เพื่อยกเลิก จากนั้นจะสรุปสถิติ เช่น จำนวน packet ที่ส่ง/รับ, เปอร์เซ็นต์ packet ที่หาย, และค่า round-trip time (min/avg/max/standard deviation) ใช้ option `-c` เพื่อกำหนดจำนวนครั้งที่ต้องการ ping แทนการรันไม่จำกัด

```bash
# ping ไปยัง google.com ต่อเนื่องจนกว่าจะกด Ctrl+C
ping google.com

# ping 5 ครั้งแล้วหยุดอัตโนมัติ พร้อมสรุปสถิติ
ping -c 5 google.com
```

---

## Transferring Data with curl

`curl` ใช้ transfer ข้อมูลไปและกลับจาก URL รองรับหลาย protocol โดย default ใช้ HTTP เมื่อเรียกโดยไม่ระบุ option จะพิมพ์เนื้อหา (เช่น HTML) ของ URL ออกทาง standard output และสามารถใช้ option `-o` เพื่อบันทึกผลลัพธ์ลงไฟล์แทนได้

```bash
# ดึงเนื้อหา HTML ของหน้า google.com มาแสดงผล
curl www.google.com

# ดึงเนื้อหาแล้วบันทึกลงไฟล์ google.txt แทนการแสดงผล
curl www.google.com -o google.txt
```

---

## Retrieving Files with wget

`wget` ใช้ดึงไฟล์ที่อยู่ที่ URL คล้ายกับ `curl` แต่เน้นการดาวน์โหลดไฟล์มากกว่า รองรับ protocol น้อยกว่า `curl` แต่มีความสามารถ recursive downloading ซึ่งมีประโยชน์เมื่อ URL ชี้ไปยัง folder ที่มีหลายไฟล์ ระหว่างดาวน์โหลดจะแสดงสถานะ เช่น resolving/connecting to server, HTTP request, และการบันทึกไฟล์ (ตั้งชื่อไฟล์ให้อัตโนมัติตามชื่อบน server)

```bash
# ดาวน์โหลดไฟล์จาก URL มาเก็บไว้ใน current directory
wget https://www.w3.org/TR/PNG/iso_8859-1.txt
```

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| hostname | คำสั่งดูหรือตั้งค่าชื่อ (hostname) และข้อมูลระบุตัวตนของเครื่อง |
| ip | คำสั่งตั้งค่าและแสดงข้อมูล network interface ของเครื่อง |
| ICMP (Internet Control Message Protocol) | protocol ที่ `ping` ใช้ส่ง echo request เพื่อทดสอบการเชื่อมต่อ |
| ping | คำสั่งทดสอบการเชื่อมต่อไปยัง host หรือ IP address ผ่าน ICMP echo request |
| Round-trip time | เวลารวมที่ใช้ในการส่ง request และรับ response กลับ (มีหน่วยเป็น milliseconds) |
| curl | คำสั่ง transfer ข้อมูลไปและกลับจาก URL รองรับหลาย protocol |
| wget | คำสั่งดึงไฟล์จาก URL รองรับ recursive downloading สำหรับดาวน์โหลดหลายไฟล์พร้อมกัน |
| Local domain (.local suffix) | suffix ที่ปรากฏต่อท้าย hostname เมื่อเครื่องมีการตั้งค่า local domain ไว้ |

---

## ❓ My Questions & Gaps

- [x] `curl` กับ `wget` เลือกใช้ตัวไหนดีเมื่อไหร่ ในสถานการณ์แบบใดที่ recursive downloading ของ `wget` จำเป็นจริง ๆ
  - **คำตอบ:** `curl` เหมาะกับงานที่ต้องการควบคุม request แบบละเอียด เช่น ยิง API, ส่ง custom header, ส่ง data แบบ POST หรือใช้ authentication เพราะรองรับ protocol หลากหลายกว่าและมี option จัดการ request/response ครบกว่า ส่วน `wget` เหมาะกับการดาวน์โหลดไฟล์แบบตรงไปตรงมา โดยเฉพาะเมื่อ URL ชี้ไปยัง folder ที่มีหลายไฟล์หรือทั้งเว็บไซต์ ซึ่งต้องใช้ recursive downloading (`-r`) เพื่อไล่ตาม link ทั้งหมดแล้วดาวน์โหลดลงมาให้อัตโนมัติ เช่น การ mirror เว็บไซต์ทั้งหมดหรือดึงไฟล์ทั้งชุดจาก directory listing โดยไม่ต้องระบุ URL ของแต่ละไฟล์เอง
- [x] round-trip time ที่ `ping` รายงาน วัดผ่าน layer ไหนของ network stack และมีปัจจัยอะไรที่ทำให้ค่าผันผวน
  - **คำตอบ:** `ping` วัด round-trip time ที่ layer 3 (Network layer) ของ network stack เพราะ ICMP echo request/reply ทำงานอยู่ที่ระดับ IP โดยตรง ไม่ผ่าน layer การ์ที่สูงกว่าอย่าง TCP/UDP (layer 4) ปัจจัยที่ทำให้ค่าผันผวน (jitter) ได้แก่ ระยะทางและจำนวน hop ระหว่างต้นทาง-ปลายทาง, ความคับคั่งของ traffic บน network (congestion), คุณภาพหรือความเสถียรของสัญญาณ (เช่น wifi เทียบกับสาย), และภาระงานของ router/server ปลายทางที่ต้องตอบ echo request ในขณะนั้น

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมในวิดีโอนี้
