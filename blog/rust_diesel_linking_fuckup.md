# /usr/bin/ld: cannot find -lpq

**Table of Contents**
- [the dream](#the-dream)
- [the awakening](#the-awakening)
- [the rescue](#the-rescue)
- [the (missing) enlightenment](#the-missing-enlightenment)
- [foodnotes](#foodnotes)

## the dream
Some weeks ago I wanted to use [`diesel`](https://diesel.rs) to interact with a postgres database and hit a stupid issue which took me several hours to solve. 😢 <br>
I hope with this blogpost I can spare this time for a couple of folks.

Diesel needs the respective client library to interact with the database, which is `libpq` in our case. Sice I am on Fedora 33, I ran `dnf install postgresql` which installs `psql` and `libpq` and naively thought that's it about it.

## the awakening

Next I created a small dummy project, copied some sample code into it aaand

...

got presented following error _(the line starting with `= note: "cc" ...` is quite huge. you may scroll for a bit :D)_:
```bash
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-Wl,--eh-frame-hdr" "-L" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.15qyx23rtbnzpkhv.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.1h2bb190lpy5o7xk.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.1t7gxgt8ko4i3jf9.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.22of3n6niu0rs729.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.23zr8afesuz0zyc8.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.29amucvcpf989818.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2du89wax471gw86v.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2eqpketvuy6tgu52.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2hlflhg4wc4dw7kp.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2m80at91bbersawl.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2mougiyu43xkwmhk.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.2os4z5p1zf5arfb0.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.31vb6g6avpamw1oj.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.36aje8v0f1i2uy6a.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.38sir2026jq73a39.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3bifm5opkpv7ax6f.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3doi5zu8x67hiwpf.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3kghscypmmxnv5mw.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3nr4y5qv54te3eji.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3q6pxwzlwz7a34sa.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3saov0mxa1amx1uf.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3tfukuxweg9wyv0w.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.3u33kv6lc6c42x0b.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.40lwnmn31jcqieb8.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.42zxqbhafhngf51t.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.44l4put5kz557sau.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.45ddbvkfh8wkptzh.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.46iszu7c87kawecq.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4bixf1s36vytps8f.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4c8xoeqjthyjeiy0.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4lkuucu5q1o4c0ji.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4lxdo6tkju0ooaz1.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4m4ybaxmep97qody.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4o821uo4uo00d4gi.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4pi85n4fcrpupn2z.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4qjzcb23c1a2mnaw.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4vyyo01ay7ebtqrr.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.4x8617erxjt45wwq.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.51hik61vhl7jpbsp.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.51jdcb83haoflrw7.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.523696iovye5690g.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.54i65h3yolhneg85.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.5a0rdcc102nv848.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.a7fbaa6skoti6ss.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.l2f8ucb6an1eqte.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.o906u8k3txssdeg.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.olj3z9kp2va65cr.rcgu.o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.w37jio9ytt9ui0w.rcgu.o" "-o" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b" "/home/urhengulas/Documents/local/target/debug/deps/local-2926535a094e809b.33rxsxztueut8ayl.rcgu.o" "-Wl,--gc-sections" "-pie" "-Wl,-zrelro" "-Wl,-znow" "-nodefaultlibs" "-L" "/home/urhengulas/Documents/local/target/debug/deps" "-L" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "/home/urhengulas/Documents/local/target/debug/deps/librusty_ulid-341285e3876b4301.rlib" "/home/urhengulas/Documents/local/target/debug/deps/librand-3a20c62adec95109.rlib" "/home/urhengulas/Documents/local/target/debug/deps/librand_chacha-33bf449d62fa2a9f.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libppv_lite86-954a0753d9a936e0.rlib" "/home/urhengulas/Documents/local/target/debug/deps/librand_core-7b6a7e598661465d.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libgetrandom-7d2ec23cbbf6e3ef.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libcfg_if-4c6be1962c556cf0.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libdoc_comment-4a396d197d5a85a1.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libserde-5a6984431d1b4da7.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libchrono-b345fd723829ae61.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libnum_integer-a98c2a89b245908f.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libnum_traits-b777a49b81d0f4b1.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libtime-aaf3a97d08c5b649.rlib" "/home/urhengulas/Documents/local/target/debug/deps/liblibc-e41c2d10207ee17e.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libdiesel-74a1714ea7bf99cc.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libpq_sys-2872124ef5e03260.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libbyteorder-dd535ee656aacf25.rlib" "/home/urhengulas/Documents/local/target/debug/deps/libbitflags-df474a8ca45884a8.rlib" "-Wl,--start-group" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-7edd956e9d8d05ea.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-448643d99e633b5d.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libminiz_oxide-c7c3d631a525883a.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libadler-d7659a11bb201e01.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libobject-3582800a023674d1.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libaddr2line-56ae8ed396eb3f30.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libgimli-b13a322082c79e4f.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_demangle-1d4694108f8cb543.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libhashbrown-79eaa6e630880f50.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_alloc-0df1a4ec96f80cf7.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-bba9b0f0f4d77257.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcfg_if-f848f9c987265235.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-0d5ea4f2d39b8e27.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-c22053fa5fc00a3b.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_core-c52e5d6301e1bd59.rlib" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-2675a9a46b5cec89.rlib" "-Wl,--end-group" "/home/urhengulas/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-6e3d052afeb9f711.rlib" "-Wl,-Bdynamic" "-lpq" "-lgcc_s" "-lutil" "-lrt" "-lpthread" "-lm" "-ldl" "-lc"
  = note: /usr/bin/ld: cannot find -lpq
          collect2: error: ld returned 1 exit status
          

error: aborting due to previous error

error: could not compile `local`

To learn more, run the command again with --verbose.
```

Since I am not really used to such error messages it took me a bit to digest it. But after some hours of searching and trying around I figured:
* it's probably something about linking _(well that was obvious from the first line)_
* it's related to `cc` (`gcc`), "The GNU Compiler Collection" ([man page](https://linux.die.net/man/1/gcc))
* it's related to `ld`, "The GNU linker" ([man page](https://linux.die.net/man/1/ld))
* `ld` can't find `-lpq` – whatever that is

## the rescue

Duck-duck-going, googeling, digging into github issues, reinstalling `rustc`, `cargo` and `libpq` – All of the usual tools failed me.

...

I didn't really make progress until my co-worker Markus said that I should check if the `libpq.so` (a shared object[¹](#quotes)) file exists in `/usr/lib64`.

I thought, "Obviously it should, since I installed `libpq` in the beginning", but just to make sure:
```bash
/usr/lib64$ ls -a | rg pq
lrwxrwxrwx.  1 root root      13 Sep  4 05:49 libpq.so.5 -> libpq.so.5.12
-rwxr-xr-x.  1 root root  323360 Sep  4 05:49 libpq.so.5.12
```

and I was like, "mhhh ... 🤔 ... dafuque?!".

For some reason `dnf install postgreql` doesn't create the library-file without the version suffix, which seems to cause `rustc` (or rather `ld`) a bit of a headache, while other tools like `psql` don't have an issue with this at all.

The fix in the end is very simple. Just create the additional symbolic link:
```bash
/usr/lib64$ ln -s libpq.so.5 libpq.so
/usr/lib64$ ls -la | rg pq
lrwxrwxrwx.  1 root root      10 Nov 28 22:54 libpq.so -> libpq.so.5
lrwxrwxrwx.  1 root root      13 Sep  4 05:49 libpq.so.5 -> libpq.so.5.12
-rwxr-xr-x.  1 root root  323360 Sep  4 05:49 libpq.so.5.12
```

## the (missing) enlightenment

Researching around I found an [informative answer on unix.stackexchange.com](https://unix.stackexchange.com/a/476) explaining how the versioning of the `.so`-files should work, but it doesn't really solve why it doesn't in this case.

If you know the riddles answer please enlighten me!

## foodnotes
1. A 'shared object', 'shared library', or just 'library' "is an assortment of pre-compiled pieces of code that can be reused in a program" _(cf. Kili, A. (2017, November 01). Understanding Shared Libraries in Linux. Retrieved November 28, 2020, from https://www.tecmint.com/understanding-shared-libraries-in-linux/)_