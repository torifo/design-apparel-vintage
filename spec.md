# FRAY — design-apparel-vintage Spec

**Status:** Approved  
**Author:** torifo  
**Created:** 2026-05-18  
**Updated:** 2026-05-18

---

## 1. Overview

### Problem Statement
サステナブルファッション・古着好きの若い女性（22–27歳）は、エコ系ブランドのサイトが「地味・説教的・デザインが古い」と感じており、思想には共感しても購買意欲が沸かない。古さを美学として昇華したビジュアル表現が不足している。

### Goal
活版印刷ポスターと古着屋の棚を美学的出発点とした架空ブランド「FRAY」のランディングページを実装する。テクスチャ・セリフ体・アース系カラーで「古さを愛でる」世界観を検証する。

### Non-Goals
- 実際の古着在庫検索
- カート・決済機能
- CMS連携

### Background
- FRAYは本デザイン研究のために作成した架空ブランドであり、実在のブランド・店舗・商品ではない
- `design-apparel-vintage` リポジトリ、`design-apparel-vintage.riumu.net` 独自ドメイン予定
- 同シリーズ4作のうちの1作（他3作と明確に異なるライトモード・温暖色設計）

---

## 2. User Stories

| ID | Persona | Want to | So that |
|----|---------|---------|---------|
| US-01 | vintage（22–27歳女性・クリエイター） | ページを開いた瞬間にブランドの哲学を感じたい | 「このブランドは本物だ」と信頼できる |
| US-02 | vintage | ブランドの思想をじっくり読みたい | 共感してから購入を検討できる |
| US-03 | vintage | 商品の素材・ストーリーを知りたい | 「意志ある消費」ができる |
| US-04 | vintage | スマートフォンでゆっくり閲覧したい | 通勤中でも没入できる |

### Acceptance Criteria (EARS notation)

**US-01: ファーストビューでの哲学提示**
- WHEN ページが読み込まれた THEN オートミール背景・セピア写真・Playfair Displayフォントのヒーローが表示される
- WHEN ヒーローが表示された THEN ブランド名「FRAY」とタグライン「Worn with intention」が読める
- WHEN ページを見た THEN デジタルっぽさが感じられず、印刷物を見ているような質感がある

**US-02: テキスト重視のブラウズ**
- WHEN ユーザーがブランドストーリーに到達した THEN 2段組テキスト（各150字以上）が読める
- WHEN ユーザーが読んだ THEN 素材・製造・サステナビリティへの言及がある

**US-03: 商品とプロセスの確認**
- WHEN ユーザーが商品セクションに到達した THEN 「入荷ラベル」付き商品カードが不規則グリッドで表示される
- WHEN プロセスセクションに到達した THEN 素材→製造→配慮の3ステップが確認できる

**US-04: モバイル対応**
- WHEN 375px幅で閲覧した THEN 全セクションが横スクロールなしで表示される
- WHEN テキストを読む THEN Playfair Display が正しく表示され読みやすい

---

## 3. Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-01 | セピア写真フルスクリーンヒーロー | P0 | picsum + CSS filter |
| FR-02 | 紙テクスチャオーバーレイ | P0 | SVG noise, ライトモード向け薄め |
| FR-03 | 不規則商品グリッド（4商品、入荷ラベル付き） | P0 | |
| FR-04 | ブランドストーリーセクション（テキスト重視） | P0 | 2段組 |
| FR-05 | テクスチャブレークセクション（余白・呼吸） | P1 | テキストなし |
| FR-06 | プロセスセクション（3ステップ） | P1 | 素材・製造・サステナ |
| FR-07 | カテゴリセクション（2カラム） | P1 | |
| FR-08 | フッター（ニュースレター + ミニマルリンク） | P2 | |
| FR-09 | スクロールリビール（opacity主体、ゆっくり） | P1 | translateY 少なめ |
| FR-10 | ナビゲーション（固定しない、活版印刷体ロゴ） | P0 | 固定なしで古道具感 |
| FR-11 | モバイルファースト対応（375px基準） | P0 | |

---

## 4. Architecture

### Page Structure

```
index.html
├── <div class="texture">          # fixed, ペーパーテクスチャ
├── <nav>                          # 固定しない、横長活版ロゴ
├── <section class="hero">         # セピアフルスクリーン + 大型セリフ体
├── <section class="story">        # ブランド思想テキスト（2段組）
├── <section class="shop">         # 不規則商品グリッド
├── <div class="texture-break">    # テクスチャのみ・余白
├── <section class="process">      # 3ステップ
├── <section class="categories">   # 2カラム
└── <footer>                       # ニュースレター + リンク
```

### Component Responsibilities

