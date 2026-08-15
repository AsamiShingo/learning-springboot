# SpringBoot Web開発 学習カリキュラム(汎用テンプレート)

対象: Javaの基礎(OOP・コレクション・例外処理・ラムダ/Stream)を一通り学んだ人が、
SpringBootでWebアプリケーション開発を段階的に学ぶためのカリキュラム。

このファイルはプロジェクト非依存のテンプレートです。学習を始めるプロジェクトに
`docs/CURRICULUM.md` が無い場合、このファイルをコピーして使います。

各Stepは **目的 / 概念 / 前提Step / 完了条件(Definition of Done)** を持ちます。
前提Stepは「この順で進めることを推奨する」という情報であり、強制ではありません。
別のStepから始めたい場合はそちらを優先してよいですが、前提が未完了だと
難易度が上がる場合がある旨をメンターは伝えます。

参考URL(公式ドキュメントや解説記事)は、このファイルには固定で書きません。
技術情報は古くなるため、学習者が今どのStepの何につまずいているかに応じて、
メンターがその場でWeb検索して提示する方針です(`SKILL.md`参照)。

## 全体像(依存関係マップ)

任意のStepから始められるように、前提関係を図にしておく。矢印は「先にやっておくと
やりやすい」という推奨関係であり、強制ではない。

```mermaid
graph LR
  S0[Step0 前提確認] --> S1[Step1 環境構築]
  S1 --> S2[Step2 最小アプリ]
  S2 --> S3[Step3 デプロイ体験]
  S2 --> S4[Step4 画面遷移/フォーム]
  S4 --> S5[Step5 画面共通化]
  S4 --> S6[Step6 バリデーションエラー]
  S2 --> S7[Step7 DB接続]
  S7 --> S8[Step8 レイヤード]
  S8 --> S9[Step9 DTO分離]
  S9 --> S10[Step10 テスト]
  S8 --> S11[Step11 セッション認証]
  S9 --> S12[Step12 REST API]
  S11 --> S12
  S11 --> S13[Step13 Security入門]
  S12 --> S13
  S13 --> S14[Step14 認可仕上げ]
  S7 --> S15[Step15 運用仕上げ]
  S3 --> S15
  S15 --> S16[Step16 JS基礎]
  S16 --> S17[Step17 CSSレイアウト]
  S16 --> S18[Step18 クライアントバリデーション]
  S16 --> S19[Step19 jQuery]
  S12 --> S20[Step20 Ajax+CSRF]
  S13 --> S20
  S19 --> S20
```

## Step0: 前提確認(Java基礎の棚卸し)
- 目的: Web開発に入る前に、Java基礎が実務レベルで使えるか確認する。
- 前提Step: なし
- 完了条件: 下記チェックリストの各項目が「実証済み」になっていること。

Step0は他のStepと違い、**単発のCLI課題では終わらせない**。以降のStep(Step1〜)で
実際にこのアプリを作っていく過程で、各項目が本物のコードとして使われているかを
その都度チェックし、実証済みにしていく。単なる知識確認ではなく「実際に使えているか」を見る。

### チェックリスト

| # | 項目 | 実証条件(このプロジェクトのどこかで使われていればよい) | 未実証なら促す先 |
|---|---|---|---|
| 1 | 基本構文(if/for/while等) | 分岐・繰り返しを使った処理がどこかにある(Controller/Serviceの条件分岐等) | Step2(最小アプリ) |
| 2 | メソッド設計(引数・戻り値・オーバーロード) | 引数・戻り値を適切に設計したメソッドがある。オーバーロードの必要性を説明できる | Step2〜Step4 |
| 3 | クラス設計・コンストラクタ設計 | Entity/DTOが、責務の合った単位で設計され、不要なsetterを持たない | Step9(DTO設計) |
| 4 | record(不変データクラス) | DTOやちょっとした値オブジェクトを`record`で表現している、または「なぜここは`record`にしなかったか」を説明できる | Step9(DTO設計) |
| 5 | interfaceによる抽象化 | `JpaRepository`の継承に加え、Serviceそのものをinterface+実装クラスに分けている、または差し替え可能な設計がある | Step8(レイヤードアーキテクチャ) |
| 6 | enum(状態・種別の型安全な表現) | ロールや種別のような固定値を、生の文字列比較ではなく`enum`で表現している | Step8(レイヤードアーキテクチャ)、既存コードの`Role`文字列比較の見直し |
| 7 | 例外処理の使い分け | 独自例外クラス(例: `〇〇NotFoundException`)を定義し、適切な層でスローしている | Step12(REST API/例外処理) |
| 8 | Collection操作 | `List`/`Map`/`Set`に対する重複除去・ソート・集計等の操作がある | Step2〜Step4(最小アプリ/フォーム) |
| 9 | ジェネリクス | `JpaRepository<Game, Long>`のような型パラメータ付きの宣言を読み書きできる | Step8(レイヤードアーキテクチャ) |
| 10 | Stream API | `.stream().filter().map()`等を使ったコレクション処理・変換がある | Step9(Entity→DTO変換) |
| 11 | Optional | `Repository`の検索結果を`Optional`のまま扱い、`orElseThrow`等で例外に変換している | Step8(Service層) |
| 12 | equals/hashCodeの契約 | Entity(特に複合主キー相当のクラス)やDTOで`equals`/`hashCode`を意図して定義・確認している(recordなら自動生成される点との対比も含む) | Step7(DB接続/Entity設計) |
| 13 | ラムダ式/関数型インターフェース | `Comparator`やStream内のラムダ、テストの`when(...).thenReturn(...)`のような関数型の記法を使っている | Step9〜Step10 |

