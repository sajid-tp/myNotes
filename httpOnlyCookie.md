### The Syntax of `res.cookie()`

```js
res.cookie(name, value, options);
```

### Arguments

| Argument | What it is |
|---|---|
| `name` | The cookie's name. For example, `'token'`. This is how the browser labels the cookie. |
| `value` | The actual data stored in the cookie, such as a JWT string. |
| `options` | An object that controls the cookie's security and behavior. |

### Example

```js
res.cookie("token", token, {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: process.env.NODE_ENV === "production" ? "strict" : "lax",
  maxAge: 30 * 24 * 60 * 60 * 1000
});
```

### Cookie Options

#### `httpOnly: true`

Prevents JavaScript running in the browser from reading the cookie through `document.cookie`.

This is important because if an attacker injects malicious JavaScript into your website through an XSS attack, the script cannot directly steal the token from an `httpOnly` cookie.

```js
httpOnly: true
```

> Without `httpOnly`, an XSS vulnerability could allow an attacker to steal users' session tokens.

#### `secure`

```js
secure: process.env.NODE_ENV === "production"
```

When `secure` is `true`, the browser sends the cookie only over HTTPS and never over plain HTTP.

The value is commonly conditional because local development often uses:

```text
http://localhost
```

If `secure: true` is used during local HTTP development, the browser may refuse to send the cookie, which can break authentication.

In production, your application should use HTTPS, so enabling `secure` protects the cookie during transmission.

#### `sameSite`

Controls whether the browser sends the cookie with cross-site requests.

```js
sameSite: "strict"
```

The cookie is sent only when the request originates from the same site. This provides strong protection against CSRF attacks.

```js
sameSite: "lax"
```

The cookie is allowed on some cross-site navigation, such as clicking a link, but is generally blocked for cross-site form submissions and requests.

```js
sameSite: "none"
```

The cookie can be sent with cross-site requests. When using `"none"`, the `secure` option must also be set to `true`.

Example:

```js
sameSite: "none",
secure: true
```

> In local development, the frontend and backend may run on different ports, such as `localhost:3000` and `localhost:5000`. Depending on your setup, `"lax"` may be more practical than `"strict"`.

#### `maxAge`

Defines how long the cookie remains in the browser, in milliseconds.

```js
maxAge: 30 * 24 * 60 * 60 * 1000
```

The calculation represents:

```text
30 days × 24 hours × 60 minutes × 60 seconds × 1000 milliseconds
```

This value sets the cookie lifetime to 30 days.

The cookie lifetime should match the JWT expiration time:

```js
const token = jwt.sign(
  { userId },
  process.env.JWT_SECRET,
  { expiresIn: "30d" }
);

res.cookie("token", token, {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: "lax",
  maxAge: 30 * 24 * 60 * 60 * 1000
});
```

If `maxAge` is shorter than the JWT expiration time, the browser deletes the cookie while the JWT may still be valid. This can log the user out earlier than expected.

If `maxAge` is longer than the JWT expiration time, the browser keeps sending an expired JWT. Authentication will still fail because `jwt.verify()` checks the JWT's expiration time.

> Keep the cookie lifetime and JWT expiration time synchronized.

### Complete Options Object

```js
res.cookie(name, value, {
  httpOnly: boolean,
  secure: boolean,
  sameSite: "strict" | "lax" | "none",
  maxAge: number,      // milliseconds
  domain: string,      // optional
  path: string,        // optional; default is "/"
  expires: Date         // optional alternative to maxAge
});
```

### Option Reference

| Option | Type | Description |
|---|---|---|
| `httpOnly` | `boolean` | Prevents browser JavaScript from accessing the cookie. |
| `secure` | `boolean` | Sends the cookie only over HTTPS when set to `true`. |
| `sameSite` | `"strict" \| "lax" \| "none"` | Controls whether the cookie is sent with cross-site requests. |
| `maxAge` | `number` | Cookie lifetime in milliseconds. |
| `domain` | `string` | Optional domain to which the cookie applies. |
| `path` | `string` | Optional URL path to which the cookie applies. The default is `"/"`. |
| `expires` | `Date` | Optional exact expiration date. It can be used instead of `maxAge`. |
