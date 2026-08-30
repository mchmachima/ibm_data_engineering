# A Brief Introduction to Shell Variables

`Tags: shell scripting, shell variables, bash, Linux`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M03 Shell Scripting                       |
| **Lesson**        | L02 A Brief Introduction to Shell Variables |
| **Date studied**  | 2026-08-27                                |

---

## Table of Contents

- [Overview](#overview)
- [What is a Shell Variable](#what-is-a-shell-variable)
- [Reading User Input into a Shell Variable](#reading-user-input-into-a-shell-variable)
- [Key Terms & Glossary](#key-terms--glossary)
- [My Questions & Gaps](#my-questions--gaps)
- [Resources](#resources)

---

## Overview

บทอ่านนี้แนะนำแนวคิดพื้นฐานของ shell variable ซึ่งเป็นวิธีเก็บและเรียกใช้ข้อมูล เช่น ตัวเลขหรือ string โดยอ้างอิงผ่านชื่อตัวแปร เนื้อหาแสดงสองวิธีหลักในการสร้าง shell variable คือการ assign ค่าโดยตรง และการใช้คำสั่ง `read` เพื่อรับ input จากผู้ใช้ ซึ่งเป็นพื้นฐานสำคัญก่อนเข้าสู่การเขียน shell script ที่โต้ตอบกับผู้ใช้ได้

---

## What is a Shell Variable

Shell variable คือวิธีเก็บและเรียกใช้หรือแก้ไขข้อมูล เช่น ตัวเลข, character string, และโครงสร้างข้อมูลอื่น ๆ โดยอ้างอิงผ่านชื่อ

```bash
# assign the value "Jeff" to a new variable called firstname
firstname=Jeff

# access and display the value using $ in front of the variable name
echo $firstname
```

บรรทัดแรก assign ค่า `Jeff` ให้กับตัวแปรใหม่ชื่อ `firstname` บรรทัดถัดไปเรียกดูค่าของตัวแปรด้วยคำสั่ง `echo` ร่วมกับเครื่องหมาย `$` ที่นำหน้าชื่อตัวแปร เพื่อดึงค่าของมันออกมา (ในที่นี้คือ string `Jeff`) นี่คือวิธีพื้นฐานที่สุดในการสร้าง shell variable และ assign ค่าให้มันในขั้นตอนเดียว

---

## Reading User Input into a Shell Variable

อีกวิธีหนึ่งในการสร้าง shell variable คือใช้คำสั่ง `read` เพื่อรับ input จากผู้ใช้ที่ command line

```bash
# wait for user to type input, then store it in the variable lastname
read lastname
Grossman

# display the value just stored by read
echo $lastname
Grossman

# echo the values of multiple variables at once
echo $firstname $lastname
Jeff Grossman
```

หลังจากพิมพ์ `read lastname` shell จะรอให้ผู้ใช้ป้อนข้อความ ค่าที่ป้อน (เช่น `Grossman`) จะถูกเก็บไว้ในตัวแปร `lastname` ทันที คำสั่ง `read` มีประโยชน์มากในการเขียน shell script เพราะสามารถใช้ prompt ให้ผู้ใช้ป้อนข้อมูลระหว่าง script กำลังรันอยู่ได้ นอกจากนี้ยังสามารถ echo ค่าของหลายตัวแปรพร้อมกันได้ในคำสั่งเดียว

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| Shell variable | วิธีเก็บและเรียกใช้ข้อมูล (ตัวเลข, string, โครงสร้างข้อมูล) โดยอ้างอิงผ่านชื่อตัวแปรภายใน shell |
| `=` (assignment) | operator ใช้ assign ค่าให้ shell variable ในขั้นตอนเดียว เช่น `firstname=Jeff` |
| `$` (variable expansion) | เครื่องหมายที่นำหน้าชื่อตัวแปรเพื่อดึงค่าของตัวแปรนั้นออกมาใช้งาน เช่น `$firstname` |
| read (command) | คำสั่งที่รับ input จากผู้ใช้ที่ command line แล้วเก็บค่าไว้ในตัวแปรที่ระบุ |
| Command line argument | ค่าที่ส่งเข้าไปให้ script ตอนเรียกใช้งาน และถูก assign เข้าตัวแปรของ shell โดยอัตโนมัติ |

---

## ❓ My Questions & Gaps

- [x] `read` ต่างจาก command line argument ที่ส่งตอนเรียก script อย่างไร และใช้แทนกันได้ในสถานการณ์ไหนบ้าง
  - **คำตอบ:** `read` รับค่าจากผู้ใช้แบบ interactive คือ script จะหยุดรอจนกว่าผู้ใช้จะพิมพ์ข้อความแล้วกด Enter ระหว่างที่ script กำลังรันอยู่ ส่วน command line argument คือค่าที่ส่งเข้าไปพร้อมกับตอนเรียก script ตั้งแต่ต้น (เช่น `./script.sh Jeff Grossman`) โดยค่าจะถูก assign เข้าตัวแปรพิเศษ (`$1`, `$2`, ...) โดยอัตโนมัติโดยไม่ต้องรอ interaction ใด ๆ เหมาะกับกรณีที่ต้องการรัน script แบบ non-interactive หรือ automate เช่นใน cron job หรือ pipeline ส่วน `read` เหมาะกับกรณีที่ต้องการโต้ตอบกับผู้ใช้ระหว่างรัน หรือไม่รู้ล่วงหน้าว่าจะต้องถามอะไรบ้าง
- [x] ถ้า assign ค่าที่มี space อยู่ในนั้น (เช่น `name=John Doe`) จะเกิดอะไรขึ้น และต้องแก้ด้วย quote อย่างไร
  - **คำตอบ:** ถ้าเขียน `name=John Doe` โดยไม่ใส่ quote shell จะตีความ `John` เป็นค่าที่ assign ให้ตัวแปร `name` และตีความ `Doe` เป็นคำสั่งแยกต่างหาก (แล้วมักจะ error ว่า `Doe: command not found`) เพราะ space คือตัวแบ่งคำ (word splitting) ของ shell วิธีแก้คือครอบค่าที่มี space ด้วย quote เช่น `name="John Doe"` หรือ `name='John Doe'` เพื่อบอก shell ว่าทั้งหมดนี้เป็นค่าเดียวของตัวแปร ไม่ใช่หลายคำสั่งแยกกัน

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมที่กล่าวถึงในบทอ่านนี้
