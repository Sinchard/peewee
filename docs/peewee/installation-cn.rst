.. _installation:

安装和测试
======================

大多数人都会通过PyPI来便捷的安装最新版本：

.. code-block:: console

    pip install peewee

如果系统中存在CPython，那么Peewee将附带安装几个C的扩展包。

* Sqlite扩展, 包含SQLite的日期处理、正则表达式、全文搜索结果排序算法的CPython实现。

通过git安装
-------------------

该项目位于https://github.com/coleifer/peewee，可以通过git来安装:

.. code-block:: console

    git clone https://github.com/coleifer/peewee.git
    cd peewee
    python setup.py install

.. note::
    在部分操作系统中，可能需要使用``sudo python setup.py install``命令将peewee安装
    到系统目录下。

如果想通过git checkout来安装SQLite扩展，您可以:

.. code-block:: console

    # Build the C extension and place shared libraries alongside other modules.
    python setup.py build_ext -i


运行测试
-------------

您可以通过运行测试套件来检查安装是否正确。

.. code-block:: console

    python runtests.py

您可以使用“runtests.py”脚本来测试Peewee的特定功能或特定的数据库驱动程序。可以
使用如下命令来查看可用的测试运行程序选项：

.. code-block:: console

    python runtests.py --help

.. note::
    如果要在Postgres或MySQL数据库运行测试，需要手动创建一个名为“peewee_test”的
    数据库。要测试Postgres扩展模块，还需要在Postgres测试数据库中安装HStore扩展:

    .. code-block:: sql

        -- install the hstore extension on the peewee_test postgres db.
        CREATE EXTENSION hstore;


可选依赖项
---------------------

.. note::
    由于大多数的Python发行版都支持SQLite，所以在使用Peewee时通常不需要标准库之外的
    其他东西。可以通过在Python控制台中运行“import sqlite3”进行测试是否支持SQLite。
    如果你想使用其他的数据库，有很多DB-API2.0兼容的驱动程序，比如MySQL的“pymysql”，
    Postgres的“psycopg2”。

* `Cython <http://cython.org/>`_: 使用SQLite的附加功能，以更高效的方式实现搜索
  结果排名。由于生成的C文件包含在包发行版中，因此Cython不再需要使用C扩展。
* `apsw <https://github.com/rogerbinns/apsw>`_: 可选的第三方SQLite库，为SQLite
  的C语音API提供了更高的性能和全面的支持。 使用 :py:class:`APSWDatabase`.
* `gevent <http://www.gevent.org/>`_ is an optional dependency for
  :py:class:`SqliteQueueDatabase` (though it works with ``threading`` just
  fine).
* `BerkeleyDB <http://www.oracle.com/technetwork/database/database-technologies/berkeleydb/downloads/index.html>`_ can
  be compiled with a SQLite frontend, which works with Peewee. Compiling can be
  tricky so `here are instructions <http://charlesleifer.com/blog/updated-instructions-for-compiling-berkeleydb-with-sqlite-for-use-with-python/>`_.
* 最后，如果使用*Flask*框架，Peewee中还包含对应的helper模块.


关于SQLite扩展的说明
-----------------------------

Peewee包含两个特定于SQLite的C扩展，它们为SQLite数据库用户提供了额外的功能和改进
的性能。Peewee将提前确定是否安装了SQLite3，并且仅在系统上有SQLite共享库可用时才
构建SQLite扩展。

但是，如果在尝试安装Peewee时收到如下错误，可以通过设置``NO_SQLite``环境变量显式
禁用SQLite C扩展的编译。

.. code-block:: console

    fatal error: sqlite3.h: No such file or directory

下面展示如何在显式禁用SQLite扩展的情况下安装Peewee：

.. code-block:: console

    $ NO_SQLITE=1 python setup.py install
