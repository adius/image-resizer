# Image Resizer

GraphicsMagick powered, multi threaded image resizing middleware.


## Installation

```sh
npm install --save image-resize-middleware
```


## Usage

```js
const path = require('path')
const express = require('express')
const imageResizer = require('image-resize-middleware')
const port = 3000
const publicDirectory = path.join(__dirname, 'public')

const app = express()

app.use(imageResizer.getMiddleware({
	basePath: path.join(publicDirectory, 'images'),
	thumbnailsPath: path.join(publicDirectory, 'thumbs'),
}))
app.use(express.static(publicDirectory))

app.listen(
	port,
	() => console.log('Listening on http://localhost:' + port)
)
```

Every request for an image at `/images/<path>/<to>/<image-file>`
which also contains a width, height, max-width or max-height query parameter
will be scaled to the specified value.

For example a request for
`http://localhost:3000/images/test.png?max-width=50&max-height=50`
will return an image which is proportionally scaled to a maximum bounding box
of 50px by 50px.

All images to be scaled are added to a queue.
The queue is worked off with one worker per core.
