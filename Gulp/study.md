# Gulp
一起学习在开发中使用Gulp

###安装Gulp
	$ npm install --global gulp
	
	$ npm install --save-dev gulp

###gulpfile文件配置
    /* File: gulpfile.js */

	var gulp  = require('gulp'),
    gutil = require('gulp-util');

	gulp.task('default', function() {
	  return gutil.log('Gulp is running!')
	});

###Example目录
    bulid/
	  |  app.css
	  |  app.min.js
	  |  index.html
	source/
	  |  javascript/
	  |  |  courage.js
	  |  |  wisdom.js
	  |  |  power.js
	  |  scss/
	  |  |  styles.scss
	  |  |  grid.scss
	  |  index.html
	gulpfile.js
	packages.json

####配置copyHtml
    gulp.task('copyHtml', function() {
	  gulp.src('source/*.html')
		  .pipe(gulp.dest('bulid'));
	});

####配置JS
    gulp.task('scripts', function(){
		return gulp.src('src/*.js')
			  	   .pipe(concat('app.min.js'))
			  	   .pipe(uplify())
			   	   .pipe(gulp.dest('bulid'));
	});

###配置css
    gulp.task('css', function(){
		return gulp.src('src/css/*.css')
		.pipe(concat('app.css'))
		.pipe(uplify())
		.pipe(gulp.dest('bulid'));
	});
	//less or sass
	gulp.task('less', function(){
		return gulp.src('src/less/*.less')
		.pipe(less())
		.pipe(concat('less.css'))
		.pipe(gulp.dest('bulid'))
		.on('error', function (err) {
	            gutil.log(err);
	            this.emit('end');
	        });
	});

###watch and default
    gulp.task('watch',function(){
		gulp.watch('src/*.js',['scripts']);
		gulp.watch('src/css/*.css',['css']);
		gulp.watch('src/less/*.less',['less']);
	});
	
	gulp.task('default', ['watch', 'scripts', 'css', 'less']);