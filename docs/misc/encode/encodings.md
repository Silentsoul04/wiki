## Alphabet Code

- AZ/az corresponds to 1-26 or 0-25

## ASCII encoding

![ascii](../../assets/img/encode/ascii.jpg)

### Features

The ascii encoding we generally use is visible characters, and mainly the following characters

- 0-9 -> 48-57
- A-Z -> 65-90
- a-z -> 97-122

### Morph

#### Binary code

Replace the number corresponding to the ascii code with a binary representation.

- only 0 and 1
- No more than 8 bits, generally 7 bits are also possible because visible characters are up to 127.
- Actually another ascii code.

#### Hex code

Replace the number corresponding to the ascii code with a hexadecimal representation.

- A-Z -> 41-5A
- a-z -> 61-7A

### Example

??? example "2018 DEFCON Quals ghettohackers: Throwback"
    The title is described below

    ```
    Anyo!e!howouldsacrificepo!icyforexecu!!onspeedthink!securityisacomm!ditytop!urintoasy!tem!
    ```

    The first point is that we should complete the content of these exclamation marks to get the flag, but after the
    completion is not enough, then we can divide the source string according to `!`, then the string length 1 
    corresponds to the letter a, length 2 Corresponding to the letter b, and so on

    ```python
    ori = 'Anyo!e!howouldsacrificepo!icyforexecu!!onspeedthink!securityisacomm!ditytop!urintoasy!tem!'
    sp = ori.split('!')
    print(''.join(chr(97 + len(s) - 1) for s in sp))
    ```

    In turn, you can get assuming that 0 characters are spaces, because this just makes the original readable.

    ```
    dark logic
    ```

## BaseXX encoding

The XX in base XX indicates how many characters are used for encoding. For example, base64 uses the following 64
character encoding. Since the 6th power of 2 is equal to 64, each 6 bits is a unit corresponding to a printable
character. . There are 24 bits for 3 bytes, corresponding to 4 Base64 units, ie 3 bytes need to be represented by 4
printable characters. It can be used as a transmission encoding for email. Printable characters in Base64 include the
letters AZ, az, numbers 0-9, which have a total of 62 characters, and the two printable symbols differ in different
systems.

![base64](../../assets/img/encode/base64.png)

See [Base64 - Wikipedia](<https://en.wikipedia.org/wiki/Base64>) for details.

If the number of bytes to be encoded cannot be divisible by 3, and there will be 1 or 2 more bytes at the end, you can
use the following method: first use the value of 0 to make up at the end, so that it can be divisible by 3, and then
Encode base64. Add one or two `=` numbers after the encoded base64 text to represent the number of bytes to complement.
That is to say, when the last octet (one byte) remains, the last 6-bit base64 byte block has four bits with a value of
0, and finally two equal signs are appended; if the last two octets remain For a section (2 bytes), the last 6-bit base
byte block has two digits of a value of 0, followed by an equal sign.

Since the complement 0 of the decoding does not participate in the operation, the information can be hidden there.

Similar to base64, base32 uses 32 visible characters for encoding, and 2&#39;s 5th power is 32, so 1 packet per 5 bits.
The 5 bytes are 40 bits, which corresponds to 8 base32 packets, that is, 5 bytes are represented by 8 base32 characters.
However, if it is less than 5 bytes, the first 5 bits smaller than 5 bits will be padded with 5 bits, and the remaining
packets will be padded with `=` until the 5 bytes are filled. It can be seen that base32 has only 6 equal
signs. E.g:

![base32](../../assets/img/encode/base32.png)

### Features

- There may be a `=` at the end of base64, but there are up to 2
- base32 may have a `=` at the end, but up to 6
- The character set will be limited depending on the base
- ** It may be necessary to add the equal sign **
- **=that is 3D**
- See [base rfc](<https://tools.ietf.org/html/rfc4648>) for more information

### Examples

??? example
    The description of the topic can be found
    in `ctf-challenge` [base64-stego directory of misc classification](<https://github.com/ctf-wiki/ctf-challenges/tree/master/misc/encode/computer/base64-stego>)
    The data.txt file.

    Use a script to read steganography information.

    ```python
    def deStego(stegoFile):
        b64table = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
        with open(stegoFile, 'r') as stegoText:
            message = ""
            for line in stegoText:
                try:
                    text = line[line.index("=") - 1:-1]
                    message += "".join([bin(0 if i == '=' else b64table.find(i))[2:].zfill(6) for i in text])[
                               2 if text.count('=') == 2 else 4:6]
                except:
                    pass
        return "".join([chr(int(message[i:i + 8], 2)) for i in range(0, len(message), 8)])
    
    print(deStego("text.txt"))
    ```

    Output:
    
    ```
    flag{BASE64_i5_amaz1ng}
    ```

## Huffman coding

See [Huffman Coding - Wikipedia](<https://en.wikipedia.org/wiki/Huffman_coding>).

## XXencoding

XXencode encodes the input text in units of every three bytes. If the last remaining data is less than three bytes, the
insufficient portion is filled with zeros. These three bytes have a total of 24 Bits and are divided into 4 groups in
6-bit units. Each group is expressed in decimal to indicate that the value that appears will only fall between 0 and 63.
Replace with the position character of the corresponding value.

```text
           1         2         3         4         5         6

 0123456789012345678901234567890123456789012345678901234567890123

 |         |         |         |         |         |         |

 +-0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

See [Xxencoding - Wikipedia](<https://en.wikipedia.org/wiki/Xxencoding>) for specific information.

### Features

- only numbers, uppercase and lowercase letters
- +, -.

## URL Encoding

See [URL Encoding - Wikipedia](<https://en.wikipedia.org/wiki/Percent-encoding>).

### Features

- numerous percent signs

## Unicode encoding

See [Unicode - Wikipedia](<https://en.wikipedia.org/wiki/Unicode>).

Note that it has four manifestations.

### Examples

??? example
    Source text: `The`

    &#x [Hex]: `&#x0054;&#x0068;&#x0065;`

    &# [Decimal]: `&#00084;&#00104;&#00101;`

    \U [Hex]: `\U0054\U0068\U0065`

    \U+ [Hex]: `\U+0054\U+0068\U+0065`
