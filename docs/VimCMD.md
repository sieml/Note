## Vim CL
    http://blog.csdn.net/niushuai666/article/details/7275406
    http://www.cnblogs.com/softwaretesting/archive/2011/07/12/2104435.html

    Linux系统$普通用户模式下:
    进入vim(进入之后的命令模式输入不要设置为半角符输入,否则会延迟):
    0.vim (直接进入)
    1.vim [filename1] [filename2]...(创建文件,如存在则打开)

    vim窗口下操作:
    1. :open file 打开新文件
    2. :split file 分上下屏幕打开一个文件
    3. :bn 切换到下一个文件
    4. :bp 切换到上一个文件
    5. :e ftp://192.1.1.2/a.txt 打开远程文件
    6. a 光标后插入
    7. i 光标前插入
    8. o 当前行之后插入一行

    9. h 左移
    10. l 右移
    11. k 上移(要有换行符)
    12. j 下移
    13. Ctrl + b 向上滚动一屏
    14. Ctrl + f 向下滚动一屏

    u 撤销（Undo）
    U 撤销对整行的操作
    Ctrl + r 重做（Redo），即撤销的撤销。
    x 删除当前字符
    yy 拷贝当前行
    y 拷贝当前字符
    p 在当前光标后粘贴,如果之前使用了yy命令来复制一行，那么就在当前行的下一行粘贴。
    v+d 即可剪切
    :e! 放弃所有修改，并打开原来文件
    Ctrl+wj 移动到下方的窗口
    Ctrl+wk 移动到上方的窗口
    :close 关闭窗口,最后一个窗口不能使用此命令，可以防止意外退出vim。

    :!command 执行shell命令
    :help or F1 显示整个帮助