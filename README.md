# Microsoft Defender for Cloudを使用したCSPM

## 1. そもそもCSPMとは

**CSPM（Cloud Security Posture Management：クラウドセキュリティ態勢管理）**とは、クラウド環境の設定を継続的に評価し、セキュリティ上の構成不備やリスクを発見・改善するための仕組みである。

例えば、次のような設定をチェックする。

- ストレージアカウントが意図せずインターネットに公開されていないか
- 仮想マシンの管理ポートが広範囲に公開されていないか
- リソースの診断ログや暗号化などが適切に設定されているか

CSPMの基本的な流れは、**基準を定義 → リソースを評価 → 問題を可視化 → 修復 → 再評価**である。
脅威を検知するだけでなく、攻撃を受ける前に設定上の弱点を減らすことが目的となる。

## 2. Microsoft Defender for Cloudとは

**Microsoft Defender for Cloud**は、Azure・AWS・Google Cloudなどのクラウド環境に対して、
セキュリティ態勢管理（CSPM）とワークロード保護（CWPP）を提供するMicrosoftのクラウドセキュリティサービスである。

| 分野 | 主な役割 | 例 |
| --- | --- | --- |
| CSPM | 設定の不備やセキュリティリスクを継続的に評価 | 推奨事項、セキュリティスコア、コンプライアンス評価 |
| CWPP | 稼働するワークロードに対する脅威検知・保護 | Defender for Servers、Defender for Storageなど |

本資料では**CSPM**を中心に説明する。

## 3. Defender for CloudのCSPMプランでできること

Defender for Cloudには、無料の**Foundational CSPM（基本CSPM）**と、有料の**Defender CSPM**がある。

| 機能 | Foundational CSPM（無料） | Defender CSPM（有料） |
| --- | :---: | :---: |
| 資産インベントリ | ○ | ○ |
| Microsoft Cloud Security Benchmarkによる評価 | ○ | ○ |
| セキュリティ推奨事項の表示 | ○ | ○ |
| セキュリティスコアの確認 | ○ | ○ |
| ワークフローの自動化 | ○ | ○ |
| 攻撃パス分析 | ― | ○ |
| リスクに基づく優先順位付け | ― | ○ |
| クラウドセキュリティエクスプローラー | ― | ○ |
| ガバナンスルールによる修復管理 | ― | ○ |
| 高度な規制コンプライアンス評価 | ― | ○ |
| エージェントレスの脆弱性評価など | ― | ○（機能ごとに条件あり） |

### 主な機能の説明

**セキュリティ推奨事項**：セキュリティ基準に適合していないリソースを特定し、改善内容を提示する。推奨事項ごとに対象リソースや修復方法を確認できる。

**セキュリティスコア（Secure Score）**：対象となる推奨事項への対応状況から、セキュリティ態勢を数値で可視化する。すべての推奨事項がスコアに影響するわけではない。

**攻撃パス分析**：リソースの構成、露出、脆弱性などの関係を分析し、攻撃者が重要な資産に到達する可能性のある経路を把握する。

**ガバナンス**：検出した問題を放置しないよう、担当者・期限を設定して修復状況を追跡する。


## 4. ベンチマーク（セキュリティ標準）について

### 4.1 ベンチマークとは

クラウド環境の設定が適切かどうかを評価するための**セキュリティ上の基準・ベストプラクティス**である。

Defender for Cloudでは、適用されたセキュリティ標準に基づいてリソースを継続評価し、
準拠状況や推奨事項を表示する。標準にはMicrosoft提供のもの、業界・規制に基づくもの、組織独自のものがある。

### 4.2 Microsoft Cloud Security Benchmark（MCSB）

**Microsoft Cloud Security Benchmark（MCSB）**は、Microsoftが提供するクラウド向けセキュリティベストプラクティスの体系である。ネットワークセキュリティ、ID管理、データ保護、ログ・脅威検出などの分野を扱う。

