- most modern internet uses concept of streams
- programs need to be loaded into the RAM (memory) before they can start working

Example
- movie on the server needs to be loaded onto the RAM before it can be sent to the user 

### Readable Stream
- connection from one location to another
- we can make a stream to start reading from out mp4 file
	- its kind of like a connecting pipe 
- it will take one chunk of the movie, and fill it in a box
	- that box with data is then sent through the stream to the RAM 
	- then we can take that piece of data and right away send it to our user to play the movie
- so they don't need to wait for all 4gb of the movie to load
	- we can load it piece by piece 
- this is what streams allow us to do
- **stream**
	- connects from one location to another
	- its like a pipe
- **buffer**
	- the chunk of data we are sending through the stream
	- like a box we fill with data to be streamed
	- buffering 

### destructuring

```java
const [firstItem] = process.argv
```
- this will grab the first item in the list and store it in the variable firstItem

```java
const list = ["hi", "john","goodbye"]
const [firstItem, secondItem, thirsItem] = list
```
- firstItem will be hi
- secondItem will be goodbye 

```java
const [,secondI, thirdI] = list;
```
- using the comma will tell it that we don't are about that item and skip it
- so it will only get the second and third item

### Streams
- <mark style="background: #ABF7F7A6;">buffer </mark>
	- is a data type
	- data structure to store and transfer arbitrary binary data
	- buffer is a NodeJS specific datatype 
- <mark style="background: #ABF7F7A6;">stream</mark>
	- abstract interface for working with streaming data
	- lets us send data from one place to another
	- a pipe that data can flow through over time
	- you do not get all the data all at once
```java
const stream = require('fs').createReadStream('./assets/divine-comedy.txt')
```

#### Streams vs Buffers
- streams keep a low memory footprint even with large amounts of data
- streams allow you to process data as soon as it arrives

### Stream Types and APIs
- all streams are <mark style="background: #ABF7F7A6;">event emitters</mark>
- a stream instance is an object that emits events when its internal state changes 
e.g.
```java
s.on('readable', () => {}) //ready to be consumed
s.on('data',(chunk) => {}) //new data is available
s.on('error', (err) => {}) // some error happened
s.on('end', () => {}) //no more data aviailable 
```
- the evens available depend from the type of stream
- you can subscribe to different events on the stream 
- once you get your 'data', it will then be processed into the callback function and do whatever you want with the data

#### Readable Stream
- readable streams represents a source from which data is consumed
- e.g. 
	- ds readStream
	- process.stdin
	- HTTP response
	- HTTP request
	- AWS S3 GetObject
- it supports two modes for data consumption: **flowing** and **paused** (or non-flowing mode)
- data is read from source automatically and chunks are emitted as soon as they are available
- when no more data is available, the **end** event is emitted 

#### Concept of Back Pressure
- <mark style="background: #ABF7F7A6;">backpressure</mark>
	- when writing large amounts of data, you should make sure you handle the **stop write signal** and the **drain event** 
	- e.g. reading data from a fast hard drive and writing that data to another really slow hard drive or server
		- the speed that we read data does not match the speed we write data
		- you get like a traffic jam
		- too much incoming chunks of data from our read stream that can't get written fast enough
	- we need to be able to have them sync up the speeds to read and write data
	  
```java
const {CreateReadStream, CreateWriteStream} = require("fs")

const [,, src, dest] = process.argv
const srcStream = createReadStream(src)
const destStream = CreateWriteStream(dest)

srcStream.on("data", (data) => {
    const canContinue = destStream.write(data)
    if(!canContinue) {
        //we are overflowing the destination so we should pause
        srcStream.pause()
        //we will resume when the destination stream is drained
        destStream.once('drain', () => srcStream.resume())
    }
})
```

#### Duplex Stream
- streams that are both readable and writeable
- e.g. net.Socket

#### Transform Stream
- Duplex streams that can modify or transform the data as it is written and read

##### Anatomy of a transform Stream
1. write data (readable stream going into 1 end of transform stream)
2. transform the data (transform stream)
3. read transformed data (writeable stream)

##### GZIP example
- we want to zip out hi.txt file and send it into our transform stream

1. write data (readable stream going into 1 end of transform stream)
2. transform the data (transform stream)
3. read transformed data (writeable stream)

- so it reads a piece of data from hi.txt. and transforms it into a piece of hi.zip
	- progressively creating this zip file using a transform stream 

### Pipes
- its like a pipe in Linux\
- lets us compose a series of pipes similar to promises
- `readable.pipe(writeableDest)`
	- connects a readable stream to a writable srream
	- transform stream can be used as a destination as well
	- it returns the destination stream allowing for a chain of pipes

Example - not real code
```java
readable
	.pipe(transform1)
	.pipe(transform2)
	.pipe(transform3)
	.pipe(writable)
```

Example - Gzip
```java
const {CreateReadStream, CreateWriteStream} = require("fs")
const {createGzip} = require('zlib')

const [,, src, dest] = process.argv
const srcStream = createReadStream(src)
const destStream = CreateWriteStream(dest)
const gzipStream = createGzip()

srcStream
    .pipe(gzipStream)
    .pipe(destStream)
```
- using this pipe operation takes care of all of our backpressure issues

##### Setup complex pipelines with pipe
```java
readable
	.pipe(decompress)
	.pipe(decrypt)
	.pipe(convert)
	.pipe(encrypt)
	.pipe(compress)
	.pipe(writeToDisk)
```

##### Handling errors (correctly)
- need to attach the error handling to every stream
- can't error handle the way promises do
```java
readable
	.on('error', handleErr)
	.pipe(decompress)
	.on('error', handleErr)
	.pipe(decrypt)
	.on('error', handleErr)
	.pipe(convert)
	.on('error', handleErr)
	.pipe(encrypt)
	.on('error', handleErr)
	.pipe(compress)
	.on('error', handleErr)
	.pipe(writeToDisk)
	.on('error', handleErr)
```
- newer versions of nodeJS can deal with this using pipeline

#### Pipeline
- `stream.pipeline(...streams, callback)`
- pipeline is a function that we call 
- we import pipeline from stream
- we give the pipeline function as many streams as we want to use 
```java
const {CreateReadStream, CreateWriteStream} = require("fs")
const {createGzip} = require('zlib')
const {pipeline} = require("stream")

const [,, src, dest] = process.argv

pipeline (
    CreateReadStream(src),
    createGzip(),
    CreateWriteStream(dest),
    function onEnd (err) {
        if (err) {
            console.error(`Error: ${err}`)
            process.exit(1)
        }
    console.log('Done!')
    }
)
```
- now if in any of these streams, if an error occurs then it will be called out by our callback function 