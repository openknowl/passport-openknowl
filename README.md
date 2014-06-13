# passport-openknowl

forked fron jaredhanson/passport-oauth2

### 1. Purpose

is to provide openknowl-passport profile parser

### 2. Usage

if you want more practical example, see https://github.com/jaredhanson/passport-facebook

```javascript

OpenknowlStrategy = require('passport-openknowl').Strategy;


passport.use(new OpenknowlStrategy({
  clientID: OPENKNOWL_APP_ID,
  clientSecret: OPENKNOWL_APP_SECRET,
  callbackURL: OPENKNOWL_CALLBACK_URL
}, function(accessToken, refreshToken, profile, done) {
     User.findOrCreate(profile.id, ...
}));

```

### 3. Customizing

#### 3.1 passport strategy

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

#### 3.2 profile

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

### 4. References

1. https://github.com/auth0/passport-linkedin-oauth2/blob/master/lib/oauth2.js#L23

