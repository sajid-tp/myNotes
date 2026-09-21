The syntax of res.cookie()
js
res.cookie(name, value, options)
Argument	What it is
name	The cookie's name — 'token' in your case, this is how the browser labels it
value	The actual data stored — your JWT string
options	An object controlling security/behavior — this is where httpOnly, secure, etc. live
What each option actually does (and why it exists)

httpOnly: true

Blocks JavaScript (running in the browser, via document.cookie) from reading this cookie at all
Why it matters: if an attacker manages to inject malicious JS into your site (an XSS attack), that script still can't steal the token — it simply doesn't have access to httpOnly cookies
Without this flag, any XSS vulnerability anywhere on your site becomes "attacker can steal every logged-in user's session token"

secure: process.env.NODE_ENV === 'production'

When true, the browser will only ever send this cookie over HTTPS, never plain HTTP
Why conditional on environment: in local dev, you're usually running on plain http://localhost, not HTTPS — if you hardcoded secure: true, the cookie would never get sent at all during development (browser silently refuses to attach it), breaking your login flow while testing. In production, you do have HTTPS, so it's safe (and important) to enforce.

sameSite

Controls whether the cookie gets sent on cross-site requests (e.g., a request from a different domain/port than your API)
'strict' — cookie is only sent when the request originates from the exact same site. Maximum protection against CSRF attacks.
'lax' — slightly relaxed; allows the cookie on some cross-site navigation (like clicking a link), but blocks it on cross-site form submissions/fetches from other origins
Why 'lax' in dev: your frontend (say, localhost:3000) and backend (say, localhost:5000) are technically different origins during local development (different ports = different origin, per browser rules). 'strict' would block the cookie from ever being attached to your API calls from the frontend, breaking auth locally. In production, frontend and backend are usually on the same domain (or configured to be treated as same-site), so 'strict' becomes safe and preferable.

maxAge

How long the cookie survives in the browser, in milliseconds (not seconds — a common trip-up)
30 * 24 * 60 * 60 * 1000 → 30 days × 24 hours × 60 minutes × 60 seconds × 1000 ms
The comment // 30 days — match JWT expiry above matters: if maxAge were shorter than the JWT's expiresIn, the cookie would delete itself before the token inside it actually expires — meaning the browser throws away a still-valid token early, logging the user out sooner than intended for no real reason. If maxAge were longer, the cookie would stick around even after the JWT inside it has expired — meaning the browser keeps sending an expired, useless token, and every request would fail jwt.verify() anyway (since exp inside the payload has passed). Either mismatch is a real bug — they should always be kept in sync.
The full options object shape (general reference)
js
res.cookie(name, value, {
  httpOnly: boolean,
  secure: boolean,
  sameSite: 'strict' | 'lax' | 'none',
  maxAge: number,      // milliseconds
  domain: string,      // optional — which domain the cookie applies to
  path: string,        // optional — which routes the cookie applies to (default '/')
  expires: Date         // alternative to maxAge — an exact expiry date instead of a duration
});
