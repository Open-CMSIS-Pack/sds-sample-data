# sds-sample-data

This directory contains small SDS demo recordings. Each `*.sds.yml` file is the metadata sidecar for one logical stream: it names the stream, describes its sample rate when specified, and declares the values, image frames, audio samples, or video frames stored in the matching `.sds` payload file or files. The numbered `.sds` files contain the binary sample data for that stream.

Most examples have a single payload named `<stream>.0.sds`. A few examples intentionally use multiple payload files, such as `accelerometer.0.sds` and `accelerometer.1.sds`, to demonstrate multi-file recordings. The `Test_Out` set also includes `Test_Out.0.p.sds`, which follows the same stream metadata as the other `Test_Out` payloads while demonstrating the alternate `.p.sds` payload filename form.

## Online help

- `.sdsio.yml` format: [Utilities - SDS-Framework](https://arm-software.github.io/SDS-Framework/main/utilities.html#sdsio-control-file-sdsioyml)
- `.sds.yml` format: [Theory of Operation - SDS-Framework](https://arm-software.github.io/SDS-Framework/main/theory.html#sds-metadata-format)
- `.sds` binary format: [Theory of Operation - SDS-Framework](https://arm-software.github.io/SDS-Framework/main/theory.html#sds-data-files)

## Repository configuration

`sds-sample-data.sdsio.yml` is the repository-level SDS I/O configuration for these samples. While each `*.sds.yml` file describes one stream, this file describes how an SDS I/O tool should find and interpret the collection as a whole:

- `interface.usb` declares USB as the intended I/O interface for tools that need an interface configuration.
- `workdir: .` tells tools to read and write payload files relative to this directory.
- `metadir: .` tells tools to find stream metadata files in this directory as well.
- `flag-info` gives human-readable names to SDS flag bits 0 through 7, demonstrating how flag metadata can be attached at the repository level.

## Demo recordings

| Metadata file | What it represents and demonstrates |
| --- | --- |
| `accelerometer.sds.yml` | A 100 Hz, 3-axis MEMS accelerometer stream. The metadata defines signed 16-bit `x`, `y`, and `z` values scaled by 0.001 G, so this example demonstrates calibrated integer sensor samples split across multiple SDS payload files. |
| `adc_raw.sds.yml` | A 1000 Hz raw ADC channel. It demonstrates a simple high-rate scalar stream with one unsigned 16-bit `adc` value and no unit or scale metadata. |
| `camera_nv12.sds.yml` | A 1 Hz NV12 YUV image test pattern. The metadata declares 128 x 96 frames with a 384-byte stride, demonstrating planar or semi-planar camera image payload metadata. |
| `camera_raw8.sds.yml` | A 2 Hz RAW8 grayscale image test pattern. It demonstrates single-channel image frame metadata for 128 x 96 frames with a 384-byte stride. |
| `camera_rgb565.sds.yml` | A 1 Hz RGB565 image test pattern. It demonstrates packed 16-bit color image data for 128 x 96 frames with a 256-byte stride. |
| `camera_rgb888.sds.yml` | A 1 Hz RGB888 image test pattern. It demonstrates 24-bit color image metadata for 160 x 120 frames with a 480-byte stride. |
| `CameraIn.sds.yml` | A 25 fps video recording with a resolution of 416 × 416 pixels in RGB888 format, captured by the Alif AppKit-E7 camera and scaled for a machine learning traffic counting application. |
| `counter.sds.yml` | A 50 Hz hardware event counter. It demonstrates a single unsigned 32-bit `count` value for monotonically changing or event-count style data. |
| `Detections.sds.yml` | Vehicle detector machine learning traffic counting application results for each camera frame recorded in `CameraIn.0.sds`. Each record contains a frame counter, vehicle count, NPU and total processing times in microseconds, and up to 16 detection boxes with coordinates, scores, and vehicle classes. Unused box entries are zero. |
| `empty.sds.yml` | An empty 100 Hz recording with `x` and `y` float channels. It demonstrates that a stream can have valid metadata and a payload file while containing zero records. |
| `gpio.sds.yml` | A 10 Hz digital GPIO state stream. It demonstrates compact digital state capture with one unsigned 8-bit `state` value. |
| `gyroscope.sds.yml` | A 200 Hz, 3-axis MEMS gyroscope stream. The metadata defines float `wx`, `wy`, and `wz` angular-rate values in dps, demonstrating unit-bearing floating-point sensor data. |
| `large_block.sds.yml` | A 1 Hz stream of large payload records, described as 1 KB each. It demonstrates block-style binary payload records using an unsigned 8-bit `payload` value definition. |
| `mic_16bit.sds.yml` | A mono 16-bit PCM microphone recording. The SDS record rate is 10 Hz, while the audio content metadata declares 16 kHz, 16-bit, 1-channel PCM audio, demonstrating embedded audio payload metadata. |
| `mic_stereo.sds.yml` | A stereo 16-bit PCM recording. The SDS record rate is 10 Hz, while the audio content metadata declares 44.1 kHz, 16-bit, 2-channel PCM audio, demonstrating multi-channel audio payload metadata. |
| `multi_frame.sds.yml` | A 100 Hz sensor with multiple frames per record. It declares signed 32-bit `ch1` and `ch2` channels, demonstrating records that group multiple values or frames together. |
| `no_metadata.sds.yml` | A deliberately sparse 10 Hz stream with an empty description and an empty unit on a float `channel1` and `channel2` value. It demonstrates how readers handle minimal or missing descriptive metadata while still having a typed channel definition. |
| `precision.sds.yml` | A 10 Hz high-precision voltage measurement. It declares a 1 MHz custom tick frequency and one double-precision `measurement` value in volts, demonstrating custom timestamp resolution and high-precision numeric data. |
| `single_record.sds.yml` | A 100 Hz recording with exactly one float `value` record. It demonstrates the smallest non-empty scalar recording case. |
| `temperature.sds.yml` | A 1 Hz ambient temperature sensor. It demonstrates a simple low-rate float stream with a `temp` value in degrees Celsius. |
| `Test_In.sds.yml` | A 16.6 kHz test input stream with empty units on unsigned 16-bit `x`, `y`, and `z` values. It demonstrates a three-channel integer input dataset spread across multiple payload files. |
| `Test_Out.sds.yml` | A 1 kHz test output stream with an empty units on unsigned 16-bit `x` and `y` values. It demonstrates a two-channel integer output dataset spread across multiple payload files, including the `.p.sds` payload filename variant. |

Together, these files cover common SDS use cases: scalar sensor channels, calibrated units and scale factors, sparse metadata, empty and single-record recordings, multi-file recordings, custom tick frequency, large binary blocks, audio payloads, still image payloads, and raw video payloads.

## Converted recordings

The `converted/` directory contains human-readable or playable versions of the SDS recordings. Value streams are converted to CSV, audio streams to WAV, and image streams to MP4. Each output keeps the source file's stream name and label; for example, `accelerometer.0.sds` becomes `converted/accelerometer.0.csv`, and `Test_Out.0.p.sds` becomes `converted/Test_Out.0.p.csv`. The matching `*.sds.yml` file supplies the value layout or media format used for conversion.
