# 「さりげなく壊れるラジオ体操」先行事例・リスク調査

調査日：2026-11-02（JST）．対象 PoC：単一 index.html，SVG 人体，Web Audio シンセ，日本語 TTS，公開リポジトリ Sh1n1230/ai-radio-taiso．「古い変な AI が作ったような」ラジオ体操で，各運動に 1 つだけ，誰も言及しない小さな破綻（背伸びの運動で腕が浮いたまま，など）を仕込む．

表記の約束：
- 【確認済】は該当ページを開いて本文を確認したもの．
- 【抜粋のみ】は検索結果の抜粋でしか確認していないもの（本文未読）．
- 【推論】は筆者（Hark）の解釈で，出典の記述ではないもの．
- 【未確認】は裏づけが取れなかったもの．
- 法的な記述は公開情報の整理であり，法的助言ではない．

---

## 0. 要点（先に結論）

1. **最大のリスクは楽曲である．** 現行「ラジオ体操第一」の伴奏曲（服部正作曲，1951 年）の著作権はかんぽ生命が保有しており，同社は「楽曲や動きを改変せず，正しいラジオ体操を行えるものであること」「『健全・健康なイメージ』を損なうものでないこと」を許諾基準に挙げている．壊れる体操はこの基準と正面から衝突するため，許諾は期待しにくい【推論】．Web Audio で自分で演奏し直しても，旋律を再現すれば作曲の著作権（複製・公衆送信）の問題は残る【推論】．**旋律は自作にする**のが安全側である．
2. **商標「ラジオ体操」の単独登録は 1 件だけ**（登録 4550800，かんぽ生命，第 16 類「雑誌，新聞」，存続中・満了 2032-03-15）．ただし日本郵政が「ラジオ体操コンクール」等を第 36・41 類で登録済みで，2025-11-20 には「ラジオ体操アプリ」等を第 9・41・42 類に出願中である（J-PlatPat で確認）．無料 OSS の題名に「ラジオ体操」を記述的に使うこと自体の危険度は高くないと考えるが【推論】，第 9 類（ソフトウェア）出願が登録されれば状況が変わりうる．題名・リポジトリ名の再検討を推す．
3. **笑いの設計は先行事例と整合している．** 8番出口（2023），I'm on Observation Duty（2018），cursed images（2015〜）はいずれも「ほぼ普通，一点だけおかしい，誰も説明しない」型で広まった．一方 Will Smith eating spaghetti（2023）は「全部が壊れている」型の典型である．ユーザーが観察した「派手な変形は笑えず，さりげない破綻が笑える」は，前者の系譜に PoC を置くという判断と読める【推論】．
4. **技術面の要注意点は 3 つ．** (a) 音は AudioContext.currentTime 基準の先読みスケジューラ（25 ms 間隔・100 ms 先読みが出発点），(b) 日本語 TTS は OS 依存で，Chrome では初回 getVoices() が空，オンライン音声は約 15 秒で切れる報告あり，(c) 5 秒を超えて自動で動くアニメーションには停止手段が必要（WCAG 2.2.2），prefers-reduced-motion への対応が必要．

---

## 1. 先行事例の比較表（15 件）

