![](//i.v2ex.co/Up79Hq7l.jpeg)

## ǰ��
���ںܶ����Ѷ��˽�����Ѿ���ʹ��LNMP�ܹ���һ��������ΪLinux ShellΪCentOS/RadHat/Fedora/Debian/Ubuntu/��ƽ̨��װLNMP(Nginx/MySQL/PHP)��LNMPA(Nginx/MySQL/PHP/Apache)��LAMP(Apache/MySQL/PHP)�����ƵĿ������������������Լ��Ǵ�SuSE/Oracle��ҵ�������߳����ģ����ڿ�Դ�Ĳ��𷽰�Ҳ����һ��һ������������������Ҳ��Ȼ����ĳЩ�ӵ������á���ƪ���½�Ϊ��ϸ�������˻���LTMP�ܹ��Ĳ�����̣�֮����ٿ��Ƕ�������ģ�����ϸ�ںͼ��ɣ��������и����ʵ�����ʵ���ֲỶӭһ����������еĴ���͸Ľ���Ҳ���æָ���¹���

>LTMP(CentOS/Tengine/MySQL/PHP)

---

## ������ʷ

2015��08��04�� - ����

�Ķ�ԭ�� - http://wsgzao.github.io/post/ltmp/

��չ�Ķ�

CentOS - http://www.centos.org/
Tengine - http://tengine.taobao.org/
Nginx - http://nginx.org/en/docs/
MySQL - http://www.mysql.com/
PHP - http://php.net/
LTMP���� - http://wsgzao.github.io/index/#LTMP


---

## LTMP�汾

1. CentOS_6.5_64
2. Tengine-2.1.0
3. MySQL_5.6.25
4. PHP_5.5.27
5. Apache_2.2.31(����)

##׼������

>������������ʻ᷽��ܶ�

``` bash
#�Ż�History��ʷ��¼
vi /etc/bashrc

#���ñ�����ʷ������ļ���С
export HISTFILESIZE=1000000000
#������ʷ��������
export HISTSIZE=1000000
#ʵʱ��¼��ʷ���Ĭ��ֻ�����û��˳�֮��Ż�ͳһ��¼����������ɶ���û�����໥���ǡ�
export PROMPT_COMMAND="history -a"
#��¼ÿ����ʷ�����ִ��ʱ��
export HISTTIMEFORMAT="%Y-%m-%d_%H:%M:%S "

#����ʱ������ѡ��
rm -rf /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

#����NetworkManager����ѡ��
/etc/init.d/NetworkManager stop
chkconfig NetworkManager off
/etc/init.d/network restart

#�ر�iptables����ѡ�� 
/etc/init.d/iptables stop
chkconfig iptables off

#����dns����ѡ��
echo "nameserver 114.114.114.114" > /etc/resolv.conf 

#�ر�maildrop
#cd /var/spool/postfix/maildrop;ls | xargs rm -rf; 
sed 's/MAILTO=root/MAILTO=""/g' /etc/crontab
service crond restart

#�ر�selinux
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config  


#�ļ���������
echo ulimit -SHn 65535 >> /etc/profile
source /etc/profile

#�޸������̺�����ļ���������
vi /etc/security/limits.conf
* soft nproc 11000
* hard nproc 11000
* soft nofile 655350
* hard nofile 655350

sed -i -e '/# End of file/i\* soft  nofile 65535\n* hard nofile 65535'  /etc/security/limits.conf

#�Ż�TCP
vi /etc/sysctl.conf

#���ð����˹��� 
net.ipv4.ip_forward = 0  
#����Դ·�ɺ˲鹦�� 
net.ipv4.conf.default.rp_filter = 1  
#��������IPԴ·�� 
net.ipv4.conf.default.accept_source_route = 0  
#ʹ��sysrq��ϼ����˽�ϵͳĿǰ���������Ϊ��ȫ�����Ϊ0�ر�
kernel.sysrq = 0  
#����core�ļ����ļ����Ƿ����pid��Ϊ��չ
kernel.core_uses_pid = 1  
#����SYN Cookies��������SYN�ȴ��������ʱ������cookies������
net.ipv4.tcp_syncookies = 1  
#ÿ����Ϣ���еĴ�С����λ���ֽڣ�����
kernel.msgmnb = 65536  
#����ϵͳ�����Ϣ������������
kernel.msgmax = 65536  
#���������ڴ�εĴ�С����λ���ֽڣ����ƣ����㹫ʽ64G*1024*1024*1024(�ֽ�)
kernel.shmmax = 68719476736  
#�����ڴ��С����λ��ҳ��1ҳ = 4Kb�������㹫ʽ16G*1024*1024*1024/4KB(ҳ)
kernel.shmall = 4294967296  
#timewait��������Ĭ����180000
net.ipv4.tcp_max_tw_buckets = 6000  
#������ѡ���Ӧ��
net.ipv4.tcp_sack = 1  
#֧�ָ����TCP����. ���TCP������󳬹�65535(64K), �������ø���ֵΪ1
net.ipv4.tcp_window_scaling = 1  
#TCP��buffer
net.ipv4.tcp_rmem = 4096 131072 1048576
#TCPдbuffer
net.ipv4.tcp_wmem = 4096 131072 1048576   
#ΪTCP socketԤ�����ڷ��ͻ�����ڴ�Ĭ��ֵ����λ���ֽڣ�
net.core.wmem_default = 8388608
#ΪTCP socketԤ�����ڷ��ͻ�����ڴ����ֵ����λ���ֽڣ�
net.core.wmem_max = 16777216  
#ΪTCP socketԤ�����ڽ��ջ�����ڴ�Ĭ��ֵ����λ���ֽڣ�  
net.core.rmem_default = 8388608
#ΪTCP socketԤ�����ڽ��ջ�����ڴ����ֵ����λ���ֽڣ�
net.core.rmem_max = 16777216
#ÿ������ӿڽ������ݰ������ʱ��ں˴�����Щ�������ʿ�ʱ�������͵����е����ݰ��������Ŀ
net.core.netdev_max_backlog = 262144  
#webӦ����listen������backlogĬ�ϻ�������ں˲�����net.core.somaxconn���Ƶ�128����nginx�����NGX_LISTEN_BACKLOGĬ��Ϊ511�������б�Ҫ�������ֵ
net.core.somaxconn = 262144  
#ϵͳ������ж��ٸ�TCP�׽��ֲ����������κ�һ���û��ļ�����ϡ�������ƽ�����Ϊ�˷�ֹ�򵥵�DoS���������ܹ���������������Ϊ�ؼ�С���ֵ����Ӧ���������ֵ(����������ڴ�֮��)
net.ipv4.tcp_max_orphans = 3276800  
#��¼����Щ��δ�յ��ͻ���ȷ����Ϣ��������������ֵ��������128M�ڴ��ϵͳ���ԣ�ȱʡֵ��1024��С�ڴ��ϵͳ����128
net.ipv4.tcp_max_syn_backlog = 262144  
#ʱ������Ա������кŵľ��ơ�һ��1Gbps����·�϶���������ǰ�ù������кš�ʱ����ܹ����ں˽������֡��쳣�������ݰ���������Ҫ����ص�
net.ipv4.tcp_timestamps = 0  
#Ϊ�˴򿪶Զ˵����ӣ��ں���Ҫ����һ��SYN������һ����Ӧǰ��һ��SYN��ACK��Ҳ������ν���������еĵڶ������֡�������þ������ں˷�������֮ǰ����SYN+ACK��������
net.ipv4.tcp_synack_retries = 1  
#���ں˷�����������֮ǰ����SYN��������
net.ipv4.tcp_syn_retries = 1  
#����TCP������time_wait sockets�Ŀ��ٻ���
net.ipv4.tcp_tw_recycle = 1  
#����TCP���Ӹ��ù��ܣ�����time_wait sockets���������µ�TCP���ӣ���Ҫ���time_wait���ӣ�
net.ipv4.tcp_tw_reuse = 1  
#1st���ڴ�ֵ,TCPû���ڴ�ѹ��,2nd�����ڴ�ѹ���׶�,3rdTCP�ܾ�����socket(��λ���ڴ�ҳ)
net.ipv4.tcp_mem = 94500000 915000000 927000000   
#����׽����ɱ���Ҫ��رգ����������������������FIN-WAIT-2״̬��ʱ�䡣�Զ˿��Գ�����Զ���ر����ӣ��������⵱����ȱʡֵ��60 �롣2.2 �ں˵�ͨ��ֵ��180�룬����԰�������ã���Ҫ��ס���ǣ���ʹ��Ļ�����һ�����ص�WEB��������Ҳ����Ϊ���������׽��ֶ��ڴ�����ķ��գ�FIN- WAIT-2��Σ���Ա�FIN-WAIT-1ҪС����Ϊ�����ֻ�ܳԵ�1.5K�ڴ棬�������ǵ������ڳ�Щ��
net.ipv4.tcp_fin_timeout = 15  
#��ʾ��keepalive���õ�ʱ��TCP����keepalive��Ϣ��Ƶ�ȣ���λ���룩
net.ipv4.tcp_keepalive_time = 30  
#�������Ӷ˿ڷ�Χ
net.ipv4.ip_local_port_range = 2048 65000
#��ʾ�ļ�������������
fs.file-max = 102400

#�������ϵ��Ż�

# Kernel sysctl configuration file for Red Hat Linux
#
# For binary values, 0 is disabled, 1 is enabled.  See sysctl(8) and
# sysctl.conf(5) for more details.

# Controls IP packet forwarding
net.ipv4.ip_forward = 0

# Controls source route verification
net.ipv4.conf.default.rp_filter = 1

# Do not accept source routing
net.ipv4.conf.default.accept_source_route = 0

# Controls the System Request debugging functionality of the kernel

# Controls whether core dumps will append the PID to the core filename.
# Useful for debugging multi-threaded applications.
kernel.core_uses_pid = 1

# Controls the use of TCP syncookies
net.ipv4.tcp_syncookies = 1

# Disable netfilter on bridges.
net.bridge.bridge-nf-call-ip6tables = 0
net.bridge.bridge-nf-call-iptables = 0
net.bridge.bridge-nf-call-arptables = 0

# Controls the default maxmimum size of a mesage queue
kernel.msgmnb = 65536

# Controls the maximum size of a message, in bytes
kernel.msgmax = 65536

# Controls the maximum shared segment size, in bytes
kernel.shmmax = 68719476736

# Controls the maximum number of shared memory segments, in pages
kernel.shmall = 4294967296
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.netfilter.nf_conntrack_max = 1000000
kernel.unknown_nmi_panic = 0
kernel.sysrq = 0
fs.file-max = 1000000
vm.swappiness = 10
fs.inotify.max_user_watches = 10000000
net.core.wmem_max = 327679
net.core.rmem_max = 327679
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

/sbin/sysctl -p

#�Զ�ѡ������yumԴ
yum -y install yum-fastestmirror

#�Ƴ�ϵͳ�Դ���rpm����http mysql php
#yum remove httpd* php*
yum remove httpd mysql mysql-server php php-cli php-common php-devel php-gd  -y

#����������
yum install -y wget gcc gcc-c++ openssl* curl curl-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel gd gd2 gd-devel gd2-devel libaio autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel 

#yum��װ�����ر��������������Ƚ�yumԴ����Ϊ�����Ƶ�Դ
���http://mirrors.aliyun.com/ 
�Ѻ���http://mirrors.sohu.com/ 
���ף�http://mirrors.163.com/

#����ԭ�ȵ�yumԴ��Ϣ
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

#�Ӱ����ƾ���վ����centos6��repo
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

#���yum�������ɻ���
yum makecache

#yum��װ���������ѡ��
yum -y install tar zip unzip openssl* gd gd-devel gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel openldap-clients openldap-servers make libmcrypt libmcrypt-devel fontconfig fontconfig-devel libXpm* libtool* libxml2 libxml2-devel t1lib t1lib-devel



#����Ŀ¼�ṹ�����ذ�װ��
mkdir -p /app/{local,data}
cd /app/local

#PCRE - Perl Compatible Regular Expressions
wget "ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz"
#Tengine
wget "http://tengine.taobao.org/download/tengine-2.1.0.tar.gz"
#MySQL
wget "https://downloads.mariadb.com/archives/mysql-5.6/mysql-5.6.25-linux-glibc2.5-x86_64.tar.gz"
#PHP
wget "http://cn2.php.net/distributions/php-5.6.11.tar.gz"
#Mhash
wget "http://downloads.sourceforge.net/mhash/mhash-0.9.9.9.tar.gz"
#libmcrypt
wget "http://downloads.sourceforge.net/mcrypt/libmcrypt-2.5.8.tar.gz"
#Mcrypt
wget "http://downloads.sourceforge.net/mcrypt/mcrypt-2.6.8.tar.gz"

```

## ����Tengine

### ��װPCRE

``` bash
tar zxvf pcre-8.37.tar.gz
cd pcre-8.37
./configure
make && make install
cd ../

```

## ��װTengine

``` bash
#���www�û�����
groupadd www
useradd -g www www
#��װTengine
tar zxvf tengine-2.1.0.tar.gz
cd tengine-2.1.0

./configure --user=www --group=www \
--prefix=/app/local/nginx \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-pcre=/app/local/pcre-8.37

make && make install
cd ../

```

### ����Nginx

>Nginx�����ļ����Ż�����Ҫ�����ÿһ��������

``` bash
#�޸�nginx.conf
vi /app/local/nginx/conf/nginx.conf

#�û����û���
user  www www;
#�������̣�һ����԰�CPU�����趨
worker_processes  auto;
worker_cpu_affinity auto;
#ȫ�ִ�����־����
# [ debug | info | notice | warn | error | crit ]
error_log  logs/error.log  error;
#PID�ļ�λ��
pid  logs/nginx.pid;
#����worker���̵������ļ������ƣ�����"too many open files"
worker_rlimit_nofile 65535;

#events�¼�ָ�����趨Nginx�Ĺ���ģʽ������������
events{
     #epoll��Linux��ѡ�ĸ�Ч����ģʽ
     use epoll;
     #����nginx�յ�һ��������֪ͨ����ܾ����ܶ������
     multi_accept on;
     #���ڶ���Nginxÿ�����̵����������
     worker_connections      65536;
}

#HTTPģ�������nginx http��������к�������
http {
    include       mime.types;
    #�����ļ�ʹ�õ�Ĭ�ϵ�MIME-type
    default_type  application/octet-stream;
    

    #����־��ʽ���趨��mainΪ��־��ʽ����
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    #����nginx�Ƿ񽫴洢������־���ر����ѡ������ö�ȡ����IO��������
    access_log off;
    # access_log logs/access.log main buffer=16k;

    #����gzipѹ����ʵʱѹ�����������
    gzip on;
    #����IE6���߸��Ͱ汾����gzip����
    gzip_disable "MSIE [1-6]\.";
    #ǰ�˵Ļ�����������澭��gzipѹ����ҳ��
    gzip_vary on;
    #����ѹ�������������Ӧ����Ӧ��
    gzip_proxied any;
    #�������ݵ�ѹ���ȼ�
    gzip_comp_level 4;
    #���ö���������ѹ���������ֽ���
    gzip_min_length 1k;
    #��ʾ����16����λΪ64K���ڴ���Ϊѹ�����������
    gzip_buffers 16 64k;
    #��������ʶ��HTTPЭ��汾
    gzip_http_version 1.1;
    #����ָ��ѹ��������
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
     

    #�򿪻����ͬʱҲָ���˻��������Ŀ���Լ������ʱ��
    open_file_cache max=200000 inactive=20s;
    #��open_file_cache��ָ�������ȷ��Ϣ�ļ��ʱ��
    open_file_cache_valid 30s;
    #������open_file_cache��ָ��������ʱ���ڼ�����С���ļ���
    open_file_cache_min_uses 2;
    #ָ���˵�����һ���ļ�ʱ�Ƿ񻺴������Ϣ��Ҳ�����ٴθ�����������ļ�
    open_file_cache_errors on; 
 
    #��������ͻ�����������ĵ����ļ��ֽ���
    client_max_body_size 30M;
    #���ÿͻ������������ȡ��ʱʱ��
    client_body_timeout 10;
    #���ÿͻ�������ͷ��ȡ��ʱʱ��
    client_header_timeout 10;
    #ָ�����Կͻ�������ͷ��headerbuffer��С
    client_header_buffer_size 32k;
    #���ÿͻ������ӱ��ֻ�ĳ�ʱʱ��
    keepalive_timeout 60;
    #�رղ���Ӧ�Ŀͻ�������
    reset_timedout_connection on;
    #������Ӧ�ͻ��˵ĳ�ʱʱ��
    send_timeout 10;
    #������Ч�ļ�����ģʽ
    sendfile on;
    #����nginx��һ�����ݰ��﷢������ͷ�ļ�������һ����һ���ķ���
    tcp_nopush on;
    #����nginx��Ҫ�������ݣ�����һ��һ�εķ���
    tcp_nodelay on;
    #�������ڱ������key�����統ǰ���������Ĺ����ڴ�Ĳ���
    limit_conn_zone $binary_remote_addr zone=addr:5m; 
    #������key�������������������ÿһ��IP��ַ���ͬʱ����100������
    limit_conn addr 100; 
 
    #FastCGI��ز�����Ϊ�˸�����վ�����ܣ�������Դռ�ã���߷����ٶ�
    fastcgi_buffers 256 16k;
    fastcgi_buffer_size 128k;
    fastcgi_connect_timeout 3s;
    fastcgi_send_timeout 120s;
    fastcgi_read_timeout 120s;
    server_names_hash_bucket_size 128;
    #����error_log�м�¼�����ڵĴ���
    log_not_found off;
    #�ر��ڴ���ҳ���е�nginx�汾���֣���߰�ȫ��
    #server_tag Apache;
    server_tokens off;
    #tengine
    server_tag off;
    server_info off;

    #������������������ļ�
    include vhosts/*.conf;

    #���ؾ������ã���ʱ�Թ���
    #upstream test.com

    #�趨������������
    server {
        #����80�˿�
        listen       80;
        #����ʹ��localhost����
        server_name  localhost;
        #������ҳ�����ļ�������
        index index.html index.htm index.php;
        #�����������Ĭ����վ��Ŀ¼λ��
        root    /app/data/localhost/;
 
        #���������ʾҳ��
        error_page  404              /404.html;
        error_page  500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
 
        #PHP �ű�����ȫ��ת���� FastCGI����. ʹ��FastCGIĬ������.     
        location ~ .*\.(php|php5)?$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }
        
        #��̬�ļ�
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
        {
            #����30�죬Ƶ�����¿�����Сһ��
            expires      30d;
        }
 
        location ~ .*\.(js|css)?$
        {
            #����1Сʱ�������¿����ô�һЩ
            expires      1h;
        }
        #��ֹ����
        location ~ /\. {
            deny all;
        }
    }
}

```

>�������ļ�
vi /app/local/nginx/conf/nginx.conf

``` bash

user  www www;
worker_processes auto;
worker_cpu_affinity auto;

error_log  logs/error.log  crit;
pid        logs/nginx.pid;

worker_rlimit_nofile 51200;
events
{
    use epoll;
    multi_accept on;
    worker_connections 51200;
}

http
{
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log off;
    #access_log logs/access.log main buffer=16k;

    server_names_hash_bucket_size 128;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 50M; 

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 60; 
    server_tokens off;
    server_tag off;
    server_info off;

    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 256k;

    #gzip on;
    #gzip_min_length  1k;
    #gzip_buffers     4 16k;
    #gzip_http_version 1.1;
    #gzip_comp_level 5;
    #gzip_types       text/plain application/x-javascript text/css application/xml;
    #gzip_vary on;

    include vhosts/*.conf;
}

```

>����serverд��vhosts
mkdir -p  /app/local/nginx/conf/vhosts/
vi /app/local/nginx/conf/vhosts/localhost.conf

``` bash
server {
    listen       80;
    server_name  localhost;
    index index.php index.html index.htm;
    access_log  logs/localhost.log  main;

    root    /app/data/localhost/;

    location / {
        index  index.php index.html index.htm;
    }

    #error_page  404              /404.html;
    #error_page  500 502 503 504  /50x.html;

    location = /50x.html {
        root   html;
    }

    location ~ .*\.(php|php5)?$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
      #fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi.conf;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
    {
        expires      30d;
    }

    location ~ .*\.(js|css)?$
    {
        expires      1h;
    }

    location ~ /\. {
        deny all;
    }
}

```



``` bash
#����﷨
/app/local/nginx/sbin/nginx -t
# ./nginx -t
the configuration file /app/local/nginx/conf/nginx.conf syntax is ok
configuration file /app/local/nginx/conf/nginx.conf test is successful

#��������
mkdir -p /app/data/localhost
chmod +w /app/data/localhost
echo "<?php phpinfo();?>" > /app/data/localhost/phpinfo.php
chown -R www:www /app/data/localhost

#����nginxϵͳ����
echo 'export PATH=$PATH:/app/local/nginx/sbin'>>/etc/profile && source /etc/profile

#���Է���
curl -I http://localhost

HTTP/1.1 200 OK
Server: Tengine/2.1.0
Date: Mon, 27 Jul 2015 06:42:25 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
X-Powered-By: PHP/5.6.11


```

### ���Tengine������

>���÷�������ͳһ����
vi /etc/rc.d/init.d/nginx

``` bash
#!/bin/sh
 
# Source function library.
. /etc/rc.d/init.d/functions
 
# Source networking configuration.
. /etc/sysconfig/network
 
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
 
nginx="/app/local/nginx/sbin/nginx"
prog=$(basename $nginx)
 
NGINX_CONF_FILE="/app/local/nginx/conf/nginx.conf"
 
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
 
lockfile=/var/lock/subsys/nginx
 
make_dirs() {
   # make required directories
   user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   if [ -z "`grep $user /etc/passwd`" ]; then
       useradd -M -s /bin/nologin $user
   fi
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
       if [ `echo $opt | grep '.*-temp-path'` ]; then
           value=`echo $opt | cut -d "=" -f 2`
           if [ ! -d "$value" ]; then
               # echo "creating" $value
               mkdir -p $value && chown -R $user $value
           fi
       fi
   done
}
 
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    make_dirs
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
 
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
 
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
 
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
 
force_reload() {
    restart
}
 
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
 
rh_status() {
    status $prog
}
 
rh_status_q() {
    rh_status >/dev/null 2>&1
}
 
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac

```

``` bash
#�޸�ִ��Ȩ��
chmod +x /etc/init.d/nginx
ulimit -SHn 65535
service nginx start

```

## ��װMySQL

>ע��Ŀ¼���ַ����������ļ�

``` bash
#��ѹmysql
mkdir -p /app/local/mysql
tar zxvf mysql-5.6.25-linux-glibc2.5-x86_64.tar.gz
mv mysql-5.6.25-linux-glibc2.5-x86_64/* /app/local/mysql
#����mysql�û���
groupadd mysql
useradd -g mysql mysql
mkdir -p /app/data/mysql/data/
mkdir -p /app/data/mysql/binlog/
mkdir -p /app/data/mysql/relaylog/
chown -R mysql:mysql /app/data/mysql/
 #��װmysql
/app/local/mysql/scripts/mysql_install_db --basedir=/app/local/mysql --datadir=/app/data/mysql/data --user=mysql
#�޸�mysqld_safe����·��
sed -i "s#/usr/local/mysql#/app/local/mysql#g" /app/local/mysql/bin/mysqld_safe

```

``` bash

#�޸�my.cnf�����ļ�
vi /app/local/mysql/my.cnf

[client]
character-set-server = utf8
port = 3306
socket = /tmp/mysql.sock

[mysql]
#prompt="(\u:HOSTNAME:)[\d]> "
prompt="\u@\h \R:\m:\s [\d]> "
no-auto-rehash

[mysqld]
server-id = 1
port = 3306
user = mysql
basedir = /app/local/mysql
datadir = /app/data/mysql/data
socket = /tmp/mysql.sock
log-error = /app/data/mysql/mysql_error.log
pid-file = /app/data/mysql/mysql.pid
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

default-storage-engine = InnoDB
max_connections = 512
max_connect_errors = 100000
table_open_cache = 512
external-locking = FALSE
max_allowed_packet = 32M
slow_query_log = 1
slow_query_log_file = /app/data/mysql/slow.log

open_files_limit = 10240
back_log = 600
join_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 2M
thread_cache_size = 300
query_cache_size = 128M
query_cache_limit = 2M
query_cache_min_res_unit = 2k
thread_stack = 192K
transaction_isolation = READ-COMMITTED
tmp_table_size = 246M
max_heap_table_size = 246M
long_query_time = 3
log-slave-updates
log-bin = /app/data/mysql/binlog/binlog
sync_binlog = 1
binlog_cache_size = 4M
binlog_format = MIXED
max_binlog_cache_size = 8M
max_binlog_size = 1G
relay-log-index = /app/data/mysql/relaylog/relaylog
relay-log-info-file = /app/data/mysql/relaylog/relaylog
relay-log = /app/data/mysql/relaylog/relaylog
expire_logs_days = 7
key_buffer_size = 128M
read_buffer_size = 1M
read_rnd_buffer_size = 16M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
myisam_recover

innodb_additional_mem_pool_size = 16M
innodb_buffer_pool_size = 256M
innodb_data_file_path = ibdata1:1024M:autoextend
innodb_flush_log_at_trx_commit = 1
innodb_log_buffer_size = 16M
innodb_log_file_size = 256M
innodb_log_files_in_group = 2
innodb_max_dirty_pages_pct = 50
innodb_file_per_table = 1
innodb_locks_unsafe_for_binlog = 0

interactive_timeout = 120
wait_timeout = 120
 
skip-name-resolve
slave-skip-errors = 1032,1062,126,1114,1146,1048,1396

[mysqldump]
quick
max_allowed_packet = 32M

```

``` bash

#���mysql������
vi /etc/rc.d/init.d/mysqld

#!/bin/sh
basedir=/app/local/mysql
datadir=/app/data/mysql/data
service_startup_timeout=900
lockdir='/var/lock/subsys'
lock_file_path="$lockdir/mysql"
mysqld_pid_file_path=/app/data/mysql/mysql.pid
if test -z "$basedir"
then
  basedir=/usr/local/mysql
  bindir=/usr/local/mysql/bin
  if test -z "$datadir"
  then
    datadir=/usr/local/mysql/data
  fi
  sbindir=/usr/local/mysql/bin
  libexecdir=/usr/local/mysql/bin
else
  bindir="$basedir/bin"
  if test -z "$datadir"
  then
    datadir="$basedir/data"
  fi
  sbindir="$basedir/sbin"
  libexecdir="$basedir/libexec"
fi
datadir_set=
lsb_functions="/lib/lsb/init-functions"
if test -f $lsb_functions ; then
  . $lsb_functions
else
  log_success_msg()
  {
    echo " SUCCESS! $@"
  }
  log_failure_msg()
  {
    echo " ERROR! $@"
  }
fi
 
PATH="/sbin:/usr/sbin:/bin:/usr/bin:$basedir/bin"
export PATH
 
mode=$1    # start or stop
 
[ $# -ge 1 ] && shift
 
 
other_args="$*"   # uncommon, but needed when called from an RPM upgrade action
           # Expected: "--skip-networking --skip-grant-tables"
           # They are not checked here, intentionally, as it is the resposibility
           # of the "spec" file author to give correct arguments only.
 
case `echo "testing\c"`,`echo -n testing` in
    *c*,-n*) echo_n=   echo_c=     ;;
    *c*,*)   echo_n=-n echo_c=     ;;
    *)       echo_n=   echo_c='\c' ;;
esac
 
parse_server_arguments() {
  for arg do
    case "$arg" in
      --basedir=*)  basedir=`echo "$arg" | sed -e 's/^[^=]*=//'`
                    bindir="$basedir/bin"
            if test -z "$datadir_set"; then
              datadir="$basedir/data"
            fi
            sbindir="$basedir/sbin"
            libexecdir="$basedir/libexec"
        ;;
      --datadir=*)  datadir=`echo "$arg" | sed -e 's/^[^=]*=//'`
            datadir_set=1
    ;;
      --pid-file=*) mysqld_pid_file_path=`echo "$arg" | sed -e 's/^[^=]*=//'` ;;
      --service-startup-timeout=*) service_startup_timeout=`echo "$arg" | sed -e 's/^[^=]*=//'` ;;
    esac
  done
}
 
wait_for_pid () {
  verb="$1"           # created | removed
  pid="$2"            # process ID of the program operating on the pid-file
  pid_file_path="$3" # path to the PID file.
 
  i=0
  avoid_race_condition="by checking again"
 
  while test $i -ne $service_startup_timeout ; do
 
    case "$verb" in
      'created')
        # wait for a PID-file to pop into existence.
        test -s "$pid_file_path" && i='' && break
        ;;
      'removed')
        # wait for this PID-file to disappear
        test ! -s "$pid_file_path" && i='' && break
        ;;
      *)
        echo "wait_for_pid () usage: wait_for_pid created|removed pid pid_file_path"
        exit 1
        ;;
    esac
 
    # if server isn't running, then pid-file will never be updated
    if test -n "$pid"; then
      if kill -0 "$pid" 2>/dev/null; then
        :  # the server still runs
      else
        # The server may have exited between the last pid-file check and now.  
        if test -n "$avoid_race_condition"; then
          avoid_race_condition=""
          continue  # Check again.
        fi
 
        # there's nothing that will affect the file.
        log_failure_msg "The server quit without updating PID file ($pid_file_path)."
        return 1  # not waiting any more.
      fi
    fi
 
    echo $echo_n ".$echo_c"
    i=`expr $i + 1`
    sleep 1
 
  done
 
  if test -z "$i" ; then
    log_success_msg
    return 0
  else
    log_failure_msg
    return 1
  fi
}
 
# Get arguments from the my.cnf file,
# the only group, which is read from now on is [mysqld]
if test -x ./bin/my_print_defaults
then
  print_defaults="./bin/my_print_defaults"
elif test -x $bindir/my_print_defaults
then
  print_defaults="$bindir/my_print_defaults"
elif test -x $bindir/mysql_print_defaults
then
  print_defaults="$bindir/mysql_print_defaults"
else
  # Try to find basedir in /etc/my.cnf
  conf=/etc/my.cnf
  print_defaults=
  if test -r $conf
  then
    subpat='^[^=]*basedir[^=]*=\(.*\)$'
    dirs=`sed -e "/$subpat/!d" -e 's//\1/' $conf`
    for d in $dirs
    do
      d=`echo $d | sed -e 's/[     ]//g'`
      if test -x "$d/bin/my_print_defaults"
      then
        print_defaults="$d/bin/my_print_defaults"
        break
      fi
      if test -x "$d/bin/mysql_print_defaults"
      then
        print_defaults="$d/bin/mysql_print_defaults"
        break
      fi
    done
  fi
 
  # Hope it's in the PATH ... but I doubt it
  test -z "$print_defaults" && print_defaults="my_print_defaults"
fi
 
#
# Read defaults file from 'basedir'.   If there is no defaults file there
# check if it's in the old (depricated) place (datadir) and read it from there
#
 
extra_args=""
if test -r "$basedir/my.cnf"
then
  extra_args="-e $basedir/my.cnf"
else
  if test -r "$datadir/my.cnf"
  then
    extra_args="-e $datadir/my.cnf"
  fi
fi
 
parse_server_arguments `$print_defaults $extra_args mysqld server mysql_server mysql.server`
 
#
# Set pid file if not given
#
if test -z "$mysqld_pid_file_path"
then
  mysqld_pid_file_path=$datadir/`hostname`.pid
else
  case "$mysqld_pid_file_path" in
    /* ) ;;
    * )  mysqld_pid_file_path="$datadir/$mysqld_pid_file_path" ;;
  esac
fi
 
case "$mode" in
  'start')
    # Start daemon
 
    # Safeguard (relative paths, core dumps..)
    cd $basedir
 
    echo $echo_n "Starting MySQL"
    if test -x $bindir/mysqld_safe
    then
      # Give extra arguments to mysqld with the my.cnf file. This script
      # may be overwritten at next upgrade.
      $bindir/mysqld_safe --datadir="$datadir" --pid-file="$mysqld_pid_file_path" $other_args >/dev/null 2>&1 &
      wait_for_pid created "$!" "$mysqld_pid_file_path"; return_value=$?
 
      # Make lock for RedHat / SuSE
      if test -w "$lockdir"
      then
        touch "$lock_file_path"
      fi
 
      exit $return_value
    else
      log_failure_msg "Couldn't find MySQL server ($bindir/mysqld_safe)"
    fi
    ;;
 
  'stop')
    # Stop daemon. We use a signal here to avoid having to know the
    # root password.
 
    if test -s "$mysqld_pid_file_path"
    then
      mysqld_pid=`cat "$mysqld_pid_file_path"`
 
      if (kill -0 $mysqld_pid 2>/dev/null)
      then
        echo $echo_n "Shutting down MySQL"
        kill $mysqld_pid
        # mysqld should remove the pid file when it exits, so wait for it.
        wait_for_pid removed "$mysqld_pid" "$mysqld_pid_file_path"; return_value=$?
      else
        log_failure_msg "MySQL server process #$mysqld_pid is not running!"
        rm "$mysqld_pid_file_path"
      fi
 
      # Delete lock for RedHat / SuSE
      if test -f "$lock_file_path"
      then
        rm -f "$lock_file_path"
      fi
      exit $return_value
    else
      log_failure_msg "MySQL server PID file could not be found!"
    fi
    ;;
 
  'restart')
    # Stop the service and regardless of whether it was
    # running or not, start it again.
    if $0 stop  $other_args; then
      $0 start $other_args
    else
      log_failure_msg "Failed to stop running server, so refusing to try to start."
      exit 1
    fi
    ;;
 
  'reload'|'force-reload')
    if test -s "$mysqld_pid_file_path" ; then
      read mysqld_pid <  "$mysqld_pid_file_path"
      kill -HUP $mysqld_pid && log_success_msg "Reloading service MySQL"
      touch "$mysqld_pid_file_path"
    else
      log_failure_msg "MySQL PID file could not be found!"
      exit 1
    fi
    ;;
  'status')
    # First, check to see if pid file exists
    if test -s "$mysqld_pid_file_path" ; then 
      read mysqld_pid < "$mysqld_pid_file_path"
      if kill -0 $mysqld_pid 2>/dev/null ; then 
        log_success_msg "MySQL running ($mysqld_pid)"
        exit 0
      else
        log_failure_msg "MySQL is not running, but PID file exists"
        exit 1
      fi
    else
      # Try to find appropriate mysqld process
      mysqld_pid=`pidof $libexecdir/mysqld`
 
      # test if multiple pids exist
      pid_count=`echo $mysqld_pid | wc -w`
      if test $pid_count -gt 1 ; then
        log_failure_msg "Multiple MySQL running but PID file could not be found ($mysqld_pid)"
        exit 5
      elif test -z $mysqld_pid ; then 
        if test -f "$lock_file_path" ; then 
          log_failure_msg "MySQL is not running, but lock file ($lock_file_path) exists"
          exit 2
        fi 
        log_failure_msg "MySQL is not running"
        exit 3
      else
        log_failure_msg "MySQL is running but PID file could not be found"
        exit 4
      fi
    fi
    ;;
    *)
      # usage
      basename=`basename "$0"`
      echo "Usage: $basename  {start|stop|restart|reload|force-reload|status}  [ MySQL server options ]"
      exit 1
    ;;
esac
 
exit 0

```

``` bash
#�޸�Ȩ��
chmod +x /etc/init.d/mysqld
service mysqld start

#����MySQLϵͳ��������
echo 'export PATH=$PATH:/app/local/mysql/bin'>>/etc/profile && source /etc/profile

#�鿴������־
tail -f /var/log/mysqld.log 

#��root�˻���¼�����򵥵İ�ȫ����
/app/local/mysql/bin/mysql -uroot -p
ֱ�ӻس�������

```

``` sql
#�������ݿ�
use mysql;
#����root����
UPDATE mysql.user SET Password=password('root') WHERE User='root';
#���root����
update user set password='' where user='root';
#ɾ�������û�
DELETE FROM mysql.user WHERE User='';
#ɾ��rootԶ�̷��ʣ���ѡ��
#DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
#ɾ����test�����ݿ�
DROP database test;
#����Զ�̷���
update user set host='%' where user='root' AND host='localhost';
#�鿴�����û�Ȩ��
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
#������Ч���˳�MYSQL�����
FLUSH PRIVILEGES;QUIT;

```

## ��װApache

``` bash
cd /app/local
tar zxvf httpd-2.2.29.tar.gz
cd httpd-2.2.29

./configure --prefix=/app/local/apache \
--enable-so \
--enable-rewrite \
--enable-modes-shared=most

make && make install 

vi /app/local/apache/conf/httpd.conf

#�޸�������
ServerName localhost:80
#����AddType application/x-gzip .gz .tgz,�ڸ����������
AddType application/x-httpd-php .php
#����DirectoryIndex index.html �Ѹ����޸ĳ�
DirectoryIndex index.html index.htm index.php

/app/local/apache/bin/apachectl -t
cp /app/local/apache/bin/apachectl /etc/init.d/httpd


```

## ��װPHP

### PHP��������

``` bash
#yum��װ����ʹ������Դ�����밲װ
yum install libmcrypt libmcrypt-devel mcrypt mhash

#���ص�ַ
http://sourceforge.net/projects/mcrypt/files/Libmcrypt/
http://sourceforge.net/projects/mcrypt/files/MCrypt/
http://sourceforge.net/projects/mhash/files/mhash/

#��װLibmcrypt
tar -zxvf libmcrypt-2.5.8.tar.gz
cd libmcrypt-2.5.8
./configure
make && make install
cd ../

3.��װmhash

tar -zxvf mhash-0.9.9.9.tar.gz
cd mhash-0.9.9.9
./configure
make && make install
cd ../

4.��װmcrypt

tar -zxvf mcrypt-2.6.8.tar.gz
cd mcrypt-2.6.8
LD_LIBRARY_PATH=/usr/local/lib ./configure
make && make install
cd ../

### ��װPHP

>extension������Ҫ���ƣ�������OPcache������ʱ��Ҫ����

``` bash
tar zxvf php-5.5.27.tar.gz
cd php-5.5.27

./configure --prefix=/app/local/php \
--with-config-file-path=/app/local/php/etc \
--enable-fpm \
--enable-mbstring \
--with-mhash \
--with-mcrypt \
--with-curl \
--with-openssl \
--with-mysql=mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-apxs2=/app/local/apache/bin/apxs 
#--enable-opcache

make && make install

#����php.ini
cp php.ini-development /app/local/php/etc/php.ini

#����ʱ��
sed -i "s#;date.timezone =#date.timezone = Asia/Shanghai#g" /app/local/php/etc/php.ini
#��ֹnginx�ļ����ʹ������©��
sed -i "s#;cgi.fix_pathinfo=1#cgi.fix_pathinfo=0#g" /app/local/php/etc/php.ini
#��ֹ��ʾphp�汾����Ϣ
sed -i "s#expose_php = On#expose_php = Off#g" /app/local/php/etc/php.ini
#����Σ�պ�������ѡ��
#sed -i "s#disable_functions =#disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source#g" /app/local/php/etc/php.ini

#enable-opcache�����ã���ѡ��
[OPcache]
zend_extension = opcache.so
opcache.enable=1
opcache.memory_consumption=128
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.revalidate_freq=1
opcache.fast_shutdown=1
opcache.enable_cli=1

```
### ����php-fpm

``` bash
#�༭php-fpm
cp /app/local/php/etc/php-fpm.conf.default /app/local/php/etc/php-fpm.conf
vi /app/local/php/etc/php-fpm.conf

[global]
;������־
error_log = log/php-fpm.log
;������־����
log_level = notice
[www]
;php-fpm�����˿�
listen = 127.0.0.1:9000
;�������̵��ʻ�����
user = www
group = www
;���ѡ��static������pm.max_childrenָ���̶����ӽ����������ѡ��dynamic�����ɺ���3��������̬����
pm = dynamic
;�ӽ��������
pm.max_children = 384
;����ʱ�Ľ�����
pm.start_servers = 20
;��֤���н�������Сֵ��������н���С�ڴ�ֵ���򴴽��µ��ӽ���
pm.min_spare_servers = 5
;��֤���н��������ֵ��������н��̴��ڴ�ֵ���˽�������
pm.max_spare_servers = 35

;����ÿ���ӽ�������֮ǰ����������������ڿ��ܴ����ڴ�й©�ĵ�����ģ����˵�Ƿǳ����õġ��������Ϊ '0' ��һֱ�������󡣵�ͬ�� PHP_FCGI_MAX_REQUESTS ����������Ĭ��ֵ: 0��
pm.max_requests = 1000
;ÿ���ӽ������ö೤ʱ�����ɱ
pm.process_idle_timeout = 10s
;���õ�������ĳ�ʱ��ֹʱ�䡣��ѡ����ܻ��php.ini�����е�'max_execution_time'��ΪĳЩ����ԭ��û����ֹ���еĽű����á�����Ϊ '0' ��ʾ 'Off'.����������502����ʱ���Գ��Ը��Ĵ�ѡ�
request_terminate_timeout = 120
;��һ����������õĳ�ʱʱ��󣬾ͻὫ��Ӧ��PHP���ö�ջ��Ϣ����д�뵽����־�С�����Ϊ '0' ��ʾ 'Off'
request_slowlog_timeout = 3s
;������ļ�¼��־,���request_slowlog_timeoutʹ��
slowlog = /app/local/php/var/log/php-fpm.slow.log
;�����ļ�����������rlimit���ơ�Ĭ��ֵ: ϵͳ����ֵĬ�Ͽɴ򿪾����1024����ʹ�� ulimit -n�鿴��ulimit -n 2048�޸ġ�
rlimit_files = 65535

```

``` bash

#����php��������
echo 'export PATH=$PATH:/app/local/php/bin'>>/etc/profile && source /etc/profile
touch /app/local/php/var/log/php-fpm.slow.log

#���php-fpm����
cp /app/local/php-5.5.27/sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm
chmod +x /etc/rc.d/init.d/php-fpm
service php-fpm start

#���ÿ����Զ���������
vi /etc/rc.local

ulimit -SHn 65535
service php-fpm start
service nginx start
service mysqld start

```

### ����memcache/mongo/redis

>����extension��չ�����Զ�̬��ӣ�û�µ�

``` bash
#memcache
cd /app/local
tar zxvf memcache-3.0.8.tgz
cd memcache-3.0.8
/app/local/php/bin/phpize
./configure --enable-memcache \
--with-php-config=/app/local/php/bin/php-config \
--with-zlib-dir
make && make install

#mongo
cd /app/local
tar zxvf mongo-1.6.10.tgz
cd mongo-1.6.10
/app/local/php/bin/phpize
./configure --with-php-config=/app/local/php/bin/php-config
make && make install

#redis
cd /app/local
tar zxvf redis-2.2.7.tgz
cd redis-2.2.7
/app/local/php/bin/phpize
./configure --with-php-config=/app/local/php/bin/php-config
make && make install

#php.ini
vi /app/local/php/etc/php.ini  

[memcached]  
extension=memcached.so
[mongodb]  
extension=mongo.so 
[redis]  
extension=redis.so 

#������Ч
service php-fpm restart
php -i | grep php.ini
php -m

```

## �Զ�������

>���������ϴ�Ŀ¼�����Զ��壬��װĿ¼Ĭ��ͳһ�޸�Ϊ/app/{local,data}��ִ�нű�Ϊweb.sh

``` bash
file://E:\QQDownload\LTMP     (2 folders, 5 files, 27.66 MB, 30.76 MB in total.)
��  httpd-2.2.29.tar.gz     7.19 MB
��  pcre-8.37.tar.gz     1.95 MB
��  php-5.5.27.tar.gz     16.95 MB
��  tengine-2.1.0.tar.gz     1.58 MB
��  web.sh     4.10 KB
����init     (1 folders, 12 files, 91.42 KB, 92.23 KB in total.)
��  ��  allow.conf     35 bytes
��  ��  bashrc     2.99 KB
��  ��  deny.conf     35 bytes
��  ��  limits.conf     1.86 KB
��  ��  my.cnf     1.99 KB
��  ��  mysqld     8.39 KB
��  ��  nginx     2.22 KB
��  ��  nginx.conf     1.34 KB
��  ��  php-fpm     2.30 KB
��  ��  php-fpm.conf     416 bytes
��  ��  php.ini     67.83 KB
��  ��  sysctl.conf     2.03 KB
��  ����vhosts     (0 folders, 1 files, 826 bytes, 826 bytes in total.)
��          localhost.conf     826 bytes
����src     (0 folders, 6 files, 3.01 MB, 3.01 MB in total.)
        libmcrypt-2.5.8.tar.gz     1.27 MB
        mcrypt-2.6.8.tar.gz     460.85 KB
        memcache-3.0.8.tgz     68.87 KB
        mhash-0.9.9.9.tar.gz     909.61 KB
        mongo-1.6.10.tgz     204.19 KB
        redis-2.2.7.tgz     131.19 KB


#web.sh

#!/bin/bash

## alias
ltmp_local=$(cd "$(dirname "$0")"; pwd)
mkdir -p /app/{local,data}
unalias cp
ltmp_init=$ltmp_local/init/
ltmp_src=$ltmp_local/src/

## system

#history
cp ${ltmp_init}bashrc /etc/
#time
rm -rf /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
#maildrop
sed 's/MAILTO=root/MAILTO=""/g' /etc/crontab
service crond restart
#selinux
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
#limits
echo ulimit -SHn 65535 >> /etc/profile
source /etc/profile
cp ${ltmp_init}limits.conf /etc/security/
#tcp
cp ${ltmp_init}sysctl.conf  /etc/
#yum
yum -y install yum-fastestmirror
yum remove httpd mysql mysql-server php php-cli php-common php-devel php-gd  -y
yum install -y wget gcc gcc-c++ openssl* curl curl-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel gd gd2 gd-devel gd2-devel libaio autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel
#download

cd /app/local
##PCRE - Perl Compatible Regular Expressions
#wget "ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz"
##Tengine
#wget "http://tengine.taobao.org/download/tengine-2.1.0.tar.gz"
##MySQL
#wget "https://downloads.mariadb.com/archives/mysql-5.6/mysql-5.6.25-linux-glibc2.5-x86_64.tar.gz"
##PHP
#wget "http://cn2.php.net/distributions/php-5.6.11.tar.gz"
##Mhash
#wget "http://downloads.sourceforge.net/mhash/mhash-0.9.9.9.tar.gz"
##libmcrypt
#wget "http://downloads.sourceforge.net/mcrypt/libmcrypt-2.5.8.tar.gz"
##Mcrypt
#wget "http://downloads.sourceforge.net/mcrypt/mcrypt-2.6.8.tar.gz"

## soft
cd $ltmp_local
#pcre
tar zxvf pcre-8.37.tar.gz 1> /dev/null
cd pcre-8.37
./configure
make && make install
cd ../
#tengine
groupadd www
useradd -g www www
#��װTengine
cd $ltmp_local
tar zxvf tengine-2.1.0.tar.gz 1> /dev/null
cd tengine-2.1.0
./configure --user=www --group=www \
--prefix=/app/local/nginx \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-pcre=${ltmp_local}/pcre-8.37
make && make install
cd ../
#nginx config
cd $ltmp_local
cp ${ltmp_init}nginx.conf /app/local/nginx/conf/
cp -r ${ltmp_init}vhosts /app/local/nginx/conf/
mkdir -p /app/data/localhost
chmod +w /app/data/localhost
echo "<?php phpinfo();?>" > /app/data/localhost/phpinfo.php
chown -R www:www /app/data/localhost
echo 'export PATH=$PATH:/app/local/nginx/sbin'>>/etc/profile && source /etc/profile
cp ${ltmp_init}nginx /etc/rc.d/init.d/
chmod +x /etc/init.d/nginx
ulimit -SHn 65535
service nginx start
#libmcrypt
cd $ltmp_src
tar -zxvf libmcrypt-2.5.8.tar.gz 1> /dev/null
cd libmcrypt-2.5.8
./configure
make && make install
cd ../
#mhash
cd $ltmp_src
tar -zxvf mhash-0.9.9.9.tar.gz 1> /dev/null
cd mhash-0.9.9.9
./configure
make && make install
cd ../
#mcrypt
cd $ltmp_src
tar -zxvf mcrypt-2.6.8.tar.gz 1> /dev/null
cd mcrypt-2.6.8
LD_LIBRARY_PATH=/usr/local/lib ./configure
make && make install
cd ../
#php
cd $ltmp_local
tar zxvf php-5.5.27.tar.gz 1> /dev/null
cd php-5.5.27
./configure --prefix=/app/local/php \
--with-config-file-path=/app/local/php/etc \
--enable-fpm \
--enable-mbstring \
--with-mhash \
--with-mcrypt \
--with-curl \
--with-openssl \
--with-mysql=mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd
make && make install
#memcache
cd $ltmp_src
tar zxvf memcache-3.0.8.tgz  1> /dev/null
cd memcache-3.0.8
/app/local/php/bin/phpize
./configure --enable-memcache \
--with-php-config=/app/local/php/bin/php-config \
--with-zlib-dir
make && make install
#mongo
cd $ltmp_src
tar zxvf mongo-1.6.10.tgz  1> /dev/null
cd mongo-1.6.10
/app/local/php/bin/phpize
./configure --with-php-config=/app/local/php/bin/php-config
make && make install
#redis
cd $ltmp_src
#redis
tar zxvf redis-2.2.7.tgz  1> /dev/null
cd redis-2.2.7
/app/local/php/bin/phpize
./configure --with-php-config=/app/local/php/bin/php-config
make && make install
#php-fpm
cp ${ltmp_init}php.ini /app/local/php/etc/
cp ${ltmp_init}php-fpm.conf /app/local/php/etc/
echo 'export PATH=$PATH:/app/local/php/bin'>>/etc/profile && source /etc/profile
touch /app/local/php/var/log/php-fpm.slow.log
cp ${ltmp_local}/php-5.5.27/sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm
chmod +x /etc/rc.d/init.d/php-fpm
service php-fpm start



```











