---
published: true
title: 'Python: ROT13 Encryption Implementation'
permalink: /2016/python-rot13-implementation
author: bradford
layout: post
categories:
  - Technology
  - Tutorials
tags:
  - Python
comments: true
---
_As I learn more about encryption, I enjoy exploring its many aspects, including this ancient method of encryption called the **Caesar Cipher**, or recently called **ROT13**._<!--more-->


[_From Wikipedia:_](https://en.wikipedia.org/wiki/Caesar_cipher)

>In cryptography, a Caesar cipher, also known as Caesar's cipher, the shift cipher, Caesar's code or Caesar shift, is one of the simplest and most widely known encryption techniques. It is a type of substitution cipher in which each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet. For example, with a left shift of 3, D would be replaced by A, E would become B, and so on. The method is named after Julius Caesar, who used it in his private correspondence.
>The encryption step performed by a Caesar cipher is often incorporated as part of more complex schemes, such as the Vigenère cipher, and still has modern application in the ROT13 system. As with all single-alphabet substitution ciphers, the Caesar cipher is easily broken and in modern practice offers essentially **no communication security**.


![Caesar Cipher]({{site.img-dir-posts}}/CaesarCipher.png)


This is a very simple script - read in each character from stdin, rotate it by the appropriate number of characters, and print out the resulting characters.


One of the more difficult parts of this simple task was learning the powerful [argparse library for Python](https://docs.python.org/3/library/argparse.html). String parsing is always a time-consuming and potentially error-prone part of programming, so I wanted to simplify the process as much as possible, and argparse definitely delivered. As you'll see below, in a few simple lines it enforces argument mutual exclusion, help text, and all the parsing that goes along with them. I won't make this post about argparse, but I love it.


The second problem I encountered was encoding. Python 3.x uses unicode encoding, which happens to be the same as ASCII as long as the characters are below 127. To keep this simple, we just encode A-Z and a-z and throw an error to stdout whenever we encounter a character outside those bounds. The logic is very simple. Go ahead and give the script a whirl, make sure you understand how it works. The '-h' argument will help you understand how the other arguments work.

### ROT13 Final Product
[Source on GitHub](https://github.com/elBradford/snippets/blob/master/rot13.py)

~~~python
import argparse
import sys

# Author: Bradford Law
# Author Website: https://bradford.la
# Description: A simple rot13 implementation for Python 3.x

# get args with argparse library:
# https://docs.python.org/3/library/argparse.html
parser = argparse.ArgumentParser(description='A simple rot13 implementation in Python.', prog='rot13')
group = parser.add_mutually_exclusive_group(required=True)
group.add_argument('-e','--encode', action='store_true', help='encode stdin')
group.add_argument('-d','--decode', action='store_true', help='decode stdin')
parser.add_argument('rot', type=int, metavar='ROT', default=13, nargs='?', help='provide rotational offset (default: %(default)s)')
args = parser.parse_args()

# if we are decoding, we simply reverse the rot
if (args.decode):
    args.rot *= -1

# parse stdin and apply rot to each character
input = sys.stdin.read()
parseflag = False
for c in input:
    charnumber = ord(c)
    # check for A-Z, A=65, Z=90
    if charnumber >= 65 and charnumber <= 90:
        charnumber -= 65
        print(chr((charnumber + args.rot)%26 + 65), end="")
    # check for a-z, a=97, z=122
    elif charnumber >= 97 and charnumber <= 122:
        charnumber -= 97
        print(chr((charnumber + args.rot)%26 + 97), end="")
    else:
        print(c, end="")
        parseflag = True

# warn user if characters couldn't be encrypted
if parseflag:
    sys.stderr.write("Some characters could not be encrypted.\n\n")
~~~

What if we want to encode a binary file with a Caesar Cipher? Great question! Base64 would be a great fit with this and would allow us to take any input data (binary, unicode, anything) and perform our rotational encryption algorithm on it. The complete product is posted below.

### ROT64 Final Product
[Source on GitHub](https://github.com/elBradford/snippets/blob/master/rot64.py)

~~~python
import argparse
import sys
import base64

# Author: Bradford Law
# Author Website: https://bradford.la
# Description: A Caesar Cipher implementation for Python 3.x that accepts any binary data,

# get args with argparse library:
# https://docs.python.org/3/library/argparse.html
parser = argparse.ArgumentParser(description='A Caesar Cipher implementation for Python 3.x that accepts any binary data from stdin or file input.', prog='rot64')
group = parser.add_mutually_exclusive_group(required=True)
group.add_argument('-e','--encode', action='store_true', help='encode stdin')
group.add_argument('-d','--decode', action='store_true', help='decode stdin')
parser.add_argument('-i','--input', type=argparse.FileType('r'), default=sys.stdin, help='specify an input file instead of stdin')
parser.add_argument('rot', type=int, metavar='ROT', default=13, nargs='?', help='provide rotational offset (default: %(default)s)')
args = parser.parse_args()

# if we are decoding, we simply reverse the rot
if (args.decode):
    args.rot *= -1

# let's build a dictionary (and its inverse) of base64 characters plus '='
base64chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/='
lup = tuple((base64chars))

# perform our rotation
def rot(string):
    rotated = ""
    for c in string:
        cloc = lup.index(c)
        rotated += lup[(cloc + args.rot)%len(lup)]
    return rotated

# encode stdin or file as base64
if args.encode:
    input64 = base64.standard_b64encode(args.input.read().encode())
    print(rot(input64.decode()), end="")
else:
    inputrot = rot(args.input.read())
    print(base64.standard_b64decode(inputrot).decode(), end="")
~~~
