# Advanced Bash Scripting

`Tags: shell scripting, bash, conditionals, arrays, for loops, Linux`

| Field             | Value                                    |
| ----------------- | ----------------------------------------- |
| **Certificate**   | IBM Data Engineering Professional Certificate |
| **Course**        | C06 Hands-on Introduction to Linux Commands and Shell Scripting |
| **Module**        | M03 Shell Scripting                       |
| **Lesson**        | L07 Advanced Bash Scripting              |
| **Date studied**  | 2026-08-27                                |

---

## Table of Contents

- [Overview](#overview)
- [Conditionals](#conditionals)
- [Logical Operators](#logical-operators)
- [Arithmetic Calculations](#arithmetic-calculations)
- [Arrays](#arrays)
- [for Loops](#for-loops)
- [Key Terms & Glossary](#key-terms--glossary)
- [My Questions & Gaps](#my-questions--gaps)
- [Resources](#resources)

---

## Overview

บทอ่านนี้เตรียมความพร้อมสำหรับ hands-on lab ของ final project ด้วยแนวคิด Bash scripting ขั้นสูงที่ยังไม่เคยครอบคลุมในคอร์สนี้มาก่อน ได้แก่ conditional statement (if-then-else), logical operator, arithmetic calculation, array, และ for loop ซึ่งเป็นพื้นฐานสำคัญสำหรับเขียนสคริปต์ที่มีตรรกะซับซ้อนขึ้น

---

## Conditionals

Conditional หรือ if statement คือวิธีบอกให้สคริปต์ทำบางอย่างเฉพาะเมื่อเงื่อนไขที่กำหนดเป็นจริงเท่านั้น

```bash
if [ condition ]
then
    statement_block_1
else
    statement_block_2
fi
```

ถ้า condition เป็น true Bash จะรันคำสั่งใน `statement_block_1` ก่อนออกจาก conditional block แล้วรันคำสั่งถัดไปหลัง `fi` ในทางกลับกัน ถ้า condition เป็น false Bash จะรันคำสั่งใน `statement_block_2` ใต้ `else` แทน

**ข้อควรระวัง:**

- ต้องมี space รอบเงื่อนไขภายในวงเล็บเหลี่ยม `[ ]` เสมอ
- ทุก `if` block ต้องจับคู่กับ `fi` เพื่อบอกจุดสิ้นสุดของ block
- `else` block เป็น optional แต่แนะนำให้ใส่ไว้ ถ้าไม่มี `else` และ condition เป็น false จะไม่มีอะไรเกิดขึ้นเลยใน block นั้น

```bash
# check whether the number of command-line arguments ($#) equals 2
if [[ $# == 2 ]]
then
  echo "number of arguments is equal to 2"
else
  echo "number of arguments is not equal to 2"
fi
```

ทั้งวงเล็บเดี่ยว `[ ]` และวงเล็บคู่ `[[ ]]` รองรับการเปรียบเทียบจำนวนเต็ม (integer) เมื่อใช้ operator ต่าง ๆ ส่วนการเปรียบเทียบ string ต้องใช้วงเล็บเดี่ยวเท่านั้น เช่น ถ้าตัวแปร `string_var` มีค่า `"Yes"` เงื่อนไขต่อไปนี้จะเป็น true:

```bash
[ $string_var == "Yes" ]
```

สามารถรวมหลายเงื่อนไขเข้าด้วยกันได้ด้วย "and" operator `&&` หรือ "or" operator `||`:

```bash
if [ condition1 ] && [ condition2 ]
then
    echo "conditions 1 and 2 are both true"
else
    echo "one or both conditions are false"
fi

if [ condition1 ] || [ condition2 ]
then
    echo "conditions 1 or 2 are true"
fi
```

---

## Logical Operators

Logical operator ต่อไปนี้ใช้เปรียบเทียบจำนวนเต็มภายใน if condition block

| Operator | ความหมาย |
| --- | --- |
| `==` | เท่ากับ (is equal to) |
| `!=` | ไม่เท่ากับ (is not equal to) |
| `<=` หรือ `-le` | น้อยกว่าหรือเท่ากับ (less than or equal to) |

```bash
# if a=2, this evaluates to true; otherwise false
$a == 2

# if a is different from 2, this evaluates to true
a != 2

# if a=2, this evaluates to true (a <= 3); this evaluates to false (a <= 1)
a <= 3
a <= 1
```

**เคล็ดลับ:** operator `!` (logical negation) เปลี่ยน true ให้เป็น false และ false ให้เป็น true ส่วน `-le` คือ notation ที่เทียบเท่ากับ `<=`

```bash
a=1
b=2
if [ $a -le $b ]
then
   echo "a is less than or equal to b"
else
   echo "a is not less than or equal to b"
fi
```

ที่แสดงในบทอ่านนี้เป็นเพียงตัวอย่าง logical operator บางส่วนเท่านั้น สามารถศึกษาเพิ่มเติมได้จากแหล่งข้อมูลอย่าง Advanced Bash-Scripting Guide

---

## Arithmetic Calculations

สามารถบวก ลบ คูณ หารจำนวนเต็มได้ด้วย notation `$(( ))`

```bash
# both display the result of adding 3 and 2
echo $((3+2))

a=3
b=2
c=$(($a+$b))
echo $c
```

Bash รองรับเฉพาะ integer arithmetic โดยตรง ไม่รองรับ floating-point arithmetic จึงจะตัดทศนิยมของผลลัพธ์ออกเสมอ (truncate) เช่น `echo $((3/2))` จะพิมพ์ผลลัพธ์ที่ถูกตัดทศนิยมออกเป็น `1` ไม่ใช่ `1.5`

| Symbol | Operation |
| --- | --- |
| `+` | addition |
| `-` | subtraction |
| `*` | multiplication |
| `/` | division |

---

## Arrays

Array คือโครงสร้างข้อมูลในตัวของ Bash เป็น list ที่คั่นด้วย space อยู่ภายในวงเล็บ

```bash
# create and populate an array
my_array=(1 2 "three" "four" 5)

# create an empty array
declare -a empty_array

# append elements to an existing array one at a time
my_array+=("six")
my_array+=(7)

# print the first item of the array (indexing starts from 0)
echo ${my_array[0]}

# print the third item of the array
echo ${my_array[2]}

# print all array elements
echo ${my_array[@]}
```

**เคล็ดลับ:** index ของ array เริ่มนับจาก 0 ไม่ใช่ 1

---

## for Loops

`for` loop ใช้ร่วมกับ indexing เพื่อวนซ้ำผ่าน element ทั้งหมดของ array

```bash
# iterate over each value in the array
for item in ${my_array[@]}; do
  echo $item
done

# OR iterate using each index of the array
for i in ${!my_array[@]}; do
  echo ${my_array[$i]}
done
```

`for` loop ต้องมี `; do` เพื่อเริ่มวนซ้ำ และต้องปิดท้ายด้วย `done` เสมอ

อีกวิธีหนึ่งในการเขียน `for` loop เมื่อรู้จำนวนรอบที่ต้องการล่วงหน้า:

```bash
# print numbers 0 through 6
N=6
for (( i=0; i<=$N; i++ )) ; do
  echo $i
done
```

ตัวอย่างการใช้ `for` loop นับจำนวนและรวมผลรวมของ element ใน array:

```bash
#!/usr/bin/env bash
# initialize array, count, and sum
my_array=(1 2 3)
count=0
sum=0
for i in ${!my_array[@]}; do
  # print the ith array element
  echo ${my_array[$i]}
  # increment the count by one
  count=$(($count+1))
  # add the ith element to the running sum
  sum=$(($sum+${my_array[$i]}))
done
echo "count: $count, sum: $sum"
```

---

## 📖 Key Terms & Glossary

| Term (ศัพท์) | คำอธิบาย (ภาษาไทย) |
| ------------- | -------------------- |
| Conditional (if statement) | โครงสร้าง `if-then-else-fi` ที่สั่งให้สคริปต์รันคำสั่งเฉพาะเมื่อเงื่อนไขที่กำหนดเป็นจริง |
| Logical operator | operator เช่น `==`, `!=`, `<=` ที่ใช้เปรียบเทียบค่าภายในเงื่อนไข |
| Logical negation (`!`) | operator ที่เปลี่ยนค่า true เป็น false และ false เป็น true |
| Arithmetic expansion (`$(( ))`) | notation ที่ใช้คำนวณเลขจำนวนเต็มใน Bash |
| Array | โครงสร้างข้อมูลแบบ list ที่คั่นด้วย space และครอบด้วยวงเล็บ เก็บได้หลาย element โดย index เริ่มที่ 0 |
| for loop | โครงสร้างการวนซ้ำที่ใช้ index หรือ element ของ array เป็นตัวควบคุมรอบ ต้องปิดด้วย `done` |

---

## ❓ My Questions & Gaps

- [ ] ทำไมการเปรียบเทียบ string ต้องใช้วงเล็บเดี่ยว `[ ]` เท่านั้น ใช้วงเล็บคู่ `[[ ]]` แล้วจะเกิดอะไรขึ้น
- [ ] ถ้าต้องการคำนวณเลขทศนิยม (floating-point) ใน Bash โดยไม่ให้ถูก truncate ต้องใช้เครื่องมือหรือวิธีอื่นใดแทน

---

## 🔗 Resources

- Advanced Bash-Scripting Guide (อ้างอิงถึงในบทอ่าน สำหรับศึกษา logical operator เพิ่มเติม)
