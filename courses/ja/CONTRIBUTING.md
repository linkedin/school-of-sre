私たちが作成した最初のコンテンツは出発点に過ぎないことを認識しており、コミュニティがコンテンツを改良し、拡張していく過程を支援してくれることを望んでいます。

コントリビューターは、投稿したコンテンツが盗用されていないことを表明するものとします。コンテンツを提出することにより、あなた（および、該当する場合は、あなたの雇用主）は、Creative Commons Attribution 4.0 International Public Licenseに基づいて、提出したコンテンツをLinkedInおよびオープンソースコミュニティにライセンスすることになります。

*リポジトリURL*: [https://github.com/linkedin/school-of-sre](https://github.com/linkedin/school-of-sre)

### コントリビュートガイドライン
以下のガイドラインに沿ってください。

* 企業や個人のプロジェクトに適用できる原則やコンセプトであること。特定のツールや技術スタックに焦点を当ててはいけません（これらは通常、時間とともに変化します）。
* [Code of Conduct](/school-of-sre/CODE_OF_CONDUCT/)を遵守すること。
* SREの役割と責任に関連していること。
* ローカルでテストされ（テストの手順を参照）、適切にフォーマットされていること。
* Pull Requestを提出する前に、まずIssueを開いて変更点を議論するのは良い習慣です。このようにすることで、他の人のアイデアを事前に取り入れることができます。

### ローカルでのビルドとテスト
PRを作成する前に、以下のコマンドを実行して、ローカルでサイトを構築して表示します。
```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs build
mkdocs serve
```

### PRの作成
[GitHub flow](https://guides.github.com/introduction/flow/)に従ってください。
本リポジトリをフォークしてfeatureブランチを作成し、変更をコミットして本リポジトリにPRを開設します。