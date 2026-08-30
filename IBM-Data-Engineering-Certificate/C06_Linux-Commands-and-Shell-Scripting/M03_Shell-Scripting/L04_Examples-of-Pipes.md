# Examples of Pipes

`Tags: shell scripting, pipes, filters, grep, JSON, Linux`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M03 Shell Scripting                       |
| **Lesson**        | L04 Examples of Pipes                     |
| **Date studied**  | 2026-08-27                                |

---

## Table of Contents

- [Overview](#overview)
- [What are Pipes](#what-are-pipes)
- [Combining sort and uniq](#combining-sort-and-uniq)
- [Applying a Command to Strings and Files with tr](#applying-a-command-to-strings-and-files-with-tr)
- [Extracting Information from JSON Files](#extracting-information-from-json-files)
- [Key Terms & Glossary](#key-terms--glossary)
- [My Questions & Gaps](#my-questions--gaps)
- [Resources](#resources)

---

## Overview

บทปฏิบัตินี้สาธิตตัวอย่างการใช้ pipe จริงเพื่อแก้ปัญหาการประมวลผลข้อมูลพื้นฐาน ตั้งแต่การรวมคำสั่ง `sort` กับ `uniq` เพื่อกรองบรรทัดซ้ำ การใช้ `tr` แปลงตัวอักษรผ่าน string หรือไฟล์ ไปจนถึงการดึงค่าฟิลด์เฉพาะออกจากไฟล์ JSON ด้วย `grep` ที่ใช้ extended regex หลังเรียนจบวิดีโอนี้ ผู้เรียนจะสามารถอธิบายว่า pipe คืออะไร ใช้ pipe รวมคำสั่งเพื่อจัดการ string และเนื้อหาไฟล์ text ได้ และใช้ pipe ดึงข้อมูลจาก URL/JSON ได้

---

## What are Pipes

Pipe คือคำสั่งใน Linux ที่อนุญาตให้นำ output ของคำสั่งหนึ่งมาใช้เป็น input ของอีกคำสั่งหนึ่ง รูปแบบทั่วไปคือ:

```
[command 1] | [command 2] | [command 3] ... | [command n]
```

ไม่มีขีดจำกัดว่าจะ chain pipe ต่อกันได้กี่ครั้ง

---

## Combining sort and uniq

`sort` เรียงลำดับบรรทัดของไฟล์ text และแสดงผลลัพธ์ ส่วน `uniq` พิมพ์ไฟล์ text โดยยุบบรรทัดที่ซ้ำกันติดกัน (consecutive) ให้เหลือบรรทัดเดียว

สมมติไฟล์ `pets.txt` มีเนื้อหา:

```
goldfish
dog
cat
parrot
dog
goldfish
goldfish
```

| คำสั่ง | ผลลัพธ์ | ปัญหา |
| --- | --- | --- |
| `sort pets.txt` | เรียงลำดับตัวอักษร แต่ยังมี `dog`, `goldfish` ซ้ำ | ไม่ตัดบรรทัดซ้ำที่ไม่ติดกัน |
| `uniq pets.txt` | ตัดบรรทัดซ้ำที่ติดกันเท่านั้น | บรรทัดซ้ำที่ไม่ติดกันยังเหลืออยู่ |
| `sort pets.txt \| uniq` | ได้รายการ unique ทั้งหมด (`cat`, `dog`, `goldfish`, `parrot`) | ไม่มี — เพราะ sort ทำให้ค่าที่เหมือนกันมาติดกันก่อน แล้ว uniq จึงตัดซ้ำได้หมด |

```bash
# print only unique lines from pets.txt
sort pets.txt | uniq
```

---

## Applying a Command to Strings and Files with tr

บางคำสั่งเช่น `tr` (translate) รับได้เฉพาะ standard input เท่านั้น ไม่รับ string หรือ filename ตรง ๆ จึงต้องใช้ pipe เพื่อส่งข้อมูลเข้าไปให้ `tr` ประมวลผล

```bash
# replace all vowels in a string with underscores
$ echo "Linux and shell scripting are awesome!" | tr "aeiou" "_"
L_n_x _nd sh_ll scr_pt_ng _r_ _w_s_m_!

# replace all consonants (complement of vowels) with underscores using -c
$ echo "Linux and shell scripting are awesome!" | tr -c "aeiou" "_"
_i_u__a_____e______i__i___a_e_a_e_o_e_

# convert all text in a file to uppercase
$ cat pets.txt | tr "[a-z]" "[A-Z]"
GOLDFISH
DOG
CAT
PARROT
DOG
GOLDFISH
GOLDFISH

# combine sort, uniq, and tr to get unique lines in uppercase
$ sort pets.txt | uniq | tr "[a-z]" "[A-Z]"
CAT
DOG
GOLDFISH
PARROT
```

`tr [OPTIONS] [target characters] [replacement characters]` คือรูปแบบคำสั่งของ `tr` โดย option `-c` ใช้ทำ complement ของชุดตัวอักษรที่ระบุ (กลับชุดตัวอักษรเป้าหมาย — จากเดิมที่ระบุ "สระ" (`aeiou`) กลายเป็นจับคู่ "พยัญชนะ" แทน) สังเกตว่าตัวอย่างสุดท้ายรวมทั้งสามคำสั่ง (`sort`, `uniq`, `tr`) เข้าด้วยกันในหนึ่ง pipeline ได้ ซึ่งแสดงให้เห็นว่า pipe สามารถ chain กันได้ไม่จำกัดจำนวน

---

## Extracting Information from JSON Files

สามารถใช้ `grep` ดึงค่าฟิลด์เฉพาะออกจากไฟล์ JSON ได้ เช่น ดึงราคาปัจจุบันของ Bitcoin (BTC) เป็น USD สมมติว่าเราคัดลอกผลลัพธ์ JSON ที่ได้จากการเรียก API ราคา cryptocurrency มาเก็บไว้ในไฟล์ชื่อ `Bitcoinprice.txt` โดยมีเนื้อหาบางส่วนดังนี้:

```json
{
  "coin": {
    "id": "bitcoin",
    "icon": "https://static.coinstats.app/coins/Bitcoin6l39t.png",
    "name": "Bitcoin",
    "symbol": "BTC",
    "rank": 1,
    "price": 57907.78008618953,
    "priceBtc": 1,
    "volume": 48430621052.9856,
    ...
  }
}
```

ฟิลด์ที่ต้องการดึงออกมาคือ `"price": [ตัวเลข].[ตัวเลข]"` ใช้คำสั่ง `cat` อ่านเนื้อหาไฟล์แล้ว pipe ต่อไปให้ `grep` กรองด้วย extended regex:

```bash
# extract the "price" field from a JSON file
$ cat Bitcoinprice.txt | grep -oE "\"price\"\s*:\s*[0-9]*\.?[0-9]*"
"price": 57907.78008618953
```

รายละเอียดของ pattern:

| ส่วนของคำสั่ง/pattern | ความหมาย |
| --- | --- |
| `-o` | ให้ grep คืนค่าเฉพาะส่วนที่ match เท่านั้น |
| `-E` | เปิดใช้ extended regex symbol เช่น `?` |
| `\"price\"` | match กับ string `"price"` |
| `\s*` | match ช่องว่าง (whitespace) จำนวนเท่าไหร่ก็ได้ (รวม 0 ตัว) |
| `:` | match ตัวอักษร `:` |
| `[0-9]*` | match ตัวเลขจำนวนเท่าไหร่ก็ได้ |
| `\.?` | match จุดทศนิยม `.` แบบ optional (มีหรือไม่มีก็ได้) |

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| uniq | คำสั่งที่พิมพ์ไฟล์ text โดยยุบบรรทัดที่ซ้ำกันติดกัน (consecutive duplicate) ให้เหลือบรรทัดเดียว |
| tr | คำสั่ง translate ใช้แทนที่ตัวอักษรในข้อความ รับ input เฉพาะจาก standard input เท่านั้น |
| Complement (regex, `-c`) | option ของ `tr` ที่กลับชุดตัวอักษรเป้าหมาย เช่นจาก "สระ" กลายเป็น "พยัญชนะ" |
| Extended regex (`-E`) | โหมด regular expression ของ grep ที่รองรับ symbol เพิ่มเติม เช่น `?`, `+` |
| `-o` (grep option) | option ที่ทำให้ grep คืนค่าเฉพาะส่วนของบรรทัดที่ match กับ pattern เท่านั้น |

---

## ❓ My Questions & Gaps

- [x] ทำไม `sort` ก่อน `uniq` จึงจำเป็น ถ้าสลับลำดับเป็น `uniq | sort` จะได้ผลลัพธ์ต่างกันหรือไม่
  - **คำตอบ:** จำเป็น เพราะ `uniq` ตัดได้เฉพาะบรรทัดที่ซ้ำกันแบบติดกัน (consecutive) เท่านั้น ไม่ได้ตัดบรรทัดซ้ำที่กระจายอยู่คนละที่ในไฟล์ การ `sort` ก่อนจะจัดให้ค่าที่เหมือนกันมาอยู่ติดกันเสมอ ทำให้ `uniq` ตัดซ้ำได้ครบทุกตัว หากสลับเป็น `uniq | sort` ผลลัพธ์จะต่างกัน (และมักผิด) เพราะ `uniq` จะทำงานบนไฟล์ที่ยังไม่ได้เรียงลำดับก่อน ตัดได้แค่บรรทัดซ้ำที่บังเอิญติดกันอยู่แล้วเท่านั้น บรรทัดซ้ำที่ไม่ติดกันจะหลุดรอดไปให้ `sort` เรียงลำดับ แต่ก็ยังคงมีค่าซ้ำเหลืออยู่ในผลลัพธ์สุดท้าย
- [x] การ parse JSON ด้วย `grep`/regex แบบนี้ มีข้อจำกัดหรือความเสี่ยงอะไรบ้างเมื่อเทียบกับการใช้เครื่องมือเฉพาะทางอย่าง `jq`
  - **คำตอบ:** การใช้ `grep`/regex parse JSON มีความเสี่ยงหลายอย่าง เช่น regex ไม่เข้าใจโครงสร้างของ JSON (nested object, array, ลำดับ key ที่ต่างกัน) จึงอาจ match ผิดฟิลด์ได้ถ้ามี key ชื่อคล้ายกันซ้อนอยู่คนละระดับ, ไม่รองรับกรณีค่าที่มี escape character หรือ string ที่มีเครื่องหมาย `"` อยู่ข้างใน, และ pattern ต้องเขียนขึ้นเฉพาะเจาะจงกับ format ของไฟล์นั้น ๆ ทำให้เปราะบางเมื่อโครงสร้าง JSON เปลี่ยนแปลงเพียงเล็กน้อย ในทางกลับกัน `jq` เป็นเครื่องมือที่เข้าใจโครงสร้าง JSON จริง ๆ จึงดึงค่าตาม path ได้แม่นยำและทนทานกว่า (เช่น `jq '.coin.price'`) เหมาะกับการใช้งานจริงมากกว่า regex ซึ่งควรใช้เฉพาะกรณีง่าย ๆ หรือไม่มีเครื่องมือ JSON parser ให้ใช้เท่านั้น

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมที่กล่าวถึงในบทปฏิบัตินี้ นอกจากไฟล์ตัวอย่าง `pets.txt` และ `Bitcoinprice.txt`
