#!/usr/bin/env python34
# encoding: utf-8
'''
@author: guozhijie
@license: (C) Copyright 2013-2017, Node Supply Chain Manager Corporation Limited.
@contact: guo.zhijie@21vianet.com
@software: pycharm
@file: pexpect20.py
@time: 2018/7/13 11:18
@desc:
'''

import pexpect
import time
import paramiko

# ssh到ftp服务器执行命令
def ssh21(cmd):
    try:
        ssh = paramiko.SSHClient()
        key = paramiko.AutoAddPolicy()
        ssh.set_missing_host_key_policy(key)
        ssh.connect('ip', port, 'root', 'password', timeout=10)
        stdin, stdout, stderr = ssh.exec_command(cmd)
        time.sleep(2)
        # print(stdout.read())
        ssh.close()
    except:
        print("ssh失败")
# 设备列表

# cisco设备
cisco_list = [
              {"hostname":"7609B","ipaddr":"8.8.8.8"}
              ]
#需要enable权限的设备
cisco_list2 = [{"hostname":"0008","ipaddr":"8.8.8.8"}]
# 获取当前日期
Today = time.strftime("%Y-%m-%d", time.localtime())
# acs账户
username = ""
passwd = ""
# 定义提示符
prompt_switch = "[$#>]"
confirm_cisco = ":"
# 成功备份h3c,cisco,juniper的设备数量
h3c_times = 0
cisco_times = 0
juniper_times = 0
# 登录到ftp服务器创建目录
ssh21("python3 /etc/script/mkdir.py")
time.sleep(2)
index = 0
index2 = 0

for device in cisco_list:
    ip = cisco_list[index].get("ipaddr")
    hostname = cisco_list[index].get("hostname")
    index += 1
    cmd_telnet = "telnet " + ip
    this = pexpect.spawn(cmd_telnet)
    index_1 = this.expect(["[uU]sername", "(?i)Unknown host", pexpect.EOF, pexpect.TIMEOUT])
    # fout = file("pexpect.txt","w")
    # this.logfile = fout
    if index_1 == 0:
        this.sendline(username)
        index_2 = this.expect(["[pP]assword", pexpect.EOF, pexpect.TIMEOUT])
        if index_2 == 0:
            this.sendline(passwd)
            this.expect(prompt_switch,timeout = None)
            this.sendline("copy running-config ftp://idcyw:idcyw@ip/{}".format(hostname))
            time.sleep(5)
            this.sendline("")
            time.sleep(5)
            this.sendline("")
            time.sleep(20)
            this.expect(prompt_switch,timeout = None)
            this.close()
            # ssh到2.21进行mv
            ssh21("mv /home/idcyw/{} /home/backup/hbyw/{}-new1".format(hostname,Today))
            # print(this.before)
            cisco_times += 1
            print("已成功备份{}台cisco设备".format(cisco_times))
        else:
            print("login failed...根本没看到'[pP]assword'啊")
            this.close(force=True)
    else:
        print("login failed...根本没看到'[uU]sername'啊")
        this.close(force=True)


for device2 in cisco_list2:
    ip = cisco_list2[index2].get("ipaddr")
    hostname = cisco_list2[index2].get("hostname")
    index2 += 1
    cmd_telnet = "telnet " + ip
    this = pexpect.spawn(cmd_telnet)
    index_1 = this.expect(["[uU]sername", "(?i)Unknown host", pexpect.EOF, pexpect.TIMEOUT])
    # fout = file("pexpect.txt","w")
    # this.logfile = fout
    if index_1 == 0:
        this.sendline(username)
        index_2 = this.expect(["[pP]assword", pexpect.EOF, pexpect.TIMEOUT])
        if index_2 == 0:
            this.sendline(passwd)
            # time.sleep(3)
            this.expect(prompt_switch,timeout = None)
            this.sendline("enable")
            # time.sleep(3)
            this.expect(confirm_cisco, timeout=None)
            this.sendline("CCIBwlb_@123")
            # time.sleep(3)
            this.expect(prompt_switch, timeout=None)
            this.sendline("copy running-config ftp://idcyw:idcyw@ip/{}".format(hostname))
            time.sleep(5)
            this.sendline("")
            time.sleep(5)
            this.sendline("")
            time.sleep(20)
            this.expect(prompt_switch,timeout = None)
            this.close()
            # ssh到2.21进行mv
            ssh21("mv /home/idcyw/{} /home/backup/hbyw/{}-new1".format(hostname,Today))
            # print(this.before)
            cisco_times += 1
            print("已成功备份{}台cisco设备".format(cisco_times))
        else:
            print("login failed...根本没看到'[pP]assword'啊")
            this.close(force=True)
    else:
        print("login failed...根本没看到'[uU]sername'啊")
        this.close(force=True)



