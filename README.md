# AIMF — AIセキュリティ＆ガバナンス 統合マスターフレームワーク

**バージョン: v0.1.0-beta（ベータ版）**

組織がAIシステム（従来型AI／生成AI・RAG／AIエージェント／マルチエージェント）を導入・運用する際に必要となるセキュリティ・ガバナンス管理策を、**NIST CSF 2.0の6機能 × 37カテゴリ × 207コントロール**として体系化したフレームワークです。国内外**19の基準・公的文書**との対応関係を機械可読（OSCAL）形式で保持しています。

> ⚠️ **β版について**: 本フレームワークはβ版として公開しています。今後、コントロールの追加・削除・ID変更を含む**破壊的変更**が行われる可能性があります。実務利用の際はバージョンを固定して参照してください。

## なぜ作ったか

この2〜3年でAIセキュリティの基準・ガイドラインが一斉に登場しましたが、対象（LLMのみ／エージェントのみ／マネジメント全般）も粒度も観点もバラバラで、「結局、自組織は何を・どこまでやればいいのか」を判断するには19の文書を行き来する必要がありました。本フレームワークは、これらを**ひとつの実務フレームワークに統一する試み**です。

## リポジトリ構成

| パス | 内容 |
|---|---|
| `catalog/aimf-catalog.json` / `.yaml` | フレームワーク本体（OSCAL Catalog 1.1.2形式、207コントロール） |
| `catalog/aimf-controls-list.csv` | コントロール一覧（ID・区分・カテゴリ・control-type・ai-scope等、Excelで閲覧可） |
| `docs/framework-guide.docx` | 解説文書・利用ガイド（構造・分類軸・19基準の要点・jqクエリ例・全コントロール一覧） |
| `docs/roadmap-tool-guide.docx` | 評価ツールの利用ガイド |
| `tool/aimf-roadmap-tool.xlsx` | 207問アセスメント＆ロードマップ作成ツール（Excel・数式のみで動作、マクロなし） |
| `slides/aimf-overview.pptx` | 概要説明スライド（12枚） |

## フレームワークの特徴

- **NIST CSF 2.0整合**: GV/ID/PR/DE/RS/RC の6機能構造。コントロールIDは `AIMF-<区分>-<カテゴリ3文字>-<連番>`（例: `AIMF-PR-AGT-004`）
- **19基準マッピング**: マッピング対象8基準（AI事業者GL、総務省AIセキュリティGL、NIST AI RMF、ISO/IEC 42001、OWASP AISVS、CSA AICM、EU AI Act、NCSC/CISA）＋参照ソース11文書（MITRE ATLAS、OWASP LLM/Agentic Top 10、ASDエージェントAIガイダンス、NIST SP 800-63-4、CIS Controls v8.1、Five Eyes声明 ほか）
- **control-type分類**: 全コントロールに `AI-Expansion`（AI特有の新設管理策・108件）／`AI-Enhancement`（既存セキュリティのAI拡張・99件）を付与
- **ai-scope分類**: 従来型AI／生成AI／エージェント／マルチエージェントの適用類型を判別可能
- **エージェントAI重視**: ASD等6機関ガイダンスの5リスクカテゴリとOWASP ASI01〜10を実装可能なコントロールへ展開

## クイックスタート

**眺める**: `catalog/aimf-controls-list.csv` をExcelで開くのが最速です。

**評価する**: `tool/aimf-roadmap-tool.xlsx` を開き、`00_はじめに`でAI利用プロファイルを設定 → `01_評価入力`で現状評価（0〜4）を入力 → `11_ダッシュボード`で結果を確認。

**プログラムから使う**（jq例・詳細は docs/framework-guide.docx 第7章）:

```bash
# AI-Expansion（AI特有統制）のみ抽出
jq '[.catalog.groups[].groups[].controls[] | select(.props[] |
  select(.name=="control-type" and .value=="AI-Expansion")) | {id, title}]' catalog/aimf-catalog.json
```

## 作成プロセスとフィードバックについて

本フレームワークは**人間とAI（Claude）の共同作成**です。人間が課題設定・原フレームワークの執筆・方針決定・採否判断を、AIが基準との突合・マッピング起案・拡張ドラフト・文書生成を担いました（詳細は `slides/aimf-overview.pptx` 参照）。その特性上、誤りや解釈の偏りが含まれ得ることをご承知おきください。

β版として公開しています。お気づきの点があれば、[Issues](../../issues) から気軽にお知らせください。コントロールID（例: `AIMF-PR-AGT-004`）を添えていただけると助かります。修正のPull Requestも受け付けています。

## ライセンスとサードパーティ表示

- 本プロジェクトの**オリジナルコンテンツ**（コントロール定義、マッピングの対応関係、文書、ツール）: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.ja)
- 本リポジトリは各参照基準への**参照（番号・IDによる対応関係）のみ**を収録し、基準本文・条項タイトル等のテキストは含みません。特にISO/IEC規格およびCIS Controlsは**番号のみ**で参照しています。各基準の著作権・商標・利用条件は [NOTICE.md](NOTICE.md) を参照してください。
- 本フレームワークを商用サービスへ組み込む場合は、NOTICE.md記載の各権利者の条件（特にISO/IEC・CIS）をご確認ください。

## 免責

本フレームワークは特定の法規制への準拠を保証するものではありません。EU AI Act等への対応は、必ず原文および専門家の助言に基づいて判断してください。
