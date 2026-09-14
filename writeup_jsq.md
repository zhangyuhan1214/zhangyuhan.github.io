Bugku CTF 计算题 Writeup

解题过程：
访问页面，出现加法计算题。输入框只能输入1位数字。

输入算式正确结果，点击验证得到flag。

难点：在于用F12审查元素，找到输入框标签 `maxlength="1"`，修改为`maxlength="3"`。

Flag: flag{a4ed91d70d574428fe4677b2d667f2aa}
