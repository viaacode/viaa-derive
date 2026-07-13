# Derivative Video Script

This is a Bash shell script designed to create derivative video files from one or more input video files. It leverages `ffmpeg` and `ffprobe` to perform various video transcodings and analyses respectively on one or more source files, supporting multiple output formats such as MPEG-4, WebM, ProRes, FFV1, and frame MD5 checksum generation.

The script aims to streamline a workflow for producing high-quality video derivatives. It was created as part of a series of tests focusing on technical characteristics of FFv1 and MKV formats by meemoo (then VIAA). Through this research, meemoo gained insight into the practical usability of these new formats and identified areas that still needed further refinement. The study was conducted in collaboration with Media Area and Packed vzw (now meemoo) and fits within the Preforma project, in which meemoo participated as part of its advisory group.

**Further reading**:

- [Resultaten onderzoek ffv1 en mkv](https://kennisbank.meemoo.be/inzichten-en-praktijk/resultaten-onderzoek-ffv1-en-mkv) (Dutch)
- [VIAA Preservation Reformatting](https://www.projectcest.be/w/images/Rapport-studie-naar-het-herformatteren-van-het-viaa-archief.pdf) (English - pdf) 

## Features

- **Multiple output formats**: Supports MPEG-4, WebM, ProRes, FFV1, and frame MD5 (framemd5) outputs.
- **Option flags**: Allows specifying custom paths and additional FFmpeg input and output options.
- **Overwrite control**: Option to overwrite output files by default.
- **Reporting**: Generates detailed reports per transcoding process.
- **Format-specific handling**: Detects specific source characteristics (e.g. JPEG 2000 SD sources) and adjusts the filter chain accordingly.
- **Audio handling**: Maps the first audio stream to the output.

## Requirements

- **ffmpeg**: Compiled with `libx264`, `libvpx`, `libopus`, `flac`, and other necessary libraries for video and audio encoding.
- **ffprobe**: Must be available in the system PATH or specified via `--ffprobepath`.
- **Bash**: The script is written for Bash shell compatibility.
- **Linux/Unix Environment**: Tested on Linux environments; may require adjustments for macOS or other Unix-like systems.

## Usage

```bash
viaaderive [options] inputvideo1 [inputvideo2 ...]
```

### Options

| Option                | Description                                                                                                                                                                                                 |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-h` or `--help`     | Print this help message.                                                                                                                                                                                    |
| `-o` or `--outputs`  | List of output formats separated by `+`. Options include `mpeg4`, `webm`, `framemd5`, `prores`, `ffv1`.                                                                                                         |
| `--ffmpegpath`       | Path to the `ffmpeg` executable.                                                                                                                                                                            |
| `--ffprobepath`      | Path to the `ffprobe` executable.                                                                                                                                                                           |
| `--inputoptions`     | Additional input options for `ffmpeg`.                                                                                                                                                                      |
| `--outputoptions`    | Additional output options for `ffmpeg`.                                                                                                                                                                     |
| `-y` or `--overwrite`| Overwrite output files by default.                                                                                                                                                                          |
| `-r` or `--report`   | Output a detailed report per transcoding.                                                                                                                                                                   |
| `--ffv1`             | Enable FFV1 output.                                                                                                                                                                                          |
| `--ffv1path`         | Specify an output file for FFV1.                                                                                                                                                                            |
| `--mpeg4`            | Enable MPEG4 output.                                                                                                                                                                                         |
| `--mpeg4path`        | Specify an output file for MPEG4.                                                                                                                                                                           |
| `--prores`           | Enable ProRes output.                                                                                                                                                                                        |
| `--prorespath`       | Specify an output file for ProRes.                                                                                                                                                                           |
| `--webm`             | Enable WebM output.                                                                                                                                                                                          |
| `--webmpath`         | Specify an output file for WebM.                                                                                                                                                                             |
| `--framemd5`         | Enable frame MD5 (framemd5) output.                                                                                                                                                                         |
| `--framemd5path`     | Specify an output file for frame MD5.                                                                                                                                                                       |

**Note:** Setting any `--*path` option automatically enables the corresponding output.

### Example

```bash
./viaaderive -o mpeg4+prores+webm+ffv1 FILE1.mp4 FILE2.mp4
```

This command will generate MPEG-4, ProRes, WebM, and FFV1 derivatives for `FILE1.mp4` and `FILE2.mp4`.

## Internal Logic

1. **Initialization**: Defines default paths for `ffmpeg` and `ffprobe`, and initializes default input/output options.
2. **Argument Parsing**: Processes command-line arguments to configure outputs, paths, and options.
3. **Output Enablement**: Determines which output formats are enabled based on the `--outputs` option or user-specified flags (`--ffv1`, `--mpeg4`, etc.).
4. **Input Analysis**: Uses `ffprobe` to gather information about the input video, such as codec, resolution, and audio channel count.
5. **Filtering and Mapping**: Constructs filter chains for video and audio processing, ensuring correct channel mapping and resolution handling.
6. **Output Generation**: Executes `ffmpeg` with the configured options to produce the desired derivative files, handling paths and overwriting as specified.

## Execution Steps

1. **Prepare Environment**: Ensure `ffmpeg` and `ffprobe` are installed and accessible.
2. **Run Script**: Execute the script with desired input files and options.
3. **Verify Outputs**: Check the designated output directories for the generated derivative files.
4. **Troubleshoot**: Use the `--report` option and adjust paths/options if necessary.

## Contributing

Contributions to improve the script, add new output formats, or enhance documentation are welcome. Please follow the existing code style and ensure all modifications are tested before submitting a pull request.

## License

This script is licensed under the MIT License. See the `LICENSE` file for more details.
