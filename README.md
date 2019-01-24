# Vue实战

知乎日志

![demo](./demo.gif)

## 跨域请求

使用request发送跨区请求。


	const http = require('http');
	const request = require('request');
	
	const hostname = '127.0.0.1';
	const port = 8010;
	
	const apiServer = http.createServer((req, res) => {
		// 调用api
	    const url = 'http://news-at.zhihu.com/api/4'+req.url;
	    const options = {
	        url: url
	    };
		// 回调函数，用于处理response
	    function callback(error, response, body) {
	        if(!error && response.statusCode === 200) {
	            res.setHeader('Content-Type', 'text/plain;charset=UTF-8');
	            res.setHeader('Access-Control-Allow-Origin', '*');
	            res.end(body);
	        }
	    }
	    request.get(options, callback);
	});
	// 发送监听
	apiServer.listen(port, hostname, () => {
	    console.log(`接口代理运行在http://${hostname}:${port}/`);
	});

## 异步请求
axios封装

	import axios from 'axios';
	const Util = {
	    apiPath: 'http://127.0.0.1:8010/'
	}
	// 创建axios对象
	Util.ajax = axios.create({
	    baseURL: Util.apiPath
	});
	Util.ajax.interceptors.response.use(res => {
	    return res.data;
	});
	export default Util;

使用方式

	import $ from '../libs/util';
	$.ajax.get('theme/'+id).then(res => {
	  this.list = res.stories.filter(item => item.type !== 1);
	})