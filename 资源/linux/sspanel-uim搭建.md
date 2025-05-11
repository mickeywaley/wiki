研究 https://github.com/Anankke/SSPanel-Uim 搭建的笔记文件

产考资料：

SSPanel-Uim 最新可用前端

https://github.com/Anankke/SSPanel-Uim

SSPanel-Uim 配套后端

https://github.com/Anankke/shadowsocks-mod

交流电报群

https://t.me/sspanel_uim

-----------------------------------------------
cd /www/wwwroot/ss.aq520.com

cd /www/wwwroot/ss.lizhenglin.com

git clone -b dev https://github.com/Anankke/SSPanel-Uim.git ${PWD}

git config core.filemode false

wget https://getcomposer.org/installer -O composer.phar

php composer.phar

php composer.phar install

chmod -R 755 ${PWD}

chown -R www:www ${PWD}

ln -s ${PWD}/sql/glzjin_all.sql /www/backup/database/

git clone -b master https://github.com/Anankke/SSPanel-Uim.git ${PWD}
-----------------------------------------------
#选一个就可以dev原版，master分支

git clone -b master https://gitee.com/mickeywaley/shadowsocks-mod.git ${PWD}
-----------------------------------------------
0x35 配置网站程序

cd /www/wwwroot/ss.aq520.com/

cd /www/wwwroot/ss.lizhenglin.com/

cp config/.config.example.php config/.config.php

cp config/appprofile.example.php config/appprofile.php

nano config/.config.php

30 22 * * * php /www/wwwroot/ss.lizhenglin.com/xcat sendDiaryMail

0 0 * * * php -n /www/wwwroot/ss.lizhenglin.com/xcat dailyjob

*/1 * * * * php /www/wwwroot/ss.lizhenglin.com/xcat checkjob

*/1 * * * * php /www/wwwroot/ss.lizhenglin.com/xcat syncnode
-----------------------------------------------
0x36 创建管理员并同步用户

依次执行以下命令：

php xcat User createAdmin

php xcat User resetTraffic

php xcat Tool initQQWry

php xcat Tool initdownload