- 各項目の状態は「未実証 / 実証済み」の2値。`docs/PROGRESS.md`にStep0専用のサブテーブルとして持つ
  (フォーマットは`SKILL.md`参照)。
- メンターは、Step1以降どのStepの完了レビュー(モード3)を行うときも、そのとき読んだコードに
  このチェックリストの項目が現れていないか確認し、現れていれば実証済みにする(専用のレビュー機会を
  別途設ける必要はない)。
- ある項目が、対応する「促す先」のStepまで進んでもまだ未実証であれば、そのStepのレビュー時に
  「ここで意図的に使ってみよう」と具体的な箇所を示して促す。

## Step1: 開発環境のセットアップ(選択制: Eclipse / VSCode)
- 目的: コードを書き始める前に、IDE上で補完・文法エラー表示・デバッグ実行が機能する状態を整える。
- 概念: Mavenプロジェクトのインポート、JDKの紐付け、ブレークポイントを使ったデバッグ実行。
- **IDEは選択制にする**:
  - **選択肢A: Eclipse** — 「Existing Maven Projects」としてインポート。`.project`/`.classpath`/`.settings`が
    自動生成される。「Debug As → Spring Boot App」等でのデバッグ実行に慣れる。
  - **選択肢B: VSCode** — Extension Pack for Java、Spring Boot Extension Packを導入。
    `launch.json`によるデバッグ構成に慣れる。
- 前提Step: Step0
- 完了条件: IDE上でコード補完・文法エラー表示が機能し、ブレークポイントを設定してデバッグ実行できる。
- 補足: **アプリの起動・動作確認自体は、IDEのRunボタンではなくターミナルからのMavenラッパーコマンド
  (`./mvnw` or `mvnw.cmd`)で行う**ことをこの時点でルール化しておく。IDEのRunボタンは裏で
  classpathや作業ディレクトリの扱いが micro に異なることがあり、CI/本番相当の挙動と
  食い違う原因になりやすいため。デバッグ実行(ブレークポイントで止めたい時)はIDE、
  それ以外の起動確認はターミナル、という役割分担を早い段階で習慣化する。

## Step2: Webの基本 + 最小のSpringBootアプリ
- 目的: HTTPリクエスト/レスポンスを理解した上で、一覧表示だけの最小アプリを作る。
- 概念: `@SpringBootApplication`、組み込みTomcat、`@Controller`、`@GetMapping`。
- 前提Step: Step1
- 完了条件: データはコード内にハードコードした`List`でよい。ブラウザから一覧ページが表示される。

## Step3: デプロイして動かす体験
- 目的: 最小構成のうちに、パッケージング/実行の仕組みを理解する。DBや認証が無い分、
  デプロイの仕組みそのものに集中できる。
- 概念: Mavenの`package`によるjar化、jarの直接起動、ポート/起動ログの見方。(任意でDockerfile)
- 前提Step: Step2
- 完了条件: IDEを使わず、コマンドラインだけでjarを起動し、ブラウザから確認できる。
- **手順はOS/IDEの環境によって変わるので、Step1で選んだ環境に応じて読み替える**:
  - **Windows**: `mvnw.cmd package` → `java -jar target\(jar名).jar`。ターミナルはコマンドプロンプト/
    PowerShell、またはEclipse/VSCodeの統合ターミナルのどちらでもよい。
  - **Mac/Linux**: `./mvnw package` → `java -jar target/(jar名).jar`。パス区切りが`/`になる点、
    実行権限(`chmod +x mvnw`)が必要な場合がある点に注意。
  - **Eclipseの場合**: 左下のTerminalビュー、または外部ターミナルから上記コマンドを実行する
    (Exportウィザードでのjar化ではなく、Mavenラッパー経由での再現性を優先する)。
  - **VSCodeの場合**: 統合ターミナル(`Ctrl+@`)からそのまま実行できる。
  - 完了条件を満たしたかどうかは環境に依らず共通(コマンドラインだけで起動しブラウザ確認できるか)。

