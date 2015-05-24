##Webpack
###简注
Webpack[官方文档](http://webpack.github.io/docs/)

[英文教程](https://github.com/petehunt/webpack-howto)和[題叶译文](http://segmentfault.com/a/1190000002551952)

###基本的使用
简单的例子就是这样一个文件, 可以把 `./app.js` 作为入口打包 `bulid.js`:
	
	// webpack.config.js
	module.exports = {
	  entry: './main.js',
	  output: {
		filename: 'bundle.js'   
	  }
	};
###CSS 及图片的引用
CSS 跟 LESS, 还有图片, 直接引用

    require('./bootstrap.css');
    require('./myapp.less');
    
    var img = document.createElement('img');
    img.src = require('./glyph.png');
实际上 CSS 被转化为 `<style>` 标签, 而图片可能被转化成 base64 格式的 dataUrl
但是要主要在 `webpack.config.js` 文件写好对应的 loader:

    // webpack.config.js
    module.exports = {
      entry: './main.js',
      output: {
	    path: './build', // This is where images AND js will go
	    publicPath: 'http://mycdn.com/', // This is used to generate URLs to e.g. images
	    filename: 'bundle.js'
      },
      module: {
	    loaders: [
	      { test: /\.less$/, loader: 'style-loader!css-loader!less-loader' }, // use ! to chain loaders
	      { test: /\.css$/, loader: 'style-loader!css-loader' },
	      {test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'} // inline base64 URLs for <=8k images, direct URLs for the rest
	    ]
      }
    };
###打成多个包
有时考虑类库代码的缓存, 我们会考虑打成多个包, 这样不难比如下边的配置, 首先 entry 有多个属性, 对应多个 JavaScript 包,然后 commonsPlugin 可以用于分析模块的共用代码, 单独打一个包出来:

	// webpack.config.js
	var webpack = require('webpack');
	var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
	
	module.exports = {
	  entry: {
	    Profile: './profile.js',
	    Feed: './feed.js'
	  },
	  output: {
	    path: 'build',
	    filename: '[name].js' // Template based on keys in entry above
	  },
	  plugins: [commonsPlugin]
	};

###对文件做 revision
这个在文档上做了说明, 可以自动生成 js 文件的 Hash([官方文档](http://webpack.github.io/docs/long-term-caching.html)）:

	output: { chunkFilename: "[chunkhash].bundle.js" }

	plugins: [
	  function() {
	    this.plugin("done", function(stats) {
	      require("fs").writeFileSync(
	        path.join(__dirname, "...", "stats.json"),
	        JSON.stringify(stats.toJson()));
	    });
	  }
	]
同时, 可以注册事件, 拿到生成的带 Hash 的文件的一个表，但是拿到那个表之后, 就需要自己写代码进行替换了.. 这有点麻烦，官网的 Issue 里提到个办法是生成 HTML 时引用 stats.json 的数据,
我此前的方案是生成 HTML 之后再进行替换, 相对赖上生成时写入更好一些