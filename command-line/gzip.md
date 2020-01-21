# gzip

```bash
$ ls -lh
-rw-r--r--  1 davidfeng  staff   5.2M Jan 20 21:07 t8.shakespeare.txt
$ gzip t8.shakespeare.txt
# no output
$ ls -lh
-rw-r--r--  1 davidfeng  staff   1.9M Jan 20 21:07 t8.shakespeare.txt.gz
# original file was deleted
$ gunzip t8.shakespeare.txt.gz
# no output
# or gzip -d filename.gz
$ ls - lh
-rw-r--r--  1 davidfeng  staff   5.2M Jan 20 21:07 t8.shakespeare.txt
```

[Ref](https://linuxize.com/post/gzip-command-in-linux/)

