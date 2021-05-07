#Perform voice activity detection (VAD) using WebRTC's implementation.

To perform VAD for all WAV files under the directory ``wav_dir/`` and write
the output to the directory ``vad_dir/`` as HTK label files:

    python get_vad.py --wav_dir wav_dir/ --output_dir vad_dir/

For each file with the ``.wav`` extension under ``wav_dir/``, there will now be
a corresponding label file with the extension ``.sad`` under ``vad_dir/``. Each
label file will contain one speech segment per line, each consisting of three
space-delimited fields:

- onset  --  the onset of the segment in seconds
- offset --  the offset of the segment in seconds
- label  --  the label for the segment; controlled by the ``--speech_label`` flag

If ``--output_dir`` is not specified, these files will be output to ``wav_dir/``.

Alternately, you may specify the files to process via a script file of paths to
WAV files with one path per line:

    /path/to/file1.wav
    /path/to/file2.wav
    /path/to/file3.wav
    ...

This functionality is enabled via the ``-S`` flag, as in the following:

   python main_get_vad.py -S some.scp --output_dir vad_dir/

which will perform VAD for those file listed in ``some.scp`` and output label files
to ``vad_dir. Note that if you use a script file, you *MUST* specify an output
directory.

WebRTC exposes several parameters for tuning it's output, which may be adjusted via
the following flags:

- ``--fs_vad``  --  controls the sample rate the audio is resampled to prior to
  performing VAD; possible values are 8 kHz, 16 kHz, 32 kHz, and 48 kHz
- ``--hoplength``  --  the duration in milliseconds of the frames for VAD; possible
  values are 10 ms, 20 ms, and 30 ms
- ``--mode``  --   the WebRTC aggressiveness mode, which controls how aggressive
  WebRTC is about filter out non-speech; 0 is least aggressive and 3 most aggressive

Optionally, label smoothing may be applied to the output of WebRTC to eliminate short,
irregular silences and speech segments. Label smoothing is done using a median filter
applied to the frame-level labeling produced by WebRTC and is controlled by the 
``--med_filt_width`` parameter.

When processing large batches of audio, it may be desireable to parallelize the
computation, which may be done by specifying the number of parallel processes to
employ via the ``--n_jobs`` flag:

   python get_vad.py --n_jobs 40 -S some.scp --output_dir vad_dir/

References
----------
- https://github.com/wiseman/py-webrtcvad.git
- https://webrtc.org/
