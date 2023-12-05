# OS

## 在 Node.js 如何取得機器的ID

**Win32/64**

> It is generated during OS installation and won't change unless you make another OS updates or reinstall. Depending on the OS version it may contain the network adapter MAC address embedded (plus some other numbers, including random), or a pseudorandom number.

*   **OSX**

    uses`IOPlatformUUID`(the same Hardware UUID)

    `ioreg -rd1 -c IOPlatformExpertDevice`

> Value from I/O Kit registry in IOPlatformExpertDevice class

*   **Linux**

    uses`/var/lib/dbus/machine-id`**(can be changed by`root`**

    **but with unpredictable consequences)**

    [http://man7.org/linux/man-pages/man5/machine-id.5.html](http://man7.org/linux/man-pages/man5/machine-id.5.html)

> The /var/lib/dbus/machine-id file contains the unique machine ID of the local system that is set during installation. The machine ID is a single newline-terminated, hexadecimal, 32-character, lowercase machine ID string. When decoded from hexadecimal, this corresponds with a 16-byte/128-bit string.
>
> The machine ID is usually generated from a random source during system installation and stays constant for all subsequent boots. Optionally, for stateless systems, it is generated during runtime at early boot if it is found to be empty.
>
> The machine ID does not change based on user configuration or when hardware is replaced.

[https://www.npmjs.com/package/node-machine-id](https://www.npmjs.com/package/node-machine-id)
