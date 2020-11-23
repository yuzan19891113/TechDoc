# XLua热更路程

1. 打开LuaPefect\(nssclient\_preview/nssunityproj/luaperfect.exe
2. 去掉asset目录
3.  加上luascript目录
4. 修改lua_globalmain.lua 与lua_\_logical.lua
5. 回到unity编辑器，XLua-generate code 去compile code
6. inject to editor
7. 调试，运行游戏，查看log，是否有error, lua（解释语言\),需要运行时才能知道错误
8. 提交lua,启动蓝盾构建lua bytes单
9.  通知服务器更新对应的xlua热更bytes

