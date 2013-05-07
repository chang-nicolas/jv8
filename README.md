# JV8

A simple way to run V8 JavaScript instances from Java.

```java
V8Runner v8 = new V8Runner();

v8.map("javaSum", new V8MappableMethod {
  @Override
  public V8Value methodToRun(V8Value[] args) {
    double sum = 0;
    for (V8Value val : args) {
      sum += val.toNumber();
    }
    return new v8.val(sum);
  }
});

double sum = v8.runJS("javaSum(6, 3, 12, 17, 4);").toNumber();

// sum == 42.0
```

Currently, only the Android platform is supported, but I intend to support major desktop platforms as well (Linux/Windows/OS X).

This **pre-alpha** software still in highly experimental stages. **Do not use this in production software.**

## Binaries
A nightly(ish) build can be downloaded [here](https://s3.amazonaws.com/static.jovianware.com/jv8/jv8.tar.gz).

The tarball should be extracted into the root of a standard android project, and contains the following:

    libs/
      jv8.jar
      armeabi/
        gdbserver # FOR DEBUG
        gdb.setup # FOR DEBUG
        libgnustl_shared.so
        libjv8.so

Note: These are debug-only builds for now; i.e. not-optimized and containing debug symbols.

## Building

### Requirements
You need to be able to build V8 for Android.
- svn
- git
- python
- More stuff I'm probably not aware of.

### Instructions
First, you must initialize and pull down the v8 git submodule:
```
# From the root of the project folder:
git submodule init
git submodule update # This will take a while.
```

Now use the (hackish) build script to create everything you need:
```
./build.sh -j4 # Use the `-h` flag to see options
```

Have a snack while you wait. This will result in 3 key files:
```
dist/
  jv8.jar
libs/armeabi/
  libgnustl_shared.so
  libjv8.so
```

There will also be GDB stuff in here; the build script does not generate production
binaries, currently, and I Do Not Recommend using this in production.
