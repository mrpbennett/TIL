# Using Gulp to Minifiy CSS from Scss

Never really touched Gulp, I always used online tools to Minify my CSS files or built in IDE tools which would do it for me.

```javascript
const sass = require('gulp-sass');
const uglycss = require('gulp-uglifycss');

// Create CSS files from SCSS files
gulp.task('sass', function() {
	return gulp
		.src('./sass/*.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('./css'));
});

// Minify CSS files
gulp.task('css', function() {
	gulp.src('./css/*.css')
		.pipe(
			uglycss({
				uglyComments: true,
			}),
		)
		.pipe(gulp.dest('./dist/'));
});
```
