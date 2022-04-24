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

             2.申请证书

                  https://freessl.cn/

                  下载证书

             3.粘贴证书

                    /www/server/nginx/conf


             4.Nginx管理--配置修改13行左右--（第一行与最后一行有空格，顶格输入）

              stream {
    # 鱼池转发
    server {        
        listen 自填写使用的端口;
        proxy_pass eth.f2pool.com:6688;  
    }
    server{    
        listen 自填写使用的端口 ssl;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_certificate /www/server/nginx/conf/mamima.xyz_chain.crt;
        ssl_certificate_key /www/server/nginx/conf/mamima.xyz_key.key;
        proxy_pass  eth.f2pool.com:6688;
    } 
     server{  #复制这段可继续添加转发端口  
        listen 自填写使用的端口 ssl;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_certificate /www/server/nginx/conf/mamima.xyz_chain.crt;
        ssl_certificate_key /www/server/nginx/conf/mamima.xyz_key.key;
        proxy_pass  asia2.ethermine.org:5555;
        proxy_ssl on;   #矿池自带ssl需加上左边代码
    }    
    }

              5.成功
              
           
