---

# My note for gitlab setup 

## GitLab-CE 下载安装及使用-Ubuntu
### 下载安装

1. 安装须知  
  安装Gitlab需要在4Gb内存以上，否则会很卡，甚至出现502错误无法访问
  下载gitlab下载地址  
  [gitlab-download](https://packages.gitlab.com/gitlab/gitlab-ce)


2. 修改安装源
   1)添加公钥   
     首先信任Gitlab的GPG公钥：   
     ( curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null ) 

    使用清华源  
     修改文件 /etc/apt/sources.list.d/gitlab-ce.list   
      deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu bionic main  


3. 安装gitlab-ce
   1) 更新源  
    $ sudo apt-get update  && sudo apt-get upgrade

   2) 安装SMTP发送邮件软件（可选）  
    $ sudo apt-get install -y postfix  

   3) 添加GitLab包仓库  
    $ curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

   4) 安装GitLab 
    $ sudo apt-get install gitlab-ce

   4) 配置 /etc/gitlab/gitlab.rb 文件

    >  letsencrypt['enable'] = false   
    
    > external_url 'http://192.168.1.9'  
    >  
    >  gitlab_rails['time_zone'] = 'Beijing'
    >  
    >  \#gitlab_rails['gitlab_email_from'] = 'noreply@idreamsky.com'
    >  
    >  gitlab_rails['manage_backup_path'] = true  
    >  gitlab_rails['backup_path'] = "/data/gitlab/backups"  
    >  
    >  gitlab_rails['backup_keep_time'] = 172800  
    >  
    >   git_data_dirs({  
    >     "default" =    >  {    
    >       "path" =    >  "/data/gitlab/git-data",    
    >       "failure_count_threshold" =    >  10,    
    >       "failure_wait_time" =    >  30,    
    >       "failure_reset_time" =    >  1800,    
    >       "storage_timeout" =    >  30    
    >      }    
    >   })    
    >    

    >  
    >  unicorn['worker_processes'] = 48
    >  
    >  postgresql['shared_buffers'] = "8GB"
    >  postgresql['max_connections'] = 800
    >  postgresql['max_worker_processes'] = 24    
    >  
    >  nginx['worker_processes'] = 8    
    >  nginx['worker_connections'] = 10000   
    >  
    >  \# 添加ip白名单    
    >  gitlab_rails['rack_attack_git_basic_auth'] = {    
    >    'enabled' =    >  true,     
    >    'ip_whitelist' =    >  ["127.0.0.1","192.168..1.9"],   
    >    'maxretry' =    >  30,  
    >    'findtime' =    >  30,  
    >    'bantime' =    >  600  
    >  }  





  重新加载配置文件(等待时间稍长)  
    $ gitlab-ctl reconfigure

  重启GItLab服务  
    $ gitlab-ctl restart
  查看GitLab状态  
    $ gitlab-ctl status
  检查80端口号  
    $ netstat -an | grep 80
  浏览器输入地址 https://192.168.1.9  配置用户名和密码  


  安装完成后：    

  gitlab-ctl reconfigure    #使配置文件生效 但是会初始化除了gitlab.rb之外的所有文件  

  gitlab-ctl status        #查看状态  

  gitlab-ctl stop          #停服务  

  gitlab-ctl start         #起服务  

  gitlab-ctl tail          #查看日志的命令（Gitlab 默认的日志文件存放在/var/log/gitlab 目录下）  



  登录测试  

  首次登录需要修改root密码  
