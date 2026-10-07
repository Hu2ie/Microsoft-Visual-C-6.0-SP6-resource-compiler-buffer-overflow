# Microsoft Visual C++ 6.0 SP6 Resource Compiler Buffer Overflow — .rc File Exploit

**Vulnerability discovered and exploit developed by porkythepig**

---

## Overview

Proof-of-concept exploit for a **buffer overflow vulnerability** in the resource compiler (`rc.exe`) shipped with **Microsoft Visual C++ 6.0 Service Pack 6**. The vulnerability is triggered when the resource compiler processes a specially crafted `.rc` (resource script) file. Successful exploitation may allow an attacker to execute arbitrary code in the context of the user running `rc.exe` — typically during the build process of a project.

> ⚠️ **Disclaimer:** This material is provided for **educational and security research purposes only**. Do not use it against systems you do not own or have explicit permission to test. The author assumes no responsibility for misuse.

---

## Vulnerability Details

| Field | Value |
|---|---|
| **Vulnerable component** | `rc.exe` (Microsoft Resource Compiler) |
| **Affected version** | Microsoft Visual C++ 6.0 SP6 |
| **Vulnerability type** | Stack-based buffer overflow |
| **Trigger** | Malformed `.rc` resource script file |
| **Discovered by** | porkythepig |
| **Exploit author** | porkythepig |

The overflow occurs while parsing certain resource definitions inside a `.rc` file. An overly long field overwrites adjacent stack memory, allowing control of the execution flow when the compiler processes the file.

---

## Impact

- **Arbitrary code execution** in the context of the user invoking `rc.exe`
- Potential for **remote code execution** if a victim is tricked into compiling an attacker-supplied `.rc` file (e.g., as part of a shared project or build pipeline)
- **Denial of service** (crash) even without successful code execution

---

## Exploit

The exploit is delivered as a crafted `.rc` file. When compiled with the affected version of `rc.exe`, it triggers the overflow and hijacks the instruction pointer.

### Requirements
- Microsoft Visual C++ 6.0 SP6 (or the standalone `rc.exe` from that package)
- Windows environment matching the target the exploit was built for
- Debugger (e.g., OllyDbg / WinDbg) recommended for analysis

### Usage
1. Place the malicious `.rc` file in your working directory.
2. Run the resource compiler against it:
   ```
   rc.exe exploit.rc
   ```
3. Observe the overflow / code execution.

> Exact offsets, payload layout, and shellcode details depend on the target environment. Adjust the exploit parameters accordingly.

---

## Mitigation

- **Upgrade** to a modern toolchain (Visual Studio 2015+ / MSBuild). The affected `rc.exe` is legacy software from 1998 and no longer receives security updates.
- **Avoid compiling untrusted `.rc` files** from unknown sources.
- **Isolate builds** in a sandbox or disposable VM when working with untrusted project files.
- Apply **ASLR / DEP / SafeSEH** where possible (note: legacy MSVC 6.0 binaries generally lack these mitigations).

---

## References

- Original advisory / write-up by **porkythepig**
- Microsoft Visual C++ 6.0 SP6 — legacy toolchain documentation

---

## License / Notice

This exploit is released for **security research and educational use**. The author is not responsible for any damage caused by the use or misuse of this information. Always obtain proper authorization before testing.

---

*Found a typo or have additional analysis? Pull requests and issues are welcome.*
