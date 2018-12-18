# Support blacklisting JWTs

### One Paragraph Explainer

By design, JWTs (JSON Web Tokens) are completely stateless, so once a valid token is signed by an issuer, the token may be verified as authentic by the application. The problem this leads to is the security concern where a leaked token could still be used and unable to be revoked, due to the signature remaining valid as long as the signature provided by the issues matches what the application is expecting.
Due to this, when using JWT authentication, an application should manage a blacklist of expired or revoked tokens to retain user's security in the case a token needs to be revoked.

### `express-jwt-blacklist` example

An example of running `express-jwt-blacklist` on a Node.js project using the `express-jwt`

```javascript
var jwt = require('express-jwt');
var blacklist = require('express-jwt-blacklist');
 
app.use(jwt({
  secret: 'my-secret',
  isRevoked: blacklist.isRevoked
}));
 
app.get('/logout', function (req, res) {
  blacklist.revoke(req.user)
  res.sendStatus(200);
});
```

### What other bloggers say

From the blog by [Marc Busqué](http://waiting-for-dev.github.io/blog/2017/01/25/jwt_secure_usage/):
> ...add a revocation layer on top of JWT, even if it implies losing its stateless nature.
