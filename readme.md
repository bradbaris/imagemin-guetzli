# imagemin-guetzli

> [Guetzli](https://github.com/google/guetzli) imagemin plugin

> Guetzli is a JPEG encoder that aims for excellent compression density at high visual quality. Guetzli-generated images are typically 20-30% smaller than images of equivalent quality generated by libjpeg. Guetzli generates only sequential (nonprogressive) JPEGs due to faster decompression speeds they offer.

## Install

```
$ npm install --save imagemin-guetzli
```

## Usage

```js
const imagemin = require('imagemin');
const imageminGuetzli = require('imagemin-guetzli');

(async () => {
	await imagemin(['images/*.jpg'], {
		destination: 'build/images',
		plugins: [
			imageminGuetzli({quality: 95})
		]
	});

	console.log('Images optimized');
})();
```

## Usage (gulp-imagemin)

```js
const gulp = require('gulp');
const imagemin = require('gulp-imagemin');
const imageminGuetzli = require('imagemin-guetzli');

gulp.task('default', () =>
    gulp.src('images/*')
        .pipe(imagemin([imageminGuetzli()]))
        .pipe(gulp.dest('dist/images'))
);
```

### Notes

Guetzli uses a large amount of memory. You should provide **300MB of memory per 1MPix** of the input image.

Guetzli uses a significant amount of CPU time. You should count on using about **1 minute of CPU per 1MPix** of input image.

Guetzli assumes that input is in **sRGB profile** with a **gamma of 2.2**. Guetzli will ignore any color-profile metadata in the image.

Guetzli is designed to work on high quality images (e.g. that haven't
been already compressed with other JPEG encoders). While it will work on other
images too, results will be poorer. You can try compressing an enclosed [sample
high quality
image](https://github.com/google/guetzli/releases/download/v0/bees.png).

Guetzli converts PNG/JPG to JPG. When using this plugin or guetzli-bin CLI, the original filename+ext is used as the output, although the image format has changed. You have to rename the file with the correct file extension (JPG) yourself afterwards.

## API

### imageminGuetzli(options?)(buffer)

#### options

Type: `object`

##### quality

Type: `number` (0–100)\
Default: `95`

Set quality in units equivalent to libjpeg quality. As per guetzli function and purpose, it is not recommended to go below `84`.

Please note that JPEG images do not support alpha channel (transparency). If the input is a PNG with an alpha channel, it will be overlaid on black background before encoding.

##### memlimit

Type: `number`\
Default: `6000`

Memory limit in MB. Guetzli will fail if unable to stay under the limit.
Note: Currently, in `imagemin-guetzli`, this will fail silently as the error is not passed along.

##### nomemlimit

Type: `boolean`

Do not limit memory usage.
