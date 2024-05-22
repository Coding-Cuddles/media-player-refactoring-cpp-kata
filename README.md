# Media Player Refactoring C++ Kata

[![CI](https://github.com/Coding-Cuddles/media-player-refactoring-cpp-kata/actions/workflows/main.yml/badge.svg)](https://github.com/Coding-Cuddles/media-player-refactoring-cpp-kata/actions/workflows/main.yml)
[![Replit](https://img.shields.io/badge/Try%20with%20Replit-black?logo=replit)](https://replit.com/new/github/Coding-Cuddles/media-player-refactoring-cpp-kata)

## Overview

This kata complements [Clean Code: SOLID, Ep. 12 - Interface Segregation Principle](https://cleancoders.com/episode/clean-code-episode-12).

The exercise involves refactoring a multimedia player system to adhere to this
principle.

The problem we have at hand involves different types of media -- audio, video,
and images. We start with a monolithic `IMediaPlayer` interface that handles
all types of media. This will be your starting point.

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
`.jpeg`, `.png` for images, and `.mp4`, `.mkv` for videos). And some players
can only handle certain formats.

We have the `MediaFile` class to represent a media file, your task is to:

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
accepts a list of media files and a corresponding list of players. It checks if
the player is compatible with the media file format before trying to
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

## Usage

You can import this project into [Replit](https://replit.com),
and it will handle all dependencies automatically.

### Prerequisites

* [CMake 3.19+](https://cmake.org)
* [Ninja](https://ninja-build.org)
* [GTest](https://github.com/google/googletest)

### Build

```console
make build
```

### Run main

```console
make run
```

### Run tests

```console
make test
```