| # | 名称 | 年 | 媒体 | 奇妙さの種類 | さりげない／派手 | 学べること | 出典の確認度 |
|---|---|---|---|---|---|---|---|
| 1 | Will Smith eating spaghetti（chaindrop） | 2023 | ModelScope text2video による 20 秒の動画 | 顔の変形，麺が口を貫通，人物が二人になる | 派手 | 全面崩壊型は「悪夢」「haunt」と受け止められた．恐怖寄りの反応が出る．後に「AI 動画の進歩を測る基準」になり，本人がパロディ動画を出した（2024）．PoC が避けたい方向の基準点になる． | 【確認済】Ars Technica．KYM・BBC は【抜粋のみ】 |
| 2 | Pepperoni Hug Spot（Pizza Later） | 2023 | GPT-4・Midjourney・Runway Gen-2・ElevenLabs 等で作った 30 秒 CM | CM の型は完全に守り，ナレーションの文法（"best pizza of life"），表情，チーズの伸びがおかしい | 中間（型は正常，細部が異常） | 「型の忠実さ」と「中身のズレ」の組み合わせ．PoC の「NHK 番組の型を守る」方針と同じ構造．Pizza Hut 公式が反応して拡散． | 【抜粋のみ】Business Insider，Today，公式サイト |
| 3 | Everybody Dance Now（Chan ほか，UC Berkeley） | 2018 論文／ICCV 2019 | 研究動画（姿勢推定経由のダンス転写） | 姿勢推定の誤りによる手足の崩れ，影を人と誤認した「余計な踊り手」 | 研究としては正常，失敗例はさりげない | 「AI が人の動きを写し損ねる」実在の失敗パターンの参照元．破綻を考案するとき，実在の失敗様式（関節の取り違え，遮蔽で消える腕）から選ぶと「それらしさ」が増す【推論】． | 論文要旨は【抜粋のみ】（arXiv 1808.07371）．失敗例の記述は個人ブログ【抜粋のみ】 |
| 4 | AI Weirdness（Janelle Shane） | 2016〜（書籍 2019） | ブログ・書籍 | 学習した AI が真面目にもっともらしい誤答を出す（塗料名 "Stanky Bean" など） | さりげない（文章） | 結果を真顔で並べる提示法．AI の「誤りの性質」を説明できる教材としても読まれた．説明責任の観点で，README で「なぜこう壊れるのか」を語れる． | 【抜粋のみ】aiweirdness.com，IEEE Spectrum |
| 5 | Cursed images | 2015〜 | Tumblr → Twitter → Reddit の画像ミーム | 何の変哲もない場面なのに「なぜこれが存在するのか」が分からない | さりげない | 初投稿者の言葉「どの部分も本質的には不穏ではない」が核心．不穏さは文脈と画質から生まれる． | 【抜粋のみ】Wikipedia，PAPER，KYM |
| 6 | The Backrooms／Kane Pixels「The Backrooms (Found Footage)」 | 2019（4chan）／2022（YouTube） | 画像ミーム → Blender 製のファウンドフッテージ短編 | リミナルスペース，VHS 風の劣化 | さりげない（雰囲気） | 「古い媒体の質感」が説得力を出す．PoC の「古い AI」設定にも，低解像度・劣化音声などの質感が効く可能性【推論】．YouTube 再生は約 9,700 万回． | YouTube ページは【確認済】（再生数表示）．経緯は【抜粋のみ】 |
| 7 | I'm on Observation Duty（Notovia） | 2018 | PC ゲーム | 監視カメラ映像の中の家具の移動など「異変」を見つけて報告する | さりげない | 「間違い探し」をゲームの規則にした原型．8番出口の開発者が影響を明言している． | 【抜粋のみ】Steam，itch.io |
| 8 | 8番出口（KOTAKE CREATE） | 2023 | PC ほか（ウォーキングシミュレーター） | 地下通路の「異変」を見逃さず引き返す | さりげない（一部は派手な異変もある） | 最重要の参照先．単純な規則と小さな異変で大ヒット（PC 版 50 万本：2024-04，全機種累計 200 万本以上：2025-12 の発表）．日本ゲーム大賞 2024 ブレイクスルー賞．「8番ライク」という亜ジャンルを生んだ．実況・二次創作で拡散． | AUTOMATON 2024-04-18 は【確認済】．売上 200 万本・受賞は【抜粋のみ】（GameWith，CESA） |
| 9 | QWOP（Bennett Foddy） | 2008 | ブラウザゲーム | 太もも・ふくらはぎを個別キーで操作するため走れない | 派手（身体の崩れ） | 「わざと悪い操作系」で身体を崩す喜劇．走者の真顔とのギャップ．プレイヤーが QWOP の動きを真似る動画が生まれた． | foddy.net は【抜粋のみ】，BBC Future【抜粋のみ】 |
| 10 | Human: Fall Flat（No Brakes Games）／Gang Beasts（Boneloaf） | 2016／2014〜2017 | 物理演算パズル・格闘 | ふにゃふにゃの体，手順アニメーションの事故 | 派手 | 物理演算由来のドタバタは「派手側」の代表．PoC が手順アニメーションを使う場合，破綻を物理任せにすると派手寄りになりやすい【推論】． | 【抜粋のみ】IGN，The Guardian，公式 |
| 11 | ラヂオ体操第4（SSF／Reebok「TAIKAN」） | 2010 | YouTube 動画（スポーツウェアのゲリラ広告） | 昭和の体操番組の型で，アクロバットを真顔で続ける | 内容は派手，演出は真顔 | ラジオ体操パロディの代表例．「70 年ぶりの続篇らしい」という真顔の説明文．企業の合法的投稿だった．再生約 700 万回．楽曲・権利処理の詳細は【未確認】． | YouTube 説明文は【抜粋のみ】．Reebok との関係は個人ブログ【抜粋のみ】 |
| 12 | 幻のラジオ体操第3（2 代目，1946〜47） | 1946／復刻 2015 | 実在の体操（日本コロムビアが復刻 CD/DVD） | 実在するのに知られていない「正規の異本」 | 実在 | 「横 肩 上」と早口の号令がラジオでは伝わらず短命に終わった．「公式なのに奇妙」な実例として，PoC の README で文脈付けに使える． | かんぽ生命 FAQ・全国ラジオ体操連盟は【抜粋のみ】．レタスクラブ【抜粋のみ】 |
| 13 | 81プロデュース「ラジオ体操第1」アテレコ | 2020 | YouTube（声優が号令を担当） | 号令を人気声優の役の声で行う | さりげない（声だけ差し替え） | 「号令の声だけ変える」変奏．コロナ禍の在宅運動需要で公開．権利処理の有無は【未確認】． | 【抜粋のみ】ねとらぼ |
| 14 | なつもん！ 20世紀の夏休み（スパイク・チュンソフト）／ぼくのなつやすみ | 2023／2000〜 | ゲーム内のラジオ体操 | 朝の儀式としてのラジオ体操．なつもん！では第1をフル収録し，NPC の動きが両脚跳びあたりでずれる | さりげない（意図的な不揃い） | NPC の「ずれ」が自然な味として紹介されている．ずれ自体は笑いではなく生活感として受容される．破綻と「味」の境界の参考になる． | 【抜粋のみ】電撃オンライン，Nintendo 公式 |
| 15 | かんぽ生命「ラジオ体操アプリ」 | 2025-11-01 | 公式アプリ（iOS・Android・ブラウザ版） | AI が利用者のラジオ体操第一を撮影して採点する | 正常 | 「AI が人間を正しく採点する」公式アプリと，「AI 自身が正しくできない」PoC は対照になる．同時に，公式が AI×ラジオ体操の領域にいるという事実でもあり，商標出願（第 9・41・42 類）と時期が重なる． | プレスリリース PDF・App Store は【抜粋のみ】 |

