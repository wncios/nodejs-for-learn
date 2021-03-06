[![HTML Webpack Plugin](http://michael-ciniawsky.github.io/postcss-load-plugins/logo.svg "HTML Webpack Plugin")](https://www.npmjs.com/package/html-webpack-plugin)


## install

```bash

npm install html-webpack-plugin webpack-dev-server

```

## others

```bash
npm init -y

npm install webpack webpack-cli  babel-loader babel-core

mkdir build
cd build
touch webpack.config.js
vi package.json
find scripts

```

## package.json

```javascript
"build":"webpack --config build/webpack.config.js",
"dev":"webpack-dev-server --config build/webpack.config.js"
```

## webpack.config.js

```javascript

var path = require('path');

var prevPath = p => path.resolve(__dirname,'../',p);

const HtmlWebpackPlugin = require('html-webpack-plugin');

const html = new HtmlWebpackPlugin({
	title: 'hello,webpack-dev-server', // html5文件中<title>部分
     // 默认是index.html，服务器中设置的首页是index.html，如果这里改成其它名字，那么devServer.index改为和它一样，最终完整文件路径是output.path+filename，如果filename中有子文件夹形式，如`./ab/cd/front.html`，只取`./front.html`
    template: prevPath('src/index.html'), //如果觉得插件默认生成的hmtl5文件不合要求，可以指定一个模板，模板文件如果不存在，会报错，默认是在项目根目录下找模板文件，才模板为样板，将打包的js文件注入到body结尾处
    inject:'head', 
});


module.exports = {
	mode:'development',
	watch:true,
	entry:prevPath('src/main.js'),
	output:{
		filename:'[name]-[hash].js',
		path:prevPath('dist'),
		hashDigestLength: 8
	},
	module:{
		rules:[
			{
				test:/\.es$/,
				use:[
					{
						loader:'babel-loader',
						options:{

						}
					}
				]
			}
		]
	},
	plugins:[
		html
	],
	devServer: {
      contentBase: path.join(__dirname, "../dist"), //网站的根目录为 根目录/dist，如果配置不对，会报Cannot GET /错误
      port: 9000, //端口改为9000
      open:true // 自动打开浏览器，适合懒人
    }
}

```