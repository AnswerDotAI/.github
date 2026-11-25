# .github

Organization-wide GitHub Actions and community defaults for Answer.AI.

## `actions/sccache`

Installs `sccache` and starts its server backed by a Cloudflare R2 URL. Skips setup when `r2-url` is empty, so external fork pull requests fall back to plain `rustc`.

```yaml
- uses: AnswerDotAI/.github/actions/sccache@main
  with:
    r2-url: ${{ secrets.SCCACHE_R2 }}
```

## `actions/setup-uv`

Installs `uv` via the organization's pinned `astral-sh/setup-uv` release so workflows track upgrades in one place.

```yaml
- uses: AnswerDotAI/.github/actions/setup-uv@main
```

## `actions/maturin-build`

Installs `maturin` (plus `patchelf`, `ziglang`, and `cargo-zigbuild` on Linux when `--zig` or `zig: true` is set) into a fixed deterministic path (`/tmp/aai-maturin` on Unix, `$RUNNER_TEMP/aai-maturin` on Windows) so `cargo-zigbuild`'s `CARGO_TARGET_*_LINKER` wrapper path stays constant and hits `sccache` across runs. Adds its `bin` directory to `PATH` and runs `maturin` (`command: setup` installs tools onto `PATH` without invoking `maturin`, for jobs that run `cargo stage --zig` first).

```yaml
- uses: AnswerDotAI/.github/actions/maturin-build@main
  with:
    args: --profile dist --out dist ${{ runner.os == 'Linux' && '--zig' || '' }}
    manylinux: '2_31'
```
