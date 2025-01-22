- 3 types of streams
	- readable streams
	- writeable streams
	- transform streams

```java
fs.createReadStream(myFile.zip)
```
- the container that the chunks of data go into in the stream is called a **buffer**
	- its kinda like a list. it holds bytes 
- pipe allows you to connect one stream to another stream
	- in our example, we're having a transform stream be our next stream
	- then we pipe to a writeable stream
		- usually writeable stream is the last step 
		- 