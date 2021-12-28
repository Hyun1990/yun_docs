main函数
=========

C程序的设计原则是把函数作为程序构成的模块。main()函数称为主函数，一个C程序总是从main()函数开始执行。

main()函数的形式
-----------------

main()函数有两种形式，分为无参数形式和带参数形式

.. code-block:: c

    int main(void) /*无参数形式*/
    {
        ...
        return 0;
    }

    int main(int argc, char *argv[]) /*有参数形式*/
    {
        ...
        return 0;
    }


main()函数的返回值
-------------------

main()函数的返回值类型是int型，return 0 表示返回给操作系统，程序正常退出。

main()函数的参数
----------------

C编译器允许main()函数没有参数或有两个参数。

* 第一个是参数是int类型，代表命令行字符串个数，被称为argc(argument count)
* 第二个参数是一个指向字符串的指针数组。命令行中的每个字符串被存储到内存中，并且分配一个指针指向它，这个指针数组被称为argv(argument value)。
  一般情况下，把程序本身的名字赋给argv[0]

.. code-block:: c

    /* test.c programming*/
    int main(int argc, char *argv[])
    {
        printf("The command line has %d arguments:\n", argc -1);

        int count = 1;
        while(--argc)
        {
            printf("%d: %s\n", count, *++argv);
            count++;
        }
        return 0;
    }

输入：

| ``$ gcc -o test test.c``
| ``$ ./test I love you``

输出：

::

        The command line has 3 arguments:
        1：I
        2：love
        3：you
