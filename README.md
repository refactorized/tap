# refactorized homebrew taps

** current status: I have no idea what I am doing **

This repo is for homebrew formulae I am trying to assemble and maybe maintain.
It probably won't go anywhere, but I will save my efforts here if it does

## structure

- `local` directory is for installations, code pulls, etc that are part of
manual investigations and not under source control.

## flam3

see also: https://github.com/Homebrew/legacy-homebrew/pull/34431

I'm going to try to create a similar formula with everything needed to get this
up and running as of 2018.  Then add it to this tap, as it's no good for core,
seeing as the flam3 repo is not 'active' per-say.

any way, step one: record shell commands here that lead to successful manual
installation:

~~$`brew install libz libpng libjpeg libxml2`~~

doesn't work - apparently libz is zlib, which is a dep for libpng anyway,
see http://brewformulas.org/Zlib

```
brew install libpng libjpeg libxml2
```

seemed to work, lib-xml is keg-only, which is probably fine (
  but unless pointed to by the final formula, it's also redundant)

- [ ] *TODO:* check libxml dependency, handle accordingly

now building via instructions at https://github.com/scottdraves/flam3

```
cd local
git clone git@github.com:scottdraves/flam3.git
cd flam3
./configure
make # generates one warning about floating point abs val function used on int
make install # trying without sudo - seems like it worked!
mkdir test
cd test
flam3-render < ../test.flam3
```

*it works!* woot, now to make it a formula...

## how about qosmic?

https://github.com/bitsed/qosmic

```
brew install qt lua libjpeg-turbo
```

also requires libpng libxml2 and flam3 itself, all previously installed

### qt caveats:

We agreed to the Qt opensource license for you.
If this is unacceptable you should uninstall.

This formula is keg-only, which means it was not symlinked into /usr/local,
because Qt 5 has CMake issues when linked.

If you need to have this software first in your PATH run:
  `echo 'export PATH="/usr/local/opt/qt/bin:$PATH"' >> ~/.bash_profile`

For compilers to find this software you may need to set:
    `LDFLAGS:  -L/usr/local/opt/qt/lib`
    `CPPFLAGS: -I/usr/local/opt/qt/include`

```
cd local
git clone git@github.com:bitsed/qosmic.git
cd qosmic
export PATH="/usr/local/opt/qt/bin:$PATH"
```

then edit `qosmic.pro` - do nothing?

```
qmake
make
```
