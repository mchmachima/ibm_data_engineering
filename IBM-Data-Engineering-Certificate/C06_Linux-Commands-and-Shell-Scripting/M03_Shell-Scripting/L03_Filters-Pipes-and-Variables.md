# Filters, Pipes, and Variables

`Tags: shell scripting, pipes, filters, environment variables, Linux`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M03 Shell Scripting                       |
| **Lesson**        | L03 Filters, Pipes, and Variables         |
| **Date studied**  | 2026-08-27                                |

---

## Table of Contents

- [Overview](#overview)
- [Filters](#filters)
- [Pipes](#pipes)
- [Shell Variables](#shell-variables)
- [Environment Variables](#environment-variables)
- [Key Terms & Glossary](#key-terms--glossary)
- [My Questions & Gaps](#my-questions--gaps)
- [Resources](#resources)

---

## Overview

วิดีโอนี้อธิบายแนวคิดของ filter และ pipe ซึ่งเป็นกลไกสำคัญที่ทำให้ shell มีพลังในการต่อคำสั่งหลาย ๆ ตัวเข้าด้วยกัน รวมถึงอธิบายความแตกต่างระหว่าง shell variable กับ environment variable และวิธี set, ดูค่า, และลบตัวแปรเหล่านั้น

---

## Filters

Filter คือ shell command หรือโปรแกรมที่รับ input จาก standard input (ปกติคือ keyboard) และส่ง output ออกทาง standard output (ปกติคือ terminal) เราสามารถมองว่า filter คือ transformer โปรแกรมที่แปลง input data ให้กลายเป็น output data ตัวอย่างของ filter ได้แก่ `wc`, `cat`, `more`, `head`, `sort`, `grep` และอื่น ๆ คุณค่าของ filter คือสามารถนำมาต่อกัน (chain) ได้ ซึ่งนำไปสู่คำสั่ง pipe

---

## Pipes

Pipe command แทนด้วยเครื่องหมาย vertical slash (`|`) ช่วยขยายความสามารถของ shell อย่างมาก โดยอนุญาตให้ chain ลำดับของ filter command เข้าด้วยกัน รูปแบบการใช้งานคือ output ของ command 1 จะกลายเป็น input ของ command 2 ต่อไปเรื่อย ๆ pipe จึงย่อมาจาก pipeline

```mermaid
flowchart LR
    A[command 1] -->|pipe| B[command 2] -->|pipe| C[command 3]
```

```bash
# pipe the output of ls to sort with -r option for reverse sorted directory listing
ls | sort -r
```

---

## Shell Variables

Shell variable คือตัวแปรที่มีขอบเขต (scope) จำกัดอยู่แค่ shell ที่มันถูกสร้างขึ้นมาเท่านั้น shell แต่ละตัวจึงมองไม่เห็น shell variable ของกันและกัน

```bash
# list all shell variables and their definitions, piped to head to show just the first 4
set | head -4

# define a new shell variable (no spaces around the = sign)
GREETINGS=hello

# display the value of the variable
echo $GREETINGS
hello

# define another variable
AUDIENCE=world

# echo multiple variables together
echo $GREETINGS $AUDIENCE
hello world

# clear (delete) a variable
unset AUDIENCE
```

การ define ตัวแปรใหม่ใช้เครื่องหมาย `=` assign ค่าให้ชื่อตัวแปรที่เลือก โดยต้องไม่มี space รอบเครื่องหมาย `=` เพื่อลบตัวแปรใช้คำสั่ง `unset` เช่น `unset AUDIENCE` จะลบตัวแปร `AUDIENCE` ทิ้ง

---

## Environment Variables

Environment variable เหมือนกับ shell variable ทุกประการ ยกเว้นว่ามี scope กว้างกว่า โดยจะคงอยู่ใน child process ใด ๆ ที่ถูก spawn โดย shell ที่ตัวแปรนั้นถือกำเนิดขึ้นมา สามารถขยาย shell variable ใด ๆ ให้กลายเป็น environment variable ได้ด้วยคำสั่ง `export`

```bash
# extend the shell variable GREETINGS to become an environment variable
export GREETINGS

# list all environment variables
env

# check whether GREETINGS was exported, by piping env output to grep
env | grep GREE
GREETINGS=hello
```

| ประเภทตัวแปร | Scope |
| --- | --- |
| Shell variable | จำกัดอยู่แค่ shell ปัจจุบันที่สร้างมัน |
| Environment variable | ขยายไปถึง child process ทุกตัวที่ shell นั้น spawn ออกไป |

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| Filter | shell command หรือโปรแกรมที่รับ input จาก standard input และส่ง output ออกทาง standard output ทำหน้าที่แปลง (transform) input data เป็น output data |
| Standard input | ช่องทาง default ที่โปรแกรมใช้รับข้อมูลเข้า ปกติคือ keyboard |
| Pipe (`\|`) | operator ที่ใช้ chain ลำดับของ filter command เข้าด้วยกัน โดย output ของคำสั่งหนึ่งกลายเป็น input ของคำสั่งถัดไป |
| Pipeline | ชื่อเต็มของคำว่า pipe คือลำดับของคำสั่งที่ถูกต่อกันด้วย pipe operator |
| set (command) | คำสั่งที่ list shell variable ทั้งหมดพร้อมค่าของมันที่มองเห็นได้จาก shell ปัจจุบัน |
| unset (command) | คำสั่งที่ใช้ลบ (clear) shell variable ทิ้ง |
| export (command) | คำสั่งที่ขยาย scope ของ shell variable ให้กลายเป็น environment variable |
| Environment variable | shell variable ที่มี scope ขยายไปถึง child process ทุกตัวที่ shell นั้น spawn ออกไป |
| env (command) | คำสั่งที่ list environment variable ทั้งหมดของระบบ |

---

## ❓ My Questions & Gaps

- [x] Environment variable ที่ export ไปแล้ว จะยังคงอยู่หลังจากปิด shell session หรือไม่ ถ้าไม่ ต้องตั้งค่าไว้ที่ไหนถึงจะ persist ข้าม session ได้
  - **คำตอบ:** ไม่คงอยู่ เพราะ environment variable ที่ตั้งด้วย `export` ในระหว่าง session จะอยู่แค่ใน memory ของ shell process นั้นและ child process ของมันเท่านั้น เมื่อปิด shell session ตัวแปรจะหายไปทันที หากต้องการให้ตัวแปร persist ข้าม session ต้องเขียนคำสั่ง `export` นั้นลงในไฟล์ startup ของ shell เช่น `~/.bashrc` หรือ `~/.bash_profile` (สำหรับ bash) เพื่อให้ค่าถูก set ใหม่ทุกครั้งที่เปิด shell session ใหม่
- [x] Pipe กับ output redirection (`>`, `>>`) ต่างกันอย่างไรในทางปฏิบัติ ควรเลือกใช้แบบไหนเมื่อไหร่
  - **คำตอบ:** Pipe (`|`) ใช้ส่ง output ของคำสั่งหนึ่งไปเป็น input ของอีกคำสั่งหนึ่งโดยตรง (process ต่อ process) เหมาะกับกรณีที่ต้องการประมวลผลข้อมูลต่อเนื่องหลายขั้นตอนโดยไม่ต้องเก็บผลลัพธ์กลางไว้ที่ไหน ส่วน output redirection (`>` เขียนทับ, `>>` append) ใช้ส่ง output ของคำสั่งไปเก็บไว้ใน**ไฟล์**แทนที่จะส่งต่อให้คำสั่งอื่น เหมาะกับกรณีที่ต้องการบันทึกผลลัพธ์ไว้ดูภายหลังหรือเก็บเป็น log ทั้งสองอย่างสามารถใช้ร่วมกันได้ในคำสั่งเดียว เช่น `command1 | command2 > output.txt`

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมที่กล่าวถึงในวิดีโอนี้
