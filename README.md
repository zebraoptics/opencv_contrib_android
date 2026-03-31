# opencv_contrib_android

Pre-built OpenCV 4.12 Android SDK with full `opencv_contrib` modules included.

## Why this exists

The official [OpenCV Android releases](https://opencv.org/releases/) ship only the standard OpenCV modules. If you need contrib modules — such as `aruco`, `charuco`, `xfeatures2d`, `tracking`, or any other extra — you have to compile OpenCV from source yourself, which involves setting up CMake, the Android NDK, and the contrib repository, and wrestling with build configuration. This repo does that work for you.

## What's included

- OpenCV **4.12** built for Android
- **All `opencv_contrib` modules** compiled in
- Ready-to-use `sdk/` directory — drop it straight into Android Studio
- Supports standard Android device ABIs (arm64-v8a, armeabi-v7a)

## Requirements

- Android Studio (Hedgehog or later recommended)
- Android NDK `27.0.12077973`
- Min SDK: `API 21`

## Installation

### 1. Clone or download this repo

```bash
git clone https://github.com/zebraoptics/opencv_contrib_android.git
```

### 2. Copy the sdk folder into your Android project

```
your-android-project/
├── app/
├── sdk/            ← copy here from this repo
│   ├── native/
│   └── java/
└── ...
```

### 3. Add the sdk as a module in Android Studio

In Android Studio: **File → New → Import Module**, then point to the `sdk/` directory.

### 4. Add the dependency in your app's `build.gradle`

```groovy
dependencies {
    implementation project(':sdk')
}
```

### 5. Load the library in your code

```kotlin
// Kotlin
System.loadLibrary("opencv_java4")
```

```java
// Java
System.loadLibrary("opencv_java4");
```

## Usage example — ChArUco detection with contrib

```kotlin
import org.opencv.objdetect.ArucoDetector
import org.opencv.objdetect.CharucoBoard
import org.opencv.objdetect.DetectorParameters

// Create a 5x7 CharucoBoard with DICT_4X4_50
val dictionary = Objdetect.getPredefinedDictionary(Objdetect.DICT_4X4_50)
val board = CharucoBoard(Size(5.0, 7.0), 0.04f, 0.02f, dictionary)

// Detect markers
val detector = ArucoDetector(dictionary, DetectorParameters())
// ... pass your Mat frame to detector
```

> These contrib APIs are unavailable in the standard OpenCV Android release — that's exactly the gap this build fills.

## Build environment

This SDK was compiled with the following setup. If you want to rebuild from source:

| Component | Version |
|-----------|---------|
| OpenCV | 4.12 |
| opencv_contrib | 4.12 |
| Android NDK | `<!-- TODO: fill in -->` |
| CMake | `<!-- TODO: fill in -->` |
| Host OS | `<!-- TODO: e.g. macOS 14 / Ubuntu 22.04 -->` |

### Build command used

```bash
# Clone sources
git clone https://github.com/opencv/opencv.git --branch 4.12
git clone https://github.com/opencv/opencv_contrib.git --branch 4.12

# Configure
cmake -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules \
      -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_NATIVE_API_LEVEL=24 \
      -DBUILD_ANDROID_PROJECTS=ON \
      ..

# Build
make -j$(nproc)
```

> Adjust `ANDROID_ABI` and `ANDROID_NATIVE_API_LEVEL` to match your target devices.

## Related project

**[android-calibration-pattern](https://github.com/zebraoptics/android-calibration-pattern)** — A companion Android app (Kotlin) that displays checkerboard and ChArUco patterns on screen with adjustable row/column counts. Useful for camera calibration without printing a physical target.

## License

OpenCV and opencv_contrib are licensed under the [Apache 2.0 License](https://github.com/opencv/opencv/blob/master/LICENSE).
This repository (build scripts and documentation) is released under MIT.

## Contributing

Issues and PRs welcome — especially if you've tested on specific device ABIs or NDK versions not listed here.