補足：
- 依頼にあった Untitled Goose Game，Getting Over It，Superliminal は今回は詳しく調べていない．Getting Over It は QWOP と同じ作者 Bennett Foddy の作品で，WIRED が「暗く可笑しい」と評している【抜粋のみ】．
- VTuber・MMD のラジオ体操動画は多数あるが，個別の権利処理は確認していない【未確認】．

---

## 2. 先行事例から読み取れること

### 2.1 何が笑いを生んだか

- **型の忠実さ×細部の異常．** Pepperoni Hug Spot は CM の構成，BGM，ナレーションという型を完全に守ったうえで，文法と表情だけが壊れていた．ラヂオ体操第4 も昭和の番組の画面・説明を真顔で守った．PoC が NHK 番組の型（ピアノ伴奏，号令，体操名の読み上げ）を守るほど，一点の破綻が効くと考えられる【推論】．
- **誰も破綻に言及しない．** 8番出口では異変について説明がなく，プレイヤーが自分で気づく．cursed images も説明を付けない．PoC の「unacknowledged glitch」はこの系譜に入る．
- **全面崩壊は恐怖に傾く．** Will Smith の動画は Vice が「一生忘れられない」，Forbes が「悪魔的」と書いた．笑いと同時に不快感が出ている．PoC で派手な変形が笑えなかった観察と整合する【推論】．

### 2.2 どう広まったか

- Will Smith：Reddit r/StableDiffusion（2023-03-23 投稿）→ X の転載で拡散（Ars Technica は「月曜に Reddit に投稿され，すぐ他の SNS に広がった」と記述）．
- Pepperoni Hug Spot：Reddit → X → Pizza Hut 公式アカウントが反応．
- 8番出口：Steam 発売前から X で注目，発売後はゲーム実況と二次創作，「8番ライク」作品群．
- 共通点は「短く，一目で分かる」こと．PoC も 1 運動ごとに切り出せる短尺（数秒〜十数秒）にすると共有しやすい【推論】．

### 2.3 PoC への具体的な示唆【推論】

- 異変は「見つけた人が言いたくなる」大きさに調整する．8番出口型なら，気づいた人に報酬がある（例：最後に異変の一覧を出す，または出さない）．出すか出さないかは作品の態度の選択で，出さない方が「誰も言及しない」設定に忠実．
- 「古い AI」の説得力は，実在した AI の失敗様式（姿勢推定の左右取り違え，遮蔽された手足の消失，フレーム間の不連続）から破綻を選ぶと増す．Everybody Dance Now 系の失敗例が参照元になる．
- ナレーションの軽い文法崩れ（Pepperoni Hug Spot の "best pizza of life"）は TTS 側の破綻候補になる．ただし号令の文言を NHK のものに寄せすぎると著作権の論点が出る（4.3 節）．

---

## 3. ラジオ体操の公式・変種・ゲームでの扱い

### 3.1 公式の体系【確認済／抜粋のみ】

| 体操 | 制定 | 作曲 | 楽曲の権利（かんぽ生命の説明） |
|---|---|---|---|
| ラジオ体操第一（3 代目） | 1951 年 | 服部正 | 全ての著作権をかんぽ生命が保有【確認済】 |
| ラジオ体操第二（3 代目） | 1952 年 | 團伊玖磨 | 放送に関する権利はかんぽ生命と NHK，その他は JASRAC 管理【確認済】 |
| みんなの体操 | 1999 年 | 佐藤敏彦 | JASRAC【確認済】 |
| ラジオ体操の歌 | 1956 年 | 作詞 藤浦洸，作曲 藤山一郎 | JASRAC【確認済】 |

