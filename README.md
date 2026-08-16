# learning-springboot スキル

Javaの基礎を学んだ人が SpringBoot で Web開発を段階的に学ぶことを支援する、
Claude Code 用のプロジェクトスキルです。「代わりに実装する人」ではなく
「一緒に振り返り、次の一歩を示すメンター」として振る舞います。

このフォルダ(`learning-springboot/`)ごとコピーすれば、他のプロジェクトでもそのまま使えます。
特定のアプリのコードには依存していません。

## 中身

```
learning-springboot/
├── README.md                        (このファイル)
├── SKILL.md                         (スキル本体。振る舞いの定義)
└── references/
    └── curriculum-template.md       (汎用カリキュラムのひな形。Step0〜23+依存関係マップ)
```

## 導入方法

1. このフォルダを対象プロジェクトの `.claude/skills/` 配下にコピーする。

   ```bash
   cp -r learning-springboot /path/to/target-project/.claude/skills/
   ```

2. 対象プロジェクトで Claude Code を起動し、以下のように話しかける。

   ```
   learning-springbootスキルを使って、今の状況を教えて
   ```

   または、学習相談・進捗確認・コードレビューの文脈であれば、
   Claude が自動的にこのスキルを選択することもあります。

3. 初回呼び出し時、Claude が以下を確認します。
   - `.claude/state/learning-springboot/CURRICULUM.md` が無ければ、`references/curriculum-template.md` を
     ベースに新規作成してよいか尋ねます(プロジェクト事情に応じて内容を調整可能)。
   - `.claude/state/learning-springboot/PROGRESS.md` が無ければ、空の進捗テーブルを新規作成してよいか尋ねます。

   どちらも「はい」と答えれば、そのプロジェクト用のカリキュラムと進捗管理表が
   `.claude/state/learning-springboot/` 配下に作られ、以降はそれを見ながらメンタリングが行われます。
   `docs/`(プロジェクト本来のドキュメント置き場)には何も作成しません。
   - あわせて、git管理下のプロジェクトであれば「`.gitignore`に`.claude/state/`を
     追加して、進捗ファイルをコミット対象から外してよいか」も確認します。

## 使い方(呼びかけの例)

| やりたいこと | 話しかけ方の例 |
|---|---|
| 全体の進み具合を見る | 「今の状況見せて」「別のステップからやりたい」 |
| 次に何をやるか決める | 「次何すればいい?」「Step5から始めたい」 |
| 実装中に詰まった | 「わからない」「ヒントが欲しい」 |
| 書いたコードを確認したい | 「レビューして」「このステップ終わったか確認して」 |
| ステップを完了として記録したい | 「Step4完了にして」 |
| チームに進捗を共有したい | 「進捗をチームに共有して」 |

Claude はこれらを次の6つのモードとして扱います。詳しい判断基準は `SKILL.md` 参照。

0. **全体状況確認** — `.claude/state/learning-springboot/PROGRESS.md` の一覧と依存関係マップを提示し、任意のStepへの移動を尊重する。
1. **スタート確認** — 選んだStepの目的・完了条件を提示する。序盤は具体例を見せ、終盤は独力を促す(段階的に手を離す)。
2. **実装中の質問対応** — いきなり答えを出さず、概念名→該当コード箇所→具体的な方針、の順で段階的にヒントを出す。
3. **完了レビュー** — 完了条件だけでなく、コードの書き方の質、過去Stepとのつながり、別の設計案についても対話する。
4. **完了処理** — コミット・タグ付け・`.claude/state/learning-springboot/PROGRESS.md` 更新を、合意を得た上で行う。
5. **チーム進捗の共有** — 依頼されたら、個人の進捗の要約(現在Step・Step0実証スコアのみ)を、Teams・メール・口頭報告など実際に使っている手段にそのまま使える短いテキストとして生成する。ファイルへの書き込みや送信そのものは行わない(学習者ごとにアプリ・スキルのコピーが別々のgit管理下にあり、ファイル経由の自動集約は成立しないため)。

参考になるURL(公式ドキュメント等)は、あらかじめリンク集を持たせるのではなく、
そのときの質問内容に応じてClaudeがその場でWeb検索して提示します。技術情報は
古くなりやすいため、固定リンクは持ちません。

## カスタマイズ

- `references/curriculum-template.md` の内容(Stepの数・順序・完了条件)は
  あくまでひな形です。対象プロジェクトの技術スタックや学習方針に合わせて、
  `.claude/state/learning-springboot/CURRICULUM.md` を作成した後に自由に編集して構いません。
- 特定プロジェクトの実際の開発履歴を教材として使いたい場合は、
  `docs/LEARNING_ROADMAP.md` のようなファイルを別途用意すると、
  このスキルが「参考資料」として扱います(無ければ無視されます)。これは
  プロジェクト本来のドキュメントなので、`.claude/state/learning-springboot/`ではなく
  `docs/`に置いたままで構いません。

## 学習者個人の状態について

`.claude/state/learning-springboot/CURRICULUM.md` と `.claude/state/learning-springboot/PROGRESS.md` は、
学習者ごとの個人的な進捗管理ファイルです。`docs/`には一切作成しません。

**なぜ`.claude/skills/learning-springboot/`(スキル本体)の中に置かないのか**:
スキル本体は、チーム/コミュニティで共有・更新される対象になり得ます(git管理、
submoduleでの取り込み、pluginとしての配布・再インストール等)。個人の進捗を
スキル本体のディレクトリ内に置いてしまうと、スキルを更新するたびに進捗ファイルが
上書き・消失するリスクがあります。そのため、進捗は`.claude/skills/`とは別系統の
`.claude/state/`配下に、明確に分離して置きます。

複数の学習者が同じベースアプリを個別にクローン/フォークして使う場合でも、
それぞれの`.claude/state/learning-springboot/`配下に閉じるため、プロジェクト本来の
ドキュメント(`docs/`配下)やスキル本体(`.claude/skills/`配下)と混ざりません。

`.claude/state/learning-springboot/`は個人の進捗ファイルなので、**デフォルトで
コミット対象から外すことを前提**にしています。初回、このディレクトリが実際に
新規作成されるタイミングで、Claudeが`.gitignore`に`.claude/state/`の行を
追加してよいか確認します(git管理下のプロジェクトの場合)。合意すれば
自動的に追加されるので、うっかり進捗ファイルをコミットしてしまう心配は
基本的にありません。`.claude/skills/`(スキル本体)をコミット対象にするか、
別リポジトリで管理するか等は、このスキルが決めるものではなく、
プロジェクトごとの運用に委ねます。

## 前提

- Claude Code(このリポジトリでの利用を想定)。
- `.claude/state/learning-springboot/CURRICULUM.md` と `.claude/state/learning-springboot/PROGRESS.md` を作成・編集する権限。
- (任意)学習の区切りごとに git タグを付ける運用にする場合は、通常の git 操作権限。
