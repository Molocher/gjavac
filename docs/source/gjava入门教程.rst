入门教程
========

开发流程
--------

.. image:: image/img.png

通常我们在测试链上进行开发调试，当开发测试完成后，再部署到正式链使用。
你需要什么：
1.用于开发Java的IDE开发环境
2.依赖的jar包 gjavac.jar
3.编译工具 uvm_ass.exe go_package_gpc.exe
4.正式链账户及一定数量的代币

1.开发环境
-----------

1.安装Java开发IDE
2.新建Java项目
3.导入依赖jar包
    以idea为例：
        1.在项目中新建一个文件夹来存放jar包，一般习惯文件夹名字为libs
        2.右键你需要添加的jar包，选择Add as Library
        3.在弹出的Create Library 窗口上点击OK就可以了
4.开发自己的合约

2.第一个合约
---------

.. contents:: example/*

3.编译
------
1.Java源码编译成字节码
    javac -Djava.ext.dirs={依赖的gjavac.jar} {源码文件夹} -d {输出目录}
2.Java字节码编译成.ass和.json文件
    进入字节码根目录，执行一下命令
    java -classpath "{依赖的gjavac.jar};" gjavac.MainKt {字节码文件列表} "-o"  "输出目录"
3.生成.out文件
    uvm_ass.exe .ass
4.生成.gpc文件
    go_package_gpc.exe -package -binary-code=.out -meta=.json
5.通过rpc形式注册合约上链
    curl -X POST -d "{\"id\": 1,\"method\": \"register_contract\", \"params\": [\"kevin\", 0.00000001, 50000,\"{gpc文件地址}\"]}"
6.合约部署成功，可以调用合约中方法
    1.调用非上链方法
    invoke_contract_offline {调用者名称} {合约地址} {调用的合约方法} "{合约方法的参数列表}"
    2.调用上链方法
    invoke_contract {调用者名称} {gas价格} {gas最大步数} {调用的合约地址} {调用的合约方法} "{调用的合约方法的参数列表}"
    .. note::

        gas 价格，即单步执行花费的 XWC 金额，最少为 0.00001
        gas 最大步数，如果实际执行步数小于该限制，按照实际收取费用，如果大于该限制，调用失败

4.开始用Java编写智能合约
    通过以上简单的例子我们有了一个直观的感受。智能合约的开发过程非常简单。而开发实际的应用则需要复杂的业务逻辑，也可能要用到更为复杂的数据结构和控制流程。这里可以参考具体的语法参考文档。
    .. _a link: https://doc.xwc.com/