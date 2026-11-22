# .github

Organization-wide GitHub Actions and community defaults for Answer.AI.

## `actions/sccache`

Installs `sccache` and starts its server backed by a Cloudflare R2 URL. Skips setup when `r2-url` is empty, so external fork pull requests fall back to plain `rustc`.

```yaml
- uses: AnswerDotAI/.github/actions/sccache@main
  with:
    r2-url: ${{ secrets.SCCACHE_R2 }}
```
