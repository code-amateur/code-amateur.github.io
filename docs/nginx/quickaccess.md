## Nginx Quick Access

### Use regex for nginx location

We came across a security requirement recently, where we have been asked to block all the unnecessary URLs that exposes information about our SSO server.

One way of resolving this problem is
1. Identify all the URIs the application is using
2. Only `proxy_pass` those URIs. This way `nginx` will throw a `403 Forbidden` error in case some one request for any other URIs.

In such cases, using Regex in `location` comes in handy.

For Example, 
In step 1, you identified below URIs the application is using.

    /auth/realms/BrokerPortal/protocol/
    /auth/realms/BrokerPortal/broker
    /auth/realms/BrokerPortal/login-actions
    /auth/realms/BrokerPortal/account

So, you can use Regex to `proxy_pass` only those URIs, nginx will block all other URIs by default.

    server {
        location ~ ^/(auth/realms/BrokerPortal/protocol|auth/realms/BrokerPortal/broker|auth/realms/BrokerPortal/login-actions|auth/realms/BrokerPortal/account)/ {
            proxy_pass http://localhost:8080/;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $http_host;
            # Other Configurations
        }
    }

The `~` means to use a regular expression for the url. The `^` means to check from the first character. This will look for a `/` followed by either of the locations and then another `/`.

This addresses our primary problem but introduced new problem. Now we can not access the admin console through default port / host name. To resolve this, we need to add a new rule.
     
     location /auth {
            # Allows request from below ranges of IP address
            allow   192.168.1.0/24;
            
            # drop rest of the world 
            deny    all;
            
            proxy_pass http://localhost:8080/;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $http_host;
            # Other Configurations
        }

Above configuration will allow requests only from whitelisted IP addresses / IP Ranges to access admin console of the SSO server.