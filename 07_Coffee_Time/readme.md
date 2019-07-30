# Coffee Time

* Category: Reversing

## Description

Run this jar executable in a virtual machine and see what happens. [coffeetime.jar](coffeetime.jar)

## Solution

OK, so we got a jar executable.
As you may know, JARs are just ZIPped Java classe packed with some other metadata files.
And as you may know Java classes can be easily decompiled to get back the original source code.
We don't even need to run the executable, let's take the other path instead.

Let's check its content:

```bash
$ unzip -l coffeetime.jar 
Archive:  coffeetime.jar
  Length      Date    Time    Name
---------  ---------- -----   ----
      167  2019-07-22 01:52   META-INF/MANIFEST.MF
        0  2019-07-22 01:52   META-INF/
        0  2019-07-22 01:52   coffeetime/
     2480  2019-07-22 01:52   coffeetime/CoffeeTime.class

---[cut]---
```

For the moment what interests us is just "CoffeeTime.class" 
(since the other classes are from libraries, 
but if you want to be sure you can check the MANIFEST.MF: you will find that: `Main-Class: coffeetime.CoffeeTime` )

Let's extract it:

```bash
$ unzip -j coffeetime.jar coffeetime/CoffeeTime.class
Archive:  coffeetime.jar
  inflating: CoffeeTime.class
```

(-j option is used to avoid creating directories)

Now, at this point we can already win the challenge by searching for strings:

```bash
$ strings CoffeeTime.class 
[...]
peaCTF{nice_cup_of_coffee}
[...]

```

If we are courious we can fully decompile the Class to see what the executable does:

```bash
$ jad CoffeeTime.class 
Parsing CoffeeTime.class...The class file version is 49.0 (only 45.3, 46.0 and 47.0 are supported)
 Generating CoffeeTime.jad
```

I won't spoil you the surprise ;)