- 出典：かんぽ生命「ラジオ体操の楽曲使用について」，全国ラジオ体操連盟 FAQ．
- 服部正は 1908-03-17 生，2008-08-02 没【抜粋のみ：Wikipedia，日刊スポーツ，NDL 典拠】．作者の死後 70 年とすると，第一の伴奏曲の保護は 2078 年末までとなる【推論：保護期間の計算は筆者】．
- 現行の第一・第二はいずれも 3 代目．初代は 1928 年，2 代目（第一〜第三）は 1946〜47 年で短命に終わった【抜粋のみ：NHK FAQ，全国ラジオ体操連盟】．

### 3.2 パロディ・変種

- ラヂオ体操第4（2010，表の #11），声優アテレコ版（2020，#13），MMD・VTuber 版多数，「幻の第3」の復刻（2015），日本コロムビアの方言版「ご当地版ラジオ体操」【抜粋のみ：ENCOUNT】．
- アニメ『ひだまりスケッチ×☆☆☆』（2010）では各話冒頭にラジオ体操の描写がある【抜粋のみ：個人ブログ】．
- さだまさし「きだまきしとTake It All JAPAN」名義のアルバム（2012）に「レイディオ体操」【抜粋のみ：Wikipedia】．中身は未確認．

### 3.3 ゲーム・アプリ

- ぼくのなつやすみ シリーズ（朝のラジオ体操が日課）【抜粋のみ】．
- なつもん！（2023，第1 をフル収録，NPC の動きが不揃い）【抜粋のみ】．
- かんぽ生命「ラジオ体操アプリ」（2025-11-01 提供開始，AI 採点）【抜粋のみ】．

---

## 4. 日本での法的・IP リスク

### 4.1 商標（J-PlatPat，2026-11-02 検索）【確認済】

検索条件：商標（検索用）「ラジオ体操」2 件，称呼（単純文字列）「ラジオタイソー」9 件，「ラヂオ体操」0 件．

| 番号 | 商標 | 権利者 | 区分 | 状態 |
|---|---|---|---|---|
| 登録 4550800 | ラジオ体操（標準文字） | 株式会社かんぽ生命保険 | 16（雑誌，新聞） | 存続中，2021 年に更新，満了 2032-03-15 |
| 登録 6450938 | ラジオ体操コンクール | 日本郵政株式会社 | 36，41 | 存続中 |
| 登録 6450939 | デジタルラジオ体操コンクール | 日本郵政株式会社 | 36，41 | 存続中 |
| 登録 6450940 | かんぽ生命 デジタルラジオ体操コンクール | 日本郵政株式会社 | 36，41 | 存続中 |
| 登録 6462377 | デジタル＼ラジオ体操＼コンクール（図形・色彩あり） | 日本郵政株式会社 | 36，41（第 41 類に「インターネットを利用した体操の教授」等） | 存続中，満了 2031-10-27 |
| 商願 2025-133678 | ラジオ体操アプリ | 日本郵政株式会社 | 09，41，42 | 審査中（2025-11-20 出願） |
| 商願 2025-133679 | かんぽラジオ体操アプリ | 日本郵政株式会社 | 09，41，42 | 審査中 |
| 商願 2025-133680 | デジタルラジオ体操アプリ | 日本郵政株式会社 | 09，41，42 | 審査中 |
| 商願 2025-133681 | ラジオ＼体操＼かんぽ生命 | 日本郵政株式会社 | 09，41，42 | 審査中 |

- NHK 名義の「ラジオ体操」商標は検索結果に無かった．
- 「ラジオ体操」単独の登録は第 16 類（紙媒体）だけで，ソフトウェア（第 9 類），娯楽・教授（第 41 類），Web サービス（第 42 類）には単独登録が無い．
- 解釈【推論，法的助言ではない】：
  - 商標権は指定商品・役務と同一・類似の範囲で，出所表示として使う場合に及ぶ．PoC が無料の OSS で，「ラジオ体操」を「題材を説明する言葉」として使う限り，現状の登録で直ちに問題になる可能性は低いと考える．
  - ただし日本郵政の第 9・42 類出願が登録された後に，リポジトリ名やページ題名を「○○ラジオ体操」として出所表示的に使うと論点になりうる．
  - キャラクター「ラタ坊」は全国ラジオ体操連盟が著作権を持ち，使用に申請が必要【確認済：連盟 FAQ・ラタ坊ページは抜粋】．使わない．

### 4.2 楽曲の著作権と，かんぽ生命の許諾基準【確認済】

