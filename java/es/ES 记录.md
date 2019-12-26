# ES 记录

1. max file descriptors [4096] ...

   修改 /etc/profile ，最后边加上 ulimit -n 65535，source命令对es没用，直接重启机器。
   
   另外，这个情况官网描述只针对于tar包，rpm安装不会有这个问题

