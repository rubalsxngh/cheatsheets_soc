## Kape

- kroll artifact parser and extracter. rather than manually searching for atificats, kape automates the process of searching and extracing the artifiacts.
- target and modules: targets are artifacts and modules are programs to collect the artifacts
- follows two rounds on target: first round, tries to copy all files it can if no OS lock and in second round, perform raw reads on files with OS locks
- kape copies the files with original timestamps
- once copied, files go through different modules. 

- source -> kape targets-> destination (where to store the output)-> kape module options -> module outputs.

- kape is an executable binary, we need not to install, can run from network drive or usb

### kape targets

- targets and compound targets
- stored in targets repo of kape
- target directory consists of multiple .tkape files, which have paths and masks of the files needed to be collected
- !disbaled hold the files we want in kape instance but not activly look for it and !local for our use if we dont wan't to sync it with kape github repo

### kape modules

- collection of program that executes on targets to collect the information
- consists of .bin, which consists of Eric Zimmerman's binaries
- contains lot of windows utilities
- .mkape extension

