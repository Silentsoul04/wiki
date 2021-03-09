When we import the python script, we will generate a corresponding pyc file in the directory, which is a persistent
storage form of pythoncodeobj to speed up the next load.

## File Structure

Pyc file consists of three major parts

- The first 4 bytes is a Maigg int, which identifies the version information of this pyc.
- The next four bytes are still an int, which is the time that pyc is generated.
- Serialized PyCodeObject, structure
  reference [include/code.h](<https://github.com/python/cpython/blob/master/Include/code.h>), serialization
  method [python/marshal](<https://github.com/python/cpython/blob/master/Python/marshal.c>)

**pyc full file parsing can refer to**

- [How the Python program is executed](<http://python.jobbole.com/84599/>)
- [PYC File Format Analysis](<http://kdr2.com/tech/python/pyc-format.html>)

**About co_code**

A string of binary streams, representing the sequence of instructions, specifically defined
in [include/opcode.h](<https://github.com/python/cpython/blob/fc7df0e664198cb05cafd972f190a18ca422989c/Include/opcode.h>)
, or refer to [python Opcodes](<http://unpyc.sourceforge.net/Opcodes.html>).

by

- The directive (opcode) is divided into parameters and no parameters, which are divided
  into <https://github.com/python/cpython/blob/fc7df0e664198cb05cafd972f190a18ca422989c/Include/opcode.h#L69>
- parameter (oparg)

The above parameters of python3.6 always occupy 1 byte. If the instruction has no parameters, it is replaced by `0x00`,
which is ignored by the interpreter during the running process. It is also the technical principle of **Stegosaurus**;
the version is lower than python3.5. The middle instruction has no arguments but no `0x00` padding.

### Example

- [0ctf-2017:py](<https://github.com/ctfs/write-ups-2017/tree/master/0ctf-quals-2017/reverse/py-137>)
- [Remember a CPython bytecode](<http://0x48.pw/2017/03/20/0x2f/>)

## Tools

### [pycdc](<https://github.com/zrax/pycdc>)

!!! info 
    Convert python bytecode to readable python source code, including disassembly (pycads) and decompilation (pycdc)

### [Stegosaurus](<https://bitbucket.org/jherron/stegosaurus/src>)

!!! info 
    Allows us to embed any Payload in a Python bytecode file (pyc or pyo). Due to the low coding density, the process of
    embedding Payload does not change the running behavior of the source code, nor does it change the file size of the 
    source file.

The principle is to use the redundant space in the python bytecode file to hide the complete payload code into these
fragmented spaces.

## Reference

- [A steganographic tool for embedding Payload in Python bytecode – Stegosaurus](<http://www.freebuf.com/sectool/129357.html>)

## Challenges

- [WHCTF-2017:Py-Py-Py](<https://www.xctf.org.cn/library/details/whctf-writeup/>)