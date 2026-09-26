# thorfin-stt-cloud

Free cloud Arabic speech-to-text backend for Thorfin Audio, running on GitHub
Actions (public repo = unlimited free runner minutes).

## How it works

1. The app uploads audio to catbox.moe (keyless) and triggers `stt.yml`
   via `workflow_dispatch` with `{audio_url, job_id, language}`.
2. The runner installs faster-whisper (pip cache), downloads the audio, and
   transcribes it with the multilingual `base` model (good Arabic, CPU).
3. `transcript.txt` is uploaded as the `transcript-<job_id>` artifact
   (7-day retention). The app polls, downloads, and deletes it.

## Trigger

```bash
curl -X POST https://api.github.com/repos/yamenfhamsy-member/thorfin-stt-cloud/actions/workflows/stt.yml/dispatches \
  -H "Authorization: Bearer $PAT" \
  -H "Accept: application/vnd.github+json" \
  -d '{"ref":"main","inputs":{"audio_url":"https://…","job_id":"abc123","language":"ar"}}'
```

The PAT needs Actions read/write on this repo.
