# Video Podcast Maker Demo：用 AI 工作流產出 OpenClaw 介紹影片

![](./192547.png)

這個 repo 用來展示 **video-podcast-maker** 的影片生成流程，以及一支實際產出的 OpenClaw 介紹 demo。

本 repo 只保留兩個重點：

```text
README.md
openclaw-intro.mp4
```

也就是：

- `README.md`：說明 video-podcast-maker 的用途、流程與本次 demo 的限制
- `openclaw-intro.mp4`：最後產出的 demo 影片

---

## video-podcast-maker 專案

本 demo 使用的工具是：

https://github.com/Agents365-ai/video-podcast-maker

**video-podcast-maker** 是一個用來產生「旁白型知識影片」的工作流工具。

它的核心概念是：

> 先把主題整理成 podcast 腳本，再產生語音、字幕、時間軸，最後交給 Remotion 製作影片畫面。

換句話說，它不是單純的影片剪輯器，也不是只產生一段 TTS 音訊。  
它比較像是一個把「主題」轉成「可播放影片」的自動化 pipeline。

---

## 它解決什麼問題？

如果要手動做一支知識型影片，通常會經過很多步驟：

```text
想主題
→ 寫腳本
→ 配音
→ 對字幕
→ 切章節
→ 做畫面
→ 對時間軸
→ 輸出影片
```

這些步驟分散在不同工具裡，很容易變成重複勞動。

video-podcast-maker 的價值是把這些流程串起來，讓產出流程變成：

```text
Topic
→ podcast.txt
→ podcast_audio.wav
→ podcast_audio.srt
→ timing.json
→ Remotion video
→ MP4
```

---

## 核心工具：Remotion

這個 workflow 的核心畫面生成工具是 **Remotion**。

Remotion 可以理解成：

> 用 React 來製作影片。

也就是說，它不是傳統剪輯軟體，而是用類似前端開發的方式，把影片畫面寫成 React component。

例如：

```text
React component
+ frame number
+ animation logic
+ audio / subtitle timing
= video frame
```

因此，Remotion 很適合製作：

- 技術解說影片
- 資訊圖動畫
- 流程圖動畫
- 字幕型知識影片
- 程式化生成的影片模板

---

## Remotion 原本的限制

Remotion 本身很適合做動畫畫面，但如果要做一支完整影片，還會遇到幾個問題：

```text
配音要怎麼產生？
字幕要怎麼對齊？
每個段落的時間軸怎麼抓？
畫面要怎麼跟旁白同步？
音樂、音訊、字幕要怎麼整合？
```

也就是說，Remotion 解決的是「影片畫面生成」問題。

但如果要做一支完整的旁白型影片，仍然需要自己額外處理：

- 腳本
- TTS 配音
- 字幕
- timing
- 音訊同步
- 後製或剪輯軟體整合

---

## video-podcast-maker 如何補上缺口？

**video-podcast-maker** 的核心價值，是補上 Remotion 在「旁白型影片製作」上的兩個缺口：

1. **語音製作**
2. **內容腳本生成**

Remotion 主要負責畫面與動畫，但語音、字幕、時間軸本來需要額外處理。  
video-podcast-maker 先把語音這一段流程串起來，讓影片可以自動產生：

```text
podcast_audio.wav
podcast_audio.srt
timing.json
```

語音問題解決後，另一個大問題就是 **內容腳本要怎麼生成**。

這部分 video-podcast-maker 也一併透過 `SKILL.md` 解決。  
它把影片製作流程拆成明確的 agent workflow，讓 coding agent 可以按照步驟完成：

```text
主題定義
→ 資料研究
→ 內容整理
→ podcast 腳本產生
→ TTS
→ 字幕與 timing
→ Remotion 畫面
→ render
```

因此，它不只是「幫 Remotion 加聲音」，而是把完整的旁白型影片 pipeline 包成一個可被 agent 執行的流程。

流程大致是：

```text
主題
→ SKILL workflow
→ 內容腳本 podcast.txt
→ TTS 語音 podcast_audio.wav
→ 字幕 podcast_audio.srt
→ section timing / timing.json
→ Remotion 根據時間軸製作動畫
→ render 成 MP4
```

換句話說，video-podcast-maker 讓影片製作流程變成：

> 先透過 SKILL 產出內容腳本，再產生語音與字幕，最後讓 Remotion 根據時間軸製作畫面。

這樣 Remotion 就不只是單純做動畫，而是可以根據旁白內容與時間軸，產生比較完整的影片。

---

## 主要產物

video-podcast-maker 通常會產出以下檔案：

| 檔案 | 用途 |
|---|---|
| `podcast.txt` | 影片旁白腳本 |
| `podcast_audio.wav` | TTS 產生的旁白音訊 |
| `podcast_audio.srt` | 對應音訊的字幕 |
| `timing.json` | 每個 section 的時間區間 |
| `phonemes.json` | 語音輔助資料，可用於更細緻的動畫或字幕同步 |
| Remotion component | 用 React / Remotion 組出影片畫面 |
| `.mp4` | 最終輸出的影片 |

