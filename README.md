# Reproduction for README collision when bundling wheels into a PEX

1. Build the two wheels:

``` term
$ pants package src::
10:52:56.64 [INFO] Wrote dist/a-project-0.1.0.tar.gz
10:52:56.64 [INFO] Wrote dist/a_project-0.1.0-py3-none-any.whl
10:52:56.64 [INFO] Wrote dist/b-project-0.1.0.tar.gz
10:52:56.64 [INFO] Wrote dist/b_project-0.1.0-py3-none-any.whl
```

2. Validate that they do not contain the READMEs:

``` term
$ zipinfo dist/a_project-0.1.0-py3-none-any.whl
Archive:  dist/a_project-0.1.0-py3-none-any.whl
Zip file size: 1206 bytes, number of entries: 5
-rw-r--r--  2.0 unx        9 b- defN 16-Jan-01 00:00 a/__init__.py
-rw-r--r--  2.0 unx       33 b- defN 16-Jan-01 00:00 a/aardvark.py
?rw-------  2.0 unx       87 b- defN 16-Jan-01 00:00 a_project-0.1.0.dist-info/WHEEL
?rw-------  2.0 unx      224 b- defN 16-Jan-01 00:00 a_project-0.1.0.dist-info/METADATA
?rw-------  2.0 unx      346 b- defN 16-Jan-01 00:00 a_project-0.1.0.dist-info/RECORD
5 files, 699 bytes uncompressed, 558 bytes compressed:  20.2%

$ zipinfo dist/b_project-0.1.0-py3-none-any.whl
Archive:  dist/b_project-0.1.0-py3-none-any.whl
Zip file size: 1203 bytes, number of entries: 5
-rw-r--r--  2.0 unx        9 b- defN 16-Jan-01 00:00 b/__init__.py
-rw-r--r--  2.0 unx       33 b- defN 16-Jan-01 00:00 b/bowling.py
?rw-------  2.0 unx       87 b- defN 16-Jan-01 00:00 b_project-0.1.0.dist-info/WHEEL
?rw-------  2.0 unx      224 b- defN 16-Jan-01 00:00 b_project-0.1.0.dist-info/METADATA
?rw-------  2.0 unx      345 b- defN 16-Jan-01 00:00 b_project-0.1.0.dist-info/RECORD
5 files, 698 bytes uncompressed, 557 bytes compressed:  20.2%
```

3. Try building the pex_binary which includes both packages:

``` term
$ pants package cmd:main
10:53:15.75 [ERROR] 1 Exception encountered:

Engine traceback:
  in `package` goal

Exception: Can only merge Directories with no duplicates, but found 2 duplicate entries in :

`README.md`: 1.) file digest=96d7f2c61ad10f92e449136b94433c482ad88ef52f8d9513dd1426c4fb3ea17e size=44:

# B's README

Dummy placeholder lorem ipsum


`README.md`: 2.) file digest=028373478b36dc388b32c231aacb2962b6530618c38ae32af9aad1e7ace29323 size=44:

# A's README

Dummy placeholder lorem ipsum
```
