# erlscrypt: Port driver for Colin Percival's "scrypt" function

[scrypt](http://www.tarsnap.com/scrypt.html), a Password-Based Key
Derivation Function from Colin Percival, from [version
1.1.6](http://www.tarsnap.com/scrypt/scrypt-1.1.6.tgz) of his library.

For general background on what `scrypt` is, and why it's useful, see
[these slides (PDF)](http://www.tarsnap.com/scrypt/scrypt-slides.pdf)
and [Colin Percival's page on
scrypt](http://www.tarsnap.com/scrypt.html).

## Windows environment setup x64
1. Install C++ build tools from https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=15
1. Select Build Tools and C++ CLI option and execute the installation 
1. Add environment variable `INCLUDE` with value `C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Tools\MSVC\14.12.25827\include;C:\Program Files (x86)\Windows Kits\NETFXSDK\4.6.1\include\um;C:\Program Files (x86)\Windows Kits\10\include\10.0.16299.0\ucrt;C:\Program Files (x86)\Windows Kits\10\include\10.0.16299.0\shared;C:\Program Files (x86)\Windows Kits\10\include\10.0.16299.0\um;C:\Program Files (x86)\Windows Kits\10\include\10.0.16299.0\winrt;`
1. Add environment variable `LIB` with value `C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Tools\MSVC\14.12.25827\lib\x64;C:\Program Files (x86)\Windows Kits\NETFXSDK\4.6.1\lib\um\x64;C:\Program Files (x86)\Windows Kits\10\lib\10.0.16299.0\ucrt\x64;C:\Program Files (x86)\Windows Kits\10\lib\10.0.16299.0\um\x64;`
1. Add `C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Tools\MSVC\14.12.25827\bin\Hostx64\x64` to environment vairiable `PATH`
1. If while compiling there is an error while linking, execute `which link` verify if this is pointing to visual studio build tools folder. If not rename the other linker that is being fetched from the path.

## Using the library

The entry points are `erlscrypt:scrypt/6` and `erlscrypt:scrypt/7`.

## erlscrypt:scrypt([nif], Passwd, Salt, N, R, P, Buflen)

Atom `nif` can be passed as optional first parameter to gain some marginal speed over
port.

Both `Passwd` and `Salt` must be binaries. `N`, `R`, and `P` control
the complexity of the password-derivation process. `Buflen` is the
number of bytes of key material to generate.

For some good choices for `N`, `R` and `P`, see [the
paper](http://www.tarsnap.com/scrypt/scrypt.pdf).

Example:

    1> erlscrypt:scrypt(<<"pleaseletmein">>, <<"SodiumChloride">>, 16384, 8, 1, 64).
    <<112,35,189,203,58,253,115,72,70,28,6,205,129,253,56,235,
      253,168,251,186,144,79,142,62,169,181,67,246,84,...>>
    2> erlscrypt:scrypt(nif,<<"pleaseletmein">>, <<"SodiumChloride">>, 16384, 8, 1, 64).
    <<112,35,189,203,58,253,115,72,70,28,6,205,129,253,56,235,
      253,168,251,186,144,79,142,62,169,181,67,246,84,...>>

## License
Please see LICENSE for more details