| Component | Responsibility |
|-----------|---------------|
| Texture overlay | 紙の質感。全体に固定オーバーレイ（ライトモード） |
| Nav | 固定しないことで「ページをめくる」感覚 |
| Hero | セピア写真+セリフ体で世界観を即座に提示 |
| Story | テキストで共感を育てる主役セクション |
| Shop | 不規則配置で「古着を発見する」感覚 |
| Texture break | 呼吸。読者に一息つかせる |
| Process | 透明性の提示。思想の証明 |

### Key Design Decisions

| Decision | Chosen | Rationale | Rejected alternatives |
|----------|--------|-----------|----------------------|
| テーマ | ライトモード（オートミール） | 古着・自然素材の暖かさ | ダークモード（ARCHと被る） |
| Display font | Playfair Display | 活版印刷感、品格あるクラシック | Cormorant Garamond（LUEURに割当済） |
| Body font | DM Serif Text | セリフ体統一で古書感、読みやすい | Georgia（汎用すぎ） |
| カーソル | デフォルト | 古道具屋にデジタルカーソルは不要。思想と一致 | カスタム（デジタルすぎる） |
| Nav固定 | しない | ページをめくる感覚。固定は現代的すぎる | sticky nav（他3サイトと同質化） |
| Grain種類 | ペーパーノイズ（暖色調） | 紙の質感。ARCHの film grain と差別化 | ARCHと同じ blue-gray grain |
| アニメーション | opacity主体（移動少なめ） | 「現れる」感覚。スライドは現代的すぎる | slideUp（ARCHと同質化） |

---

## 5. Design System

### Color Palette
```css
--bg:           #F2EBE0;   /* オートミール・羊皮紙 */
--bg-sub:       #E8DDD0;   /* やや沈んだ生成り */
--bg-card:      #FDFAF6;
--fg:           #2C2018;   /* 深コーヒー */
--fg-muted:     #8B7355;   /* ウォームタン */
--border:       #D4C5B0;   /* くすんだベージュ */
--accent:       #8B5C3E;   /* テラコッタ */
--accent-warm:  #C4914A;   /* アンバー */
--accent-deep:  #3D3028;   /* 深ウォルナット */
```

### Typography
```css
--font-display: 'Playfair Display', 'Noto Serif JP', serif;
--font-body:    'DM Serif Text', 'Noto Serif JP', serif;
--font-italic:  'Playfair Display', italic;  /* 引用・キャプション */
```
- Google Fonts CDN から読み込み
- 全体をセリフ体で統一（body もセリフ）

### Spacing & Motion
```css
--ease: cubic-bezier(0.4, 0, 0.2, 1);  /* ゆったり・機械的 */
/* reveal: opacity 1s, translateY 8px（少なめ） */
/* hero: fade-in 1.2s, no loader animation */
```

---

## 9. Testing Strategy (Visual QA)

| Layer | Scenarios |
|-------|-----------|
| Desktop (1280px) | テクスチャ可視性、2段組テキスト、不規則グリッド配置 |
| Mobile (375px) | シングルカラム表示、セリフ体可読性、横スクロールなし |
| アニメーション | ゆっくりフェードイン確認、loaderなし（直接表示） |
| ホバー | 商品カードセピア解除、ゆっくりズーム |
| フォント | Playfair Display・DM Serif Text 正常適用確認 |

---

## 10. Implementation Notes

- **セピアフィルター**: `filter: sepia(0.6) brightness(0.85) contrast(1.05)` を画像に適用
- **ペーパーテクスチャ**: SVG `feTurbulence` を `position:fixed` 疑似要素、`opacity:0.04`、ライトモード向けに暖色（baseFrequency下げめ）
- **不規則グリッド**: `grid-template-columns: repeat(12, 1fr)` で 1番目→1/5、2番目→5/9 に `margin-top:4rem`、3番目→9/13 という配置
- **ナビ非固定**: `position: static` — スクロールで上に消える（意図的）
- **入荷ラベル**: `transform: rotate(-2deg)` で手書き感、枠線スタイルにstamp風border
- **ローダー不使用**: 直接ヒーロー表示。派手な演出はブランド思想と矛盾

---

## 11. Open Questions

| # | Question | Owner | Due | Status |
|---|----------|-------|-----|--------|
| 1 | ブランドストーリーのテキスト量（現在2段落）は増やすか | torifo | 実装時 | Open |
| 2 | `design-apparel-vintage.riumu.net` のDNS設定タイミング | torifo | 後日 | Open |
| 3 | 商品画像はセピア単色か、わずかに色付きか | torifo | 実装時 | Open |

---

## References

- [spec.md（ナビゲーター）](../spec.md)
- Font: [Playfair Display](https://fonts.google.com/specimen/Playfair+Display), [DM Serif Text](https://fonts.google.com/specimen/DM+Serif+Text)
- Inspiration: [Flamingo vintage](https://flamingo-vintage.com), [Ragtag](https://ragtag.jp)
