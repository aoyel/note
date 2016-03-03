#Install 




#Config


```
global
        maxconn 51200
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /usr/local/haproxy
        uid 99
        gid 99
        daemon
        nbproc 1
        pidfile /usr/local/haproxy/logs/haproxy.pid

defaults
        mode http
        #retries 2
        option redispatch
        option abortonclose
        timeout connect 5000ms
        timeout client 30000ms
        timeout server 30000ms
        #timeout check 2000
        log 127.0.0.1 local0 err #[err warning info debug]
        balance roundrobin
# option httplog
# option httpclose
# option dontlognull
# option forwardfor

listen state
        bind 0.0.0.0:8888
        option httplog
        stats refresh 30s
        stats uri /
        stats realm Haproxy Manager
        stats auth smile:smile
        #stats hide-version

listen http-in
        bind *:80
        option httpclose
        option forwardfor
        server s1 192.168.1.120:80 check weight 1 minconn 1 maxconn 3 check inter 40000
        server s2 192.168.1.122:80 check weight 1 minconn 1 maxconn 3 check inter 40000

```
