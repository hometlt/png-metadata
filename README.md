# png-metadata
library to read and write PNG Metadata

w3 PNG Chunks specification: https://www.w3.org/TR/PNG-Chunks.html

The Metadata in PNG files: https://dev.exiv2.org/projects/exiv2/wiki/The_Metadata_in_PNG_files
 
 
 ### example with buffers
```javascript
  function loadFileAsBlob(url){
    return new Promise((resolve, reject) => {
      var xhr = new XMLHttpRequest();
      xhr.open('GET', url, true);
      xhr.responseType = 'blob';
      xhr.onload = function(e) {
        if (this.status === 200) {
          resolve(this.response);
          // myBlob is now the blob that the object URL pointed to.
        }else{
          reject(this.response);
        }
      };
      xhr.send();
    })
  };

  //Browser
  const blob = await loadFileAsBlob('1000ppcm.png');
  const buffer = await blob.arrayBuffer();

  //NodeJS
  const buffer = fs.readFileSync('1000ppcm.png')

  //read metadata
  metadata = readMetadata(buffer);
```

### metadata format

image with 300 dpi

```javascript
{
  "pHYs": { 
    "x": 30000,
    "y": 30000,
    "units": RESOLUTION_UNITS.INCHES
  },
  "tEXt": {
    "Title":            "Short (one line) title or caption for image",
    "Author":           "Name of image's creator",
    "Description":      "Description of image (possibly long)",
    "Copyright":        "Copyright notice",
    "Software":         "Software used to create the image",
    "Disclaimer":       "Legal disclaimer",
    "Warning":          "Warning of nature of content",
    "Source":           "Device used to create the image",
    "Comment":          "Miscellaneous comment"
  }
}
```

### writing metadata

```javascript
writeMetadata(buffer,metadata);
```

### save image from canvas

```javascript
canvas.toBlob(blob => {
    let newBlob = fabric.util.png.writeMetadataB(blob, metadata);
    saveAs(newBlob, title);
});
```
