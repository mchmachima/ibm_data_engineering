# Useful Commands for Wrangling Text Files

`Tags: Linux, Shell, text-files, data-wrangling`

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
- [Sorting Lines with sort](#sorting-lines-with-sort)
- [Removing Repeated Lines with uniq](#removing-repeated-lines-with-uniq)
- [Matching Patterns with grep](#matching-patterns-with-grep)
- [Extracting Slices and Fields with cut](#extracting-slices-and-fields-with-cut)
- [Merging Files with paste](#merging-files-with-paste)
- [Key Terms & Glossary](#-key-terms--glossary)
- [My Questions & Gaps](#-my-questions--gaps)
- [Resources](#-resources)

---

## Overview

วิดีโอนี้สอนคำสั่งที่ใช้จัดการและดึงข้อมูลจากไฟล์ text ได้แก่ `sort` สำหรับเรียงลำดับ, `uniq` สำหรับตัดบรรทัดซ้ำ, `grep` สำหรับค้นหา pattern, `cut` สำหรับตัดส่วนของแต่ละบรรทัด และ `paste` สำหรับรวมหลายไฟล์เข้าด้วยกัน คำสั่งเหล่านี้เป็นเครื่องมือพื้นฐานสำหรับ wrangling ข้อมูลแบบ text บน command line

---

## Sorting Lines with sort

`sort` เรียงบรรทัดของไฟล์ตามลำดับตัวอักษร-ตัวเลข (alpha-numerically) แล้วพิมพ์ผลลัพธ์ที่เรียงแล้วออกทาง standard output ใช้ option `-r` เพื่อเรียงย้อนกลับ (reverse order)

```bash
# เรียงบรรทัดของ pets.txt ตามลำดับตัวอักษร
sort pets.txt

# เรียงย้อนกลับ (reverse order)
sort -r pets.txt
```

---

## Removing Repeated Lines with uniq

`uniq` กรองบรรทัดที่ซ้ำกันออก แต่จะลบเฉพาะบรรทัดที่ซ้ำกัน **ติดกัน (consecutive)** เท่านั้น ถ้าบรรทัดซ้ำถูกคั่นด้วยบรรทัดอื่น จะยังถือว่าไม่ซ้ำและไม่ถูกลบ

```bash
# ลบบรรทัดที่ซ้ำกันแบบติดกันออกจาก pets.txt
uniq pets.txt
```

---

## Matching Patterns with grep

`grep` ("global regular expression print") คืนบรรทัดของไฟล์ที่ match กับ pattern ที่ระบุ เช่น regular expression โดย default จะ case-sensitive และสามารถใช้ option `-i` เพื่อค้นหาแบบ case-insensitive

```bash
# ค้นหาบรรทัดที่มีอักขระต่อเนื่อง "ch" (case-sensitive)
grep ch people.txt

# ค้นหาแบบ case-insensitive (จะเจอทั้ง "ch" และ "Ch")
grep -i ch people.txt
```

---

## Extracting Slices and Fields with cut

`cut` ดึงส่วนที่ต้องการออกจากแต่ละบรรทัดของไฟล์ ใช้ได้สองแบบหลัก: ดึงตามตำแหน่งตัวอักษร (character) หรือดึงตาม field ที่คั่นด้วย delimiter

```bash
# ดึงตัวอักษรตำแหน่งที่ 2 ถึง 9 ของแต่ละบรรทัด
cut -c 2-9 people.txt

# ดึง field ที่ 2 (นามสกุล) โดยใช้ space เป็น delimiter
cut -d ' ' -f 2 people.txt
```

---

## Merging Files with paste

`paste` รวมบรรทัดจากหลายไฟล์เข้าด้วยกันแบบเรียงคอลัมน์ (บรรทัดที่ N ของแต่ละไฟล์มารวมกันเป็นบรรทัดที่ N ของผลลัพธ์) โดย default ใช้ tab เป็น delimiter คั่นระหว่างคอลัมน์ และเปลี่ยนได้ด้วย option `-d`

```bash
# รวม first.txt, last.txt, yob.txt เป็นตารางเดียว (คั่นด้วย tab)
paste first.txt last.txt yob.txt

# รวมไฟล์เดียวกัน แต่คั่นด้วย comma แทน
paste -d "," first.txt last.txt yob.txt
```

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| sort | คำสั่งเรียงลำดับบรรทัดของไฟล์ตามตัวอักษร-ตัวเลข รองรับการเรียงย้อนกลับด้วย `-r` |
| uniq | คำสั่งกรองบรรทัดที่ซ้ำกันแบบติดกันออกจากผลลัพธ์ |
| grep | คำสั่งค้นหาบรรทัดที่ match กับ pattern ที่ระบุ (Global Regular Expression Print) |
| cut | คำสั่งดึงส่วนของแต่ละบรรทัด ทั้งแบบตำแหน่งตัวอักษร (`-c`) และแบบ field (`-d`, `-f`) |
| paste | คำสั่งรวมบรรทัดจากหลายไฟล์เข้าด้วยกันแบบเรียงคอลัมน์ default คั่นด้วย tab |
| Delimiter | ตัวอักษรที่ใช้บ่งบอกจุดแบ่งระหว่าง field ในแต่ละบรรทัด เช่น space หรือ comma |
| Field | ส่วนของบรรทัดที่ถูกแบ่งด้วย delimiter เช่น ชื่อและนามสกุลที่คั่นด้วย space |
| Regular expression | pattern สำหรับจับคู่ข้อความที่ `grep` ใช้ในการค้นหา |

---

## ❓ My Questions & Gaps

- [x] ถ้าต้องการลบบรรทัดซ้ำทั้งไฟล์ (ไม่ใช่แค่ที่ติดกัน) ต้องใช้ `sort` ร่วมกับ `uniq` อย่างไร
  - **คำตอบ:** เนื่องจาก `uniq` ลบได้เฉพาะบรรทัดซ้ำที่ติดกันเท่านั้น จึงต้อง `sort` ไฟล์ก่อนเพื่อให้บรรทัดที่เหมือนกันมาอยู่ติดกัน แล้วค่อยส่งผลลัพธ์ต่อให้ `uniq` ผ่าน pipe เช่น `sort pets.txt | uniq` วิธีนี้จะลบบรรทัดซ้ำได้ทั้งไฟล์ไม่ว่าบรรทัดซ้ำจะอยู่ห่างกันแค่ไหนในไฟล์เดิม
- [x] `grep` รองรับ regular expression แบบเต็ม (extended regex) หรือไม่ ต้องใช้ option อะไรเพิ่ม
  - **คำตอบ:** `grep` แบบ default รองรับเฉพาะ Basic Regular Expression (BRE) ซึ่งอักขระพิเศษบางตัวเช่น `+`, `?`, `|`, `()` ต้อง escape ด้วย backslash ถึงจะทำงานเป็น metacharacter ถ้าต้องการใช้ Extended Regular Expression (ERE) ที่ไม่ต้อง escape อักขระเหล่านี้ ต้องเพิ่ม option `-E` (หรือใช้คำสั่ง `egrep` ซึ่งเทียบเท่ากับ `grep -E`)

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมในวิดีโอนี้
