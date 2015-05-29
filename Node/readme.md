# Node Restful API
使用Node.js来搭建一个简单的restful API接口

###文件目录

    - app/
    ----- models/
    ---------- bear.js  
    - node_modules/    
    - package.json      
    - server.js         

###package.js
    {
    "name": "node-api",
    "main": "server.js",
    "dependencies": {
        "express": "~4.12.3",
        "mongoose": "~4.0.1",
        "body-parser": "~1.12.3"
       }
    }

###设置server.js
    // call the packages we need
	var express    = require('express');       
	var app        = express();                
	var bodyParser = require('body-parser');
	
	// configure app to use bodyParser()
	// this will let us get the data from a POST
	app.use(bodyParser.urlencoded({ extended: true }));
	app.use(bodyParser.json());
	
	var port = process.env.PORT || 8080;     

未完待续....
  