dd if=/dev/zero of=/tmp/bigfile bs=1M count=100
    
    if 输入
    of 输出
    bs 每次多大
    count 次数
    
cat /etc/passwd /etc/shadow > /dev/null 2>/tmp/errorfile
    
    正确输出到 /dev/null
    错误输出到 /dev/null
    
[toc]
# Linux系统管理
## 系统日志管理
### rsyslog
rsyslog.service 为服务  
配置文件： /etc/rsyslog.conf（定义日志存放位置）
>/etc/rsyslog.conf  
>  
>>1）*.info;mail.none;authpriv.none;cron.none  /var/log/messages
>>
>>     将.info级别及以上的，除去mail.none;authpriv.none;cron.none的日志放到/var/log/messages 中  
>>2）x.y
>>
>>     x是类别
>>     y是级别
>>3）@@remote-host:514
>>
>>     通过514端口将数据传输到远程服务器上
>>     其中@@为TCP  @为UDP  不要忘了将服务打开
>>     ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
>>     # Provides UDP syslog reception
>>     #$ModLoad imudp
>>     #$UDPServerRun 514
>>     
>>     # Provides TCP syslog reception
>>     #$ModLoad imtcp
>>     #$InputTCPServerRun 514
>>     ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑


日志级别：
    
    #define KERN_EMERG    "<0>"  /* system is unusable               */
    #define KERN_ALERT    "<1>"  /* action must be taken immediately */
    #define KERN_CRIT     "<2>"  /* critical conditions              */
    #define KERN_ERR      "<3>"  /* error conditions                 */
    #define KERN_WARNING  "<4>"  /* warning conditions               */
    #define KERN_NOTICE   "<5>"  /* normal but significant condition */
    #define KERN_INFO     "<6>"  /* informational                    */
    #define KERN_DEBUG    "<7>"  /* debug-level messages             */

日志描述:
![日志](log1.png)

### logrotate（日志轮巡）  
配置文件：  
/etc/logrotate.conf   核心配置文件  
/etc/logrotate.d/     平常的轮巡配置文件的目录文件
>/etc/logrotate.d/  
>>
>>     /var/log/mylog { #针对mylog日志做轮巡
>>     weekly #一周
>>     create #创建一个新文件
>>     rotate 2 #保留两个日志文件(不含现在的，含的话则是3个)
>>     dateext #旧的文件加上时间戳
>>> 手动轮巡
>>>                   
>>>          若测试则先 date命令修改时间  再  logrotate /etc/logratate.conf