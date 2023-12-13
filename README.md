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

## Here some examples to get started:

PyCrypto
```
import hashlib

from zokrates_pycrypto.curves import Decaf377
from zokrates_pycrypto.eddsa import PrivateKey, PublicKey
from zokrates_pycrypto.utils import write_signature_for_zokrates_cli

if __name__ == "__main__":

    raw_msg = "This is my secret message"
    msg = hashlib.sha512(raw_msg.encode("utf-8")).digest()

    # sk = PrivateKey.from_rand()
    # Seeded for debug purpose
    key = 5145218980968378210524378404035776788013036277399811805365337506608398016000
    sk = PrivateKey(key, curve=Decaf377)
    sig = sk.sign(msg)

    pk = PublicKey.from_private(sk)
    is_verified = pk.verify(sig, msg)
    print(is_verified)

    path = 'decaf377_zokrates_inputs.txt'
    write_signature_for_zokrates_cli(pk, sig, msg, path)
```

ZoKrates ZOK

```
from "ecc/decaf377.zok" import verifyEddsa;

def main(private field[2] R, private field S, field[2] A, u32[8] M0, u32[8] M1) -> bool {
    bool isVerified = verifyEddsa(R, S, A, M0, M1);
    return isVerified;
}
```

ZoKrates cli
```
zokrates compile -i decaf377dsl.zok -c bls12_377
zokrates setup -b ark -s gm17
zokrates compute-witness -a decaf377_zokrates_inputs 
zokrates generate-proof -b ark -s gm17
zokrates verify
```
