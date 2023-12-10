# Bachelor-Thesis

Wellcome to my bachelor thesis code collection repository.

Clone this repository using `git clone --recursive [URL]` to make sure all submodules are also cloned correctly.

## Other repositories I used but did not modify were:

- For SafeCurve criteria testing on all embedded curves, I used [Jubjub supporting evidence](https://github.com/daira/jubjub).
- For the generator group generation I used the following python code from [cryptobook.nakov.com](https://cryptobook.nakov.com/asymmetric-key-ciphers/elliptic-curve-cryptography-ecc):

```
from tinyec.ec import SubGroup, Curve

field = SubGroup(p=17, g=(15, 13), n=18, h=1)
curve = Curve(a=0, b=7, field=field, name='p1707')
print('curve:', curve)

for k in range(0, 25):
    p = k * curve.g
    print(f"{k} * G = ({p.x}, {p.y})")
```