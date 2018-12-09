#Introduction
This example deploys a HAProxy in load balance mode with 2 NGINX web servers, in ROUND-ROBIN as backends. In this case,
and to be able to identify the ROUND-ROBIN mechanism, the index.html of the NGINX are differnt.

#RUN
```bash
docker-compose up
```
And the console will keep the ping opened to both backends, you should see something like:
```bash
...
Starting my_load_balance_service_nginx_2                      ... done
Starting my_load_balance_service_nginx_1                      ... done
Starting docker-compose-haproxy-lb_haproxy_img_1_b7a4765d1d04 ... done
Attaching to my_load_balance_service_nginx_2, my_load_balance_service_nginx_1, docker-compose-haproxy-lb_haproxy_img_1_b7a4765d1d04
my_load_balance_service_nginx_1 | 192.168.0.33 - - [09/Dec/2018:19:07:54 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
haproxy_img_1_b7a4765d1d04 | <7>haproxy-systemd-wrapper: executing /usr/local/sbin/haproxy -p /run/haproxy.pid -db -f /usr/local/etc/haproxy/haproxy.cfg -Ds 
haproxy_img_1_b7a4765d1d04 | [ALERT] 342/190754 (8) : sendmsg logger #1 failed: No such file or directory (errno=2)
my_load_balance_service_nginx_2 | 192.168.0.33 - - [09/Dec/2018:19:07:55 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_1 | 192.168.0.33 - - [09/Dec/2018:19:07:56 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_2 | 192.168.0.33 - - [09/Dec/2018:19:07:57 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_1 | 192.168.0.33 - - [09/Dec/2018:19:07:58 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_2 | 192.168.0.33 - - [09/Dec/2018:19:07:59 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_1 | 192.168.0.33 - - [09/Dec/2018:19:08:00 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_2 | 192.168.0.33 - - [09/Dec/2018:19:08:01 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
my_load_balance_service_nginx_1 | 192.168.0.33 - - [09/Dec/2018:19:08:02 +0000] "HEAD / HTTP/1.1" 200 0 "-" "-" "-"
...
```

To shutdown:
```bash
docker-compose down
``` 

#Testing
Open the browser of your host and visit http://192.168.0.33/ and you will see the page of one of the servers

To see the HAProxy stats, open the browser to http://localhost/haproxy?stats (admin:admin)

## Performance tests
Using ApacheBench toosl (ab), we can do some simple performance tests:
```bash
ab -n 10000 -c 30 http://192.168.0.33/
```
 

#Notes
Since in this example we are binding the port 80, you should shutdown any other service that could be running at the same port (such us Apache2)
