- 50% mc 50% code 
- MC will have a lot of questions that people got wrong on midterm

### MC Questions
- 1-2 on streams
	- do you always see a speed improvement if you choose to use streams? 
		- no, not always. only faster for large files 
	- PNGjs 
		- look at the example code and see what type of stream the specific section is
			- the one that is getting piped 
- 1 database related
	- browser -> express -> SQL database
		- issue is that the webserver doesn't speak the same language as the database 
			- all webservers need to install a **"database driver"** to talk to the database 
		- when you want to communicate between a server and database:
			- browsers shouldn't directly talk to a database 
				- this is a security vulnerability 
				- the browser should talk to the web server only 
			- all webservers need to install a **"database driver"** to talk to the database 
- express questions
	- refer to the express slides and express workshop videos to study 
		- especially the PDF 
	- know req.query vs req.params
- midterm based questions
	- questions that ppl got wrong last time

### Written Code Question
- we get 2 files that we need to read (maybe JSON files)
	- 1 file has a bunch of soccer players with stats 
	- 1 file has games that the players participated in and the stats 
- 4 questions we have to answer based on these files
	- Q1, Q2, Q3: only need to read the first file
		- e.g. "who is the tallest soccer player?"
	- Q4: will need to look at both files 
		- e.g. "who is the player that scored the most in 2024?"
- for the code, start with variables that read the files
	- then we can use that for the rest of the exam 
	- read both of the files at the same time
		- not read one and then wait 
		- look up promise.all() for this 