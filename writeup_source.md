Bugku CTF source（ID168）超详细 Writeup

 一、题目信息
题目类型：Web 漏洞 —— Git 源码泄露
题目考点：网站未删除 .git 目录导致源码泄露、Git 历史提交回溯、恢复被修改 / 覆盖的 Flag

题目描述：
访问靶场页面仅显示简单页面内容，无直接 Flag。
网站存在 **\.git 源码泄露漏洞**，开发者多次修改、覆盖 Flag，真实 Flag 保存在 Git 历史提交记录中，需要通过 Git 历史记录回溯找到最终正确 Flag。

二、漏洞原理

Git 是代码版本管理工具，网站开发时会生成一个隐藏文件夹 .git，用来保存网站所有文件的每一次修改、删除、更新记录。

正常网站上线必须删除 .git文件夹。
本题网站未删除\.git 目录，导致攻击者可以：

1. 完整下载网站所有源码仓库

2. 查看所有历史提交记录

3. 找回被开发者修改、覆盖、替换掉的真实 Flag

三、解题环境准备（必须提前装好）

解题需要两个工具，缺一不可：

1. Python3（用于运行 git\-dumper 下载 Git 仓库）

2. Git For Windows（用于本地解析 Git 历史记录）

安装完成后，在 CMD 执行工具安装命令：

pip install git-dumper -i https://pypi.tuna.tsinghua.edu.cn/simple

四、完整标准解题步骤

步骤 1：启动 Bugku 靶场，获取靶场地址

开启题目靶场，获得完整靶场访问链接：
`http://IP:端口`

访问 `http://IP:端口/.git`
可以正常列出目录，确认 **Git 源码泄露漏洞存在**。

步骤 2：使用 git\-dumper 下载完整 Git 仓库

打开 CMD，执行下载命令，将远端 Git 仓库完整下载到本地：


git-dumper http://靶场IP:端口/.git source168

执行完成提示 Updated 2 paths from the index
代表网站所有 Git 源码、历史版本全部下载成功。

步骤 3：进入下载好的 Git 仓库文件夹

cd source168

步骤 4：查看所有历史提交记录

使用命令查看网站**全部修改记录**：

git reflog

执行后可以看到多条 commit 提交记录，所有修改 Flag 的操作都会被完整记录。

所有标注 `flag is here?` 的提交，都是开发者修改 Flag 的关键记录。

步骤 5：逐次查看历史提交内容，回溯 Flag

使用 `git show + 对应哈希值` 查看每一次文件修改详情：

1. 查看第一次 Flag 修改记录

```bash
git show fdce35e
```

本次提交将原始 Flag 修改为假 Flag。

2. 查看关键最终提交记录

```bash
git show 40c6d51
```

本次提交再次覆盖更新 Flag，产生**最终正确 Flag**。

五、解题核心逻辑

Git 每次提交会显示两种内容：

- `- 开头`：旧内容（被删掉、被替换掉的内容）

- `+ 开头`：新内容（本次提交真正写入的内容）

开发者多次覆盖假 Flag，最后一次更新的绿色新增内容，即为题目真实 Flag。

六、最终正确 Flag

```Plain Text
flag{gitis_good_distributed_version_control_system}
```

七、本题总结

1. 网站上线未删除 `.git` 目录，造成完整源码与版本历史泄露

2. 所有文件修改、删除、覆盖记录都会被 Git 永久保存

3. 即使前台无 Flag、Flag 被多次覆盖，依然可以通过 `git reflog` \+ `git show` 回溯出原始、最终文件内容

4. 是 CTF Web 基础最经典的 **Git 源码泄露题型**

八、Git 泄露通用标准解题模板

```bash
下载远端泄露的 .git 仓库
git-dumper http://目标IP:端口/.git 本地文件夹名

进入仓库目录
cd 本地文件夹名

查看全部历史提交
git reflog

查看指定提交详情，获取被覆盖/隐藏的Flag
git show 哈希值
```

> （注：部分内容可能由 AI 生成）