- Azureでは、Defender for Cloudの有効化により、**MCSBが既定のセキュリティ標準として有効になる**。
- 対象リソースが評価され、問題があれば推奨事項が生成される。
- MCSBの一部の推奨事項はセキュリティスコアの算出にも使用される。
- MCSBは「評価基準」であり、すべての設定を自動修復する機能ではない。

**例：**「管理ポートをインターネットに公開しない」「データ保護に適切な設定を適用する」などの観点で構成を評価する。

### 4.3 Azure CSPMとは

**Azure CSPM**は、Defender for Cloudのセキュリティ標準一覧に表示される、Azure向けのセキュリティ評価項目をまとめた**イニシアティブ（評価項目の集合）**である。

**MCSBではAzure Policyを利用した構成評価が中心である一方、Azure CSPMにはAzure Policyのポリシー定義による評価ではなく、<br>
Defender for Cloud自身の機能や収集したセキュリティ情報から生成される推奨事項が含まれる**点である。

つまり、MCSBとはAzureに由来するのか、Defenderに由来するのかが異なる。

| 比較項目 | MCSB | Azure CSPM |
| --- | --- | --- |
| Defender for Cloud上での位置づけ | セキュリティ標準／イニシアティブ | セキュリティ標準／イニシアティブ |
| 評価の主な仕組み | Azure Policyを利用した構成評価にDefender for Cloudの評価を含む | Defender for Cloudの評価のみ含む |
| Azure Policyとの関係 | 対応するAzure Policyイニシアティブを利用 | 別の管理軸となる |
| 推奨事項の表示 | Defender for Cloudで確認 | Defender for Cloudで確認 |

### 4.4 その他の組み込みセキュリティ標準

必要に応じて、次のような標準を追加できる（対応クラウドや利用条件は標準ごとに異なる）。

- CIS Microsoft Azure Foundations Benchmark
- NIST SP 800-53
- PCI DSS
- ISO 27001など、利用可能な標準・マッピング（提供状況はポータルで確認）

これらの規制コンプライアンス評価には、CSPMの有料プランの有効化が飛鳥である。

### 4.5 Azure Policyをオンボードして独自の評価基準を追加する方法

組織独自のルールを評価したい場合は、Azure Policyの定義を利用できる。

**概略手順（レガシ方式）：**

1. Azure Policyで、組織のセキュリティ要件に対応したポリシー定義を作成する。
2. 複数の定義をまとめたイニシアティブを作成する。
3. Defender for Cloudの環境設定・セキュリティポリシー関連画面から、カスタム標準としてオンボードする。
4. 対象スコープで評価結果と推奨事項を確認する。

**重要：** Microsoftは、Azure Policyによるカスタム推奨事項・標準の作成を**レガシ方式**として案内しており、新しいカスタム推奨事項機能を推奨している。新規構築では、利用できるカスタム推奨事項の作成方式、ライセンス要件、ポータルでの操作手順を確認して選択する。


## 5. ガバナンスルールについて

### 5.1 概要

**ガバナンスルール（Governance Rules）**は、Defender for Cloudが検出した推奨事項に対して、**修復担当者と修復期限を自動的に割り当てる**機能である。有料のDefender CSPMプランが必要となる。

推奨事項を検出するだけでなく、対応を担当者のタスクとして管理し、期限超過を追跡できる。

### 5.2 主な機能

| 機能 | 内容 |
| --- | --- |
| 対象条件の指定 | 重大度、リスクレベル、カテゴリ、特定の推奨事項などを指定 |
| 担当者の割り当て | メールアドレス、リソースタグに基づいて所有者を設定 |
| 修復期限の設定 | 7日、14日、30日、90日などの期間を指定 |
| 通知 | 担当者に未対応・期限超過のタスクをメール通知（既定では週次） |
| 進捗管理 | 所有者・サブスクリプション・推奨事項別に対応状況を追跡 |
| ガバナンスレポート | 期限内、期限超過、未割り当てなどを確認 |
| 猶予期間 | 条件を満たすMCSB推奨事項について、期限までセキュリティスコアへの影響を猶予できる |

