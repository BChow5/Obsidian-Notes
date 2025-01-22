in the terminal for new projects:
- npm install init -y
- npm install express express-sessions

### How does session work?
- how does our browser keep the "fingerprint" for our session?
```javascript
const express = require("express")
const session = require("express-session")

const app = express();

// Use the session middleware
app.use(session({ secret: 'keyboard cat', cookie: { maxAge: 60000 }}))

// Access the session as req.session
app.get('/', function(req, res, next) {
  if (req.session.views) { // checks if we have a session. will be false on first visit
    req.session.views++
    res.setHeader('Content-Type', 'text/html')
    res.write('<p>views: ' + req.session.views + '</p>')
    res.write('<p>expires in: ' + (req.session.cookie.maxAge / 1000) + 's</p>')
    res.end()
  } else {
    req.session.views = 1 // this runs on the first visit 
    // this is a command that creates a session for the user
    // we have a hidden step here because we need the browser to know about the unique session id 
    res.end('welcome to the session demo. refresh!')
  }
})

app.listen(8000, () => {
    console.log("server is running")
});
```
- SessionStore #final 
	- hidden area in express
	- a unique id gets generated for a new session and is inserted into the SessionStore
		- we associate a dictionary with it that stores whatever we want
			- in this example we store `views:1`
- call a hidden command `SET-COOKIE` that express will run
	- inside it gives some name and some value 
		- key = SID, value = the dictionary in the store 
	- up to us when we want the cookie to expire 
- browser has the CookieStore
	- this stores the dictionary  

- method="POST" to hide the info from being in the url for our forms 
- middleware functions - intercept a request
