# Lab 6 - Deploy Algonquin Pet Store to Azure Kubernetes Service (AKS)
**Name:** Angus MacGillivary<br>
**Course:** CST8915<br>
**Section:** 11 <br> 
**Lab:** 6<br>
**Date:** November 18, 2026<br>

## Video Demo
[Link](https://youtu.be/GzgmXlQ5THU)

## Investigation into New Routing Behaviour
In this deployment of the Algonquin Pet Store, there is only one external IP address exposed, as opposed to three.  Upon examining `nginx.conf` in the [Store-Front-L6 repo](https://github.com/ramymohamed10/store-front-L6/blob/main/nginx.conf), we discover the following section:

```
nginx.conf


# Forward product requests to the Product Service.
    location /products {
        proxy_pass http://product-service:3030/products;
        proxy_http_version 1.1;
    }

    # Forward order requests to the Order Service.
    location /orders {
        proxy_pass http://order-service:3000/orders;
        proxy_http_version 1.1;
    }

    # Proxy the dashboard; /rabbitmq automatically redirects to /rabbitmq/.
    location /rabbitmq/ {
        # Use the path from map above (e.g., /rabbitmq/ becomes /).
        proxy_pass http://rabbitmq_management$rabbitmq_uri;
        proxy_http_version 1.1;
    }
```

This forwards all requests on the `/products`, `/orders` and `/rabbitmq` endpoints to their respective services on the internal network, allowing users to reach these services with only one external IP address.

