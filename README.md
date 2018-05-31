# gulp
gulp 经常用的配置

package.json

{
  "name": "test",
  "version": "1.0.0",
  "description": "This is backstage !",
  "devDependencies": {
    "babel-preset-es2015": "^6.24.0",
    "browser-sync": "^2.18.8",
    "gulp": "^3.9.1",
    "gulp-babel": "^6.1.2",
    "gulp-sass": "^3.1.0"
  }
}

.babelrc

{
  "presets": ["es2015"]
}

gulpfile.js

var gulp = require("gulp");
var babel = require("gulp-babel");
var sass = require('gulp-sass');
var pump = require('pump');
var uglify = require('gulp-uglify');
var autoprefixer = require('gulp-autoprefixer');
var browserSync = require('browser-sync').create();

gulp.task("pageJS", function () { return gulp.src('src/js/*.js') .pipe(babel()) .pipe(gulp.dest('./dist/js')); });

gulp.task('PageSass',function() { return gulp.src('src/css/*.scss') .pipe(sass()) .pipe(gulp.dest('./dist/css')) });

gulp.task('AutoCss', () =>
    gulp.src('./dist/css/index.css')
        .pipe(autoprefixer({
            browsers: ['last 2 versions'],
            cascade: false
        }))
        .pipe(gulp.dest('./dist/css'))
);

gulp.task('FormateIndexJS', function (cb) {
  pump([
        gulp.src('dist/js/index.js'),
        uglify(),
        gulp.dest('./dist/js')
    ],
    cb
  );
});

gulp.task('html',function() { gulp.src('./dist/index.html') });

gulp.task('default', ['server']);

gulp.task('server', function() {

  browserSync.init({
      port: 2016,
      server: {
          baseDir: ['./dist/']
      }
  });

  gulp.watch('src/js/*.js', ['pageJS']);
  gulp.watch('dist/js/index.js', ['FormateIndexJS']);
  gulp.watch('src/css/index.scss', ['PageSass']);
  gulp.watch('dist/css/index.css', ['AutoCss']);

  gulp.watch('*.html',['html']).on('change', browserSync.reload);
  gulp.watch('src/css/*.scss').on('change', browserSync.reload);
  
});
