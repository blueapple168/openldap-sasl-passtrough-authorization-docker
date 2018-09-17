# openldap-sasl-passtrough-authorization-docker

## Example for build your own container with OpenLDAP Pass-Trough authentication with SASL from Dockerfile
```sh
docker build -t openldap --build-arg \
--build-arg LDAP_SERVERS="ldap://YOUR_MAIN_SERVER_IP:389/" \
--build-arg LDAP_SEARCH_BASE="MAIN_SERVER_SEARCH_BASE" \
--build-arg LDAP_FILTER="(sAMAccountName=%U)" \
--build-arg LDAP_BIND_DN="MAIN_SERVER_USER_BIND" \
--build-arg LDAP_PASSWORD="MAIN_SERVER_USER_PASSWORD" \
--build-arg LDAP_DN_BASE="MAIN_SERVER_DN_BASE" \
--build-arg LDAP_ORGANIZATION="ORGANIZATION_NAME_FOR_FIRST_OU" \
--build-arg LDAP_ROOT_DN="MANAGER_USER" \
--build-arg LDAP_ROOT_PW="TEXT_PLAIN_PASSWORD OR \{SSHA\}SSHA_GENERATED_CODE_PASSWORD" \
.
```
Running slapd（运行 slapd）
slapd(8) is designed to be run as a stand-alone server. This allows the server to take advantage of caching, manage concurrency issues with underlying databases, and conserve system resources. Running from inetd(8) is NOT an option.
slapd(8) 被设计成独立运行的服务器。它允许服务利用缓存、用潜在的数据库管理并发内容以及避免浪费系统资源。不要从 inetd(8) 中运行。

Command-Line Options（命令行选项）
slapd(8) supports a number of command-line options as detailed in the manual page. This section details a few commonly used options.
slapd(8) 支持许多命令行选项，这在手册页中有详细说明。本部门详细说明一些常用的选项。

-f <filename>

This option specifies an alternate configuration file for slapd. The default is normally /usr/local/etc/openldap/slapd.conf.
该选项为 slapd 指定另一个配置文件。默认的是通常在 /usr/local/etc/openladp/slapd.conf

译者注：Ubuntu 中为 /etc/openladp/slapd.conf

-h <URLs>

This option specifies alternative listener configurations. The default is ldap:/// which implies LDAP over TCP on all interfaces on the default LDAP port 389. You can specify specific host-port pairs or other protocol schemes (such as ldaps:// or ldapi://). For example, -h "ldaps:// ldap://127.0.0.1:666" will create two listeners: one for LDAP over SSL on all interfaces on the default LDAP/SSL port 636, and one for LDAP over TCP on the localhost (loopback) interface on port 666. Hosts may be specified using IPv4 dotted-decimal form or using host names. Port values must be numeric.
该选项指定另一个监听配置。默认是 ldap:///，这意味着 LDAP 是架构在 TCP 之上的其所有接口都是基于默认的 LDAP 端口 389。 您可以特别指定其主机-端口或其它的协议模式（如 ldaps:// 或 ldapi://）。举个例子， -h "ldaps:// ldap://127.0.0.1:666" 将创建两个监听器：一个是 架构在 SSL 之上且其所有接口都是基于默认的 LDAP／SSL 端口 636。而另一个则是架构在本机接口（环回口）之上的其接口基于 666 端口。主机可以使用 IPv4 的十进制加点的表示形式，或使用主机名。端口值必须是数字。

-n <service-name>

This option specifies the service name used for logging and other purposes. The default service name is slapd.
该选项为日志和其他目的指定服务名。默认服务名是 slapd。

-l <syslog-local-user>

This option specifies the local user for the syslog(8) facility. Values can be LOCAL0, LOCAL1, LOCAL2, ..., and LOCAL7. The default is LOCAL4. This option may not be supported on all systems.
本选项为 syslog(8) 指定一个本地用户。其值可以是 LOCAL0、LOCAL1、LOCAL2...到 LOCAL7。缺省值为 LOCAL4。本选项可能不会被所有系统支持。

