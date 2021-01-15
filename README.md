# Udacity C++ Capstone Project
## Audacity Export Parallelization Case Study
This project is the Option 1 choice of choosing your own project.

The goal was to parallelize the Export to MP3 functionality of the popular audio editor Audacity.
This repository was forked from Audacity and contains a prototype of the parallelization. It was found that code either deeper in the application and/or in supporting libraries does not appear to be thread-safe after all, as exception occurs when running tasks asynchronously. Therefore, to keep within the scope of this case study, tasks are created with the "deferred" option. This allows the parellization code to still be run, but performance benefits for exporting larger files would only be seen if these were resolved. 


### Build Instructions on Udacity VM
These steps assume that all files present during development are in the review environment, as dependent CMake and wxWidgets builds are used by the preparation script. If not, the instructions found in the original Audacity README should be followed to work towards a build.
1. Clone the audacity fork of wxWidgets (which has bug fixes in it) in /home/workspace with: `git clone https://github.com/audacity/wxWidgets`
2. Build wxWidgets by running the following commands from the base directory of wxWidgets (as described at: https://github.com/audacity/wxWidgets/blob/audacity-fixes-3.1.3/docs/gtk/install.md)
    ```
    mkdir buildgtk
    cd buildgtk
    ../configure --with-gtk
    make
    make install
    ldconfig
    ```
3. Clone this repository in /home/workspace with: `git clone https://github.com/GuacoIV/audacity.git`
4. From the terminal, in folder /home/workspace/audacity/build, run `./Prepare_VM_for_Build.sh`
5. Then, run the Visual Studio Code build configuration by pressing F5

### Testing the Project
Note that because TurboVNC does not provide audio from Udacity VM's, there are some debug assertions that occur when opening Audacity. These can be ignored.
To test, drag the test file "/home/workspace/audacity/This Sick Beat.wav" (or another audio file of choice) into the Audacity editor. Then export to MP3 via File > Export > Export as MP3.
In order to hear the resulting file, it should be brought to another machine via a file sharing service.

### Class Structure
Audacity starts with a very large set of existing classes. For the purposes of this project, the `ExportMP3` and added `ExportWorkerResult` classes are most relevant. The purpose of the `ExportWorkerResult` class is to provide the getter of the task `future` with multiple pieces of information. The `Mixer` and `MP3Exporter` classes are also tangentially related, as the `ExportMP3` class must now create and use one of each for each thread. For some bearings, the majority of the code changes occurred in the function `ProgressResult ExportMP3::Export`.

### Rubric Fulfillment
- The project demonstrates an understanding of C++ functions and control structures.
    - In the `Export` function, `for` loops and a `while` loop are added: lines 1834, 1991, 2012 
    - In the `Worker` function, a `while` loop with a check for when to `break` is used: lines 1727, 1733
- The project reads data from a file and process the data, or the program writes data to a file.
    - At time of writing, ExportMP3.cpp line 2030 does most of the writing of the output MP3 file, with other lines accounting for metadata, etc. Though infrastructure for the writing was in place already, it was understood that the writing to this file would need to be sequential and in a single thread or else synchronized.
- The project uses multithreading.
    - See ExportMP3.cpp lines 2000 and 2050
- A promise and future is used in the project.
    - `std::async` uses a promise and a future; see lines 2000 and 2050
- The project uses move semantics to move data, instead of copying it, where possible.
    - Since we're dealing with potentially large arrays of data, move semantics were used to move buffers and `Mixer`s: found throughout `Export` and `Worker` functions
    

=========================
[![Audacity](https://forum.audacityteam.org/styles/prosilver/theme/images/Audacity-logo_75px_trans_forum.png)](https://www.audacityteam.org) 
=========================

[**Audacity**](https://www.audacityteam.org) is an easy-to-use, multi-track audio editor and recorder for Windows, Mac OS X, GNU/Linux and other operating systems. Developed by a group of volunteers as open source.

- **Recording** from any real, or virtual audio device that is available to the host system.
- **Export / Import** a wide range of audio formats, extendible with FFmpeg.
- **High quality** using 32-bit float audio processing.
- **Plug-ins** Support for multiple audio plug-in formats, including VST, LV2, AU.
- **Macros** for chaining commands and batch processing.
- **Scripting** in Python, Perl, or any language that supports named pipes.
- **Nyquist** Very powerful built-in scripting language that may also be used to create plug-ins.
- **Editing** multi-track editing with sample accuracy and arbitrary sample rates.
- **Accessibility** for VI users.
- **Analysis and visualization** tools to analyze audio, or other signal data.

## Getting Started

For end users, the latest Windows and macOS release version of Audacity is available from the [Audacity website](https://www.audacityteam.org/download/).
Help with using Audacity is available from the [Audacity Forum](https://forum.audacityteam.org/).
Information for developers is available from the [Audacity Wiki](https://wiki.audacityteam.org/wiki/For_Developers).
