---
layout: post
title: Remote Debugging in Java
---

It’s a brief tutorial how to debugging remotely in Java by attaching to running jvm.

```python
-Xdebug
-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=10092
```

-Xdebug: Enables debugging support in the VM<br />
jdwp: Java Debug Wire Protocol<br />
suspend=n : Allows jdb to connect running jvm any time<br />
address: Any process to debug will be connected via the port number written in the address..<br />

We configured our program to accepts debugging requests. The remaining part is about how to connect it. I use IntellijIDEA as Java IDE , so the following part contains IntellijIDEA configuration for remote debugging.

![alt text](/images/remote_debugging.png "Logo Title Text 1")

On Run/Debug configuration windows, you can add new Remote Configuration as on the image above. Port should be same with the address on the jvm options. I attached a process that runs locally on my machine, you can override it.

__Sources:__
* http://download.oracle.com/otn_hosted_doc/jdeveloper/904preview/jdk14doc/docs/tooldocs/windows/jdb.html
