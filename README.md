---
title: "MD5 in R"
output: html_notebook
---

For years I have used the MD5 algorithm to hash ID's for randomization when running A/B tests.  I know what MD5 does but not how!  Here I reproduce the algorithm in order to gain understanding.  The MD5 algorithm is already implemented in the `digest` package - this notebook is purely for learning.  

References I used to create this include:

* https://en.wikipedia.org/wiki/MD5
* https://www.scribd.com/doc/35954574/MD5-With-Example

First we start with a message - here my message is "Hello World!":

```{r}
msg <- 'Hello World!'
```
We get so used to working with objects in R that it is easy to forget that they are all represented by [bits](https://en.wikipedia.org/wiki/Bit) which roll up to [bytes](https://en.wikipedia.org/wiki/Byte) and [words](https://en.wikipedia.org/wiki/Word_(computer_architecture))!    MD5 requires us to work with bytes and words so we need to find ways to convert our message which is currently a character object. 

```{r}
msg_hex_bytes <- charToRaw(msg)
msg_decimal_bytes <- as.integer(charToRaw(msg))
```

Now we have the message as raw hex bytes ( `r msg_hex_bytes` ) or as raw decimal bytes ( `r msg_decimal_bytes` ).  The length of this message is `r length(msg_hex_bytes)` bytes.

MD5 can operate with messages of any lengths but always returns a 128 bit message.  MD5 requires that we pad the message so that it can be broken down into chunks of length 512 bits.  The padding works as follows _"first a single bit, 1, is appended to the end of the message. This is followed by as many zeros as are required to bring the length of the message up to 64 bits fewer than a multiple of 512. The remaining bits are filled up with 64 bits representing the length of the original message, modulo 264."_ - [wikipedia](https://en.wikipedia.org/wiki/MD5).

Let's convert our message to bits:
```{r}
msg_bits <- rawToBits(charToRaw(msg))
```
Our message in bits is (`r msg_bits`) and the length of the message in bits is `r length(msg_bits)`.


