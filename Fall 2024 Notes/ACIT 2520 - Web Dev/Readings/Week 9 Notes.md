## Create a virtual server

- The route
path -> file://path/users/username/document/html will omit path to file:///users.....

problem: only allows us to read files on our own computer

solution: file:/// -> http://ipaddress/desktop/index.html means changing protocols to http (Hypertext Transfer Protocol)

- ExpressJS


- Minimalist framwork
	- Flask in Python
	- Express in JS
- Monolithic
	- Logging
	- Auth
	- Django
	- Nest.js

Better to start from minimalist

- Nodemon
  can monitor if there's any changes to the code interactively
- EJS (Embeded JavaScript) or JSX
	- Templating Engines
- Railway (database hosting platform)
- express-session
  record the information of a user for a period of time
  problem:
  not working with logging with social accounts (Oauth -> super complicated)
- passport.js (for Oauth):
  `npm install passport-local`
  need to set things done

## package.json
- Telling other developer which modules are used in the project
Steps:
1. Start a new project
2. type `npm init` in the terminal
3. pressing `Enter`
4. generate `package.json`
5. `npm install something` to install any modules we need
6. create `.gitignore` and add the name of the files to `.gitignore` to avoid github upload the whole module to cloud
For other developers:
1. download the project
2. run `npm install` to install any modules are used in the project if there's a `package.json` file

## nodemon
- The module to let the server auto refresh when there's any changes made to the code
- run it by typing `nodemon file` in the terminal

## Goal: Serving Dynamic Content