かんぽ生命「申請手続きについて」の審査観点（原文要約）：
- 正しいラジオ体操の普及推進に寄与すること．
- 直接的にも間接的にも営利目的としないこと．
- 「健全・健康なイメージ」を損なわず，公序良俗に反しないこと．
- **ラジオ体操の楽曲や動きを改変せず，正しいラジオ体操を行えるものであること**（楽曲のイメージを著しく損なう様態など，著作者人格権の侵害にあたる場合は許諾できない）．
- 営利目的でなくても「複製・譲渡」「公衆送信（インターネット配信・公開等）」は申請が必要．
- 音源・動画・楽譜をオリジナル制作する場合はデモの事前提出が必要．アバター等のキャラクターを使う動画も事前提出．
- 個人の動画投稿で申請不要なのは，収益化していないチャンネルで個人の楽しみの範囲でラジオ体操第一を行っている場合のみ．YouTuber・VTuber・収益化チャンネルは申請が必要．
- 著作権法 30〜50 条（権利制限）に該当する場合は申請不要．

NHK の Q&A も，第一は「かんぽ生命」が著作権を持ち，音源は別に許諾が要る（市販 CD の発行元，NHK 音源は NEP 経由の有償提供）と説明している【確認済（抜粋本文）】．

PoC への当てはめ【推論】：
- 壊れる体操は「動きを改変」「正しいラジオ体操を行えない」に当たり，許諾基準を満たさない可能性が高い．申請しても許諾は得にくいと見るのが妥当．
- Web Audio で一から合成しても，著作隣接権（演奏・レコード）は回避できるが，作曲の著作権は回避できない．全国ラジオ体操連盟の 1 級指導士の記事も「音源を自作すれば著作隣接権は自分が持つ」と述べており，楽曲の著作権は別論である【抜粋のみ：ラジーン】．
- 公開 Web ページは公衆送信に当たり，「個人の楽しみ」の例外にも入らない．

### 4.3 号令・ナレーションと動き

- 号令の文言：NHK 番組の号令台本は言語の著作物になりうる．逐語的に再現すると問題になりうるという意見がある【抜粋のみ：Wikipedia ノート，権威ある出典ではない】．「背伸びの運動」のような運動名そのものは短い記述で創作性が乏しいと考えられるが【推論】，裁判例・公式見解は確認できていない【未確認】．
- 動き（振付）：舞踊の著作物として保護されうるかは論点だが，体操という機能的な動作の著作物性について確かな出典は得られなかった【未確認】．かんぽ生命の基準は「動きを改変しない」ことを許諾条件にしているが，これは許諾を求める場合の条件であり，動きが著作物であるという意味ではない【推論】．
- かんぽ生命はラジオ体操の指導や解説を有資格者以外が行うことを，教本・映像制作について認めていない（許諾の文脈）【確認済】．PoC は「指導」を名乗らない方がよい．

### 4.4 パロディの扱い（日本法）【抜粋のみ】

- 日本の著作権法にはパロディを明示的に許す規定がない．文化庁パロディワーキングチーム報告書（2013-03）は，現時点で立法措置を急ぐべきではないという整理をした（報告書 PDF の本文は未読，抜粋と NDL カレントアウェアネスで確認）．
- 解釈上は引用（32 条）や「本質的特徴の直接感得」の有無で判断され，基準は確立していない（大阪大学論文，法律事務所コラム【抜粋のみ】）．
- 二次創作は権利者の黙認に支えられている面が大きいという指摘がある（福井健策弁護士，弁護士ドットコム【抜粋のみ】）．「パロディだから大丈夫」とは言えない．

### 4.5 リスク一覧と推奨対策

| # | リスク | 根拠 | 深刻度【推論】 | 推奨対策 |
|---|---|---|---|---|
| R1 | 第一の伴奏曲の旋律を再現すると著作権侵害（複製・編曲・公衆送信），改変により同一性保持権の論点 | かんぽ生命の許諾基準，保護期間（推定 2078 年末まで） | 高 | **旋律・和声進行を自作する．** 「ピアノ伴奏，行進曲風，ニ長調，2 拍子」程度の様式模倣にとどめ，主題の音程・リズム型を似せない．自作である旨と作曲過程を README に残す（説明責任）． |
| R2 | 公式の権利者が「健全なイメージを損なう改変」として問題視する | かんぽ生命の基準文言 | 中 | 非公式であることを明記（「かんぽ生命・NHK・全国ラジオ体操連盟とは無関係です」）．公式ロゴ，ラタ坊，NHK 番組の画面構成の直接的模写は使わない． |
| R3 | 商標「ラジオ体操」関連 | J-PlatPat：単独登録は 16 類のみ，日本郵政が 9・41・42 類に出願中 | 低〜中（出願の帰趨次第） | 題名は「AI が覚えた朝の体操」等の別名にし，「ラジオ体操」は説明文中の記述的使用にとどめる．リポジトリ名 ai-radio-taiso は現状なら問題が顕在化しにくいと考えるが，第 9 類登録が出たら見直す． |
| R4 | 号令の逐語再現 | 言語の著作物になりうる（出典は弱い） | 中 | 号令文は自作する．運動名は一般的な記述にとどめ，NHK 放送の言い回し（「腕を前から上にあげて，大きく背伸びの運動から」等）を逐語で使わない． |
| R5 | 誤った動きを「正しい体操」と誤認して真似る | かんぽ生命が「正しいラジオ体操」を重視 | 低 | 「実際の体操の参考にしないでください」と明記．危険な姿勢（首の過伸展など）を破綻として採用しない． |
| R6 | 光の点滅による発作，動きによる前庭障害 | WCAG 2.3.1，2.3.3，MDN | 中 | 1 秒に 3 回を超える点滅をしない．画面全体の急な拡大・パンを避ける．prefers-reduced-motion で破綻演出を静的な表現に置き換える． |
| R7 | 自動再生の動きに停止手段がない | WCAG 2.2.2（5 秒超の自動の動きには一時停止・停止手段） | 中 | 再生はユーザーのクリックで開始し，一時停止・停止ボタンを常に表示する．Chrome の自動再生ポリシー上もクリック起点が必要（5.1）． |
| R8 | GitHub 上での削除要請 | 権利者からの通知の可能性 | 低 | R1・R2・R4 を守れば根拠を減らせる．出典と自作の証跡をリポジトリに残す． |

