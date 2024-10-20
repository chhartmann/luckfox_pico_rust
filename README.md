A template for programming the https://www.luckfox.com/Luckfox-Pico with Rust.
It includes a build container to be able to directly start with one click in github codespaces.

The elf file can simply be started in the dev container with qemu-arm and is configured in the tasks settings for vscode.
Debugging with gdb is also possible with qemu-arm and is configured in the lauch settings for vscode.

Note: The standard library provided with the default image provided with the Luckfox-Pico default image is not compatible with Rust.
A image and toolchain with a newer standard library can be build with the Luckfox-Pico-SDK. 
This is done in https://github.com/chhartmann/luckfox-pico where the image can be downloaded from the release artifacts.
