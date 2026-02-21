# 🎙️ Podcast Transcriber with Speaker Diarization

Apple PodcastのURLを貼るだけで、**話者分離つき文字起こし**テキストを自動生成する Google Colab ノートブックです。

## ✨ 特徴

- **URLを貼って▶を押すだけ** — Apple PodcastのURLから音声取得〜文字起こし〜話者分離まで全自動
- **完全無料** — Google Colab（T4 GPU）+ Hugging Face の無料枠で動作
- **高精度な日本語対応** — faster-whisper (large-v3) による文字起こし
- **話者分離** — pyannote.audio で「誰が話しているか」を自動識別
- **テキストファイル出力** — そのままブログ記事の下書きやAI要約に活用可能

## 📄 出力イメージ

```
[00:00] SPEAKER_00:
  皆さんこんにちは。今日のゲストは...

[00:15] SPEAKER_01:
  よろしくお願いします。今日はAIの活用について
  お話しできればと思います。

[01:02] SPEAKER_00:
  なるほど、具体的にはどういった分野で...
```

## 🚀 使い方

### 事前準備（初回のみ・5分）

1. **Hugging Face アカウント作成**（無料）
   - https://huggingface.co/join でアカウント作成
   - 確認メールのリンクをクリック

2. **アクセストークン発行**
   - https://huggingface.co/settings/tokens → 「Create new token」（Read権限）

3. **3つのモデルの利用規約に同意**（それぞれのページで「Agree and access repository」をクリック）
   - [pyannote/segmentation-3.0](https://huggingface.co/pyannote/segmentation-3.0)
   - [pyannote/speaker-diarization-3.1](https://huggingface.co/pyannote/speaker-diarization-3.1)
   - [pyannote/speaker-diarization-community-1](https://huggingface.co/pyannote/speaker-diarization-community-1)

### 実行手順

1. [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Bun7/podcast-transcriber/blob/main/Podcast_Transcribe.ipynb)をクリック

2. **GPU を有効化**: メニュー → `ランタイム` → `ランタイムのタイプを変更` → **T4 GPU**

3. **STEP 1** を実行（パッケージインストール） → 完了後 `ランタイム` → `セッションを再起動する`

4. **STEP 2** でHugging Faceトークンを設定（Colabシークレット 🔑 に `HF_TOKEN` として保存推奨）

5. **STEP 3** でポッドキャストURLと設定を入力

6. **STEP 4** の▶を押して実行 → 完了後 txt ファイルをダウンロード

## ⚙️ 設定オプション

| パラメータ | デフォルト | 説明 |
|-----------|-----------|------|
| `PODCAST_URL` | `""` | Apple PodcastのエピソードURL（空欄でファイルアップロード） |
| `WHISPER_MODEL` | `"large-v3"` | Whisperモデル（`medium` / `large-v3`） |
| `LANGUAGE` | `"ja"` | 音声の言語 |
| `NUM_SPEAKERS` | `None` | 話者数（`None`=自動検出、`2`=対談など） |

## 📊 処理時間の目安

T4 GPU + large-v3 モデルの場合：

| 音声の長さ | 処理時間 |
|-----------|---------|
| 15分 | 約2〜3分 |
| 30分 | 約3〜5分 |
| 1時間 | 約6〜12分 |

※初回はWhisperモデル（約3GB）のダウンロードで+数分

## 🔧 トラブルシューティング

### numpy の ImportError が出る
→ STEP 1 実行後に **セッションを再起動** してから STEP 2 以降を実行してください。

### 403 Forbidden / GatedRepoError
→ Hugging Face で**3つのモデルすべて**に利用規約の同意が必要です。特に `speaker-diarization-community-1` を忘れがちです。

### torchvision::nms does not exist
→ STEP 1 に以下を追加：
```
!pip install -q torchvision --force-reinstall
```
追加後、セッションを再起動してください。

### ValueError: requested chunk ... resulted in N samples
→ MP3のサンプル数不整合です。本ノートブックでは音声をWAV (16kHz mono) に変換して回避しています。

### 話者分離の精度が低い
→ `NUM_SPEAKERS` に正確な話者数を指定してください。BGMが多い番組は精度が下がる傾向があります。

## 🛠️ 使用技術

| ツール | 用途 |
|-------|------|
| [faster-whisper](https://github.com/SYSTRAN/faster-whisper) | 高速音声認識（CTranslate2ベース） |
| [pyannote.audio](https://github.com/pyannote/pyannote-audio) | 話者分離（CNRS開発） |
| [Google Colab](https://colab.research.google.com/) | 無料GPU実行環境 |
| iTunes Search API | Podcast RSSフィード取得 |
| ffmpeg | 音声フォーマット変換 |

## 📝 ライセンス

MIT License

## 🙏 謝辞

- [OpenAI Whisper](https://github.com/openai/whisper) — 音声認識モデル
- [pyannote.audio](https://github.com/pyannote/pyannote-audio) — 話者分離モデル
- [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) — Whisper高速実装
