### 编译objc4

##### 下载源码: [apple source](https://opensource.apple.com/release/macos-1101.html)

##### 解决编译错误

- `unable to find sdk 'macosx.internal'`  
  - 解决: `Build Settings -> Base SDK -> macOS`  比如我的: 文件夹 common, `Header Search Paths` 填入`$(SRCROOT)/common` 把缺少的文件放入common文件夹下

- `<sys/reason.h> not found` 
  - 解决: `下载xnu-7195.50.7.100.1, 找到bsd/sys/reason.h文件, 在工程根目录引入此文件`

- `<mach-o/dyld_priv.h>`  
  - 解决: 下载`dyld-832.7.1` 源码, `include/mach-o/dyld_priv.h` , `bridgeos(3.0) 错误全部注释掉`

- `<os/lock_private.h>`  
  - 解决: 下载`libplatform-254-*` 源码, `private/os/lock_private.h`

- `<os/base_private.h>`  
  - 解决: 下载`libplatform-126-*` 源码, `include/os/base_private.h`

- `<pthread/tsd_private.h>` 
  - 解决: 下载`libpthread-454-*` 源码, `private/pthread/tsd_private.h`

- `<System/machine/cpu_capabilities.h>` 
  - 解决: `xnu-7195.50.7.100.1` `osfmk/machine/cpu_capabilities.h`

- `<os/tsd.h>` 
  - 解决: `xnu-*` `libsyscall/os/tsd.h`

- `<pthread/spinlock_private.h>` 
  - 解决: 下载`libpthread-454-*` 源码, `private/pthread/spinlock_private.h`

- `<System/pthread_machdep.h>` 
  - 解决: 下载`libc-825-40-1` , `pthread/pthread_machdep.h`

- `<CrashReporterClient.h>` 
  - 解决: 下载`libc-825-40-1` , `include/CrashReporterClient.h`

- `<os/feature_private.h> file not found"` 
  - 解决:  `直接注释掉, 以及os_feature_enabled_simple也注释掉`

- `*dyld_fall_2020_os_versions*` 
  - 解决: `直接注释`

- `dyld_platform_version_macOS_10_11` 
  - 解决: `直接注释`

- `objc-shared-cache.h` 
  - 解决: `dyld-832.7.1` , `include/objc-shared-cache.h`

- `dyld_fall_2018_os_versions` 
  - 解决: `直接注释`

- `<os/linker_set.h>` 
  - 解决: `直接注释`

- `<Cambria/Traps.h>/<Cambria/Cambria.h>` 
  - 解决: `直接注释`

- `<kern/restartable.h>` 
  - 解决: `xnu-7195.50.7.100.1` `osfmk/kern/restartable.h`

- `_simple.h` 
  - 解决: `Libc-825.40.1` , `gen/_simple.h`

- `Block_private.h` 
  - 解决: `libclosure-78` , `Block_private.h`

- `oah_is_current_process_translated` 
  - 解决:  `直接注释`

- `CRGetCrashLogMessage` 
  - 解决: `CrashReporterClient.h 有宏定义, 所以在 Build Settings -> Preprocessor Macros 中加入：LIBC_NO_LIBCRASHREPORTERCLIENT 即可`

- `LINKER_SET_FOREACH`  
  - 解决: `直接注释`

- `<os/reason_private.h>/<os/variant_private.h>`  
  - 解决: `直接注释`

- `OrderFiles/libobjc.order` 
  - 解决: `Build Settings->Linking->Order File改为工程根目录下的libobjc.order, $(SRCROOT)/libobjc.order`

- `ld: library not found for -lCrashReporterClient` 
  - 解决: `Build Settings -> Linking -> Other Linker Flags: 删除"-lCrashReporterClient"`

- `ld: library not found for -loah` 
  - 解决: `Build Settings -> Linking -> Other Linker Flags: 删除"-loah"`

- `error: SDK "macosx.internal" cannot be located.` 
  - 解决: `Target-objc的Build Phases->Run Script里 macosx.internal 改 macosx`

---

##### 调试Target

- 新建Target, 命名zwlhc
- 添加绑定关系: Target->zwlhc->build phases: 
  - Dependencies 选择依赖objc
  - Link Binary With Librarier 选择 libobjc.A.dylib
- 新建类, 引入main.m中, 下断点即可。
  - 如遇无法下断点, 找到 Build Setting -> Enable Hardended Runtime, 设置为NO