---

## 5. 技術リファレンス

### 5.1 Web Audio のスケジューリング【確認済】

出典：Chris Wilson「A tale of two clocks」（web.dev，2013）．
- setTimeout・setInterval は主スレッドのレイアウトや GC で数十 ms 以上ずれる．音は AudioContext.currentTime（オーディオのハードウェア時計）で予約する．
- 方式：短い間隔で起きるタイマーが，「現在時刻＋先読み時間」までに鳴らすべき音をまとめて start(time) で予約する．
- 出発点の数値：**25 ms ごとに呼び，100 ms 先まで予約する**．機械や処理が重ければ先読みを伸ばす．
- 描画は requestAnimationFrame で行い，進行位置は audioContext.currentTime を読んで決める（音の時計に絵を合わせる）．
- PoC への適用【推論】：SVG 人体の姿勢も「拍番号→姿勢」の関数にし，rAF の中で currentTime から現在の拍を求めて描く．こうすると破綻の「腕が浮いたまま」も時刻で決定的に再現できる．

自動再生ポリシー【抜粋のみ：Chrome for Developers，MDN】：
- Chrome 71 以降，Web Audio も自動再生ポリシーの対象．ユーザー操作の前に作った AudioContext は suspended で始まり，クリック後に resume() が必要．MDN の要約は「ユーザー操作の中で作成・再開する」．

### 5.2 Web Speech API（speechSynthesis）の日本語【一部確認済】

- getVoices() は端末で使える声の一覧を返す．MDN の例は voiceschanged で一覧を作り直す【確認済】．
- Chrome は初回の getVoices() が空で，後から voiceschanged が来る．Safari はすぐ返すため，Mac だけで試すと不具合に気づきにくい【確認済：RedBison】．
- Firefox は voiceschanged を発火しない場合がある（Bugzilla 1237082，Stack Overflow 2017）．Safari も発火しないという 2015 年の報告がある．いずれも古い情報で，現行版での挙動は【未確認】．
- 声の一覧は OS が提供する．Windows・macOS・iOS・Android・Linux で異なり，Chrome は独自の Google 音声，Edge は Online (Natural) を追加する【確認済：RedBison】．ja-JP の声の有無も OS と言語パック次第である【推論】．
- localService が false の声はサーバー合成で，テキストが送信される【確認済：RedBison】．
- Chrome のオンライン音声は長い発話が約 15 秒で途切れることがある．文単位に分けて順に speak() する【確認済：RedBison．公式文書ではない】．
- rate・pitch は発話開始時に読まれ，途中変更は効かない．ページ遷移時は cancel() を呼ぶ【確認済：RedBison】．
- PoC への示唆【推論】：speechSynthesis には時刻指定の API がなく，オーディオ時計と正確には同期できない．号令は「拍の少し前に speak() を呼ぶ」程度の緩い同期になる．ja-JP の声が無い環境では，字幕表示に切り替えるか，号令を Web Audio の効果音で代替する．声が無いこと自体を「古い AI」の破綻として扱う手もある．

### 5.3 アクセシビリティ【確認済】

- prefers-reduced-motion：利用者が端末で動きを減らす設定をしているかを検出する．拡大やパンは前庭障害の引き金になりうる．MDN の例は，拡大アニメーションを不透明度の変化に置き換えている．Baseline（2020-01 以降，主要ブラウザで利用可）．
- WCAG 2.2：
  - 2.2.2 Pause, Stop, Hide（A）：自動で始まり 5 秒を超えて続く動きには，一時停止・停止・非表示の手段を設ける【抜粋のみ：W3C Understanding】．
  - 2.3.1 Three Flashes or Below Threshold（A）：1 秒に 3 回を超えて点滅しない【抜粋のみ】．
  - 2.3.3 Animation from Interactions（AAA）：操作に起因する不要な動きを無効化できるようにする【抜粋のみ】．
