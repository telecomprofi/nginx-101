# nginx-101
Some nginx tips and tricks

nginx reverse proxy config (aka weighted load balancer)

source: https://phoenixnap.com/kb/nginx-reverse-proxy

 Unlink Default Configuration File
In the terminal, enter the following:

sudo unlink /etc/nginx/sites-enabled/default

Step 5: Create New Configuration File
To create a new configuration file, enter:

cd /etc/nginx/sites-available/

sudo vi custom_server.conf

Replace custom_server with a name that’s meaningful to you. In the new file, enter:

server {

listen 80;

location / {

proxy_pass http://my_server;

}

}

This is a very basic Nginx reverse proxy example. Nginx is set to listen for all traffic on port 80 for all traffic.

The proxy_pass command directs all traffic on port 80 to http://my_server. Just change http://my_server to the location of your choice, and Nginx will intercept client requests and route them to the location you specify. Once you’ve finished, save the file and exit.

Step 6: Link and Activate Configuration File
To activate the new Nginx file, enter:

ln -s /etc/nginx/sites-available/custom_server.conf 
/etc/nginx/sites-enabled/custom_server.conf

As usual, replace custom_server with the name of the configuration file you created in Step 5.

Step 7: Test and Restart Nginx
To test Nginx:

sudo service nginx configtest

To restart Nginx:

sudo service nginx restart

Optional Nginx Configuration Options
Proxy Buffers
By default, Nginx buffers traffic for servers that it proxies for. Buffers improve server performance as a server response isn’t sent until the client finishes sending a complete response.

To turn the buffer off, open the configuration file from Step 5. Under the location/section, add the following:

proxy_buffering off;

Request Headers
Headers provide the server information about the requests made, or about the client.

Nginx redefines two of the header fields: host is configured for $proxy_host, and connection is configured for close.  If you use those headers, be sure to change the behavior in the configuration file.

If any header strings are empty, Nginx simply eliminates those fields.

To change the way Nginx handles heathers, use the following commands in your configuration file:

location / {

proxy_set_header Host $host;

}

This example tells Nginx to set host to the $host variable.

To prevent a header field from being passed to the proxied server, use an empty string as follows:

location / {

proxy_set_header header-variable "";

}

Load Balancing
You can use the configuration file to route traffic to several servers. To use this configuration, your configuration file will look similar to this example:

http   {

server   {

proxy_pass http://my_server

}

}

In other words, the HTTP configuration goes outside the server configuration from Step 5.

To create a name for a group of servers, Use the upstream command:


```
http   {

server   {

proxy_pass http://my_server

}

}
```

```
http   {

upstream server_group   {

server my.server1.com weight=3;

server my.server2.com;

}

server  {

location / {

proxy_pass http://server_group;

}

}

}
```



