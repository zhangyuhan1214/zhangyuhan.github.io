Bugku_POST writeup
题目描述：
打开网页，可以看到当what的值等于flag，页面就打印flag，POST参数不能直接写在URL地址栏。

解题步骤：
1. 访问靶场链接，阅读页面给出的PHP代码。
2. 打开cmd命令行，输入curl POST指令，提交what=flag。
3. 执行命令，返回的响应内容中出现flag。

难点：按win+R，输入curl -X POST -d "what=flag" http://160.202.254.160:11521

总结：
这道题和GET不同，GET参数放在URL，POST参数放在数据包主体，需要单独构造POST请求提交数据。

flag{aa956c51ea8bc982278b6ffe194c5b52}
