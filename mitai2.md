[FREE] AudioEq Extension: 5-Band Hardware Equalizer & Asynchronous Real-time Spectrum Visualizer

Hello App Inventors! 👋

I am excited to share a brand new, high-performance extension called **AudioEq** (`com.luckyh9h.audioeq`), developed using NiotronIDE. This component allows you to natively analyze audio streams and modify device sound signatures in real-time.

### Why use AudioEq?
Traditionally, capturing live audio data in App Inventor causes heavy thread locking, resulting in laggy Canvas animations or abrupt app crashes. **AudioEq solves this by performing all heavy Digital Signal Processing (DSP) operations—including Fast Fourier Transform (FFT) and Root Mean Square (RMS) calculations—on an independent background thread.** It then safely routes the parsed lists back to the UI thread, giving you smooth 60FPS visualizers.

### Key Features 🌟
1. **Thread-Safe Architecture**: No main thread hogging. buttery smooth animations.
2. **Built-in Session ID Cracker**: MIT AI2's native `Player` hides its `AudioSessionId`. This extension includes a unique `GetPlayerSessionId` block that uses Java reflection to fetch that ID instantly.
3. **5-Band Hardware Equalizer Control**: Dynamically adjust Bass, Mids, and Treble directly at the Android hardware layer.
4. **Symmetrical Waveform Optimization**: Returns easy-to-map lists designed for drawing symmetric audio wave bars.

---

### Blocks API Documentation 🧱

#### 🟩 Functions
* **Initialize(int audioSessionId)**: Start tracking audio. Pass your player's audio session ID here.
* **Release()**: Terminates background worker threads and unbinds hardware listeners. Call this on screen destroy or stop.
* **GetPlayerSessionId(Component player)**: Input your native `Player` block here to safely bypass AI2 constraints and fetch the hidden audio channel number.
* **GetNumberOfBands()**: Returns total equalizer bands (usually 5).
* **GetCenterFreq(short band)**: Returns the frequency of a target band in Hz.
* **GetMinBandLevel() / GetMaxBandLevel()**: Typically returns `-15` and `15` representing the supported decibel (dB) boundaries.
* **SetBandLevel(short band, short level)**: Boost or cut a specific frequency range.
* **GetBandLevel(short band)**: Check current gain values.

#### 🟨 Events
* **OnVolumeChanged(int amplitude)**: Triggers continuously with the live volume magnitude (0-128).
* **OnFftDataReady(YailList fftList)**: Triggers as new frequencies clear the background pipeline. Ideal for Canvas looping.

---

### How to Draw a Symmetrical Spectrum Graph 🎨
Map the data into a counting loop inside the `OnFftDataReady` event block:
* Loop index `i` from `1` to `32`
* `barWidth` = `Canvas1.Width / 32`
* `scaledHeight` = `(SelectItem(fftList, i) / 128) * (Canvas1.Height / 2)`
* **Draw Line Blocks**:
  * `X1` = `(i - 1) * barWidth`,  `Y1` = `(Canvas1.Height / 2) - scaledHeight`
  * `X2` = `(i - 1) * barWidth`,  `Y2` = `(Canvas1.Height / 2) + scaledHeight`

### Requirements ⚙️
* Minimum Android Version: Android 5.0 (API 21)
* Permissions required at runtime: `android.permission.RECORD_AUDIO`

### Downloads 📥
* Please check the attachments below for the `.aix` file.
* Make sure to upload an image named `audioeq.png` to your asset library to render the component icon properly in the editor view!

Let me know if you have any feedback or if you build an awesome music app with this! 

*Built with love by luckyh9h.*
