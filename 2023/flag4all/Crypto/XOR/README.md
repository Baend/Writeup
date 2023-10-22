# XOR

## EN

XOR is perfect if the key length is the same as the message length, but that's a lot and I'm lazy, 6 characters is enough not to be broken by a brute-force attack.

Auteur : Raccoon (BZHack Friends)

## Files
 - [output.txt](./output.txt)
 - [source.py](./source.py)

## Solve

```python
#!/usr/bin/env python

full_xor_key = bytes.fromhex('889a67aab9bbfce644dca4bcf8e311dfa3bfade115def7eeaab31089f3f1')

def xor(msg, key):
        return bytes([char^key[i%len(key)] for i, char in enumerate(msg)])

flag_xor_key = bytes.fromhex('889a67aab9f1')
key = xor(flag_xor_key, b'FLAG{}')

full = xor(full_xor_key, key)
print(full)
```

```
└─$ python solve.py
b'FLAG{720b1f06572a3c7335bde6d1}'
```