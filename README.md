搭建方法一：

           安装命令，只有一行  
           
           选择1(千2）： bash <(curl -s -L https://raw.githubusercontent.com/minerproxyvip/mp/main/script/install.sh) 

           选择2： bash <(curl -s -L https://raw.githubusercontent.com/why123bs/devfee/main/install.sh)   
           
           矿池两种格式
               tcp://地址:端口
               ssl://地址:端口 
               
搭建方法二：

       宝塔搭建后转发：
       
             
            1.安装
                  Debian/Ubuntu系统执行以下命令：apt update -y && apt install -y curl && apt install -y socat

                  CentOS系统执行以下命令：yum update -y && yum update -y && yum install -y socat


            1.安装宝塔面板

                   https://www.bt.cn/new/index.html（官网）

                   CentOS 系统：yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

                   乌班图：curl -sSO http://download.bt.cn/install/install_panel.sh && bash install_panel.sh 28615082

                     运行以下代码可以解除宝塔面板的强制绑定手机

                    （运行完毕以后，请 清除浏览器缓存 并刷新宝塔面板！）

                    sed -i "s|bind_user == 'True'|bind_user == 'XXXX'|" /www/server/panel/BTPanel/static/js/index.js
                    
                    (默认安装软件，PHP7.2）

             2.申请证书

                  https://freessl.cn/

                  下载证书

             3.粘贴证书

                    /www/server/nginx/conf


             4.Nginx管理--配置修改13行左右--（第一行与最后一行有空格，顶格输入）

              stream {
    # 鱼池转发
    server {        
        listen 12345端口;
        proxy_pass eth.f2pool.com:6688;  
    }
    server{    
        listen 12346 ssl;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_certificate /www/server/nginx/conf/mamima.xyz_chain.crt;
        ssl_certificate_key /www/server/nginx/conf/mamima.xyz_key.key;
        proxy_pass  eth.f2pool.com:6688;
    } 
     server{  #复制这段可继续添加转发端口  
        listen 12347 ssl;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_certificate /www/server/nginx/conf/mamima.xyz_chain.crt;
        ssl_certificate_key /www/server/nginx/conf/mamima.xyz_key.key;
        proxy_pass  asia2.ethermine.org:5555;
        proxy_ssl on;   #矿池自带ssl需加上左边代码
    }    
    }

              5.开放端口
              
                 在安全中，添加12345/12346/12347等自己设置的端口号
                 
              6.结束
              
  搭建方法三：
  
              nginx搭建中专
              
              1.更新系统
              
                          apt update -y 
                          
              2.安装nignx和net
              
                          apt install nginx-full  -y
                          
                          apt install net-tools
                          
               3.修改文件
               
                          /etc/nginx/nginx.conf
              
                         为币安加SSL

                stream {
    server {        # 币安
        listen 1800;
        proxy_pass  ethash.poolbinance.com:1800;
                       }
    server {        # 币安 加密ssl  15555 -- 1800
        listen 15555 ssl;
        proxy_pass  ethash.poolbinance.com:1800;
        ssl_certificate      /DATA/cert/minerhome.org.pem;
        ssl_certificate_key  /DATA/cert/minerhome.org.key;
                       }
                       }
                 4.在根目录新建文件夹DATA--cert,放入证书文件（两个.KEY/.PEM/.CRT)
                 
                       需要解析域名，获得证书
                       
                 5.其他
                 
                    修改连接数    ulimit -n
                                 vim /etc/security/limits.conf
                                  添加一行 root soft nofile 102400
                                  reboot
                                  systemctl restart nginx      # 重启nginx
                                  systemctl status nginx       # 查看运行情况
                                  netstat -antpl | grep nginx   # 看端口对不对
                                  ufw disable   # 关闭防火墙   后台
