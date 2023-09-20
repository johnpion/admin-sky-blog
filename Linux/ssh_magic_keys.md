# Sending Magic Keys through SSH

There might be situations when a server hangs, and you don't have KVM/IPMI access or physical access to it, but you urgently need to perform a hard reboot - what can you do in such a case?

If the server was nearby, you could send Alt+Prtn+(R,E,I,S,U,B) manually.

Solution:
```shell
echo 1 > /proc/sys/kernel/sysrq
echo b > /proc/sysrq-trigger
```
Drawback: In this case, the filesystem is not properly unmounted.

This method proved helpful when an engineer had to travel for two hours to reach a secured area to reboot a server. The hardware issue was subsequently resolved.
