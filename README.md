# minimal-debian
Design notes and scripts for an idea for a base Debian image that takes full advantages of the Debian alternatives system to replace core software with `busybox`.

## Target Features
- `apt` package manager  available
    - minimum: `dpkg`
    - wish: `aptitude`
- `busybox` full alternatives coverage
- Minimal new packages
- New packages are virtual
- Debian alternatives system
- Fully usable 
    - Could install most packages
    - `build-essentials` would just install and run
    - `games-console` would just install and run
    - Could get to a full up desktop system like KDE with the right `apt install`
    - Simple quick server SSH and HTTP
- Full LSB, POSIX coverage
    - MTA
    - vi
    - lpr
- `busybox` based init system??
- basic client side networking (i.e. DHCP client support)
- Work with base Debian Linux kernels
- Scripts should be busybox compatible
    - can we cut python, perl, etc out of base install completely
- Is login support needed? is boot to root console satisfactory? similar to single user mode?
- Make full use of virtual filesystems (i.e TMPFS)
    - /tmp
    - /var
        - log, lock, run...
        - can logs be stored compressed in virtual memmory?
    - resizable virtual memmory file instead of requiring partitiion?
- remove docs and localizations
    - `localepurge`
    - virtual fs /usr..doc*
    - trim docs and localization packages for base image
    - make this optional in script.
    
## Guidelines and Razors
- Choose the smallest package size (including new dependencies)
- Choose the most functionality
- Don't break upstream compatibility

## Process
1. Get list of all busy box tools supported by Debian version of busybox
   - `busybox` vs `busybox-static`
2. Get list of all Debian Essential packages and packages in base minimal install
   - tools in these packages should have priority
3. Create virtual packages for each of these tools that
   - `Depends` on busybox
   - `Provides` the tools package
   - updates the Debian alternatives system with busybox link as an alternative to the tool
   - creates additional links if absolutely needed
4. Filter alternatives to Essential package
5. Create script to build filesystem image
   - `debootstrap`
   - scripted configuration updates
   - Test a couple different use case.
       - Normal system base install from which to run full install
       - Super tiny removable disk image
       - Build filesystem into kernel image as initramfs
       - As contained EFI package
6. Create virtual packages for the rest of the tools in busybox.
7. Find similar swiss-army knife tools to bring in??

## TODO
- [] Choose version of Debian
- [] Chose version of busybox
- [] get list of busybox tools
- [] get list of debian minimal install packages 