- PoC への適用【推論】：reduce のときは体操を静止画の連続（姿勢カード）にし，破綻は「腕だけ別の札が残る」ような静的表現に変える．字幕を付けて TTS が無い環境にも対応する．

### 5.4 GitHub Pages【抜粋のみ】

- 公開元は「ブランチ（ルートまたは /docs）」か「GitHub Actions のワークフロー」．ビルド工程が不要なら，ブランチから公開するのが推奨されている．単一の index.html ならブランチ公開で足りる．
- 入口は index.html，index.md，README.md のいずれか．
- GitHub Pages のサイトはリポジトリが非公開でも公開される．GitHub Free ではリポジトリを公開にする必要がある．

---

## 6. 参照 URL 一覧

### 先行事例
- Ars Technica「AI-generated video of Will Smith eating spaghetti astounds with terrible beauty」（2023-03-30）：https://arstechnica.com/information-technology/2023/03/yes-virginia-there-is-ai-joy-in-seeing-fake-will-smith-ravenously-eat-spaghetti
- Know Your Meme「AI Will Smith Eating Spaghetti」：https://knowyourmeme.com/sensitive/memes/ai-will-smith-eating-spaghetti
- BBC Bitesize「How Will Smith eating spaghetti became the 'test' of AI video」：https://www.bbc.co.uk/bitesize/articles/z4rsmbk
- Vice（2023-03-28）：https://www.vice.com/en/article/ai-will-smith-eating-spaghetti-hill-haunt-you-for-the-rest-of-your-life
- Ars Technica（Will Smith 本人のパロディ，2024-02-20）：https://arstechnica.com/information-technology/2024/02/will-smith-parodies-viral-ai-generated-video-by-actually-eating-spaghetti
- Pepperoni Hug Spot 公式：https://www.pepperonihugspot.pizza/pepperonihugspot
- Business Insider（Pepperoni Hug Spot）：https://www.businessinsider.com/ai-generated-pizza-ad-video-reddit-2023-4
- Today.com：https://www.today.com/food/trends/ai-generated-pizza-commercial-pepperoni-hug-spot-rcna82463
- Everybody Dance Now（arXiv）：https://arxiv.org/abs/1808.07371
- Everybody Dance Now プロジェクトページ：https://carolineec.github.io/everybody_dance_now
- 「Everybody Dance Faster」（失敗例の記述）：https://smellslike.ml/posts/everybody-dance-faster/
- AI Weirdness「New paint colors invented by neural network」：https://www.aiweirdness.com/new-paint-colors-invented-by-neural-17-05-17
- IEEE Spectrum（Janelle Shane）：https://spectrum.ieee.org/blogger-behind-ai-weirdness-thinks-todays-ai-is-dumb-and-dangerous
- Wikipedia「Cursed image」：https://en.wikipedia.org/wiki/Cursed_image
- PAPER「Why You Can't Look Away From Cursed Images」：https://www.papermag.com/cursed-images-break-the-internet
- Wikipedia「The Backrooms」：https://en.wikipedia.org/wiki/The_Backrooms
- YouTube「The Backrooms (Found Footage)」：https://www.youtube.com/watch?v=H4dGpz6cnHo
- Steam「I'm on Observation Duty」：https://store.steampowered.com/app/1046820/Im_on_Observation_Duty/
- Steam「The Exit 8」：https://store.steampowered.com/app/2653790/The_Exit_8/
- AUTOMATON（8番出口 PC 版 50 万本，影響元）：https://automaton-media.com/articles/newsjp/20240418-290121/
- 日本ゲーム大賞 2024 受賞作品（8番出口）：https://awards.cesa.or.jp/2024/year/prize/14.html
- GameWith（8番出口 累計 200 万本以上）：https://gamewith.jp/gamedb/10621/articles/51261
- QWOP（foddy.net）：https://www.foddy.net/blog/2010/10/qwop
- BBC Future「Can video games ever be funny?」：https://www.bbc.com/future/article/20140320-are-these-video-games-funny
- IGN「Human: Fall Flat Review」：https://www.ign.com/articles/2016/07/28/human-fall-flat-review
- The Guardian「Gang Beasts」：https://www.theguardian.com/technology/2014/jul/21/gang-beasts-funniest-fighting-game-ever-made
- YouTube「ラヂオ体操第4」：https://www.youtube.com/watch?v=Ep4JAf8hECE
- ブログ（ラヂオ体操第4 と Reebok）：https://tenmei.cocolog-nifty.com/matcha/2010/05/post-ed6d.html
- ねとらぼ（81プロデュース アテレコ）：https://nlab.itmedia.co.jp/cont/articles/3303874/
- 電撃オンライン（なつもん！ のラジオ体操）：https://dengekionline.com/articles/204163
- かんぽ生命「ラジオ体操アプリ」プレスリリース：https://www.jp-life.japanpost.jp/information/assets/pdf/2025/1101pr-01.pdf