## Step4: Thymeleafによる画面遷移とフォーム
- 目的: 一覧→詳細の画面遷移と、フォーム送信の受け取りを実装する。
- 概念: `Model`、`th:each`/`th:if`、リダイレクト vs フォワード、`@ModelAttribute`。
- 前提Step: Step2
- 完了条件: 一覧→詳細の画面遷移ができ、フォーム送信でデータを追加できる(永続化はまだ不要)。

## Step5: 画面共通化の基礎(Thymeleaf Fragment)
- 目的: Step4以降、画面数が増えていく前に、ヘッダー/フッターなど共通部分を1箇所にまとめておく。
  CSSによる装飾は行わず、構造上の重複を避けることだけを目的とする(最低限のフロントエンドの範囲内)。
- 概念: `th:fragment`、`th:replace`/`th:insert`によるレイアウト共通化。
- 前提Step: Step4
- 完了条件: ヘッダー/フッターが共通フラグメント化されており、以降の画面追加(Step6以降)でコピペが発生しない。
- 補足: これを後回しにすると、Step6以降に増える画面すべてを後で書き直す手戻りが発生するため、
  ここで先に済ませておく。

## Step6: フォームバリデーションエラーの扱い方
- 目的: 入力エラーを検知するだけでなく、画面にフィードバックするところまで実装する。
- 概念: `@Valid`、`BindingResult`、`th:errors`、エラー時の入力値保持。
- 前提Step: Step4
- 完了条件: 不正な入力を送るとエラーメッセージが表示され、他の入力済み項目は保持されたまま再入力できる。

## Step7: DB接続の基礎(選択制: H2 / PostgreSQL)
- 目的: ハードコードした`List`から実DBへの切り替えを体験する。
- 概念: `spring-boot-starter-data-jpa`、`@Entity`/`@Id`、datasource設定、`ddl-auto`。
- 前提Step: Step2(Step4・Step6を先に済ませておくとフォームと連動させやすい)
- 完了条件: DBに保存したデータが一覧画面に表示される。H2かPostgreSQLのいずれかを選んで接続できている。
- 補足: 同じEntity/Repositoryコードのまま設定変更だけでH2⇄PostgreSQLを切り替えられることを確認すると、JPAの抽象化の意味が実感できる。

## Step8: レイヤードアーキテクチャへのリファクタリング
- 目的: Controllerに書いた処理をcontroller/service/repository/entityに分割する。
- 概念: 関心の分離、コンストラクタインジェクション、`JpaRepository`。
- 前提Step: Step7
- 完了条件: ControllerがRepositoryに直接依存せずServiceを経由している。DBアクセスはRepositoryに閉じている。

## Step9: DTO設計とAPI/画面の分離
- 目的: Entityの直接返却をやめ、リクエスト/レスポンス用のDTOに分離する。
- 概念: リクエストDTO/レスポンスDTO、バリデーションの層分担(構造的検証はDTO、業務的検証はService)。
- 前提Step: Step8
- 完了条件: Controllerの引数・戻り値にEntityが直接現れない。

## Step10: テストの導入
- 目的: レイヤーごとの責務に応じたテストを書く。
- 概念: モック/スタブ、`@WebMvcTest`、`@DataJpaTest`。
- 前提Step: Step9
- 完了条件: 主要なServiceメソッドにMockitoテストがあり、代表的なControllerに`@WebMvcTest`がある。

## Step11: セッションによる認証の基礎(手作り)
- 目的: `HttpSession`を使って自前のログイン機能を実装し、認証の基本概念を体で理解する。
- 概念: セッションスコープ、Cookie、平文パスワード保存の危険性。
- 前提Step: Step8(レイヤーが確立していること)
- 完了条件: ログイン/ログアウトができ、未ログイン時は保護されたページにアクセスできない。

## Step12: REST APIと例外処理の設計
- 目的: `@RestController`でJSON APIを作り、業務エラーを適切なHTTPステータスにマッピングする。
- 概念: `@RestController`、`@ExceptionHandler`/`@ControllerAdvice`。
- 前提Step: Step9, Step11
- 完了条件: JSON APIがDTOを返し、異常系(存在しないID指定など)で適切なHTTPステータスが返る。
- 補足: セッション認証済みのHTML画面と、別途認証が必要なJSON APIという「2種類の入口」が
  併存する状態を作ることが、次のSecurity導入の動機付けになる。

