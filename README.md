FFmpeg README
=============

FFmpeg is a collection of libraries and tools to process multimedia content
such as audio, video, subtitles and related metadata.

## This Fork

This fork is an updated version of the [ossr version](https://github.com/ossrs/ffmpeg-webrtc/pull/1#usage) that adds WHIP support to FFMPEG.

## Compiling on Windows

Compiling this version on Windows with nvenc has only been successfully attempted using MSYS2. Find instructions below.

1. Install MSYS2 from https://msys2.github.io/
2. Install prerequisites in the mingw64 shell:
```shell
$ pacman -Syu
$ pacman -S \
    make \
    diffutils \
    yasm \
    nasm \
    pkg-config \
    mingw-w64-x86_64-pkg-config \
    mingw-w64-x86_64-gcc \
    mingw-w64-x86_64-opus \
    mingw-w64-x264 \
    mingw-w64-x86_64-x265
```
3. Install the [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)
4. Clone the `ffnvcodec` repository and install it in the mingw64 shell:
```shell
$ git clone https://git.videolan.org/git/ffmpeg/nv-codec-headers.git
$ cd nv-codec-headers
$ make install PREFIX=/usr
```
5. Create a folder named `nv_sdk` **in a path without spaces** and copy all files from `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\[version]\include` and library files from `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\[version]\lib\x64` to your `nv_sdk` folder.
6. Create a folder named `cuda_sdk` **in a path without spaces** and copy the two folders `bin` and `lib` from `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\[version]` to your `cuda_sdk` folder.
7. Launch a VS Developer Command Prompt, navigate to `C:\msys64` and launch `.\msys2_shell.cmd -use-full-path -mingw64`
8. Navigate to your ffmpeg directory and run configure (with adjusted paths!):
```shell
$ ./configure \
    --enable-muxer=whip \
    --enable-yasm \
    --enable-asm \
    --enable-openssl \
    --enable-version3 \
    --enable-libx264 \
    --enable-gpl \
    --enable-libx265 \
    --enable-nonfree \
    --enable-cuda-nvcc \
    --enable-libnpp \
    --enable-nvenc \
    --enable-libopus \
    --extra-ldflags="-static -static-libgcc -static-libstdc++ -L/c/Users/[...]/nv_sdk -L/c/Users/[...]/cuda_sdk/lib/x64" \
    --extra-cflags="-I/c/Users/[...]/nv_sdk -I/c/Users/[...]/cuda_sdk/include -I/c/Users/[...]/nv-codec-headers/include" \
    --enable-static \
    --disable-shared \
    --disable-w32threads \
    --pkg-config-flags="--static"
```
If this command succeeded, continue to compiling:
```shell
$ make -j8
```

Good luck.

Additional resources and guides:
- https://trac.ffmpeg.org/wiki/CompilationGuide/MinGW
- https://docs.nvidia.com/video-technologies/video-codec-sdk/12.0/ffmpeg-with-nvidia-gpu/index.html
- https://stackoverflow.com/questions/41870137/ffmpeg-error-libnpp-not-found-in-windows

## Libraries

* `libavcodec` provides implementation of a wider range of codecs.
* `libavformat` implements streaming protocols, container formats and basic I/O access.
* `libavutil` includes hashers, decompressors and miscellaneous utility functions.
* `libavfilter` provides means to alter decoded audio and video through a directed graph of connected filters.
* `libavdevice` provides an abstraction to access capture and playback devices.
* `libswresample` implements audio mixing and resampling routines.
* `libswscale` implements color conversion and scaling routines.

## Tools

* [ffmpeg](https://ffmpeg.org/ffmpeg.html) is a command line toolbox to
  manipulate, convert and stream multimedia content.
* [ffplay](https://ffmpeg.org/ffplay.html) is a minimalistic multimedia player.
* [ffprobe](https://ffmpeg.org/ffprobe.html) is a simple analysis tool to inspect
  multimedia content.
* Additional small tools such as `aviocat`, `ismindex` and `qt-faststart`.

## Documentation

The offline documentation is available in the **doc/** directory.

The online documentation is available in the main [website](https://ffmpeg.org)
and in the [wiki](https://trac.ffmpeg.org).

### Examples

Coding examples are available in the **doc/examples** directory.

## License

FFmpeg codebase is mainly LGPL-licensed with optional components licensed under
GPL. Please refer to the LICENSE file for detailed information.

## Contributing

Patches should be submitted to the ffmpeg-devel mailing list using
`git format-patch` or `git send-email`. Github pull requests should be
avoided because they are not part of our review process and will be ignored.
