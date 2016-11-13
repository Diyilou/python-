###使用python建立SSH链接，并使用SFTP传数据到服务器
----
代码如下：
<pre>
  #!/usr/bin/python
# -*- coding: UTF-8 -*-

import paramiko

from config.server import server_data

import sys
reload(sys)
sys.setdefaultencoding('utf8')

# 建立单独的scp链接

＃　hostname: 主机名　例如：192.168.1.107
＃  username: 用户名　例如：root （一般都是root用户，除非特别设置过）
＃ password：服务器密码　例如：　...... （自己想呗！）
＃ prot: 服务器端口   例如：22 （默认端口号22，但是有些主机做过更改，具体需要看情况）
＃ sites : 当前服务器上的网站（一台服务器布置一个网站的请款下），是一个json数据集，
＃　例如　: sites: {domain: 'www.xxxx.com', 'name': 'xxx网站'}
def trans (hostname, username, password, port, sites):
    
    scp = paramiko.Transport((hostname, port))
    scp.connect(username=username, password=password)

    sftp = paramiko.SFTPClient.from_transport(scp)

    for v in sites:
        print v
        u = '/host/sites/' + v['domain'] + '/css/main.css'
        print u
        sftp.put('./upload/main.css', u)

    sftp.close()

# 执行每个网站
def main ():

    i = 0    
    // 循环服务器数据，然后向每个服务器上传文件
    for data in server_data:
        i = i + 1
        print i
        trans(data['hostname'], data['username'], data['password'], data['port'], data['sites'])
    
    print 'ok'

if __name__ == '__main__':
    main()
</pre>

####特别的：给出server_data的数据格式：

<pre>
server_data = [
    {
        'hostname': '190.168.1.107',
        'id': 107,
        'username': 'root',
        'port': 22000,
        'password': 'xxxxxxxxxxxxx',
        'sites': [
            {
                'name': '百度'.decode('utf-8'),
                'domain': 'baidu.com'
            },
            {
                'name': '谷歌'.decode('utf-8'),
                'domain': 'google.com'
            },
            {
                'name': '知乎'.decode('utf-8'),
                'domain': 'zhihu.com'
            }
        ]
    },
    {
        'hostname': '192.168.10.18',
        'id': 108,
        'username': 'root',
        'port': 22,
        'password': 'xxxxxxxxxxx',
        'sites': [
            {
                'name': '淘宝'.decode('utf-8'),
                'domain': 'taobao.com'
            },
            {
                'name': '京东'.decode('utf-8'),
                'domain': 'jd.com'
            }
        ]
    }
 ];
</pre>


感谢分享，感谢开源！