-u user -g group

These options specify the user and group, respectively, to run as. user can be either a user name or uid. group can be either a group name or gid.
这些选项分别指定用户和用户组来运行。用户可以是用户名或 uid。用户组也可以是组名或 gid。

-r directory

This option specifies a run-time directory. slapd will chroot(2) to this directory after opening listeners but before reading any configuration files or initializing any backends.
本选项指定一个运行时目录。slapd 在打开监听器之后读任何配置文件或初始化任何后台之前将 chroot(2) 该目录。
-d <level> | ?

This option sets the slapd debug level to <level>. When level is a `?' character, the various debugging levels are printed and slapd exits, regardless of any other options you give it. Current debugging levels are
本选项在 <level>中设置 slapd 的调试级别。当级别是 '?' 字符时，将打印出各种调试级别并退出 slapd，而不管您所给的其它任何选项。当前调试级别是：

Table: Debugging Levels （调试级别）
Level（级别）	Description（说明）
0	no debugging（没有调试）
1	trace function calls（跟踪函数调用）
2	debug packet handling（跟踪包处理）
4	heavy trace debugging（大量跟踪调试）
8	connection management（连接管理）
16	print out packets sent and received（打印出发送和接收的包）
32	search filter processing（搜索过滤器处理过程）
64	configuration file processing（配置文件处理过程）
128	access control list processing（访问控制列表处理过程）
256	stats log connections/operations/results（连接／操作／结果 统计日志）
512	stats log entries sent（条目发送统计日志）
1024	print communication with shell backends（使用 shell 后台打印通讯）
2048	print entry parsing debugging（打印条目解析调试过程）

You may enable multiple levels by specifying the debug option once for each desired level. Or, since debugging levels are additive, you can do the math yourself. That is, if you want to trace function calls and watch the config file being processed, you could set level to the sum of those two levels (in this case, -d 65). Or, you can let slapd do the math, (e.g. -d 1 -d 64). Consult <ldap_log.h> for more details.
您可以在调试选项中一次性为所有所需级别启用多个调试级别。或者，由于调试级别是可以相加的，因此您也可以自己加出来。也就是说，如果您想跟踪函数调用同时又要查看正在处理的配置文件，那么您可以设置该级别为这两个级别之和（在本例中是-d 65）。或者，您可以让 slapd 自己计算（如 -d 1 -d 64）。更多细节请参阅 <ldap.h>。

Note: slapd must have been compiled with -DLDAP_DEBUG defined for any debugging information beyond the two stats levels to be available.
注意：为了使得两个统计级别以外的调试信息生效，slapd 必须要在编译时定义 -DLDAP_DEBUG。

Starting slapd（开始 slapd）

In general, slapd is run like this:
通常使用如下命令运行 slapd：

/usr/local/etc/libexec/slapd [<option>]*

where /usr/local/etc/libexec is determined by configure and <option> is one of the options described above (or in slapd(8)). Unless you have specified a debugging level (including level 0), slapd will automatically fork and detach itself from its controlling terminal and run in the background.
/usr/local/etc/libexec 是由配置脚本所决定的，而 <option> 则是上述选项之一（或在 slapd(8)中描述）。除非您已经指定了一个调试级别（包括级别 0），否则 slapd 将会自动从它的控制终端派生和分离出它自己并在后台运行。

Stopping slapd（停止 slapd）

To kill off slapd safely, you should give a command like this
要安全停止 slapd，您需要给出如下命令：

kill -INT `cat /usr/local/var/slapd.pid`

where /usr/local/var is determined by configure.
/usr/local/var 目录的位置是由配置脚本决定的。

Killing slapd by a more drastic method may cause information loss or database corruption.
用更强制的方法停用 slapd 也许会引起信息丢失或数据库不正确。

[OpenLDAP Documents](http://wiki.ubuntu.org.cn/OpenLDAPAdminGuide/RunningSlapd)
