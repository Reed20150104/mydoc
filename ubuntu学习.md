## ubuntu学习
###ubuntu软件安装
1、apt方式
(1)普通安装：apt-get install softname....
(2)修复安装：apt-get -f install softname ... (-f Atemp to correct broken dependencies)
(3)重新安装：apt-get --reinstall install softname1 ...;
2、Dpkg方式
dpkg -i package_name.deb

3、源码安装
tar zxf fileName.tar.gz && cd fileName && ./configure && make && sudo make install
###ubuntu软件卸载
1.apt方式
(1)移除式卸载：apt-get remove softname ...(移除软件包，当包尾部有+时，意为安装)
(2)清除式卸载：apt-get --purge remove softname ...(同时清除配置)
  清除式卸载：apt-get purge softname ...;(同上)
2.dpkg方式
(1)移除式卸载：dpkg -r pkg1 pkg2 ...;
(2)清除式卸载：dpkg -P pkg1 pkg2...;
###ubuntu中软件包的查询方法




