### The Syntax of `jwt.sign()`

```js
jwt.sign(payload, secretOrPrivateKey, options, callback);
```

### Arguments

| Argument | Required? | What it is |
|---|---:|---|
| `payload` | Yes | An object, string, or `Buffer` containing the data to embed in the JWT. |
| `secretOrPrivateKey` | Yes | A secret string for HMAC algorithms such as `HS256`, or a private key for RSA/ECDSA algorithms such as `RS256`. |
| `options` | No | Optional settings such as `expiresIn`, `algorithm`, `issuer`, and `audience`. |
| `callback` | No | An optional callback that runs asynchronously with `(err, token)`. If omitted, the method runs synchronously and returns the token directly. |

### Example

```js
const jwt = require("jsonwebtoken");

const token = jwt.sign(
  { userId: 123 },
  process.env.JWT_SECRET,
  { expiresIn: "1h" }
);

console.log(token);
```

### Callback Example

```js
jwt.sign(
  { userId: 123 },
  process.env.JWT_SECRET,
  { expiresIn: "1h" },
  (err, token) => {
    if (err) {
      console.error(err);
      return;
    }

    console.log(token);
  }
);
```

> **Note:** Keep your secret key private. Do not store sensitive information directly in the JWT payload because its contents can be decoded.
