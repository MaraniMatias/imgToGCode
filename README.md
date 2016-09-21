# Image to gCode
Convert jpg, png, gif to gcode  with NodeJS and [lwip](https://www.npmjs.com/package/lwip#installation).

Generate GCode with absolute coordinates, finds a black pixel if you follow the trail.

There may be mistakes, I am working to accommodate.

### Installation
```bash
$ npm install img2gcode
```

## Quick Start
Depending on the configuration between tool and image height generates better code.

```Javascript
var img2gcode = require("img2gcode");

  img2gcode({  // It is mm
    toolDiameter: 2,
    scaleAxes: 700,
    deepStep: -1,
    whiteZ: 0,
    blackZ: -2,
    sevaZ: 2,
    dirImg:__dirname+'/img-and-gcode/test.jpeg'
  }).then((data) => {
    console.log(data.config);
    console.log(data.dirgcode);
  });
```

### Options
- `toolDiameter` (number) Image height in mm.
- `scaleAxes` (number)
- `deepStep` (number) Depth per pass.
- `dirImg` (string)
- `whiteZ` (number) White pixels.
- `blackZ` (number) Maximum depth (Black pixels).
- `sevaZ` (number) Safe distance.
- `info` (string) Displays information. ["none" | "console" | "emitter"] default: none

### Events
  Only if Options.info it is "emitter"
- `log` Displays information.
- `tick` Percentage of black pixels processed. 0 (0%) to 1 (100%).
- `error` Displays error.
- `complete` Emits at the end with "then".

### Method
- `then`
  This function is called to finish saving the file GCode.
  Receives as parameters an object: { config , dirgcode }

### Examples

```Javascript
var img2gcode = require("img2gcode");
var ProgressBar = require("progress"); // npm install progress

var bar = new ProgressBar('Analyze: [:bar] :percent :etas', { total: 100 });

img2gcode
  .start({  // It is mm
    toolDiameter: 1,
    scaleAxes: 700,
    deepStep: -1,
    whiteZ: 0,
    blackZ: -2,
    sevaZ: 1,
    info: "emitter", // ["none" | "console" | "emitter"]
    dirImg: __dirname + '/img-and-gcode/test.png'
  })
  .on('log', (str) => {
    console.log(str);
  })
  .on('tick', (perc) => {
    bar.update(perc)
  })
  .then((data) => {
    console.log(data.dirgcode);
  });
```

### License.
I hope someone else will serve ([MIT](http://opensource.org/licenses/mit-license.php)).
Author:
Marani Matias Ezequiel.