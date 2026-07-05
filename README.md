# Matt Pocock Agent Skills 日本語版

## なぜ日本語版を用意するのか

- 日本語を母語とする開発者が、skill の意図をそのまま読み取れるようにするため
- 日本語でやり取りする coding agent に、より自然な instruction を渡すため
- 日本語の開発チームや issue 運用に組み込みやすくするため

## このリポジトリについて

これは [`sscodeai/skills`](https://github.com/sscodeai/skills) の日本語ローカライズ版です。ドキュメントと skill instructions を日本語化し、ディレクトリ名、skill name、コマンド、コードブロック、パス、URL、package/tool/API identifiers は動作を壊さないよう保持します。

この日本語版は、単に読みやすくするためだけのものではありません。日本語で agent と会話する場合、skill instruction も自然な日本語で書かれている方が、意図の取り違えや中途半端な日英混在を減らせます。

本リポジトリは、上流をコンテンツソースとして同期します。上流の Git 履歴や管理状態をそのまま同期するものではありません。メンテナンス規則は [`.skills/translate-skill/SKILL.md`](./.skills/translate-skill/SKILL.md) を参照してください。

翻訳方針は **skill-guided content localization** です。上流 `sscodeai/skills` を英語コンテンツの source of truth とし、自然言語の説明だけを日本語化します。ディレクトリ名、skill name、frontmatter key、コマンド、コードブロック、パス、URL、package/tool/API identifiers、動作に関わる labels は保持します。ユーザー向けのインストール先は `sscodeai/skills-ja-JP` に統一します。

## リリース記録と検証

### リリース記録

- 2026-07-05: Synced upstream `sscodeai/skills@272f99b`, local commit `247525d`. Initialized Japanese localization repository, translation workflow, validation scripts, license translation, and Japanese README entrypoint.

### 最新 main 検証

`sscodeai/skills@272f99b` を基準にした初期化結果:

- [x] `node scripts/check-translation.mjs` が通る。
- [x] 公開 skill index は `.claude-plugin/plugin.json` と README の公開一覧で整合している。
- [x] 元の `LICENSE` は変更せず、非公式日本語訳 `LICENSE.ja-JP.md` を追加している。
- [x] ユーザー向け install command は `sscodeai/skills-ja-JP` を指している。
- [x] コードブロック、コマンド、パス、URL、identifiers を保持する翻訳ルールを追加している。
- [!] `node scripts/audit-english.mjs` は、翻訳前の英文本文を多数検出する。これは今後の翻訳レビュー用キューであり、初期化時点では blocking check ではない。

## 30 秒セットアップ

```bash
npx skills@latest add sscodeai/skills-ja-JP
```

使いたい skills と、インストール先の coding agents を選んでください。初回は必ず [`/setup-matt-pocock-skills`](./skills/engineering/setup-matt-pocock-skills/SKILL.md) を選び、agent の中で実行して issue tracker、labels、docs ディレクトリを設定します。

[![skills.sh](https://skills.sh/b/sscodeai/skills-ja-JP)](https://skills.sh/sscodeai/skills-ja-JP)

<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

## 原版 README の翻訳

以下は上流 README の日本語訳です。初期化時点では、このセクション以降を順次ローカライズしていきます。

## Reference

これらの skills は、「誰が呼び出せるか」という軸で分かれています。**User-invoked** skills は、`/grill-me` のようにユーザーが明示的に入力したときだけ使われます。役割は orchestration です。**Model-invoked** skills は、ユーザーが呼び出すことも、task に合う場合に agent が自動で使うこともできます。こちらは再利用可能な discipline を担います。User-invoked skill は model-invoked skills を呼び出せますが、別の user-invoked skill は呼び出しません。

### Engineering

コード作業で日常的に使う skills です。

**User-invoked**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)** - 今の状況に合う skill や flow を尋ねるための router。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** - 追質問セッションを行いながら、プロジェクトの domain model を作り、用語を磨き、`CONTEXT.md` と ADRs を更新します。
- **[triage](./skills/engineering/triage/SKILL.md)** - triage roles の state machine に沿って issues を進めます。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** - codebase の deepening opportunities を探し、HTML report として提示してから、選んだ候補について grilling します。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** - engineering skills 用に issue tracker、triage labels、domain docs layout を設定します。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** - plan、spec、PRD を vertical slices に分け、独立して扱える issues にします。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** - 現在の会話を PRD にまとめ、issue tracker に公開します。追加の interview はせず、すでに話した内容を統合します。
- **[implement](./skills/engineering/implement/SKILL.md)** - PRD または issue set に基づいて実装します。

**Model-invoked**

- **[prototype](./skills/engineering/prototype/SKILL.md)** - design question に答えるための throwaway prototype を作ります。
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** - 難しい bug や performance regression に対して、reproduce -> minimise -> hypothesise -> instrument -> fix -> regression-test の診断 loop を回します。
- **[research](./skills/engineering/research/SKILL.md)** - high-trust primary sources をもとに調査し、引用付きの findings を Markdown に残します。
- **[tdd](./skills/engineering/tdd/SKILL.md)** - red-green-refactor loop で test-driven development を行います。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** - プロジェクトの domain model を構築・改善し、用語や edge-case scenarios を検証して `CONTEXT.md` と ADRs を更新します。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** - deep modules を設計するための vocabulary と discipline を提供します。
- **[code-review](./skills/engineering/code-review/SKILL.md)** - fixed point 以降の diff を Standards と Spec の 2 軸で review します。

### Productivity

コードに限定されない汎用 workflow tools です。

**User-invoked**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)** - plan や design について、decision tree のすべての枝が解決するまで質問します。
- **[handoff](./skills/productivity/handoff/SKILL.md)** - 現在の会話を handoff document に圧縮し、別の agent が作業を続けられるようにします。
- **[teach](./skills/productivity/teach/SKILL.md)** - 現在のディレクトリを stateful teaching workspace として使い、複数 session にわたって新しい skill や概念を教えます。
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** - 良い skills を書き、編集するための reference です。

**Model-invoked**

- **[grilling](./skills/productivity/grilling/SKILL.md)** - plan や design について、decision tree のすべての枝が解決するまで interview します。`grill-me` と `grill-with-docs` の背後にある reusable loop です。
