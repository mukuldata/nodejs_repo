# OAuth

### **Table of Contents**

1. **What is OAuth?**
2. **How OAuth Works?**
3. **Why Use OAuth for Authentication?**
4. **Setting Up Google OAuth**
5. **Setting Up Facebook OAuth**
6. **Setting Up GitHub OAuth**
7. **Conclusion**

***

### **1. What is OAuth?**

OAuth (Open Authorization) is a protocol that allows users to **log in** to applications using their existing accounts (Google, Facebook, GitHub, etc.) instead of creating new credentials.

#### **Example:**

When you click **"Login with Google"**, OAuth allows the website to access your Google profile without storing your password.

***

### **2. How OAuth Works?**

OAuth authentication process includes the following steps:

1. **User clicks "Login with Google/Facebook/GitHub".**
2. **The app redirects the user** to the OAuth provider (Google, Facebook, GitHub).
3. **User grants permission** to share profile data.
4. **OAuth provider sends back a token** to the app.
5. **The app uses the token** to fetch user details and authenticate.

***

### **3. Why Use OAuth for Authentication?**

✔ **No need to store passwords** → More secure.\
✔ **Faster logins** → Users don’t have to remember another password.\
✔ **Supports multiple authentication providers** (Google, Facebook, GitHub, etc.).

***

### **4. Setting Up Google OAuth in Node.js**

#### **Step 1: Install Dependencies**

```bash
npm install express passport passport-google-oauth20 express-session dotenv
```

#### **Step 2: Create a Google OAuth App**

1. Go to **Google Developer Console** → [https://console.developers.google.com](https://console.developers.google.com/).
2. Create a new project → Enable **Google+ API**.
3. Go to **Credentials** → Create **OAuth Client ID**.
4. Add Redirect URI: `http://localhost:3000/auth/google/callback`.
5. Copy **Client ID** and **Client Secret**.

#### **Step 3: Configure Google OAuth in Node.js**

📌 **server.js**

```javascript
const express = require('express');
const passport = require('passport');
const session = require('express-session');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
require('dotenv').config();

const app = express();

// Session middleware
app.use(session({ secret: 'mysecret', resave: false, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());

// Configure Google OAuth
passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
    return done(null, profile);
}));

passport.serializeUser((user, done) => done(null, user));
passport.deserializeUser((obj, done) => done(null, obj));

// Routes
app.get('/auth/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

app.get('/auth/google/callback', passport.authenticate('google', {
    successRedirect: '/dashboard',
    failureRedirect: '/'
}));

app.get('/dashboard', (req, res) => {
    res.send(`Welcome ${req.user.displayName}`);
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

#### **Step 4: Add Environment Variables**

📌 **.env**

```
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
```

#### **Step 5: Run the Application**

```bash
node server.js
```

Now, open `http://localhost:3000/auth/google`, and you'll be able to log in with Google!

***

### **5. Setting Up Facebook OAuth in Node.js**

#### **Step 1: Install Dependencies**

```bash
npm install passport-facebook
```

#### **Step 2: Create a Facebook App**

1. Go to **Facebook Developer Portal** → [https://developers.facebook.com](https://developers.facebook.com/).
2. Create a new app and navigate to **Facebook Login**.
3. Add **Valid OAuth Redirect URI**: `http://localhost:3000/auth/facebook/callback`.
4. Copy **App ID** and **App Secret**.

#### **Step 3: Configure Facebook OAuth in Node.js**

📌 **server.js**

```javascript
const FacebookStrategy = require('passport-facebook').Strategy;

passport.use(new FacebookStrategy({
    clientID: process.env.FACEBOOK_APP_ID,
    clientSecret: process.env.FACEBOOK_APP_SECRET,
    callbackURL: "/auth/facebook/callback",
    profileFields: ['id', 'displayName', 'email']
}, (accessToken, refreshToken, profile, done) => {
    return done(null, profile);
}));

app.get('/auth/facebook', passport.authenticate('facebook'));

app.get('/auth/facebook/callback', passport.authenticate('facebook', {
    successRedirect: '/dashboard',
    failureRedirect: '/'
}));
```

#### **Step 4: Add Environment Variables**

📌 **.env**

```
FACEBOOK_APP_ID=your_facebook_app_id
FACEBOOK_APP_SECRET=your_facebook_app_secret
```

#### **Step 5: Run the Application**

Now, open `http://localhost:3000/auth/facebook`, and you'll be able to log in with Facebook!

***

### **6. Setting Up GitHub OAuth in Node.js**

#### **Step 1: Install Dependencies**

```bash
npm install passport-github
```

#### **Step 2: Create a GitHub OAuth App**

1. Go to **GitHub Developer Settings** → [https://github.com/settings/developers](https://github.com/settings/developers).
2. Click **"New OAuth App"**.
3. Add **Authorization Callback URL**: `http://localhost:3000/auth/github/callback`.
4. Copy **Client ID** and **Client Secret**.

#### **Step 3: Configure GitHub OAuth in Node.js**

📌 **server.js**

```javascript
const GitHubStrategy = require('passport-github').Strategy;

passport.use(new GitHubStrategy({
    clientID: process.env.GITHUB_CLIENT_ID,
    clientSecret: process.env.GITHUB_CLIENT_SECRET,
    callbackURL: "/auth/github/callback"
}, (accessToken, refreshToken, profile, done) => {
    return done(null, profile);
}));

app.get('/auth/github', passport.authenticate('github'));

app.get('/auth/github/callback', passport.authenticate('github', {
    successRedirect: '/dashboard',
    failureRedirect: '/'
}));
```

#### **Step 4: Add Environment Variables**

📌 **.env**

```
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
```

#### **Step 5: Run the Application**

Now, open `http://localhost:3000/auth/github`, and you'll be able to log in with GitHub!

***

### **7. Conclusion**

🔹 **Google OAuth** – Best for **widespread usage**.\
🔹 **Facebook OAuth** – Used for **social media apps**.\
🔹 **GitHub OAuth** – Best for **developer-based applications**.

&#x20;**OAuth provides a secure way to authenticate users without handling passwords directly.**
