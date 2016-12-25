## physmem

<!-- Brandon Azad -->

physmem is a physical memory inspection tool and local privilege escalation targeting macOS up
through 10.12.1. It exploits either [CVE-2016-1825] or [CVE-2016-7617] depending on the deployment
target. These two vulnerabilities are nearly identical, and exploitation can be done exactly the
same. They were patched in OS X El Capitan [10.11.5] and macOS Sierra [10.12.2], respectively.

[CVE-2016-1825]: https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2016-1825
[CVE-2016-7617]: https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2016-7617
[10.11.5]: https://support.apple.com/en-us/HT206567
[10.12.2]: https://support.apple.com/en-us/HT207423

Because these are logic bugs, exploitation is incredibly reliable. I have not yet experienced a
panic in the tens of thousands of times I've run a program (correctly) exploiting these
vulnerabilities.

### CVE-2016-1825

CVE-2016-1825 is an issue in IOHIDevice which allows setting arbitrary IOKit registry properties.
In particular, the privileged property IOUserClientClass can be controlled by an unprivileged
process. I have not tested platforms before Yosemite, but the vulnerability appears in the source
code as early as Mac OS X Leopard.

### CVE-2016-7617

CVE-2016-7617 is an almost identical issue in AppleBroadcomBluetoothHostController. This
vulnerability appears to have been introduced in OS X El Capitan. It was reported by Ian Beer of
Google's Project Zero (issue [974]) and Radu Motspan.

[974]: https://bugs.chromium.org/p/project-zero/issues/detail?id=974

### Building

Build physmem by specifying your deployment target on the command line:

    $ make MACOSX_DEPLOYMENT_TARGET=10.10.5

### Running

You can read a word of physical memory using the read command:

    $ ./physmem read 0x1000
    a69a04f2f59625b3

You can write to physical memory using the write command:

    $ ./physmem write 0x1000 0x1122334455667788
    $ ./physmem read 0x1000
    1122334455667788

You can exec a root shell using the root command:

    $ ./physmem root
    sh-3.2# whoami
    root

### License

The physmem code is released into the public domain. As a courtesy I ask that if you reference or
use any of this code you attribute it to me.