### ラジオ体操の公式情報
- かんぽ生命「ラジオ体操の楽曲使用について」：https://www.jp-life.japanpost.jp/radio/abt_csr_rdo_cr.html
- かんぽ生命「申請手続きについて」：https://www.jp-life.japanpost.jp/radio/abt_csr_rdo_cr_reg.html
- NHK「ラジオ体操第一と第二のテーマ曲を使いたい。」：https://www.nhk.or.jp/nijishiyou-qa/675504.html
- 全国ラジオ体操連盟「ラジオ体操の楽曲と楽曲の使用手続き」：https://www.radio-exercises.org/archives/1386
- 全国ラジオ体操連盟「ラジオ体操の歴史（年表）」：https://www.radio-exercises.org/taisou/history_list
- 全国ラジオ体操連盟「ラタ坊」：https://www.radio-exercises.org/ratabou/ratabou
- NHK「ラジオ体操 よくあるご質問」：https://www.nhk.jp/p/radio-taisou/rs/N9N4P87RLP/faqpage/
- かんぽ生命「ラジオ体操の歴史 Q&A（幻の第3）」：https://www.jp-life.japanpost.jp/radio/faq/abt_csr_rdo_faq_cat01.html
- 服部正 Wikipedia：https://ja.wikipedia.org/wiki/%E6%9C%8D%E9%83%A8%E6%AD%A3
- 服部正 NDL 典拠：https://id.ndl.go.jp/auth/ndlna/00009130

### 商標・法
- J-PlatPat 商標検索：https://www.j-platpat.inpit.go.jp/t0100
- J-PlatPat 登録 4550800 固定アドレス：https://www.j-platpat.inpit.go.jp/c1801/TR/JP-2000-137114/40/ja
- J-PlatPat 登録 6462377 固定アドレス：https://www.j-platpat.inpit.go.jp/c1801/TR/JP-2020-142388/40/ja
- ブランドテラス（商標「ラジオ体操」一覧）：https://patent-i.com/tm/mark/%E3%83%A9%E3%82%B8%E3%82%AA%E4%BD%93%E6%93%8D/
- 文化庁 パロディワーキングチーム報告書（2013-03）：https://www.bunka.go.jp/seisaku/bunkashingikai/chosakuken/hosei/parody/pdf/h25_03_parody_hokokusho.pdf
- 大阪大学「パロディ目的での著作物の利用に関する一考察」：https://ir.library.osaka-u.ac.jp/repo/ouka/all/81471/27601714.pdf
- 弁護士ドットコム（福井健策弁護士）：https://www.bengo4.com/c_18/n_12466/
- ラジーン（音源自作と著作隣接権）：https://raji-nn.or.jp/radio-exercise-sheet-music/

### 技術
- web.dev「A tale of two clocks」：https://web.dev/articles/audio-scheduling
- Chrome for Developers「Autoplay policy in Chrome」：https://developer.chrome.com/blog/autoplay
- MDN「Web Audio API best practices」：https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Best_practices
- MDN「SpeechSynthesis: getVoices()」：https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/getVoices
- RedBison「Text to Speech and Speech Recognition in the Browser」：https://redbison.dev/blog/web-speech-api-guide
- Mozilla Bugzilla 1237082：https://bugzilla.mozilla.org/show_bug.cgi?id=1237082
- Stack Overflow（Safari の voiceschanged）：https://stackoverflow.com/questions/27779663/voiceschanged-event-not-fired-in-safari
- MDN「prefers-reduced-motion」：https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion
- W3C「Understanding SC 2.2.2 Pause, Stop, Hide」：https://www.w3.org/WAI/WCAG22/Understanding/pause-stop-hide.html
- W3C「Understanding SC 2.3.1 Three Flashes or Below Threshold」：https://www.w3.org/WAI/WCAG22/Understanding/three-flashes-or-below-threshold
- W3C「Understanding SC 2.3.3 Animation from Interactions」：https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions
- GitHub Docs「Configuring a publishing source for your GitHub Pages site」：https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- GitHub Docs「Creating a GitHub Pages site」：https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

---

## 7. 確認できなかったこと

- ラヂオ体操第4，81プロデュース版，なつもん！ の楽曲・号令の権利処理の内容．
- 体操の動き（振付）に著作物性が認められるかについての裁判例・公式見解．
- 号令の運動名（「背伸びの運動」等）の著作物性．
- 現行の Firefox・Safari における voiceschanged の挙動，各 OS の既定の ja-JP 音声の一覧．
- 文化庁パロディ WT 報告書の本文（抜粋と二次資料で結論のみ確認）．
- 日本郵政の 2025 年出願（第 9・41・42 類）の審査結果．