---

## 使用到的技術

### 1. video-podcast-maker

負責整體影片產出流程。

它會協助整理主題、產生腳本、生成 TTS 音訊、字幕與 timing，並讓後續 Remotion 可以依照這些素材做畫面。

更精準地說，它的核心不是單一工具指令，而是一套 `SKILL.md` 定義的影片製作工作流。  
這個 SKILL 讓 agent 知道應該如何從主題開始，逐步完成研究、腳本、語音、字幕、時間軸與 Remotion 影片生成。

### 2. MiniMax M2.7

這次 demo 實際使用的是 **MiniMax M2.7** 作為 coding agent 背後的模型。

需要特別說明的是，這和 video-podcast-maker 原本較推薦或常見搭配的 **GPT、Claude、Gemini** 等較高能力模型不同。  
因此，實際生成品質、畫面設計能力、Remotion component 穩定度、動畫規劃能力，可能會和官方示範或使用高階模型時有差異。

本 demo 可以視為：

> 使用 MiniMax M2.7 跑 video-podcast-maker workflow 的實測結果。

也就是說，這個 demo 展示了流程可以被跑通，但不代表 video-podcast-maker 在更強模型下的最佳產出上限。

### 3. Edge TTS

用來產生中文旁白音訊。

這次 demo 使用繁體中文語音，讓影片可以直接用旁白形式呈現，而不是單純顯示文字。

### 4. Remotion

Remotion 是用 React 產生影片的框架。

在這個 workflow 中，Remotion 的角色是把：

```text
音訊
字幕
section timing
React 視覺元件
```

組合成可以播放與輸出的影片。

---

## 整體流程

這次 demo 的產出流程可以概括成：

```text
1. 定義影片主題
2. 透過 video-podcast-maker 的 SKILL workflow 產生 podcast 腳本
3. 確認腳本內容與長度
4. 使用 Edge TTS 產生旁白
5. 產生 SRT 字幕與 timing.json
6. 使用 Remotion 建立影片畫面
7. 在 Remotion Studio 預覽
8. Render 成 MP4
```

---

## Demo 主題：OpenClaw

這次實際產出的 demo 是一支 OpenClaw 介紹影片。

影片主題是：

> OpenClaw 是什麼？  
> 為什麼它不是單一 Coding Agent，而更像 Agent routing / orchestration layer？

影片內容介紹了：

- OpenClaw 的定位
- 為什麼單一 Agent 在任務變多時容易混亂
- Agent、Route、Binding、Tool、Agent Team 等核心概念
- 多 Agent 工作流的基本想法
- OpenClaw 和一般 Coding Agent 的差異

---

## Demo 影片

請查看 repo 中的影片檔案：

```text
openclaw-intro.mp4
```

這支影片展示了 video-podcast-maker 的完整產出流程：

- 從主題生成旁白腳本
- 使用 TTS 產生語音
- 自動產生字幕
- 依照 timing 安排影片段落
- 用 Remotion 產生動態畫面
- 最後輸出成 MP4

---

## 這個 demo 想展示什麼？

這個 demo 的重點不是單純介紹 OpenClaw，而是展示：

> video-podcast-maker 可以把一個技術主題，轉成一支有旁白、有字幕、有時間軸、有畫面的影片。

也就是從：

```text
一個主題
```

變成：

```text
一支完整影片
```

中間包含腳本、語音、字幕、時間軸與畫面生成。

---

## 實測觀察：MiniMax M2.7 生成版本

這支影片是目前使用 **MiniMax M2.7** 生成十幾版後，效果相對比較好的一版。

這版相對比較能呈現 OpenClaw 的運作方式，例如：

- 多個 Agent 的分工
- 任務如何流入系統
- Route / Binding 如何把任務導向不同 Agent
- OpenClaw 和單一 Coding Agent 的差異

不過，這次 demo 的生成過程中，使用的是 **MiniMax M2.7**，而不是 GPT、Claude、Gemini 這類 video-podcast-maker 原本較推薦或常見搭配的模型。

因此產出結果有幾個需要注意的地方：

- 腳本與 TTS 流程可以順利完成
- Remotion component 可以產生可播放影片
- 但動畫設計、畫面節奏、scene 對齊與整體視覺穩定度，和更高能力模型可能有差距
- 實際製作時仍需要人工多次預覽與修正
- 若使用更強的 coding model，理論上 Remotion 視覺設計與一次生成成功率可能會更好

所以這個 repo 比較適合作為：

```text
video-podcast-maker workflow 實測 demo
```

而不是：

```text
video-podcast-maker 最佳視覺品質展示
```