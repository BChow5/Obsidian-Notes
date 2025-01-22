### What is express js?
- web application framework for node JS
- light weight and minimalist
- provides simple structure and organization for your web apps

### installing Express
1. ensure you have node.js installed 
2. open your termianl and crate a project directory
	1. mkdir myProj && cd myProj
3. type `npm init`
	- this command makes package.json. a list of all dependencies requires for oyur project
4. type `npm install express`
	- should update your package.json to have express and a node_modules folder containing express (and other express files needed)

### hello express
![[Pasted image 20241212190426.png]]

- express started with node app.js
- this is essentially starting a web server for us
- anything you do in express, you can do in node.js
	- express sits on top of Node.js
- you can create a server in node without using Express

### basic express server
![[Pasted image 20241212190619.png]]

### Serving back a string of HTML
![[Pasted image 20241212190645.png]]

### Serving a list of strings as HTML
![[Pasted image 20241212190707.png]]

### Req.query vs req.params
- req.query is when the parameters are options
- req.params when the parameters are mandatory

**req.query**
![[Pasted image 20241212190758.png]]

![[Pasted image 20241212190831.png]]

**req.params**
![[Pasted image 20241212190814.png]]
![[Pasted image 20241212190900.png]]\

- req.params is stricter than req.query but both can be used to get info from the user

### Serving static content
![[Pasted image 20241212191011.png]]

### Serving dynamic content
![[Pasted image 20241212191030.png]]![[Pasted image 20241212191107.png]]
![[Pasted image 20241212192150.png]]
![[Pasted image 20241212192158.png]]