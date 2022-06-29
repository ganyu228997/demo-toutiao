dist目录旨在由 HTTP 服务器提供服务（除非您已配置为相对值），因此如果您直接通过协议publicPath打开它将不起作用。在本地预览生产版本的最简单方法是使用 Node.js 静态文件服务器：1.npm install -g serve   2.serve -s dist
