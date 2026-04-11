进入ida字符串界面，首先看到一个非常标准的base64表
推测含有base64加密
![[Pasted image 20251129120322.png]]
进入主函数，看到了rc4加密算法
![[Pasted image 20251129120737.png]]
写出解密脚本
```python
v6 = [  
    -56045301,  
    1126325548,  
    1037697210,  
    2123048962,  
    1640073719,  
    -454381817,  
    -2146442625,  
    -691840689,  
    1448341866,  
    586039113,  
    -1321770811  
]  
  
v12 = -1872737962 & 0xffffffff  # 32-bit  
  
key = b"flag{do_you_find_it_?}"  
  
  
  
def rc4(key, data):  
    S = list(range(256))  
    j = 0  
  
    for i in range(256):  
        j = (j + S[i] + key[i % len(key)]) & 0xff  
        S[i], S[j] = S[j], S[i]  
  
    i = 0  
    j = 0  
    out = bytearray()  
  
    for b in data:  
        i = (i + 1) & 0xff  
        j = (j + S[i]) & 0xff  
        S[i], S[j] = S[j], S[i]  
        K = S[(S[i] + S[j]) & 0xff]  
        out.append(b ^ K)  
  
    return bytes(out)  
  
  
  
  
cipher_bytes = bytearray()  
  
for t in v6:  
    t &= 0xffffffff  
  
  
    raw1 = (t ^ 0x22222222) ^ v12  
    raw2 = t ^ v12  
  
    tmp = raw1 ^ v12  
    if (tmp & 0x80000000) == 0:   # tmp > 0  
        raw = raw1  
    else:  
        raw = raw2  
  
    # 按小端分成 4 字节  
    cipher_bytes += raw.to_bytes(4, "little")  
  
  
  
plaintext = rc4(key, cipher_bytes)  
  
print("RC4 :", plaintext)
```
发现解密出来是一段base64（b'ZmxhZ3tlYjM1NjRmODlzZjQ1YjI0b2FjMTJkYjhlY30='），往上看发现有一个first函数，点击去看到典型的base64加密算法
![[Pasted image 20251129121728.png]]
直接进行base64解码得到flag
```python
import base64
flag=base64.b64decode("ZmxhZ3tlYjM1NjRmODlzZjQ1YjI0b2FjMTJkYjhlY30=")
print(flag)
```