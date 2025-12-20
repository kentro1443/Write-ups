# Transformation (picoCTF 2021)
## Analysis

The challenge gives us a snippet of Python code and a file named enc containing strange Chinese/Unicode characters.

The code:

```python
''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag)2)])
```

This loop iterates through the flag 2 characters at a time.
1. ```ord(flag[i]) << 8```: Takes the first character (8 bits) and shifts it to the top (multiplying by 256 = 2^8)

2. ``` + ord(flag[i + 1]) ```: adds the second character (8 bits) to the bottom

3. ``` chr(...) ```: converts the 16-bit combined number into 1 single Unicode character

## Logic

To solve this, I will loop through every characters of the _enc_ file and split each one back into its 2 original parts:

- Part 1: Shift right by 8 ( >>8 )
- Part 2: Mask with 255 ( &255 ) to retrieve the second letter

## Solution

```python
enc = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸弲㘶㠴挲ぽ"
flag = ""
for char in enc:
    unicode_value = ord(char) # get the Unicode value of the character
    first_letter = chr(unicode_value >>8) # top half
    second_letter = chr(unicode_value & 255) # mask with 255 (11111111) to isolate the bottom
    flag += first_letter + second_letter
print(flag)
```

Final Flag: ``` picoCTF{16_bits_inst34d_of_8_26684c20} ```



