# Media player refactoring kata in C++

[![CI](https://github.com/Coding-Cuddles/media-player-refactoring-cpp-kata/actions/workflows/main.yml/badge.svg)](https://github.com/Coding-Cuddles/media-player-refactoring-cpp-kata/actions/workflows/main.yml)
[![C++17](https://img.shields.io/badge/C%2B%2B-17-blue.svg)](https://en.cppreference.com/w/cpp/17)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## Overview

This kata complements [Clean Code: SOLID, Ep. 12 - Interface Segregation Principle](https://cleancoders.com/episode/clean-code-episode-12).

The exercise involves refactoring a multimedia player system to adhere to this
principle.

The problem involves different types of media: audio, video, and images. We
start with a monolithic `IMediaPlayer` interface that handles all types of
media. This will be your starting point.

## Instructions

### Exercise 1

In the first part, your task is to refactor the existing code such that each
type of media player (audio, video, and image) has its own specific interface,
instead of the monolithic `IMediaPlayer` interface. You should create
`IAudioPlayer`, `IVideoPlayer`, and `IImagePlayer` interfaces, each with a
relevant method, and update the `AudioPlayer`, `VideoPlayer`, and `ImagePlayer`
classes to implement these new interfaces.

This part includes unit tests that ensure each type of media player is
functioning correctly. After you have completed your refactoring, all unit
tests should pass.

### Exercise 2

In the second part, we will deal with the compatibility of different players
with different file types.

Before, we had a separate player for each media type. We want to have media
files that come in different formats (e.g., `.mp3`, `.flac`, `.wav` for audio,
`.jpeg`, `.png` for images, and `.mp4`, `.mkv` for videos), and some players can
only handle certain formats.

We have the `MediaFile` class to represent a media file. Your task is to:

1. Update the player interfaces to take `MediaFile` objects, e.g.:

   ```cpp
   class IAudioPlayer {
   public:
       virtual void play_audio(const MediaFile& file) = 0;
   };
   ```

2. Create specialized players that can only handle certain formats (i.e.,
   `Mp3Player`, `FlacPlayer`, `WavPlayer`).

   ```cpp
   class Mp3Player : public IAudioPlayer {
   public:
       void play_audio(const MediaFile& file) override {
           if (file.format != "mp3")
               throw std::invalid_argument("Invalid file format for Mp3Player!");

           // Implementation...
       }
   };
   ```

   The same kind of specialization will be done for `FlacPlayer`, `WavPlayer`,
   and respective video and image players.

3. Add corresponding unit tests, e.g.:

   ```cpp
   TEST(AudioPlayerTest, Mp3PlayerHandlesMp3)
   {
       Mp3Player mp3_player;
       MediaFile mp3_file;
       mp3_file.format = "mp3";
       EXPECT_NO_THROW(mp3_player.play_audio(mp3_file));
   }

   TEST(AudioPlayerTest, Mp3PlayerRejectsNonMp3)
   {
       Mp3Player mp3_player;
       MediaFile flac_file;
       flac_file.format = "flac";
       EXPECT_THROW(mp3_player.play_audio(flac_file), std::invalid_argument);
   }
   ```

### Exercise 3

In the third part, we introduce the concept of a `MediaListPlayer`. This class
accepts a list of media files and a corresponding list of players. It checks
whether the player is compatible with the media file format before trying to
play/display the file.

For the `MediaListPlayer`, we can update the `play_list` method to take a list
of `IAudioPlayer`, `IVideoPlayer`, and `IImagePlayer` instead of
`IMediaPlayer`.

In the `play_list` method, we should use the appropriate player based on the
type of the media file. This may require additional checks or mappings from
file type to player.

Your task is to refactor the code to segregate interfaces based on the
different file formats and adapt the `MediaListPlayer` to work with the new
classes and interfaces.

Refactor the C++17 implementation without changing its existing behavior.
Setup is complete when all four existing tests pass.

## Prerequisites

Required:

- [Git](https://git-scm.com/downloads)
- A compiler with C++17 support. Choose one:
  - [GCC](https://gcc.gnu.org/) 10+ on Linux
  - [LLVM Clang](https://llvm.org/) 14+ on Linux
  - [Apple Clang](https://developer.apple.com/xcode/) 17+ on macOS
  - [MSVC](https://visualstudio.microsoft.com/) 2022 on Windows
- [CMake 3.24 or later](https://cmake.org)

Optional:

- [GNU Make](https://www.gnu.org/software/make/), for shorter commands. Every
  required task also has direct CMake and CTest commands. Make may be
  unavailable on Windows.

You do not need to install GoogleTest separately. CMake finds an installed
copy or downloads the pinned release when needed.

## Set up the kata

1. Clone the repository:

   ```console
   git clone https://github.com/Coding-Cuddles/media-player-refactoring-cpp-kata.git
   ```

2. Enter the repository directory:

   ```console
   cd media-player-refactoring-cpp-kata
   ```

3. Build and run the tests. Use Make when it is installed:

   ```console
   make test
   ```

   Otherwise, use CMake and CTest directly:

   ```console
   cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
   cmake --build build --config Debug
   ctest --test-dir build --build-config Debug --output-on-failure
   ```

The first run may download and build GoogleTest. CTest should report
`100% tests passed` with four passing tests. If a command reports a missing
compiler or CMake, install that prerequisite and run the setup commands again.
Setup is complete when CTest reports `100% tests passed`.

## Work on the kata

Work through the three exercises in order. Refactor `media_player.h` and add
the corresponding tests to `test_media_player.cpp` as each exercise requires.

After each change, use Make when it is installed:

```console
make test
```

Otherwise, use CMake and CTest directly:

```console
cmake --build build --config Debug
ctest --test-dir build --build-config Debug --output-on-failure
```

Continue when CTest reports `100% tests passed`.

## Make command reference

Make is optional. Run `make` or `make help` to list these commands in the
terminal.

| Command             | Result                                    |
| ------------------- | ----------------------------------------- |
| `make all`          | Build and run the test suite              |
| `make help`         | List public Make targets                  |
| `make build`        | Configure and build without running tests |
| `make test`         | Build and run the test suite              |
| `make format`       | Format tracked C++ and header files       |
| `make format-check` | Check formatting without changing files   |
| `make clean`        | Remove generated build artifacts          |
