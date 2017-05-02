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
var browserSync = require('browser-sync').create();
  
gulp.task("CardCreates", function () {
  return gulp.src('src/CardCreates/*.js')
    .pipe(babel()) 
    .pipe(gulp.dest('./pages/CreateCard'));
});

gulp.task("CreateStore", function () {
  return gulp.src('src/CreateStore/*.js')
    .pipe(babel()) 
    .pipe(gulp.dest('./pages/CreateStore'));
});

gulp.task("ManagementCard", function () {
  return gulp.src('src/ManagementCard/*.js')
    .pipe(babel()) 
    .pipe(gulp.dest('./pages/ManagementCard'));
});

gulp.task("Login", function () {
    return gulp.src('src/Login/*.js')
        .pipe(babel())
        .pipe(gulp.dest('./pages/Login'));
});

gulp.task("ManagementStore", function () {
    return gulp.src('src/ManagementStore/*.js')
        .pipe(babel())
        .pipe(gulp.dest('./pages/ManagementStore'));
});

gulp.task("Kit", function () {
    return gulp.src('src/Kit/*.js')
        .pipe(babel())
        .pipe(gulp.dest('./pages/Kit'));
});

gulp.task('IndexSass',function() {
    return gulp.src('src/scss/index.scss')
    .pipe(sass())
    .pipe(gulp.dest('./static/css'))
});

gulp.task('LoginSass',function() {
    return gulp.src('src/scss/login.scss')
        .pipe(sass())
        .pipe(gulp.dest('./static/css'))
});

gulp.task('html',function() {
    gulp.src('index.html')
});

gulp.task('default', ['server']);

gulp.task('server', function() {

    browserSync.init({
        port: 2016,
        server: {
            baseDir: ['./']
        }
    });

    gulp.watch('src/CardCreates/*.js', ['CardCreates']);
    gulp.watch('src/CreateStore/*.js', ['CreateStore']);
    gulp.watch('src/ManagementCard/*.js', ['ManagementCard']);
    gulp.watch('src/ManagementStore/*.js', ['ManagementStore']);
    gulp.watch('src/Login/*.js', ['Login']);
    gulp.watch('src/Kit/*.js', ['Kit']);
    gulp.watch('src/scss/index.scss', ['IndexSass']);
    gulp.watch('src/scss/login.scss', ['LoginSass']);

    gulp.watch('*.html',['html']).on('change', browserSync.reload);
    gulp.watch('src/scss/*.scss').on('change', browserSync.reload);

});
