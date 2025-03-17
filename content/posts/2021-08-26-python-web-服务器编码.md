---
layout: post
title:  "简单的python web服务器（附编码设置）"
date: 2021-08-26 10:52:01 +0800
categories: python
slug: p20210826105201
---

### A basic web server implemented by python3.6
Note: the **encoding-related configuration** in line:

​         `self.send_header('Content-type','text/html; charset=utf-8')`

注意：python web服务器 编码问题



```python
import csv
from http.server import BaseHTTPRequestHandler, HTTPServer
class handler(BaseHTTPRequestHandler):
    
    Page = '''\
        <html>
        <meta content="text/html;charset=utf-8" http-equiv="Content-Type">
        <meta content="utf-8" http-equiv="encoding">
        <body>
        <p>Hello, I am great Chinese!</p>
        </body>
        </html>
    '''
    
    def parse_csv(self):
        lines = []
        with open('my_content.csv', 'r') as file:
            reader = csv.reader(file)
            for row in reader:
                lines.append(row)
        file.close()
        return str(lines[0])

    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type','text/html; charset=utf-8')
        self.end_headers()
        message = self.parse_csv()
        #message = 'Hello beijing 北京'
        self.wfile.write(bytes(message, "utf-8"))
        #self.wfile.write(self.Page.encode('utf-8'))

if __name__ == '__main__':
    server = HTTPServer(('', 8080), handler)
    #server = HTTPServer(('0.0.0.0', 8080), handler)  # 0.0.0.0 for public accessible
    server.serve_forever()
```

You can access the content by '127.0.0.1:8080' in browser that running in local machine on which the python web is running on.
