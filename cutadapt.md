删除原有文件夹，在家目录下重新解压缩

```
tar zxvf cutadapt-1.11.tar.gz
```
进入cutadapt-1.11文件夹，运行


```
python setup.py install
```
出现错误


```
running install
error: can't create or remove files in install directory

The following error occurred while trying to add or remove files in the
installation directory:

    [Errno 13] Permission denied: '/usr/lib64/python2.6/site-packages/test-easy-install-43166.write-test'

The installation directory you specified (via --install-dir, --prefix, or
the distutils default setting) was:

    /usr/lib64/python2.6/site-packages/

Perhaps your account does not have write access to this directory?  If the
installation directory is a system-owned directory, you may need to sign in
as the administrator or "root" account.  If you do not have administrative
access to this machine, you may wish to choose a different installation
directory, preferably one that is listed in your PYTHONPATH environment
variable.

For information on other options, you may wish to consult the
documentation at:

  http://peak.telecommunity.com/EasyInstall.html

Please make the appropriate changes for your system and try again.

```
考虑到之前安装软件经常是由于环境变量问题导致的，查看天河二号手册第13页，以此操作


```
module avail
```
查看到了最新的python版本，加载之


```
module load Python/3.5.1-icc
```
在运行
```
python setup.py install
```
