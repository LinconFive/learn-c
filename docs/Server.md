### Main

[C2][Main](index.md)😃.

### NPM

- html
```
# ls
assets  css  index.html  js
```

- install

```
apt install nodejs -y

apt install npm -y

npm init

npm test

npm install npm@latest

node -v

npm -v

npm install pm2

npm install http-server

npm init

# ls
assets  css  index.html  js  node_modules  package.json

node_modules/pm2/bin/pm2 --name HelloWorld start npm -- start

```

- show 
```
node_modules/pm2/bin/pm2 start node_modules/http-server/bin/http-server --name HelloHttp -- -a 192.168.10.164 -p 8000 -d false

node_modules/pm2/bin/pm2 restart node_modules/http-server/bin/http-server --name HelloHttp -- -a 192.168.10.164 -p 8000 -d false

node_modules/pm2/bin/pm2 stop node_modules/http-server/bin/http-server

pm2 delete 0

pm2 logs

pm2 list
```

- upload
```
npm install http 
npm install fs 
npm install formidable

node_modules/pm2/bin/pm2 start file/file.js --name HelloHttpd

# server.js
var formidable = require('formidable'),
    http = require('http'),
    util = require('util');

http.createServer(function(req, res) {
  if (req.url == '/upload' && req.method.toLowerCase() == 'post') {
    // parse a file upload
    var form = new formidable.IncomingForm();

    form.parse(req, function(err, fields, files) {
      if (err)
          console.log("fail") // process error

      // Copy file from temporary place
      // var fs = require('fs');
      // fs.rename(file.path, <targetPath>, function (err) { ... });

      // Send result on client
      res.writeHead(200, {'content-type': 'text/plain'});
      res.write('received upload:\n\n');
      res.end(util.inspect({fields: fields, files: files}));
    });

    return;
  }

  // show a file upload form
  res.writeHead(200, {'content-type': 'text/html'});
  res.end(
    '<form action="/upload" enctype="multipart/form-data" method="post">'+
    '<input type="text" name="title"><br>'+
    '<input type="file" name="upload" multiple="multiple"><br>'+
    '<input type="submit" value="Upload">'+
    '</form>'
  );
}).listen(8001);

# node server.js

```

- download

### Apache
```
apt-get install apache2

# ls /etc/apache2/sites-available

/etc/init.d/apache2 start
# /etc/init.d/apache2 stop

# at this point we should see def page
# :)
# now enable cgi
chmod +x /usr/lib/cgi-bin/eg.sh

#!/bin/bash
echo "Content-type: text/html"
echo ''
echo 'CGI Bash Example'

cd /etc/apache2/mods-enabled
ln -s ../mods-available/cgi.load

/etc/init.d/apache2 restart

# at this point you should be able to see cgi
```

### Python3 CGI

this is only good for temp use or local network
```
# cat /tmp/py/cgi-bin/hello.py
#!/usr/bin/python

print("Content-type: text/html\n")
print("<html>")
print("<body>")
print("<h1>Hello %s</h1>" % "World")
print("</body>")
print("</html>")

# cd /tmp/py
# chmod -R 755 cgi-bin
# python3 -m http.server --cgi 8000

# then goto http://104.168.132.229:8000/cgi-bin/hello.py

```

### SweetHttp

<a href="#top">Back to top</a>
