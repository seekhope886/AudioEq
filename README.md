# AudioEq<img width="32" height="32" alt="audioeq" src="https://github.com/user-attachments/assets/6769fdba-e0d0-4178-a988-c6d79d7bc505" />

**Audio Equalizer &amp; Real-time Spectrum Visualizer**
# AudioEq Extension for MIT App Inventor 2

![License](https://shields.io)
![Platform](https://shields.io)
![IDE](https://shields.io)

---

## 🇹🇼 繁體中文說明

`com.luckyh9h.audioeq` 是一個專為 MIT App Inventor 2 (以及 Kodular、Niotron 等衍生平台) 設計的高效能、獨立執行緒安全的**音訊等化器 (Equalizer) 與即時頻譜視覺化 (Visualizer)** 擴充元件。

本元件核心採用 Android 原生 `AudioEffect` 框架，在獨立背景執行緒中進行高速的數位訊號處理 (DSP)，算好數據後再安全切換回 AI2 UI 主執行緒，完美避免傳統 AI2 元件在處理高頻率音訊數據時產生的畫面卡頓或閃退問題。

### 🌟 核心特色
* **獨立執行緒安全 (Thread-Safe)**：所有複雜的 FFT 與 RMS 運算均在背景執行緒完成，畫面表現流暢。
* **全幅對稱波形支援**：即時回傳經簡化處理的頻譜強度清單，極易配合 Canvas 畫布繪製雙向對稱或單向跳動的專業音響特效。
* **5 頻段硬體等化器**：支援直接存取 Android 系統底層音訊硬體，自由微調超低音、中音到高音的增益值 (Gain)。
* **專利「挖 ID」反射補丁**：內建特殊函數，可直接傳入 AI2 原生 `Player` 元件，強制提取隱藏的 `AudioSessionId` 進行精準對接。

### 📥 安裝與權限設定
1. 下載本專案編譯完成的 `com.luckyh9h.audioeq.aix` 檔案。
2. 匯入至您的 MIT AI2 專案，並將 `AudioEq` 拖入 UI 設計畫布中（本元件為不可見元件）。
3. **重要圖標設定**：請在專案的 Assets（資源庫）中上傳一張名為 `audioeq.png` 的圖片，拼塊端即會顯示專屬圖標。
4. **動態權限**：請務必在 `Screen1.Initialize` 時，呼叫動態請求錄音權限積木請求 `android.permission.RECORD_AUDIO`。

### 🧱 拼塊 API 說明

#### 函數方法 (Functions)
* `Initialize(audioSessionId)`: 初始化工具。傳入播放器的 SessionID 即可啟動。
* `Release()`: 釋放硬體音訊與背景執行緒資源。關閉 APP 或更換音源前務必呼叫。
* `GetPlayerSessionId(player)`: 傳入 AI2 的 `Player` 元件，自動破解並回傳其隱藏的 ID。
* `GetNumberOfBands()`: 取得目前裝置支援的等化器頻段總數（通常為 5 個）。
* `GetCenterFreq(band)`: 取得指定頻段的中心頻率（單位：Hz）。
* `GetMinBandLevel()` / `GetMaxBandLevel()`: 取得等化器可調整的最小/最大分貝限制（通常為 -15 到 15 dB）。
* `SetBandLevel(band, level)`: 設定指定頻段的增益分貝值。
* `GetBandLevel(band)`: 取得指定頻段目前設定的增益分貝值。

#### 事件監聽 (Events)
* `OnVolumeChanged(amplitude)`: 當總音量振幅改變時即時觸發 (0 ~ 128)。
* `OnFftDataReady(fftList)`: 當頻譜數據更新時觸發，回傳各頻段強度的數字清單。

---

## 🇺🇸 English Description

`com.luckyh9h.audioeq` is a high-performance, thread-safe **Audio Equalizer & Real-time Spectrum Visualizer** extension designed for MIT App Inventor 2 and its derivative platforms (e.g., Kodular, Niotron).

Powered by the Android native `AudioEffect` framework, this extension performs heavy Digital Signal Processing (DSP) like FFT and RMS on a dedicated background thread. It safely dispatches data back to the AI2 UI main thread, completely eliminating UI freezing, lag, or crashes often caused by high-frequency audio data handling in standard extensions.

### 🌟 Key Features
* **Thread-Safe Architecture**: Heavy frequency calculations run asynchronously in the background to ensure buttery-smooth UI rendering.
* **Full-Width Symmetrical Waveform Display**: Returns clean, normalized spectrum lists optimized for drawing beautiful symmetric or single-direction equalizer bars on a Canvas.
* **5-Band Hardware Equalizer**: Directly interfaces with the Android hardware layer to fine-tune Bass, Mid, and Treble gain levels.
* **Reflection-Based Session ID Patch**: Built-in utility function to programmatically extract the hidden `AudioSessionId` from the native AI2 `Player` component for seamless synchronization.

### 📥 Setup & Permissions
1. Download the compiled `com.luckyh9h.audioeq.aix` file.
2. Import it into your MIT AI2 project and drag `AudioEq` onto your designer screen (it is a non-visible component).
3. **Icon Customization**: Upload an image named `audioeq.png` to your project's Assets to display a custom icon in the blocks editor.
4. **Runtime Permission**: You **must** request `android.permission.RECORD_AUDIO` dynamically inside the `Screen1.Initialize` block before triggering the component.

### 🧱 Blocks API Reference

#### Functions
* `Initialize(audioSessionId)`: Initializes the equalizer and visualizer. Pass a valid audio session ID to start capturing.
* `Release()`: Releases native audio resources and terminates background threads. Call this before switching audio tracks or closing the app.
* `GetPlayerSessionId(player)`: Takes an AI2 `Player` component as an argument and uses Java Reflection to retrieve its hidden `AudioSessionId`.
* `GetNumberOfBands()`: Returns the total number of equalizer frequency bands supported by the device (typically 5).
* `GetCenterFreq(band)`: Returns the center frequency for a given band index in Hz.
* `GetMinBandLevel()` / `GetMaxBandLevel()`: Returns the minimum/maximum decibel range supported by the hardware (typically -15 to 15 dB).
* `SetBandLevel(band, level)`: Sets the gain level in decibels for the specified frequency band.
* `GetBandLevel(band)`: Gets the current decibel level set for the specified frequency band.

#### Events
* `OnVolumeChanged(amplitude)`: Triggered in real time when the overall volume amplitude changes (returns values between 0 and 128).
* `OnFftDataReady(fftList)`: Triggered when raw spectrum updates are processed. Returns a YailList containing amplitude values for frequency bars.

---

## 🎨 Recommended Canvas Drawing Algorithm (Blocks Implementation)

When `when AudioEq1.OnFftDataReady(fftList)` is triggered, use the following logic to render a full-width symmetrical spectrum graph on a Canvas:

1. Call `Canvas1.Clear`.
2. Define local variables:
   * `barWidth` = `Canvas1.Width / 32`
   * `maxFftValue` = `128`
3. Add a loop block `for each i from 1 to 32 by 1`:
   * `currentMagnitude` = select list item (`list: fftList`, `index: i`)
   * `xPosition` = `(i - 1) * barWidth`
   * `scaledHeight` = `(currentMagnitude / maxFftValue) * (Canvas1.Height / 2)`
   * Set `Canvas1.LineWidth` = `barWidth`
   * Call `Canvas1.DrawLine`:
     * `x1` = `xPosition`
     * `y1` = `(Canvas1.Height / 2) - scaledHeight`
     * `x2` = `xPosition`
     * `y2` = `(Canvas1.Height / 2) + scaledHeight`

---

## 📄 License
This project is licensed under the **MIT License**. Feel free to modify, distribute, or use it commercially.

*Developed with ❤️ via NiotronIDE by luckyh9h.*