### 5.3 活用例

**要件：** 重大度が高い推奨事項は、担当者を決めて14日以内に修復したい。

```text
Defender for Cloudがセキュリティ上の問題を検出
                      ↓
            推奨事項を生成
                      ↓
       ガバナンスルールの条件に一致
                      ↓
          担当者・期限を自動割り当て
                      ↓
             メールで通知
                      ↓
         担当者が設定を修復
                      ↓
          推奨事項を再評価
                      ↓
         対応状況をレポートで確認
```

### 5.4 設定手順（概要）

1. 対象サブスクリプションで**Defender CSPM**プランを有効化する。
2. Azure Portalで **Microsoft Defender for Cloud → 環境設定 → ガバナンスルール** を開く。
3. **ガバナンスルールの作成**を選択する。
4. ルール名、適用スコープ、優先度を指定する。
5. 対象となる推奨事項または重大度・リスクレベルを指定する。
6. 担当者（メールアドレスまたはリソースタグ）を設定する。
7. 修復期限、通知、必要に応じて猶予期間を設定する。
8. 保存後、ガバナンスレポートで割り当てと対応状況を確認する。

**注意点：**

- ガバナンスルールは、原則として**修復作業そのものを自動実行する機能ではない**。
- 既存の推奨事項へ適用する際は、ルールの再実行や既存割り当ての扱いを確認する。
- 複数ルールが競合する場合は、スコープやルールの優先度に従って処理される。
- メール通知には所有者の設定や通知条件が関係する。運用開始前にテストする。

## 6. 運用イメージとまとめ

Defender for Cloudを用いたCSPMの運用は、以下のように整理できる。

``` mermaid
flowchart TD
    A["① 基準を定める<br/>MCSB・追加のセキュリティ標準<br/>カスタム推奨事項"]
    B["② 評価する<br/>CSPM・セキュリティ推奨事項"]
    C["③ 優先順位を決める<br/>Secure Score・リスク優先順位付け"]
    D["④ 修復を管理する<br/>ガバナンスルール<br/>担当者・期限の割り当て"]
    E["⑤ 改善を確認する<br/>推奨事項・ガバナンスレポート"]

    A --> B
    B --> C
    C --> D
    D --> E
    E -->|"継続的な見直し・改善"| A

    classDef standard fill:#dbeafe,stroke:#2563eb,color:#1e40af
    classDef evaluate fill:#d1fae5,stroke:#059669,color:#065f46
    classDef prioritize fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef remediate fill:#ffedd5,stroke:#ea580c,color:#9a3412
    classDef improve fill:#ede9fe,stroke:#7c3aed,color:#5b21b6

    class A standard
    class B evaluate
    class C prioritize
    class D remediate
    class E improve

```

**要点：**

- CSPMはクラウド設定を継続評価して、セキュリティ態勢を改善する仕組みである。
- Defender for Cloudでは、無料のFoundational CSPMだけでもMCSBによる基本評価を始められる。
- 高度なリスク分析、ガバナンス、規制コンプライアンスには有料のDefender CSPMなどの前提条件がある。
- 独自基準はカスタム推奨事項やAzure Policyとの連携で拡張できるが、レガシ方式と新方式の違いに注意する。
- ガバナンスルールにより、推奨事項を**「発見して終わり」ではなく「担当者が期限までに対応する」運用**につなげられる。

## 7. 参考資料（Microsoft Learn）

- [クラウド セキュリティ体制管理（CSPM）とは](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/concept-cloud-security-posture-management)
- [Microsoft Defender for Cloudの概要](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/defender-for-cloud-introduction)
- [Defender for Cloudの規制コンプライアンス](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/concept-regulatory-compliance-standards)
- [Microsoft Cloud Security Benchmark](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/concept-regulatory-compliance)
- [カスタム標準と推奨事項の作成](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/create-custom-recommendations)
- [ガバナンスルール](https://learn.microsoft.com/ja-jp/azure/defender-for-cloud/governance-rules)