## Step13: Spring Security入門
- 目的: Step11で体感した手作り認証の弱点(平文パスワード等)をSpring Securityで解決する。
- 概念: `SecurityFilterChain`、`UserDetailsService`、BCrypt、CSRF。
- 前提Step: Step11, Step12
- 完了条件: パスワードがハッシュ化されて保存され、Spring Securityのフィルタ経由でログインが機能する。

## Step14: 認可の作り込みとSecurity統合の仕上げ
- 目的: 画面用/API用など用途別に`SecurityFilterChain`を分割し、ログイン後のユーザー特定を
  `SecurityContextHolder`経由に統一する。
- 概念: `@PreAuthorize`、ロールベース認可、`@WithMockUser`によるSecurityのテスト。
- 前提Step: Step13
- 完了条件: ログイン後のユーザー特定が`SecurityContext`経由になっている。ロールごとのアクセス制御にテストがある。

## Step15: 運用を意識した仕上げ(任意/発展)
- 目的: 開発時の手軽さ(H2)から一歩進めて、より実務に近い運用面を扱う。
- 概念: `application-{profile}.properties`によるプロファイル分け、PostgreSQLへの切り替え、
  ロギング設計、Spring Boot Actuator。
- 前提Step: Step7, Step3
- 完了条件: 環境ごとに設定を切り替えて起動できる。

## フロントエンド編(Step16〜20、バックエンド完了後)

Step0〜15(バックエンド)の間は、Step5の画面共通化を除いて最低限のThymeleaf
(CSS装飾無し、素のHTML相当)で進め、フロントエンドはバックエンドが一通り完了してから
本格的に着手する。理由は、Ajax通信やCSRFトークン連携がバックエンドのREST API(Step12)・
Security(Step13/14)の理解を前提とするため。

## Step16: JavaScript基礎(DOM操作・イベント・Promise)
- 目的: jQueryやAjaxに入る前に、素のJavaScriptでDOM操作・イベントハンドリング・非同期処理の基本を理解する。
- 概念: `document.querySelector`、イベントリスナー、`Promise`、`async`/`await`。
- 前提Step: Step15
- 完了条件: 素のJavaScriptだけで、ボタンクリックに応じて画面の一部を書き換えられる。

## Step17: CSSレイアウト
- 目的: 装飾ではなくレイアウト崩れを直せるレベルのCSSを身につける(画面構造の共通化はStep5で対応済み)。
- 概念: Flexbox/Grid、メディアクエリ(レスポンシブ)。
- 前提Step: Step16
- 完了条件: 主要画面がFlexbox/Gridで崩れずに表示される。

## Step18: クライアントサイドバリデーション
- 目的: Step6のサーバーサイドバリデーションと対比しながら、即時フィードバック用のクライアント側チェックを実装する。
- 概念: HTML5バリデーション属性(`required`等)、JavaScriptによる入力チェック、サーバー側検証との役割分担。
- 前提Step: Step16
- 完了条件: 明らかな入力ミス(未入力・形式違反)は送信前にクライアント側で気づける。最終チェックはサーバー側(Step6)に残っている。

## Step19: jQuery入門
- 目的: 素のJavaScriptで書いていたDOM操作・イベント処理をjQueryで書き直し、何が簡潔になるかを比較する。
- 概念: セレクタ、`.on()`によるイベントバインド、要素の生成・追加・削除。
- 前提Step: Step16
- 完了条件: Step16で書いたDOM操作の一部をjQueryで書き換え、コード量・書き方の違いを説明できる。

## Step20: Ajax通信とCSRF連携
- 目的: 画面遷移を伴わずにサーバーとJSONをやり取りする。Spring SecurityのCSRF保護下でPOSTリクエストを送る。
- 概念: `$.ajax`/`fetch`、JSONのシリアライズ/デシリアライズ、CSRFトークンをヘッダー/パラメータに埋め込む方法、ブラウザ開発者ツール(Networkタブ)でのデバッグ。
- 前提Step: Step12(REST API)、Step13(Security/CSRF)、Step19
- 完了条件: 画面遷移無しでStep12のREST APIにPOSTでき、CSRF保護が有効なままリクエストが成功する。ブラウザの開発者ツールでリクエスト/レスポンスを確認できる。

## 発展メニュー(任意、順不同)

本編で扱わなかったが、今後拡充していきたい要素。優先順位や着手順は都度相談する。

- ファイルアップロード(`MultipartFile`)
- 国際化(i18n、`messages.properties`)
- OpenAPI/Swaggerによるドキュメント化
- シークレット管理(認証情報の環境変数化)
- キャッシュ・非同期処理(`@Async`)
