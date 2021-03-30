# File & Disk Tools

## Remove all Comments from File
```bash
sed -e '/^#/d' <file>
```

## Split a File
This will not split single lines:
```bash
split --line-bytes=<size> <file_to_split>
```

The size argument is an integer and optional unit (example: 10K is
10\*1024). Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,… (powers
of 1000).

## Un-Split a File
```bash
cat <file_to_split>-part-* > <file_to_split>
```

## Remove Duplicates
```bash
sort <file> | uniq -u > <new_file>
```

## Delete Disk
```bash
shred -vn 1 /dev/<device>
```
