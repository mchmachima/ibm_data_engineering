# Viewing File Content

`Tags: Linux, Shell, file-content, text-files`

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
- [Printing Entire Files with cat](#printing-entire-files-with-cat)
- [Paging Through Files with more](#paging-through-files-with-more)
- [First and Last Lines with head and tail](#first-and-last-lines-with-head-and-tail)
- [Counting with wc](#counting-with-wc)
- [Key Terms & Glossary](#-key-terms--glossary)
- [My Questions & Gaps](#-my-questions--gaps)
- [Resources](#-resources)

---

## Overview

วิดีโอนี้สอนคำสั่งพื้นฐานสำหรับดูเนื้อหาไฟล์ใน Linux ได้แก่ `cat`, `more`, `head`, `tail` และ `wc` ซึ่งแต่ละคำสั่งเหมาะกับสถานการณ์ต่างกัน เช่น ไฟล์สั้นดูทั้งหมดได้เลย ไฟล์ยาวควรดูทีละหน้าหรือดูเฉพาะส่วนต้น/ท้าย และการนับจำนวนบรรทัด/คำ/ตัวอักษรในไฟล์

---

## Printing Entire Files with cat

`cat` พิมพ์เนื้อหาทั้งไฟล์ออกทาง standard output ในครั้งเดียว เหมาะกับไฟล์สั้น ๆ แต่ถ้าไฟล์ยาวมากจะไหลผ่านหน้าจอจนดูไม่ทัน

```bash
# แสดงเนื้อหาทั้งหมดของ numbers.txt
cat numbers.txt
```

---

## Paging Through Files with more

`more` แสดงเนื้อหาไฟล์ทีละหน้า (page) โดย page คือขนาดของ terminal window ปัจจุบัน กด spacebar เพื่อไปหน้าถัดไป และกด `Q` เพื่อออกจากโปรแกรมกลับสู่ command prompt

```bash
# เปิดดู numbers.txt แบบทีละหน้า
more numbers.txt
```

---

## First and Last Lines with head and tail

`head` แสดง 10 บรรทัดแรกของไฟล์เป็นค่า default ส่วน `tail` แสดง 10 บรรทัดสุดท้าย ทั้งสองคำสั่งใช้ option `-n` เพื่อกำหนดจำนวนบรรทัดที่ต้องการแทนค่า default

```bash
# แสดง 10 บรรทัดแรกของไฟล์ (default)
head numbers.txt

# แสดง 3 บรรทัดแรกเท่านั้น
head -n 3 numbers.txt

# แสดง 10 บรรทัดสุดท้ายของไฟล์ (default)
tail numbers.txt

# แสดง 3 บรรทัดสุดท้ายเท่านั้น
tail -n 3 numbers.txt
```

---

## Counting with wc

`wc` ("word count") นับจำนวนบรรทัด คำ และตัวอักษรในไฟล์ โดย output จะเรียงเป็น line, word, character count ตามลำดับ ตัวเลข character count จะมากกว่าที่นับด้วยมือเพราะ `wc` นับ newline character รวมด้วย (หนึ่งในนั้นคือ end-of-file marker)

| Option | ความหมาย |
| --- | --- |
| `-l` | แสดงเฉพาะจำนวนบรรทัด (line count) |
| `-w` | แสดงเฉพาะจำนวนคำ (word count) |
| `-c` | แสดงเฉพาะจำนวน byte (byte count) |

```bash
# แสดง line, word, character count ของไฟล์
wc pets.txt

# แสดงเฉพาะ line count
wc -l pets.txt
```

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| cat | คำสั่งพิมพ์เนื้อหาทั้งไฟล์ออกทาง standard output ในครั้งเดียว (concatenate) |
| more | คำสั่งแสดงเนื้อหาไฟล์แบบทีละหน้า (page by page) กด spacebar เพื่อไปหน้าถัดไป และ `Q` เพื่อออก |
| head | คำสั่งแสดงบรรทัดแรกของไฟล์ default 10 บรรทัด ปรับจำนวนได้ด้วย `-n` |
| tail | คำสั่งแสดงบรรทัดสุดท้ายของไฟล์ default 10 บรรทัด ปรับจำนวนได้ด้วย `-n` |
| wc | คำสั่งนับจำนวนบรรทัด คำ และตัวอักษรในไฟล์ (word count) |
| Standard output | ช่องทาง default ที่โปรแกรมใช้แสดงผลลัพธ์ ปกติคือหน้าจอ terminal |
| Page (ในบริบท more) | เนื้อหาไฟล์ส่วนที่พอดีกับขนาด terminal window ปัจจุบัน |

---

## ❓ My Questions & Gaps

- [x] `less` ต่างจาก `more` อย่างไร เพราะดูเหมือนหลายที่แนะนำให้ใช้ `less` แทน `more`
  - **คำตอบ:** `less` เลื่อนได้ทั้งไปข้างหน้าและย้อนกลับ (ใช้ลูกศรขึ้น/ลง หรือ PageUp/PageDown) และมีฟีเจอร์ search ในตัว (`/` ค้นไปข้างหน้า, `?` ค้นย้อนกลับ) ส่วน `more` เลื่อนได้ทางเดียวคือไปข้างหน้าเท่านั้น และในบางระบบรุ่นเก่าต้องโหลดทั้งไฟล์ก่อนถึงจะเริ่มแสดงผลได้ ในขณะที่ `less` โหลดแบบ incremental จึงเปิดไฟล์ขนาดใหญ่ได้เร็วกว่า ด้วยเหตุนี้ `less` จึงถูกแนะนำให้ใช้แทน `more` ในการใช้งานทั่วไป (มีคำพูดติดปากว่า "less is more")
- [x] `wc` นับ newline character รวมใน character count เสมอหรือไม่ในทุก encoding (เช่นไฟล์ UTF-8 ที่มีอักขระหลาย byte)
  - **คำตอบ:** ใช่ `wc` นับ newline character รวมเสมอไม่ว่า encoding ใด เพราะ newline (`\n`) เป็น byte เดียวเสมอใน UTF-8 ประเด็นที่ต้องระวังคือความแตกต่างระหว่าง option `-c` กับ `-m`: `-c` นับเป็น byte count (ตัวอักษรหลาย byte ใน UTF-8 เช่นภาษาไทยจะถูกนับมากกว่า 1 ต่อตัวอักษร) ส่วน `-m` นับเป็น character count จริง (นับตาม locale จึงนับอักขระหลาย byte เป็น 1 ตัวถูกต้อง) ดังนั้นถ้าต้องการนับจำนวนตัวอักษรที่แท้จริงในไฟล์ที่มีอักขระหลาย byte ควรใช้ `-m` แทน `-c`

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมในวิดีโอนี้
