# random-base56

Generate random token base56 which is alphanumeric upper and lower omitting letters 0 and I and digits zero and one.

This is suitable for secret URLs, whereas base64 includes slash in its charset.

```javascript
const assert = require('assert');
const crypto = require('crypto');
const Letters24 = 'ABCDEFGHJKLMNPQRSTUVWXYZ'; // exclude I and O since too similar to 0 and 1
const Digits8 = '23456789'; // omit 0 and 1 to avoid potential confusion with O and I (and perhaps L)
const Symbols = [Digits8, Letters24, Letters24.toLowerCase()].join('');
assert.equal(Symbols.length, 56);
const length = parseInt(process.env.length || '16');
const string = crypto.randomBytes(length)
.map(value => Symbols.charCodeAt(Math.floor(value*Symbols.length/256)))
.toString();
console.log(string);
```
where we omit letters `O` and `I` and digits `0` and `1` to avoid potential confusion if transcribed by humans.

We can build using its `Dockerfile` as follows:
```
docker build -t random-base56 https://github.com/evanx/random-base56.git
```
where we have tagged the image, so run:
```
docker run -t random-base56 
```
which gives random output e.g. `zQPv2WXCuy43nueh`

Use `length` envar to change from default `16`
```
$ docker run -e length=32 59a47707d0fb
CMZRUgDU5RxwzhDFh7fV5EKAKz6HmXdb
```

