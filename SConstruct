#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import os
import copy
SetOption("random", 1)

# 使用环境变量，CCFLAGS表示的是编译器选项，LINKFLAGS表示的是需要链接的库文件，CPPPATH表示的是程序编译时需要查找的头文件的路径
env = Environment(CCFLAGS='-fpermissive -g -O2 -pthread -std=c++11', LINKFLAGS='-pthread -Wl,--start-group', CPPPATH=["#src", "#depend/rapidjson/include", "#depend"])

# compile cryptopp
env.Command(None, None, "cd depend/cryptopp && make")

# 将静态库fly放入LIBS中
env.Replace(LIBS=[File('depend/cryptopp/libcryptopp.a')])
Export("env")

# 调用SConscript，将编译产生的静态库放在build/fly目录下
fly = SConscript("src/SConscript", variant_dir="build/fly", duplicate=0)
# 将编译产生的静态库放到build/bin目录下
env.Install("build/bin", fly)

# 将静态库fly放入LIBS中，后面test_server编译会使用到fly静态库
env.Append(LIBS=fly)
Export("env")

# # 表示将test_server 放到build目录下
test_server = SConscript("test/SConscript1", variant_dir="build/test_server", duplicate=0)
env.Install("build/bin", test_server)

test_client = SConscript("test/SConscript2", variant_dir="build/test_client", duplicate=0)
env.Install("build/bin", test_client)

test_server_wsock = SConscript("test/SConscript3", variant_dir="build/test_server_wsock", duplicate=0)
env.Install("build/bin", test_server_wsock)
