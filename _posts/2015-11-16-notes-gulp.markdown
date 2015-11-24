Install gulp in your project folder. cd into your project folder, then:
$ npm install -D gulp

Add this text to your gulpfile.js:

var gulp = require('gulp');

gulp.task('default');


Then run gulp from the terminal

$ gulp

It should give you something like this: 

[11:07:18] Using gulpfile ~/Dev/gulp-exercises/01-run/gulpfile.js
[11:07:18] Starting 'default'...
[11:07:18] Finished 'default' after 54 μs

=========

## Default Task

Open up gulpfile.js and check out the gulp.task defined. It is the default task. When you run gulp it will automatically run the default task.

Let's extend what the default task does:

gulp.task('default', function () {
    console.log('running default task')
});

Let's create another task:

gulp.task('other', function() {
  console.log('I am the other.')
})

Run this with:

$ gulp other

You can add this new task to the set of default tasks by putting it in the second argument of the default task:

gulp.task('default', ['other'], function() {
  console.log('Hey, I\'m a task.')
})

So when you run 

$gulp

It does the 'other' task, then runs whatever is in the 'default' body:

°°° gulp
[11:16:31] Using gulpfile ~/dev/earthquakes2/gulpfile.js
[11:16:31] Starting 'other'...
I am the other.
[11:16:31] Finished 'other' after 110 μs
[11:16:31] Starting 'default'...
Hey, I'm a task.
[11:16:31] Finished 'default' after 29 μs


========

## Copy js files from ./src to ./build

Make sure you have a src folder in the root of your project. All your 'source' files should be kept here.
We're going to copy the html files from the source folder to a new folder called 'build'. 
If the build folder does not yet exist, gulp will create it.

gulp.task('default', function () {
    gulp
        .src('./src/**/*.html')
        .pipe(gulp.dest('./build'))
})
Run gulp:

$ gulp
[11:39:19] Using gulpfile ~\Dev\gulp-exercises\03-src-dest-pipe\gulpfile.js
[11:39:19] Starting 'default'...
[11:39:19] Finished 'default' after 7.1 ms
A ./build folder should have been created.

### Glob files?

## Testing with Mocha
Install gulp-mocha

To install the gulp plugin gulp-mocha:

$ npm install -D gulp-mocha


### Create a test task

var gulp = require('gulp');
var mocha = require('gulp-mocha');

gulp.task('default', ['test']);

gulp.task('test', function () {
    return gulp.src('./test/*.js', {read: false})
        .pipe(mocha())
})
Run the task

$ gulp
