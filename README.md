# Phonon TTS for Unraid

An Unraid Community Applications template for [ptts-openai-server](https://github.com/gradium-ai/xn-ptts/tree/main/ptts-openai-server),
the official speech server for **Phonon**, [Gradium](https://gradium.ai/phonon)'s open-weight on-device text-to-speech
models. It serves an **OpenAI-compatible API** (`POST /v1/audio/speech`) and the **Wyoming protocol**, so it plugs into
Open WebUI, SillyTavern, Home Assistant and anything else with an "OpenAI TTS" setting — fully local, CPU-only, and
fast enough to speak faster than real time even on a Raspberry Pi.

Report template or Unraid problems in this repository's [issues](https://github.com/Aurial/phonon-unraid/issues), and
problems with the server itself to the [xn-ptts project](https://github.com/gradium-ai/xn-ptts/issues).

## Install

Search for **Phonon TTS** in the Apps tab. The defaults work out of the box:

- **API port** `8880` — point any OpenAI-TTS client at `http://<tower>:8880/v1`
- **Language** `en` — text normalization and the catalog voices; must match the model's language
- **Model and voices cache** — under about 1 GB, downloaded on first start and kept across restarts and updates

The first start downloads the default model and voices and logs little until they load — watch the log. Later starts
go straight to serving.

> **The server has no authentication on any port.** Run it on a trusted network; never expose it to the internet.

## Home Assistant

Set **Wyoming address** to `0.0.0.0:10200` and map the **Wyoming port** too (both under Advanced). Then in Home
Assistant add the **Wyoming Protocol** integration with this server's address and port `10200`: Phonon appears as a
text-to-speech engine you can pick for your voice assistant. It streams — speech starts at the first sentence of a
reply, not the last.

## Models and voices

- **Default:** leave **Model** empty — the server ships a built-in default model.
- **Other models:** `hf://owner/name` (add `@commit_sha` to pin the version). The **Phonon 40M** models are gated:
  accept their terms on Hugging Face first, then set an **HF token**.
- **Voices:** the public catalog (`gradium/phonon-voices`) is cached for your language; `GET /v1/audio/voices` lists
  what the running server speaks. **OpenAI voice names** and **Voice aliases** make clients with fixed voice lists
  (they send `alloy`, `nova`, …) work unchanged.
- **Ignore speed** is on by default: OpenAI clients always send a `speed` parameter; the server accepts it and speaks
  at the normal rate.

## Updates

The template follows `latest`, which Gradium moves with each release; Unraid shows the update on the Docker tab.
Pick a pinned tag (such as `0.5.0`) under **Branch** at install to stay on one release.

## Credits and licence

[Phonon](https://gradium.ai/phonon) and the [xn-ptts](https://github.com/gradium-ai/xn-ptts) server are by
[Gradium](https://gradium.ai) and contributors, Apache-2.0. The models themselves carry their own licences on
Hugging Face (the 31M models are Apache-2.0; the 40M models are gated — check their model cards). The icon in this
repository is an original work by the template maintainer, MIT. The template files are MIT licensed; see
[LICENSE](LICENSE).
