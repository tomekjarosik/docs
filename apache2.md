`a2ensite`, `a2dissite` - enable or disable an apache2 site / virtual host
[link](https://manpages.debian.org/jessie/apache2/a2dissite.8.en.html)

```
<Proxy balancer://mycluster>
    # Define back-end servers:

    # Server 1
    BalancerMember http://0.0.0.0:8080/

    # Server 2
    BalancerMember http://0.0.0.0:8081/
</Proxy>

<VirtualHost *:*>
    # Apply VH settings as desired
    # However, configure ProxyPass argument to
    # use "mycluster" to balance the load

    ProxyPass / balancer://mycluster
</VirtualHost>
```
