# EC2 Codex Resume

The EC2 Codex host is reachable as `codex-ec2`.

Quick commands:

- `codex-ec2-ssh`
  Opens a normal shell on the EC2 box after printing the important status lines.
- `codex-ec2-resume`
  Attaches to a persistent `tmux` session named `codex`. If it does not exist yet, it creates one and starts `codex` inside it.

Current remote layout:

- `codex` is installed at `/home/ec2-user/.local/bin/codex`
- Ollama runs as a systemd service bound to `127.0.0.1:11434`
- Imported model: `qwen2.5-coder-7b-ec2:latest`
- Secondary disk is mounted at `/mnt`
- `/home/ec2-user/models` -> `/mnt/models`
- `/usr/local/lib/ollama` -> `/mnt/ollama-lib`
- Ollama model store is `/mnt/ollama-models`

Useful remote checks:

```bash
systemctl status ollama --no-pager
OLLAMA_HOST=http://127.0.0.1:11434 ollama list
codex --version
codex login status
```

Shutdown/restart notes:

- The `ollama` service is enabled and will come back on boot.
- The `/mnt` disk mount is persisted in `/etc/fstab`.
- The `codex` binary and auth remain under the `ec2-user` home directory.

Known limitation:

- In this environment, Codex CLI is still authenticating through your ChatGPT account and was not reliably selecting the Ollama provider, even when invoked with `--oss --local-provider ollama`. The inference server is healthy and ready, but Codex provider selection may still need a newer client flow or different auth path.
