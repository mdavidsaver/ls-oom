# Understanding the Linux Out of Memory Killer

The `ls-oom` script prints the OOM score for all processes,
as well as the adjustment values which influence it.

Ordering by increasing OOM score, and increasing preference
to be killed.

https://www.kernel.org/doc/html/latest/filesystems/proc.html#proc-pid-oom-adj-proc-pid-oom-score-adj-adjust-the-oom-killer-score

```
$ ./ls-oom |head
score   adj     scr_adj pid     name
0       0       0       100     kthrotld
0       0       0       101     irq/122-pciehp
0       0       0       102     irq/123-pciehp
0       0       0       103     irq/125-pciehp
0       0       0       104     acpi_thermal_pm
0       0       0       105     ipv6_addrconf
0       0       0       10      rcu_tasks_trace
0       0       0       114     kstrp
0       0       0       117     zswap-shrink
...
871     5       300     3248    chrome
872     5       300     3182    chrome
```

## Changes via systemd

```
systemctl edit <name>.service
```

Add

```
[Service]
OOMScoreAdjust=-500
```

This will usually create/update `/etc/systemd/system/<name>.service.d/override.conf`

Must `restart` service to see change reflected.

## Notes

- sshd

The OpenSSH daemon (sshd) automatically adjusts the score
of the main daemon process to `-1000` to prevent itself
from being reaped.

- sssd

The System Security Services Daemon (sssd) does not adjust its score.

```
systemctl edit sssd.service
systemctl edit sssd-nss.service
```
