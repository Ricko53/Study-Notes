# Grunt

###Grunt全局安装
    $ npm install -g grunt-cli

###Example文件目录
项目源文件在public/src 目录下通过Grunt自动化部署到public/bulid 文件中

    - publci
	----- bulid
	---------- css
	--------------- style.css      // generated from the src/style.less file
	--------------- style.min.css  // generated from the style.css file above
	---------- js
	--------------- app.min.js     // generated from all the src/js files
	
	----- src
	---------- css 
	--------------- style.less
	---------- js
	--------------- controllers
	-------------------- MainCtrl.js
	--------------- services
	-------------------- TestService.js
	--------------- app.js
	
	----- views
	---------- index.html
	
	- Gruntfile.js
	- package.json
	- server.js
###server.js
    var express = require('express');
	var app     = express();
	
	app.use(express.static(__dirname + '/public'));
	
	app.get('/', function(req, res) {
	    res.sendfile('./public/views/index.html');
	});
	
	app.listen(8080);
	console.log('Magic happens on 8080');

###Gruntflie.js配置
    // Gruntfile.js
	module.exports = function(grunt) {
	
	  grunt.initConfig({
	
	    jshint: {
	      all: ['public/src/js/**/*.js'] 
	    },
	
	    // take all the js files and minify them into app.min.js
	    uglify: {
	      build: {
	        files: {
	          'public/bulid/js/app.min.js': ['public/src/js/**/*.js', 'public/src/js/*.js']
	        }
	      }
	    },
	
	    // CSS TASKS ===============================================================
	    // process the less file to style.css
	    less: {
	      build: {
	        files: {
	          'public/bulid/css/style.css': 'public/src/css/style.less'
	        }
	      }
	    },
	
	    // take the processed style.css file and minify
	    cssmin: {
	      build: {
	        files: {
	          'public/bulid/css/style.min.css': 'public/bulid/css/style.css'
	        }
	      }
	    },
	
	    // COOL TASKS ==============================================================
	    // watch css and js files and process the above tasks
	    watch: {
	      css: {
	        files: ['public/src/css/**/*.less'],
	        tasks: ['less', 'cssmin']
	      },
	      js: {
	        files: ['public/src/js/**/*.js'],
	        tasks: ['jshint', 'uglify']
	      }
	    },
	
	    // watch our node server for changes
	    nodemon: {
	      dev: {
	        script: 'server.js'
	      }
	    },
	
	    // run watch and nodemon at the same time
	    concurrent: {
	      options: {
	        logConcurrentOutput: true
	      },
	      tasks: ['nodemon', 'watch']
	    }   
		
	  });
	
	  grunt.loadNpmTasks('grunt-contrib-jshint');
	  grunt.loadNpmTasks('grunt-contrib-uglify');
	  grunt.loadNpmTasks('grunt-contrib-less');
	  grunt.loadNpmTasks('grunt-contrib-cssmin');
	  grunt.loadNpmTasks('grunt-contrib-watch');
	  grunt.loadNpmTasks('grunt-nodemon');
	  grunt.loadNpmTasks('grunt-concurrent');
	
	  grunt.registerTask('default', ['less', 'cssmin', 'jshint', 'uglify', 'concurrent']);
	
	};