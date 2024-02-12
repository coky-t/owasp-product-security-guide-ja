---

layout: col-sidebar
title: OWASP Product Security Guide
tags: example-tag
level: 2
type: Documentation
pitch: The OWASP Product Security Guide project educates developers and organizations on security considerations for various products, offering a curated list of vulnerabilities and promoting awareness and solutions within the development community.

---

<img src="https://raw.githubusercontent.com/OWASP/www-project-product-security-guide/main/Asset/OWASP%20Product%20Security%20Guide%20Logo.png" width="500" height ="300">

このページは OWASP プロダクトセキュリティガイドです。三つのパートがあります。
1. [プロダクトはどのように攻撃されていますか？](#how-products-are-being-attacked)  
2. [プロダクトをどのように保護しますか？](#how-to-secure-products)
3. [プロダクトセキュリティにおける AI/LLM の役割](#role-of-AI_LLM-product-security)  

アプリケーションセキュリティはコードレベルの脆弱性に焦点を当てますが、プロダクトセキュリティは設計上の欠陥やサプライチェーンのリスクなどのより広範な側面に対処します。プライバシーとセキュリティに対する懸念の高まりは AI/LLM に起因しており、データ侵害、バイアスがあるアルゴリズム、個人情報の操作などの脅威が生じています。このガイドは安全でプライバシーを保護するプロダクトの設計、作成、テスト、調達に関する洞察を提供するドキュメントとして機能します。

また、[この有用な録画](https://youtu.be/ol-z_ShulCc?si=xmPFkpjrwrxNYQSX) や、2023 年 2 月 15 日にダブリンで開催された OWASP Global AppSec イベントでの [Rob van der Veer の講演](https://sched.co/1F9DT) の [スライド](https://github.com/OWASP/www-project-ai-security-and-privacy-guide/blob/main/assets/images/20230215-Rob-AIsecurity-Appsec-ForSharing.pdf?raw=true) を参照してください。そしてこのガイドの AppSec Podcast エピソード ([音声](https://www.buzzsprout.com/1730684/12313155-rob-van-der-veer-owasp-ai-security-privacy-guide)、[動画](https://www.youtube.com/watch?v=SLdn3AwlCAk&))) や [September 2023 MLSecOps Podcast](https://mlsecops.com/podcast/a-holistic-approach-to-understanding-the-ai-lifecycle-and-securing-ml-systems-protecting-ai-through-people-processes-technology) をチェックしてください。ショートストーリーをお望みであれば、[5分プロダクトセキュリティクイックトーク](https://youtu.be/D6YRQYHVHao?si=Ua_TG5tqy_YiYaVG) をご覧ください。

<p align="left"><a href="https://youtu.be/D6YRQYHVHao?si=cMom_KcEa4sIVt6k" target="_blank" rel="noopener noreferrer"><img src="https://raw.githubusercontent.com/OWASP/www-project-product-security-guide/main/Asset/talkvideo.jpeg" width="160" height="160" border="1"/> </a></p>


プルリクエスト、issue の投稿 ([リポジトリ](https://owasp.org/www-project-product-security-guide/#) を参照)、プロジェクトリーダーへの電子メールを通じてご意見を提出してください。このガイドをさらに良くしていきましょう。Qualcomm のプロダクトセキュリティリーダーである [Scott Bauer](https://www.linkedin.com/in/scott-bauer-90a55531/overlay/about-this-profile/) の多大な貢献に感謝します。

# プロダクトはどのように攻撃されていますか？ <a name="how-products-are-being-attacked"></a>
SDLC と [サプライチェーンの脆弱性](https://www.fortinet.com/resources/cyberglossary/supply-chain-attacks) はさまざま方法で悪用されます。[SDLC](https://mediasmarts.ca/digital-media-literacy/digital-issues/cyber-security/cyber-security-software-threats) では、攻撃者は開発、テスト、デプロイメントフェーズの弱点を悪用して、悪意のあるコードを注入したり、ツールやライブラリを侵害します。サプライチェーン攻撃は信頼できるベンダーやサードパーティコンポーネントに侵入してマルウェアを配布したり、アップデートを改竄して、ダウンストリームシステムを侵害するものです。コードインジェクション、依存関係の混乱、ハイジャックなどの技法は認証、認可、配布の弱点を悪用し、侵害、データ窃取、システム侵害につながります。これらはソフトウェア開発と配布のライフサイクル全体にわたる [堅牢なセキュリティ対策の重要な必要性](https://jfrog.com/blog/the-importance-of-prioritizing-product-security/) を強調しています。プロダクトはマルウェアの注入、フィッシング、サプライチェーンの侵害などさまざまな攻撃ベクトルに直面しており、ライフサイクル全体を通じた包括的なセキュリティ対策の必要性を浮き彫りにしています。

# プロダクトをどのように保護しますか？ <a name="how-to-secure-products"></a>

## フェーズ 1: 要件

この初期ステージでは、さまざまな利害関係者から新機能要件を集めます。今後のリリースに向けて収集される機能要件に関連する <span style="color:blue">セキュリティ上の考慮事項</span> を認識することが極めて重要です。

- **<span style="color:blue">機能要件の例:</span>** ユーザーはメンバーシップを更新する前に連絡先情報を検証できる必要があります。

- **<span style="color:blue">セキュリティ上の考慮事項の例:</span>** ユーザーは自分の連絡先情報のみを参照でき、他の人のものはできないようにします。

## フェーズ 2: 設計

このステージはスコープ内の要件を実際のアプリケーションにどのように現れるべきかという計画に変換します。機能要件は実行するアクションの概要を示し、セキュリティ要件は回避するアクションに焦点を当てます。

- **<span style="color:blue">機能設計の例:</span>** ページはデータベースの CUSTOMER_INFO テーブルからユーザーの名前、電子メール、電話、住所を取得し、画面に表示する必要があります。

- **<span style="color:blue">セキュリティ上の懸念事項の例:</span>** データベースから情報を取得する前に、ユーザーが有効なセッショントークンを持っていることを検証しなければなりません。ない場合、ユーザーはログインページにリダイレクトされます。

## フェーズ 3: 開発

設計を実装するときには、セキュリティの観点からのコード品質を確保することが焦点になります。確立されたセキュアコーディングガイドラインとコードレビューによって、これらの標準への準拠を検証します。これは手動で行うことも、[静的アプリケーションセキュリティテスト (SAST)](https://en.wikipedia.org/wiki/Static_application_security_testing) などのテクノロジを使用して自動化することもできます。最近のアプリケーション開発者は、機能提供を迅速化するために、自分のコードだけでなく、通常はフリーのオープンソースコンポーネントからの、既存の機能に依存することも考慮しなければなりません。現在導入されているアプリケーションの 90% 以上がこのようなコンポーネントを利用しており、[ソフトウェア構成分析 (Software Composition Analysis, SCA)](https://www.g2.com/categories/software-composition-analysis) ツールを使用して評価されます。

この場合、セキュアコーディングガイドラインには以下を含みます。
- パラメータ化した読み取り専用の SQL クエリを使用してデータベースからデータを読み取り、これらのクエリを不正な目的で悪用できる可能性を最小限に抑えます。
- ユーザー入力に含まれるデータを処理する前にユーザー入力を検証します。
- データベースからユーザーに送り返すデータをサニタイズします。
- オープンソースライブラリを使用する前に脆弱性をチェックします。

## フェーズ 4: 検証

検証フェーズではアプリケーションが当初の設計と要件を満たしていることを確認するためのテストサイクルを実施します。さまざまなテクノロジを使用した自動セキュリティテストを導入する場でもあります。これらのテストに合格しない限り、アプリケーションはデプロイされません。このフェーズには検証とリリースを制御するために CI/CD パイプラインなどの自動化ツールを含むことがよくあります。

このフェーズでの検証には以下を含みます。
- アプリケーションのクリティカルパスを表現する自動テスト
- 基盤となるアプリケーションの正確性を検証するアプリケーション単体テストの自動実行
- プロダクション環境で使用されるアプリケーションシークレットを動的に入れ替える自動デプロイメントツール

## フェーズ 5: 保守と改善

物語はアプリケーションのリリース後も続きます。当初は見落とされていた脆弱性がリリースからかなり後に表面化するかもしれません。これらの問題は開発者が作成したコードに存在することもありますが、アプリケーションを構成する基本的なオープンソースコンポーネントで検出されることが増えています。この結果、アプリケーションの保守者によってプロダクションで特定されたこれまで知られていない脆弱性である [ゼロデイ](https://en.wikipedia.org/wiki/Zero-day_(computing)) が増加します。

その後、開発チームはこれらの脆弱性にパッチを適用しなければなりませんが、そのプロセスではアプリケーションの機能を大幅に書き換える必要があるかもしれません。このステージの脆弱性は倫理的ハッカーによる外部のペネトレーションテストや [バグバウンティ](https://hackerone.com/bug-bounty-programs) プログラムを通じた提出に起因するものもあります。このようなプロダクションの問題に対処するには、慎重な計画と将来のリリースへの組み込みが必要です。

## プロダクトセキュリティにおける AI/LLM の役割 <a name="role-of-AI_LLM-product-security"></a>

**SDLC とサプライチェーンにおける AI/LLM の利点:**

- <span style="color:blue">**自動テスト (Automated Testing):**</span> LLM はコードの潜在的な問題を分析し、手動テストよりも早くバグや脆弱性を特定できます。
- <span style="color:blue">**コード生成 (Code Generation):**</span> AI は定型的なコードの生成などの反復的なタスクを自動化し、開発者をより複雑な作業に解放します。
- <span style="color:blue">**脅威検出 (Threat Detection):**</span> AI は膨大なデータを分析し、不審なアクティビティや潜在的なサプライチェーン攻撃を特定できます。
- <span style="color:blue">**セキュリティ堅牢化 (Security Hardening):**</span> AI はセキュリティのベストプラクティスを提案し、開発者がより安全なコードを書けるように支援できます。

**リスクと課題:**

- <span style="color:blue">**AI バイアス (AI Bias):**</span> トレーニングデータにバイアスがあると、AI モデルにバイアスが生じ、脆弱性や差別的慣行をもたらす可能性があります。
- <span style="color:blue">**ポイズニング攻撃 (Poisoning Attacks):**</span> 悪意のある人物はトレーニングデータやモデルを操作して、有害なコードや誤った情報を注入する可能性があります。
- <span style="color:blue">**ブラックボックス問題 (Black Box Problem):**</span> AI の複雑な性質は、意思決定がどのように行われるかを理解することを困難にし、根本原因分析やセキュリティ監査の妨げになります。
- <span style="color:blue">**依存関係管理 (Dependency Management):**</span> 外部 AI モデルおよびライブラリへの依存関係を管理することはさらなるセキュリティリスクをもたらします。

**リスクの軽減:**

- <span style="color:blue">**データ品質 (Data Quality):**</span> AI モデルのための高品質でバイアスのないトレーニングデータに投資します。
- <span style="color:blue">**セキュリティ意識 (Security Awareness):**</span> AI セキュリティリスクとベストプラクティスについて開発者とセキュリティチームをトレーニングします。
- <span style="color:blue">**モデルテストと監査 (Model Testing & Auditing):**</span> AI モデルのバイアス、脆弱性、意図しない結果を定期的にテストおよび監査します。
- <span style="color:blue">**依存関係管理 (Dependency Management):**</span> 外部 AI の依存関係を管理および更新するためのセキュアプラクティスを導入します。
- <span style="color:blue">**透明性と説明可能性 (Transparency & Explainability):**</span> より透明性が高く、説明可能な AI モデルの開発を提唱します。

**その他のリソース:**

- <span style="color:red">**Retrieval vs. poison — Fighting AI supply chain attacks:**</span> [Link](https://www.elastic.co/security-labs/elastic-users-protected-from-suddenicon-supply-chain-attack)
- <span style="color:red">**Detecting Poisoned AI Models:**</span> [Link](https://arxiv.org/pdf/2204.00032)
- <span style="color:red">**Secure the AI Software Supply Chain:**</span> [Link](https://www.linuxfoundation.org/resources/publications/open-source-software-supply-chain-security)
