# passport-openknowl

forked fron jaredhanson/passport-oauth2

## Purpose

is that provide openknowl-passport profile parser

## Customizing

### 1. passport strategy

```javascript

// lib/strategy.js

function OAuth2Strategy(options, verify) {
  ...
  ...
  // modify here
  this.name = 'openknowl';
  options.profileUrl = options.profileURL || 'https://ac-dev.openknowl.com/api/me';
  options.authorizationURL = options.profileURL || 'https://ac-dev.openknowl.com/api/token';
  options.tokenuRL = options.profileURL || 'https://ac-dev.openknowl.com/api/token';
  ...
  ...
```

### 2. profile

```javascript

// lib/strategy.js

OAuth2Strategy.prototype.userProfile = function(accessToken, done) {
  // modify here
  this._oauth2.get(this.profileUrl, accessToken, function (err, body, res) {
    if (err) { return done(new InternalOAuthError('failed to fetch user profile', err)); }

    try {
      var json = JSON.parse(body);

      var profile = { provider: 'linkedin' };

      profile.id = json.id;
      profile.name = json.name;
      profile.emails = json.email;
      profile.imageUrl = json.image;
      
      profile._raw = body;
      profile._json = json;

      done(null, profile);
    } catch(e) {
      done(e);
    }
  });  
};
```

### 3. References

1. https://github.com/auth0/passport-linkedin-oauth2/blob/master/lib/oauth2.js#L23

