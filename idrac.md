```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key 1285491434D8786F
sudo sh -c "echo deb http://linux.dell.com/repo/community/openmanage/910/xenial xenial main > /etc/apt/sources.list.d/linux.dell.com.sources.list"
sudo apt-get update
sudo apt-get install srvadmin-idracadm8
sudo ln -svf /usr/lib/x86_64-linux-gnu/libssl.so.1.0.2 /opt/dell/srvadmin/lib64/libssl.so
```

from localhost:
```
racadm sslkeyupload -t 1 -f xxx.key
racadm sslcertupload -t 1 -f xxx.crt
```

https://blog.nanpuyue.com/2018/039.html
