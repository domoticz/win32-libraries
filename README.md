# win32-libraries
Optional libraries to compile under windows (visual studio 2019)
Unpack in msbuild folder

(See INSTALL.txt in main repository)

## Rebuilding the libraries from vcpkg

The libraries are built for x86 with the v143 toolset (the GitHub runner is
windows-2022), using the triplets in `triplets/`:

- `x86-windows-v143`: zlib openssl curl sqlite3 lua jsoncpp minizip mosquitto pthreads
  (DLLs, the import libraries go in `lib`, the DLLs in `Redist`)
- `x86-windows-static-v143`: boost-asio boost-exception boost-thread boost-signals2
  boost-date-time boost-algorithm boost-system boost-logic boost-tuple boost-smart-ptr
  boost-optional boost-chrono (static, static CRT /MT like the Release|Win32 build)

    vcpkg install --overlay-triplets=<this repo>\triplets --x-install-root=<dir> --triplet x86-windows-v143 zlib openssl curl sqlite3 lua jsoncpp minizip mosquitto pthreads
    vcpkg install --overlay-triplets=<this repo>\triplets --x-install-root=<dir> --triplet x86-windows-static-v143 boost-asio boost-exception boost-thread ...

vcpkg's boost turns auto-linking off (BOOST_ALL_NO_LIB), so the boost libraries
are listed by name in the Release|Win32 link settings of `msbuild\domoticz.vcxproj`;
update those names (and `z.lib` and friends) when a version changes.
`include` also keeps jwt-cpp, picojson and python, and `openzwave` is not from vcpkg.
