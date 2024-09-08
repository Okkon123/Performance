JVM 提供了一些常见的命令行工具，用于监控、调试、分析和管理 Java 应用程序的运行状态。以下是 JVM 常见的命令工具及其用途：

### 1. **`jps`** - Java Process Status Tool

`jps` 用于列出当前运行的 Java 虚拟机进程，类似于 Linux 的 `ps` 命令，但专门用于 Java 进程。

bash

复制

```
jps
```

常用选项：

- **`-v`**：显示传递给 JVM 的参数。
- **`-l`**：显示 Java 应用程序的完整包名。
- **`-m`**：显示传递给主类的参数。

bash

复制

```
jps -v
```

### 2. **`jstack`** - Java Stack Trace

`jstack` 用于生成 Java 进程的线程快照（线程栈追踪），通常用于定位线程死锁、线程阻塞、性能瓶颈等问题。

bash

复制

```
jstack <pid>
```

常用选项：

- **`-l`**：显示锁的附加信息。
- **`-F`**：强制输出线程快照。

bash

复制

```
jstack -l <pid>
```

### 3. **`jmap`** - Java Memory Map

`jmap` 用于生成 Java 进程的内存映射，能够查看堆内存使用情况、生成堆转储文件等。

bash

复制

```
jmap <pid>
```

常用选项：

- **`-heap`**：显示堆的详细信息，包括新生代、老年代、永久代的使用情况。
- **`-histo`**：显示对象的内存分布，按类列出实例数量和内存使用量。
- **`-dump`**：生成堆转储文件，用于进一步诊断内存问题。

bash

复制

```
jmap -heap <pid>
jmap -histo <pid>
jmap -dump:live,format=b,file=heapdump.hprof <pid>
```

### 4. **`jstat`** - JVM Statistics Monitoring Tool

`jstat` 用于监控 JVM 的内存、垃圾回收等运行时状态，适合进行性能分析和调优。

bash

复制

```
jstat <option> <pid> [interval] [count]
```

常用选项：

- **`-gc`**：显示垃圾收集统计信息。
- **`-gcutil`**：显示垃圾收集的使用情况（堆的使用率）。
- **`-class`**：显示类加载相关信息。

bash

复制

```
jstat -gc <pid> 1000 10    # 每秒显示一次 GC 信息，共显示 10 次
```

### 5. **`jconsole`** - Java Monitoring and Management Console

`jconsole` 是一个 GUI 工具，用于通过 JMX（Java Management Extensions）连接到 JVM 并监控内存、线程、类加载器、垃圾回收等运行时信息。

bash

复制

```
jconsole
```

当 `jconsole` 启动时，可以选择要监控的 Java 进程。

### 6. **`jvisualvm`** - Java VisualVM

`jvisualvm` 是一个图形化的 JVM 监控和分析工具，整合了监控、性能分析、内存分析、线程分析等功能。它比 `jconsole` 功能更加强大，可以用来做 CPU 和内存的快照和分析。

bash

复制

```
jvisualvm
```

功能包括：

- 查看实时的 CPU 使用率、内存使用率、线程活动等。
- 生成和分析堆转储（heap dump）。
- 分析线程快照（thread dump）。
- 动态生成和分析 CPU 性能快照。

### 7. **`jcmd`** - JVM Diagnostic Command

`jcmd` 是一个多功能工具，可以执行各种诊断命令，结合了 `jmap`、`jstack`、`jstat` 等工具的功能。它能够对 Java 进程发出命令，以进行线程转储、堆转储、垃圾回收、类加载信息等操作。

bash

复制

```
jcmd <pid> help   # 列出指定进程支持的命令
```

常用命令：

- **`jcmd <pid> GC.run`**：手动触发垃圾回收。
- **`jcmd <pid> Thread.print`**：打印线程信息（类似于 `jstack`）。
- **`jcmd <pid> VM.system_properties`**：显示系统属性。
- **`jcmd <pid> VM.uptime`**：显示应用程序的运行时间。

bash

复制

```
jcmd <pid> GC.run
jcmd <pid> Thread.print
jcmd <pid> VM.system_properties
jcmd <pid> VM.uptime
```

### 8. **`jinfo`** - Java Configuration Info

`jinfo` 用于查看和修改 JVM 运行时的配置信息，比如系统属性、JVM 参数等。

bash

复制

```
jinfo <pid>
```

常用选项：

- **`-sysprops`**：显示系统属性。
- **`-flags`**：显示 JVM 的所有启动参数。

bash

复制

```
jinfo -flags <pid>
jinfo -sysprops <pid>
```

### 9. **`javap`** - Java Class Disassembler

`javap` 用于反编译 `.class` 文件，显示字节码指令。适用于查看类的内部结构、方法签名、字节码等。

bash

复制

```
javap <class_file>
```

常用选项：

- **`-c`**：显示字节码指令。
- **`-v`**：显示详细信息。
- **`-s`**：输出内部类型签名。

bash

复制

```
javap -c MyClass
javap -v MyClass
```

### 10. **`keytool`** - Java Key and Certificate Management Tool

`keytool` 是用于管理密钥库和证书的工具。它可以创建和管理证书、密钥对，生成证书签名请求（CSR）等。

bash

复制

```
keytool -genkeypair -alias mykey -keyalg RSA -keystore mykeystore.jks
```

常用命令：

- **`-genkeypair`**：生成密钥对。
- **`-list`**：列出密钥库中的条目。
- **`-export`**：导出证书。
- **`-import`**：导入证书。

bash

复制

```
keytool -list -keystore mykeystore.jks
keytool -export -alias mykey -file mycert.cer -keystore mykeystore.jks
keytool -import -alias mycert -file mycert.cer -keystore mykeystore.jks
```

### 11. **`jar`** - Java Archive Tool

`jar` 用于创建、查看、提取 `.jar` 文件。

bash

复制

```
jar -cvf myapp.jar -C classes/ .
```

常用选项：

- **`-c`**：创建一个新的 JAR 文件。
- **`-v`**：在处理过程中显示详细信息。
- **`-f`**：指定输出的 JAR 文件名。
- **`-x`**：解压 JAR 文件。

bash

复制

```
jar -tvf myapp.jar   # 查看 JAR 文件内容
jar -xvf myapp.jar   # 解压 JAR 文件
```

### 12. **`jdb`** - Java Debugger

`jdb` 是 Java 的调试工具，可以用于调试 Java 程序，设置断点、单步执行、检查变量等。

bash

复制

```
jdb -attach <pid>
```

也可以在启动 Java 程序时指定调试参数：

bash

复制

```
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8000 -jar myapp.jar
jdb -connect com.sun.jdi.SocketAttach:hostname=localhost,port=8000
```

### 总结

这些 JVM 命令行工具可以帮助开发人员进行 Java 应用程序的监控、调试、分析和优化。根据不同的需求，可以选择合适的工具来诊断性能问题、分析内存使用、调试线程死锁等问题。