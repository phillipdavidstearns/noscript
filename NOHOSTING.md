# NØ HOSTING

A quick guide to hosting your own simple http (insecure!) server on your Pi

#### 1. Setup a directory for your page on your Pi (using `cd` & `mkdir` etc.)
#### 2. Create an index file

```
echo '<html>Hello World!</html>' > index.html
```
#### 3. Use python's SimpleHTTPServer module to serve up content


```
python -m SimpleHTTPServer 8080
```

You can specify any other port from 1-65535 instead of 8080

####4. Open your browser and navigate to the IP address of your Pi and specify the port

```
<your.pi.ip.address>:<port>
```

For example:
```
192.168.1.102:8080
```