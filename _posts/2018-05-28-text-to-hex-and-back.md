---
layout: post
title:  "Text to Hex and back"
date:   2018-05-28 02:07:00 +0200
tags:   linux security
---
*In case you want to know the exact binary representation of your data on disk or convert binary data to text. We use hexadecimal numbers as intermediate representation.*

## Preliminaries
I'm using Ubuntu 18.04 as my OS. All tools used are installed on this system by default.

## Get the hexadecimal representation of a string and vice versa
You can use `xxd` to get the hexadecimal representation of a string. In the example below the letter 'a'.
```console
$ echo -n a | xxd
00000000: 61
```

You can use the same program to do the reverse, convert hexadecimal numbers into strings.
```console
$ echo -n 61 | xxd -r -p
a
```

## Convert hexadecimal to binary
Using the `bc` tool you can convert hexadecimal to binary, or any base to another base.
```console
$ echo "ibase=16;obase=2;61" | bc
1100001
```

Do notice that in the example below the resulting binary number has less than 8 digits. Character codes stored on disk would have 8 or 16 digits. Use the `wc` tool to count the digits. Notice we have to use `xargs` to echo the terminal output of the `bc` command into `wc`.

```console
$ echo "ibase=16;obase=2;61" | bc | xargs echo -n | wc -m
7
```

To get an accurate representation of how this would be stored on disk we need to add a 0 to the head of the binary number. You can do this using the `printf` command.

```console
$ echo "ibase=16;obase=2;61" | bc | xargs printf %08d
01100001
```

## Resources
- [Hex string to bytes][stack]

[stack]: https://stackoverflow.com/questions/1604765/linux-shell-scripting-hex-string-to-bytes
