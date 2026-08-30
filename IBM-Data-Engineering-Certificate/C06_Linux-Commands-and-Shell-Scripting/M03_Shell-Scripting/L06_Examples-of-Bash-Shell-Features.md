# Examples of Bash Shell Features

`Tags: shell scripting, bash, metacharacters, quoting, redirection, command substitution, Linux`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M03 Shell Scripting                       |
| **Lesson**        | L06 Examples of Bash Shell Features       |
| **Date studied**  | 2026-08-27                                |

---

## Table of Contents

- [Overview](#overview)
- [Metacharacters](#metacharacters)
- [Quoting](#quoting)
- [Input/Output Redirection](#inputoutput-redirection)
- [Command Substitution](#command-substitution)
- [Command Line Arguments](#command-line-arguments)
- [Key Terms & Glossary](#key-terms--glossary)
- [My Questions & Gaps](#my-questions--gaps)
- [Resources](#resources)

---

## Overview

บทอ่านนี้ขยายความจากวิดีโอ "Useful Features of the Bash Shell" ด้วยตัวอย่างที่ละเอียดขึ้นของแต่ละฟีเจอร์ ได้แก่ metacharacter, quoting, I/O redirection, command substitution, และ command line argument พร้อมตัวอย่าง command และผลลัพธ์จริงที่ใช้งานได้ทันที

---

## Metacharacters

Metacharacter คืออักขระที่มีความหมายพิเศษซึ่ง shell ตีความเป็นคำสั่ง (instruction)

| Metacharacter | ความหมาย |
| --- | --- |
| `#` | นำหน้า comment |
| `;` | ตัวคั่นคำสั่ง (command separator) |
| `*` | wildcard สำหรับ filename expansion |
| `?` | wildcard แทนตัวอักษรตัวเดียวใน filename expansion |

### Pound `#`

ใช้แทน comment ในสคริปต์หรือไฟล์ config ข้อความหลัง `#` ในบรรทัดนั้นจะถูก shell มองข้ามไปทั้งหมด

```bash
#!/bin/bash
# This is a comment
echo "Hello, world!"  # This is another comment
```

Comment มีประโยชน์ในการอธิบายโค้ดหรือไฟล์ config ให้ผู้อ่านคนอื่นเข้าใจบริบทและจุดประสงค์ของโค้ดนั้น ควรใส่ comment ไว้ในจุดที่จำเป็นเพื่อให้โค้ดอ่านง่ายและดูแลรักษาได้ง่ายขึ้น

### Semicolon `;`

ใช้คั่นหลายคำสั่งในบรรทัดเดียว โดยคำสั่งจะถูกรันตามลำดับที่ปรากฏ

```bash
$ echo "Hello, "; echo "world!"
Hello,
world!
```

output ของแต่ละคำสั่ง `echo` จะถูกพิมพ์คนละบรรทัด ตามลำดับที่ระบุไว้ในบรรทัดคำสั่ง เหมาะสำหรับกรณีที่ต้องการรันหลายคำสั่งตามลำดับในบรรทัดเดียว

### Asterisk `*`

ใช้เป็น wildcard แทนลำดับตัวอักษรใด ๆ ก็ได้ รวมถึงไม่มีตัวอักษรเลย

```bash
ls *.txt
```

ในตัวอย่างนี้ `*.txt` เป็น wildcard pattern ที่ match กับไฟล์ใน directory ปัจจุบันที่มีนามสกุล `.txt` ทุกไฟล์ คำสั่ง `ls` จะแสดงชื่อไฟล์ที่ match ทั้งหมด

### Question mark `?`

ใช้เป็น wildcard แทนตัวอักษรเพียง 1 ตัว

```bash
ls file?.txt
```

ในตัวอย่างนี้ `file?.txt` จะ match ไฟล์ที่ชื่อขึ้นต้นด้วย `file` ตามด้วยตัวอักษรใด ๆ 1 ตัว และลงท้ายด้วย `.txt`

---

## Quoting

Quoting คือกลไกที่ใช้ลบความหมายพิเศษของอักขระ, space, หรือ metacharacter อื่น ๆ ใน argument ของคำสั่งหรือใน shell script ใช้เมื่อต้องการให้ shell ตีความอักขระแบบ literal

| Symbol | ความหมาย |
| --- | --- |
| `\` | escape การตีความของ metacharacter |
| `" "` | ตีความ metacharacter ภายใน string ตามปกติ |
| `' '` | escape metacharacter ทั้งหมดภายใน string |

### Backslash `\`

ใช้เป็น escape character บอกให้ shell คงการตีความแบบ literal ของอักขระพิเศษ เช่น space, tab, และ `$` ตัวอย่างเช่น ถ้ามีไฟล์ที่ชื่อมี space อยู่ สามารถใช้ backslash ตามด้วย space เพื่อจัดการ space นั้นแบบ literal ได้

```bash
touch file\ with\ space.txt
```

### Double quotes `" "`

เมื่อ string ถูกครอบด้วย double quotes ตัวอักษรส่วนใหญ่จะถูกตีความแบบ literal ยกเว้น metacharacter ที่ยังถูกตีความตามความหมายพิเศษ เช่น การเข้าถึงค่าตัวแปรด้วยเครื่องหมาย `$`

```bash
$ echo "Hello $USER"
Hello <username>
```

### Single quotes `' '`

เมื่อ string ถูกครอบด้วย single quotes ทุกตัวอักษรและ metacharacter ภายในจะถูกตีความแบบ literal ทั้งหมด

```bash
$ echo 'Hello $USER'
Hello $USER
```

สังเกตว่าแทนที่จะพิมพ์ค่าของ `$USER` ออกมา single quotes ทำให้ terminal พิมพ์ string `$USER` ตามตัวอักษรแทน

---

## Input/Output Redirection

| Symbol | ความหมาย |
| --- | --- |
| `>` | redirect output ไปยังไฟล์ แบบเขียนทับ (overwrite) |
| `>>` | redirect output ไปยังไฟล์ แบบ append |
| `2>` | redirect standard error ไปยังไฟล์ แบบเขียนทับ |
| `2>>` | redirect standard error ไปยังไฟล์ แบบ append |
| `<` | redirect เนื้อหาของไฟล์ให้เป็น standard input |

I/O redirection คือกระบวนการกำหนดทิศทางการไหลของข้อมูลระหว่างโปรแกรมกับแหล่ง input/output ของมัน โดย default โปรแกรมจะอ่าน input จาก standard input (keyboard) และเขียน output ไปที่ standard output (terminal) แต่เมื่อใช้ I/O redirection จะสามารถ redirect input/output ของโปรแกรมไปยัง/จากไฟล์หรือโปรแกรมอื่นได้

### Redirect output `>`

```bash
ls > files.txt
```

คำสั่งนี้จะสร้างไฟล์ `files.txt` ถ้ายังไม่มี แล้วเขียนผลลัพธ์ของคำสั่ง `ls` ลงไป **คำเตือน:** ถ้าไฟล์นั้นมีอยู่แล้ว output จะเขียนทับเนื้อหาเดิมทั้งหมด

### Redirect and append output `>>`

```bash
ls >> files.txt
```

คำสั่งนี้ append ผลลัพธ์ของ `ls` ต่อท้ายไฟล์ `files.txt` โดยยังคงเนื้อหาเดิมที่มีอยู่แล้วไว้

### Redirect standard error `2>`

```bash
ls non-existent-directory 2> error.txt
```

เนื่องจาก directory ไม่มีอยู่จริง shell จะสร้างไฟล์ `error.txt` ถ้ายังไม่มี แล้ว redirect error message ของคำสั่ง `ls` ไปยังไฟล์นั้น **คำเตือน:** ถ้าไฟล์นั้นมีอยู่แล้ว error message จะเขียนทับเนื้อหาเดิมทั้งหมด

### Append standard error `2>>`

```bash
ls non-existent-directory 2>> error.txt
```

คำสั่งนี้ append error message ต่อท้ายไฟล์ `error.txt` โดยไม่เขียนทับเนื้อหาเดิม

### Redirect input `<`

```bash
sort < data.txt
```

คำสั่งนี้จะ sort เนื้อหาของไฟล์ `data.txt`

---

## Command Substitution

Command substitution ใช้รันคำสั่งหนึ่งแล้วนำ output ของมันไปใช้เป็นส่วนหนึ่งของ argument ในคำสั่งอื่น เขียนได้โดยครอบคำสั่งด้วย backtick (`` `command` ``) หรือใช้ syntax `$()` เมื่อคำสั่งที่ครอบไว้ถูก execute output ของมันจะถูกแทนที่ในตำแหน่งนั้น และนำไปใช้เป็น argument ในคำสั่งอื่นได้ วิธีนี้มีประโยชน์มากในการ automate งานที่ต้องใช้ output ของคำสั่งหนึ่งเป็น input ให้อีกคำสั่งหนึ่ง

```bash
# store the current directory path in a variable, move elsewhere, then return using the stored path
$ here=$(pwd)
$ cd path_to_some_other_directory
$ cd $here
```

---

## Command Line Arguments

Command line argument คือ input เพิ่มเติมที่ส่งเข้าไปให้โปรแกรมตอนรันจาก command line interface โดยระบุต่อท้ายชื่อโปรแกรม สามารถใช้ปรับพฤติกรรมของโปรแกรม ส่ง input data หรือระบุตำแหน่ง output ได้ Command line argument ใช้สำหรับส่งค่าให้ shell script

```bash
$ ./MyBashScript.sh arg1 arg2
```

ตัวอย่างนี้ส่ง argument สองตัวคือ `arg1` และ `arg2` ให้กับสคริปต์ `MyBashScript.sh` ซึ่งสามารถเข้าถึงได้จากภายในสคริปต์

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| Wildcard | อักขระที่ใช้แทนตัวอักษรหนึ่งตัวหรือหลายตัวใน filename pattern เช่น `*` และ `?` |
| Escape character | อักขระ (เช่น backslash) ที่ใช้ระบุว่าตัวอักษรถัดไปควรถูกตีความตามตัวอักษร ไม่ใช่ตามความหมายพิเศษ |
| Redirect output (`>`) | operator ที่ redirect standard output ของคำสั่งไปยังไฟล์ที่ระบุ แบบเขียนทับ |
| Redirect and append (`>>`) | operator ที่ redirect standard output ไปยังไฟล์แบบ append ต่อท้าย |
| Redirect standard error (`2>`) | operator ที่ redirect standard error ของคำสั่งไปยังไฟล์ แบบเขียนทับ |
| Append standard error (`2>>`) | operator ที่ redirect standard error ไปยังไฟล์แบบ append ต่อท้าย |
| Redirect input (`<`) | operator ที่ redirect เนื้อหาของไฟล์ให้เป็น standard input ของคำสั่ง |

---

## ❓ My Questions & Gaps

- [ ] ถ้าใช้ `2>&1` เพื่อ redirect ทั้ง standard output และ standard error ไปยังไฟล์เดียวกัน จะต้องเขียน syntax อย่างไร และลำดับการเขียนมีผลหรือไม่
- [ ] Command substitution ด้วย `$()` กับ backtick ต่างกันในทางปฏิบัติอย่างไร มีข้อจำกัดของ backtick ที่ `$()` แก้ไขได้หรือไม่

---

## 🔗 Resources

- ไม่มีลิงก์หรือเอกสารอ้างอิงเพิ่มเติมที่กล่าวถึงในบทอ่านนี้
