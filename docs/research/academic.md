# 「さりげなく壊れるラジオ体操」PoC のための学術文献レビュー

作成日：2026-11-02（JST）
対象：2019 年ごろの古くて妙な AI が作ったように見える，ブラウザ上のラジオ体操アニメーション．各運動に「1 つだけ，誰も言及しない微小な破綻」を入れる設計（背伸びで腕が浮いたまま，膝が一度だけ逆に曲がる，ねじり後に頭が戻らない，影が逆に動く，号令と動作が少しずれる，カウントの重複，最後の息を吐く動作の欠落）．ユーザーテストでは洗濯機や椅子に変身するような大きな変化は「やりすぎで冷める」と評価され，微小な破綻のほうが面白いと評価された．音声は 126 BPM のシンセ伴奏，リズミカルな「はい」，日本語 TTS．

## 検証方法と注意

- 全 26 件について書誌を実際に開いて確認した．DOI があるものは Crossref API（`https://api.crossref.org/works/<DOI>`）で著者，年，誌名，巻，頁を照合した．要旨は OpenAlex，Europe PMC，出版社ページ，arXiv，機関リポジトリの PDF で確認した．
- 要旨や本文まで確認できず，書誌情報だけを確認した文献には「書誌のみ確認」と書いた．その文献の「主な知見」は広く知られた内容に基づいており，引用前に本文で確かめる必要がある．
- 「デッドパン（無表情・無反応のユーモア）」そのものを実験的に扱った論文は，今回の調査範囲では見つけられなかった．代わりに Attardo & Pickering (2011) のタイミング研究を使った（§4）．

---

## 1．ユーモア理論：なぜ「小さな破綻」が「大きな変身」に勝つのか

### [1] McGraw, A. P., & Warren, C. (2010). Benign violations: Making immoral behavior funny. *Psychological Science*, 21(8), 1141–1149. https://doi.org/10.1177/0956797610376073
- **主な知見**：笑いと面白さは，「規範の違反」であると同時に「無害（benign）」だと受け取られる状況で生じる．違反を無害にする条件として，(a) 状況を許容する別の規範がある，(b) 違反された規範への関与が弱い，(c) 心理的距離がある，の 3 つを挙げた．5 つの実験で，状況を「間違っているが，間違っていない」と同時に受け取ることが，笑いを媒介していると示した．
- **PoC への含意**：破綻は「ラジオ体操として正しい」という解釈と「何かがおかしい」という解釈を同時に成り立たせる必要がある．洗濯機への変身は「体操の規範」から完全に外れるので，違反ではなく別ジャンルのギャグとして処理され，「同時性」が崩れる．破綻は 1 運動につき 1 つの関節や 1 つの属性に限り，残りの体は完璧に体操を続ける．そうすれば「正しい」側の解釈が保たれる．

### [2] McGraw, A. P., Warren, C., Williams, L. E., & Leonard, B. (2012). Too close for comfort, or too far to care? Finding humor in distant tragedies and close mishaps. *Psychological Science*, 23(10), 1215–1223. https://doi.org/10.1177/0956797612443831
- **主な知見**：重い違反（悲劇）は心理的距離が遠いほど面白くなり，軽い違反（ちょっとした失敗）は距離が近いほど面白くなる．この交互作用が 5 つの研究で示された．
- **PoC への含意**：今回の破綻は「軽い違反」にあたる．軽い違反は，見る人が自分の体で知っている近い題材ほど面白くなる．ラジオ体操は多くの日本人が体で覚えているので，この条件に合う．逆に，膝の逆曲がりを関節が折れたように見せるなど，痛みや怪我を連想させる「重い」表現にすると，近さが不利に働く．膝の逆曲がりは 1 フレームから数フレームにとどめ，表情や効果音で痛みを示さない．

### [3] Warren, C., & McGraw, A. P. (2016). Differentiating what is humorous from what is not. *Journal of Personality and Social Psychology*, 110(3), 407–430. https://doi.org/10.1037/pspi0000041
- **主な知見**：文献上の「不調和（incongruity）」は，驚き，並置，非典型性，違反の少なくとも 4 通りに定義されており，どの定義でも面白いものと面白くないものを十分に区別できない．6 つの研究で，「良性違反」のほうが区別の精度が高いと示した．驚くべき出来事や悲劇が誤って面白いと予測されることを減らせる．
- **PoC への含意**：「意外性を大きくすれば面白い」という設計は根拠が弱い．変身ネタが冷めたのは，驚きは大きくても「無害な違反」の構造を欠いていたからだと説明できる．評価実験では，驚きの量とは別に，「おかしいと思ったか」と「許せると思ったか」の 2 軸を測るとよい．

### [4] Warren, C., Barsky, A., & McGraw, A. P. (2021). What makes things funny? An integrative review of the antecedents of laughter and amusement. *Personality and Social Psychology Review*, 25(1), 41–65（オンライン 2020）. https://doi.org/10.1177/1088868320961909
- **主な知見**：20 を超えるユーモア理論を，驚き，同時性，優越，違反の評価，無害さの評価という 5 つの先行要因に整理した．実証的に面白さを予測するのは同時性，違反，無害さであり，驚きと優越は予測しないと結論した．
- **PoC への含意**：設計の軸は「驚きの大きさ」ではなく「同時性」である．破綻は「正常な体操」の文脈の中に埋め込み，誰も言及しない（ナレーションも効果音も反応しない）状態を保つ．こうすると，見る人の中で「正常」と「異常」が同時に成り立つ．

---

## 2．不気味の谷，身体運動の知覚，生物学的運動

### [5] Mori, M., MacDorman, K. F., & Kageki, N. (2012). The uncanny valley [From the field]. *IEEE Robotics & Automation Magazine*, 19(2), 98–100. https://doi.org/10.1109/MRA.2012.2192811 （原著：森政弘 1970「不気味の谷」『Energy』7(4)．原著は今回開いていない）
- **主な知見**（書誌のみ確認）：人間らしさが高まると親和感も上がるが，人間にかなり近いが完全ではない段階で親和感が急落する「谷」がある．動きはその山と谷を増幅するという仮説を含む．
- **PoC への含意**：キャラクターは写実的にしない．棒人間，ローポリ，2019 年風の粗いレンダリングにとどめ，谷の手前に置く．写実的な体で関節が逆に曲がると，笑いではなく恐怖に転ぶ危険がある．

### [6] Mathur, M. B., & Reichling, D. B. (2016). Navigating a social world with robot partners: A quantitative cartography of the Uncanny Valley. *Cognition*, 146, 22–32. https://doi.org/10.1016/j.cognition.2015.09.008
- **主な知見**：実在するロボットの顔 80 体と編集した顔のセットで，好感度に不気味の谷が現れることを示した．投資ゲームでは，社会的信頼という暗黙の判断にも谷が及んだ．
- **PoC への含意**：顔は谷に最も落ちやすい部位である．顔には破綻を入れないか，顔の要素をそもそも描かない（のっぺらぼう，単純な記号）．破綻は腕，膝，頭部の向き，影など，顔以外の部位に置く．

### [7] Saygin, A. P., Chaminade, T., Ishiguro, H., Driver, J., & Frith, C. (2012). The thing that should not be: Predictive coding and the uncanny valley in perceiving human and humanoid robot actions. *Social Cognitive and Affective Neuroscience*, 7(4), 413–422（オンライン 2011）. https://doi.org/10.1093/scan/nsr025
- **主な知見**：fMRI の反復抑制法を用いた．人間（見た目も動きも生物的），ロボット（見た目も動きも機械的），アンドロイド（見た目は生物的，動きは機械的）を比べると，行為知覚系は見た目や動きそのものではなく，両者の「不一致」に強く反応した．著者らはこれを予測誤差の増大と解釈している．
- **PoC への含意**：面白さと不気味さはどちらも「予測とのずれ」から生じる．どちらに転ぶかは，ずれが局所的で一時的か（面白い），全身的で持続的か（不気味）で決まる，と仮定できる．破綻は 1 部位，1 回，短時間とし，次のカウントでは元に戻すか，戻らないことが目立たないようにする．

### [8] Johansson, G. (1973). Visual perception of biological motion and a model for its analysis. *Perception & Psychophysics*, 14(2), 201–211. https://doi.org/10.3758/BF03212378
- **主な知見**（書誌のみ確認）：関節に付けた光点 10 数個の動きだけで，人は歩行などの人間の動作を即座に知覚できる（点光源ディスプレイ）．
- **PoC への含意**：人間は関節の動きの異常に非常に敏感なので，破綻の振幅は小さくても気づかれる．膝の逆曲がりは 10〜20 度程度，腕の浮遊は 1〜2 拍程度でも十分に検出されると予想される（H1 で検証する）．点と線で描く棒人間でも，破綻は伝わる．

### [9] Piwek, L., McKay, L. S., & Pollick, F. E. (2014). Empirical evaluation of the uncanny valley hypothesis fails to confirm the predicted effect of motion. *Cognition*, 130(3), 271–277. https://doi.org/10.1016/j.cognition.2013.11.001
- **主な知見**：全身の CG キャラクターと，品質をパラメータで操作した動きのセットを用いた．動きの品質を上げると受容度が系統的に上がり，静止画で谷の最も深い位置にあったキャラクターも，動かすと受容されやすくなった．動きが谷を深めるという予測は支持されなかった．
- **PoC への含意**：ベースの体操モーションの品質は高く保つ．滑らかで正確な体操が「正常」の基準を作り，その上の 1 点の破綻が際立つ．全体をぎこちなくして「AI っぽさ」を出すと，不快感が増え，破綻も埋もれる．

### [10] Kätsyri, J., Förger, K., Mäkäräinen, M., & Takala, T. (2015). A review of empirical evidence on different uncanny valley hypotheses: Support for perceptual mismatch as one road to the valley of eeriness. *Frontiers in Psychology*, 6, 390. https://doi.org/10.3389/fpsyg.2015.00390
- **主な知見**：素朴な不気味の谷仮説と，動きによる増幅の仮説は一貫して支持されなかった．一方，人工的な特徴と人間的な特徴の「知覚的不一致」が不気味さを生むという仮説は，よく支持された．
- **PoC への含意**：不一致は不気味さへの近道でもある．影の逆走や号令とのずれのような不一致を，写実度の高い要素（リアルな影や肉声など）と組み合わせると，不気味さが強まるおそれがある．影は単純な楕円にし，音声は明らかな TTS にするなど，「人工物らしさ」をそろえて不一致の深刻さを下げる．

---

## 3．グリッチ美学，2019 年ごろの AI 生成メディアの失敗パターン，AI の「変さ」の受容

### [11] Menkman, R. (2011). *The Glitch Moment(um)*. Network Notebooks 04. Amsterdam: Institute of Network Cultures. ISBN 978-90-816021-6-7. https://networkcultures.org/blog/publication/no-04-the-glitch-momentum-rosa-menkman/ （PDF：https://networkcultures.org/_uploads/NN%234_RosaMenkman.pdf）
- **主な知見**（出版社の紹介ページで確認）：グリッチを，ノイズ，失敗，事故との関係で位置づける．グリッチが人工物とフィルタのあいだ，根本的な破断と商品化のあいだを行き来することを論じ，グリッチの技術的側面と文化的側面を区別する必要を説く．
- **PoC への含意**：グリッチの面白さは「媒体そのものが一瞬見える」ことにある．洗濯機への変身は「作り手のアイデア」として見えるが，小さな破綻は「生成システムの限界」として見える．後者のほうが「2019 年の AI が作った」という設定に合う．破綻は「意図された芸」ではなく「システムの癖」に見えるように作る．

### [12] Shane, J. (2019). *You Look Like a Thing and I Love You: How Artificial Intelligence Works and Why It's Making the World a Weirder Place*. New York: Voracious / Little, Brown（ハードカバー 2019 年．確認したのは 2021 年のペーパーバック版ページ，ISBN 9780316525220）. https://www.hachettebookgroup.com/titles/janelle-shane/you-look-like-a-thing-and-i-love-you/9780316525220/
- **主な知見**（出版社ページで確認）：ブログ「AI Weirdness」の著者が，機械学習が人間的なものを間違える滑稽で，ときに不穏なやり方を一般向けに解説した本．
- **PoC への含意**：2019 年前後の「AI の変さ」が，大衆的な笑いのジャンルとして成立していたことを示す文化的な根拠になる．Shane の素材の多くは，AI が真面目に，自信満々に間違える形をとる．PoC でも，AI（アニメ内の人物と号令）は破綻に一切気づかず，真面目に体操を続けるのがよい．

### [13] Chan, C., Ginosar, S., Zhou, T., & Efros, A. A. (2019). Everybody dance now. *Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)*, 5932–5941. https://doi.org/10.1109/ICCV.2019.00603 （arXiv:1808.07371，PDF で本文を確認）
- **主な知見**：姿勢（OpenPose のキーポイント）を中間表現にして，ある人のダンスを別の人に転写する手法．論文の Limitations 節と Figure 9 は失敗例として次を挙げる．ゆったりした服や髪の破綻，**OpenPose が検出し損ねたため右腕が消える**，シャツのしわのテクスチャ破綻，フレームごとの生成で起きるちらつき（flickering），学習データにない極端な姿勢（逆立ちなど）での破綻．
- **PoC への含意**：2019 年当時に実際にあった失敗の語彙として最も直接的な資料である．次のような破綻は「本物らしい」．(a) キーポイントの検出漏れで腕が数フレーム消える，または前のフレームの位置に留まる（背伸びで腕が浮いたままになる案と符合する），(b) 小さなちらつき，(c) 学習分布外の姿勢（体を回す，体を前後に曲げる運動など可動域の大きい運動）でだけ起きる破綻．破綻をどの運動に割り当てるかの根拠になる．

### [14] Karras, T., Laine, S., Aittala, M., Hellsten, J., Lehtinen, J., & Aila, T. (2020). Analyzing and improving the image quality of StyleGAN. *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)*, 8107–8116. https://doi.org/10.1109/CVPR42600.2020.00813
- **主な知見**：StyleGAN（2019）に特徴的な画像の破綻を分析し，正規化，段階的成長（progressive growing），正則化を改めることで対処した（要旨で確認）．本文では，水滴状の破綻や，段階的成長に起因して歯や目などの細部が顔の向きに追従せず画面上の位置に張り付く現象が論じられている（この細部は要旨にはない．引用時は本文で確認すること）．
- **PoC への含意**：「影が逆に動く」「頭がねじりから戻らない」は，「一部の要素が体の動きに追従せず，別の座標系に張り付く」という当時の GAN の癖と同じ形をしている．破綻の見た目を「誤った座標系への固定」として統一すると，2019 年の AI らしさが一貫する．

### [15] Bansal, H., Lin, Z., Xie, T., Zong, Z., Yarom, M., Bitton, Y., Jiang, C., Sun, Y., Chang, K.-W., & Grover, A. (2024). VideoPhy: Evaluating physical commonsense for video generation. arXiv:2406.03520. https://doi.org/10.48550/arXiv.2406.03520
- **主な知見**：テキストから動画を生成するモデルが，物理的な常識に従うかを評価するベンチマーク．人手評価では，最も性能の高い CogVideoX-5B でも，指示と物理法則の両方に従った動画は 39.6% にとどまった．
- **PoC への含意**：2019 年より新しいモデルでも，物理的に破綻した動画は珍しくない．このため，観客は AI 動画の破綻を見慣れつつある．PoC では「物理の破綻」（影や重力）と「指示との不一致」（号令と動作のずれ）の両方を，種類の異なる破綻として使い分けられる．ただし，見慣れた破綻ほど驚きが減るおそれもある．「2019 年らしさ」は，現在の生成動画とは違う「粗い姿勢ベース」の見た目で出す．

### [16] Ragot, M., Martin, N., & Cojean, S. (2020). AI-generated vs. human artworks. A perception bias towards artificial intelligence? *Extended Abstracts of the 2020 CHI Conference on Human Factors in Computing Systems*, 1–10. https://doi.org/10.1145/3334480.3382892
- **主な知見**：565 人が人間または AI（GAN）の作品を，好み，美しさ，新奇性，意味の 4 次元で評価した．「人間作」と提示された絵画は，「AI 作」と提示された絵画より有意に高く評価された．
- **PoC への含意**：「AI が作った」という枠付けは評価を変える．評価実験では，枠付け（「2019 年の AI が作った」と明示するか，しないか）を独立変数にするか，全条件で統一する．枠付けが「変さ」を許す方向に働く（良性化の手がかりになる）可能性もあり，探索的仮説 H6 として検証できる．

---

## 4．タイミング，リズムへの同期，音楽における期待違反

### [17] Huron, D. (2006). *Sweet Anticipation: Music and the Psychology of Expectation*. Cambridge, MA: MIT Press. https://doi.org/10.7551/mitpress/6575.001.0001
- **主な知見**（書誌のみ確認）：期待に関わる反応を ITPRA（Imagination，Tension，Prediction，Reaction，Appraisal）の 5 つの段階に分けて論じる．予測が当たること自体が快をもたらすこと，驚きへの最初の否定的な反応が，その後の評価で肯定的に反転しうること（対比的な価）などを論じている．
- **PoC への含意**：126 BPM の拍と「はい」の号令が，強い予測の土台になる．破綻はこの予測の網の中で 1 回だけ起こすと，「一瞬の違和感が直後に無害だとわかる」という反転が起きやすい．カウントを重ねる（「ご，ろく，ろく，しち」）のは，予測される時間構造そのものを使った破綻であり，最も「音楽的な」ギャグになる．

### [18] Koelsch, S., Vuust, P., & Friston, K. (2019). Predictive processes and the peculiar case of music. *Trends in Cognitive Sciences*, 23(1), 63–77. https://doi.org/10.1016/j.tics.2018.10.006
- **主な知見**：音楽を聴くことは，次に何が起きるかの仮説を絶えず生成し，注意によってその不確実性を解消する能動的な過程である．予測符号化の枠組みで，精度による重み付けと注意の選択を定式化した．
- **PoC への含意**：規則的な伴奏が続くと予測の精度が高まり，小さな逸脱でも大きな予測誤差として目立つ．したがって，破綻の振幅は小さくてよい．「最後の息を吐く動作が欠ける」のは，最も予測精度が高い終止部分での欠落なので，小さくても強く効くと予想される．

### [19] Witek, M. A. G., Clarke, E. F., Wallentin, M., Kringelbach, M. L., & Vuust, P. (2014). Syncopation, body-movement and pleasure in groove music. *PLoS ONE*, 9(4), e94446. https://doi.org/10.1371/journal.pone.0094446
- **主な知見**：ウェブ調査で，シンコペーションの度合いを変えたファンクのドラムブレイクを評価させた．「体を動かしたくなる度合い」と快は，シンコペーションが中程度のとき最も高く，逆 U 字型の関係を示した．
- **PoC への含意**：予測からの逸脱には最適な中間量がある．これは直接にはユーモアの研究ではないが，「破綻の大きさ」と「面白さ」が逆 U 字になるという仮説（H1）の類推的な根拠になる．洗濯機（逸脱が大きすぎる）と無破綻（逸脱がない）の間に最適点がある．

### [20] Large, E. W., & Jones, M. R. (1999). The dynamics of attending: How people track time-varying events. *Psychological Review*, 106(1), 119–159. https://doi.org/10.1037/0033-295X.106.1.119
- **主な知見**（書誌のみ確認）：注意は内的な振動子として外部のリズムに同期（エントレインメント）し，予期された時点に注意の焦点が集まる．テンポの変動にも追従する．
- **PoC への含意**：破綻は拍の上，特に強拍（1 拍目と 3 拍目）に置くと気づかれやすく，拍と拍のあいだに置くと見逃されやすいと予想される．気づかれやすさを調整するつまみとして，破綻の位相（拍上か拍間か）を使える（H3）．

### [21] Attardo, S., & Pickering, L. (2011). Timing in the performance of jokes. *Humor: International Journal of Humor Research*, 24(2), 187–215. https://doi.org/10.1515/humr.2011.015 （頁は Crossref に記載がなく，誌面情報による．引用前に確認すること）
- **主な知見**：20 件のジョークの実演を分析したところ，オチの前後で話す速度は有意に変わらず，オチの直前に間（ポーズ）が入るという通説も支持されなかった．準備したジョークと即興のジョークにも差はなかった．
- **PoC への含意**：「オチの前に間を置く」「オチで声色を変える」といった強調は，ユーモアに必須ではない．むしろ韻律を変えずに続けるほうが，デッドパン的に働くと考えられる．TTS の号令は，破綻の瞬間にも抑揚，速さ，音量を一切変えない．効果音やカメラの寄りも入れない．

---

## 5．ラジオ体操の文化研究

### [22] 津上英輔 (2010)．運動を管理する音楽：服部正作曲〈ラジオ体操第一〉の分析．『成城美学美術史』16，1–15．成城大学．PDF：https://seijo.repo.nii.ac.jp/record/791/files/art-016-tugami.pdf （CiNii：https://cir.nii.ac.jp/crid/1050001337472212992）
- **主な知見**（本文で確認）：現行（第三代，1951 年）のラジオ体操第一の伴奏は，号令なしで音楽が動作を誘発し，制御するよう作られている．弱拍（2 拍目と 4 拍目）で動作を止めて「間」を作り，付点のリズム動機で次の動作への準備を促す．前楽節の半終止から後楽節の頭へ和声的につなぐことで，前の動作の「8」の最中に次の動作の「1」が準備される．実質的な考案者である遠山喜一郎の「“間”が命なんだ……その“間”がないと“間抜け”だ．伸びすぎたら“間延び”だ」という言葉を引いている．なお，高橋秀実の「テンポは一分間に約七〇拍」という記述も引用されている．
- **PoC への含意**：これは PoC にとって最も重要な文献の 1 つである．(a) 「背伸びで腕が浮いたまま」は，遠山の言う「間」をほんの少し伸ばす「間延び」であり，体操自体の美学を裏返した破綻として理論的に位置づけられる．(b) 破綻は，動作と音楽が密に結びついた箇所（「8」から「1」へのつなぎ）に置くと，ずれが最も目立つ．(c) 126 BPM という設定は，高橋の「約 70 拍」という記述と食い違う．数え方（四分音符か二分音符か）の違いかもしれないので，実際の音源で確認してから「正常」の基準テンポを決めるとよい．

### [23] 権学俊 (2018)．近代日本における身体の国民化と規律化．『立命館産業社会論集』53(4)，31–49．PDF：https://www.ritsumei.ac.jp/file.jsp?id=371228
- **主な知見**（本文で確認）：ラジオ体操は 1928 年，簡易保険局が米メトロポリタン生命の放送体操を範にして始めた．全国で「同じ時間」「同じリズム」「同じ動き」を一斉に行わせることで，人々の連帯感と国家意識を形成する装置として働いた．戦時下には参加者が延べ 1 億人を超える規模に拡大した．先行研究（黒田勇『ラジオ体操の誕生』1999 年など）の整理も含む．
- **PoC への含意**：ラジオ体操の核は「全員が同じ動きをする」という強い同調の規範である．良性違反理論 [1] の言葉で言えば，見る人がこの規範に強く関与しているほど違反は検出されやすく，同時に「たかが体操」という軽さが違反を無害にする．歴史的に重い文脈（規律，動員）を前面に出すと，無害さが失われるおそれがある．PoC の語り口は「普通の朝の体操」にとどめる．

### [24] 崎田嘉寛・当山倫子・近藤雄大・若槻稜磨・松浦早希 (2024)．国民保健体操（初代ラジオ体操）の再現とバイオメカニクス的分析．『体育史研究』41，87–103．https://doi.org/10.24686/taiikushi.41.0_87
- **主な知見**（書誌のみ確認．J-STAGE の書誌ページを開いた）：失われた初代ラジオ体操を映像で再現し，バイオメカニクスの観点から分析した研究資料．同じ研究グループの 2021 年の学会発表要旨では，NHK「テレビ体操」（2015 年）の映像から関節ごとの可動域を採点し，柔軟性の要素を代ごとに比較する方法をとっている．
- **PoC への含意**：「正しいラジオ体操」の関節角度の基準を，映像から定量的に取る方法の前例になる．破綻の大きさ（例：膝を正常から何度逆に曲げるか）を，正常な可動域に対する比として定義する際の参考になる．

---

## 6．（任意）実装：姿勢ベース，手続き的なキャラクターアニメーション

### [25] Holden, D., Komura, T., & Saito, J. (2017). Phase-functioned neural networks for character control. *ACM Transactions on Graphics*, 36(4), 1–13 (Article 42). https://doi.org/10.1145/3072959.3073663
- **主な知見**：ネットワークの重みを，動作の「位相」を入力とする周期関数で計算する PFNN を提案した．周期的な運動（歩行など）を位相で明示的に扱うことで，LSTM などより高品質で軽量な制御を実現した．
- **PoC への含意**：ニューラルネットは不要だが，「位相で駆動する」という考え方はそのまま使える．各運動を 8 拍または 16 拍の位相 φ∈[0,1) の関数として関節角を定義し，破綻は特定の関節の位相関数だけを局所的に差し替えて実装する（例：背伸びで腕の φ を 1 拍分止める）．こうすれば，破綻の大きさ，位置，長さを 1 つのパラメータで操作でき，評価実験の条件も作りやすい．

### [26] Cao, Z., Hidalgo, G., Simon, T., Wei, S.-E., & Sheikh, Y. (2021). OpenPose: Realtime multi-person 2D pose estimation using part affinity fields. *IEEE Transactions on Pattern Analysis and Machine Intelligence*, 43(1), 172–186（オンライン 2019）. https://doi.org/10.1109/TPAMI.2019.2929257
- **主な知見**（書誌のみ確認）：画像から複数人の 2D 関節キーポイントを実時間で推定する，2019 年前後に標準的だった手法．
- **PoC への含意**：2019 年風の見た目は，OpenPose 形式の骨格（関節点を線で結んだ色付きの棒人間）として描くと，当時の生成パイプライン [13] の中間表現そのままになり，説得力が出る．破綻も「キーポイントの信頼度が一瞬下がる」「1 点が前のフレームに留まる」という形で実装すれば，技術的に本物らしくなる．

---

## 7．設計への統合（文献から導いたパラメータの目安）

| 設計項目 | 推奨 | 主な根拠 |
|---|---|---|
| 1 運動あたりの破綻数 | 1 つ（同時に複数の部位を壊さない） | [1][4] 同時性，[7] 局所的なずれ |
| 破綻の大きさ | 中程度．正常な可動域から「気づくが痛くない」範囲．例：膝の逆曲がり 10〜20 度，腕の浮遊 1〜2 拍 | [19] 逆 U 字，[2] 軽い違反，[8] 高い検出感度（数値は仮説であり，H1 で決める） |
| 持続 | 1 回だけ．次の 8 拍では元に戻る（「頭が戻らない」は例外として，静かに持続させる） | [7] 持続的なずれは不気味さにつながる |
| 位置（時間） | 強拍の上，または「8」から「1」へのつなぎ | [20] 注意の同期，[22] 動作と音楽の結合点 |
| 部位 | 腕，膝，頭部の向き，影，カウント．顔は使わない | [6] 顔は谷に落ちやすい，[13] 実在した失敗の語彙 |
| 反応 | 号令，伴奏，効果音，カメラのいずれも反応しない | [21] 韻律の強調は不要，[4] 同時性の維持 |
| 写実度 | 低く保つ（OpenPose 風の棒人間，単純な影，明らかな TTS） | [5][10] 写実的な不一致は不気味，[26] |
| ベースの動き | 正確かつ滑らか | [9] 動きの品質は受容度を上げる，[22] |

---

## 8．検証可能な仮説

- **H1（大きさの逆 U 字）**：面白さの評定は，破綻の大きさに対して逆 U 字になる（2 次の項が負）．不気味さの評定は，大きさに対して単調に増える．根拠：[19][2][7]．
- **H2（無反応の優位）**：同じ破綻でも，号令や効果音が破綻に反応する条件（「あれ？」，効果音，間を空ける）より，反応しない条件のほうが面白い．根拠：[4][21][12]．
- **H3（拍の位相）**：強拍上の破綻は拍間の破綻より検出率が高い．検出された試行に限れば，強拍上の破綻のほうが面白さも高い．根拠：[20][18][22]．
- **H4（写実度）**：写実的なキャラクターでは，棒人間（OpenPose 風）より不気味さが高く，面白さが低い．根拠：[5][7][10]．
- **H5（頻度）**：破綻を 1 運動あたり 1 つにした条件は，1 運動あたり 3 つ以上にした条件より面白い．後者は「全体が壊れている」と受け取られ，正常な基準が失われる．根拠：[1][4][9]．
- **H6（探索的．AI という枠付け）**：「2019 年の AI が生成した映像」と提示すると，提示しない場合より破綻が「無害」と判断されやすく，面白さが上がる．根拠：[16][12]（[16] は AI 作品が一般に低く評価されることを示しており，方向は逆になる可能性もある）．

## 9．小規模な評価方法（案）

- **刺激**：運動ごとに 8 拍または 16 拍のクリップ（約 4〜8 秒，126 BPM）．破綻の種類 7 つ × 大きさ 5 段階（0＝なし，1〜4）を基本とし，H2 の反応の有無，H3 の拍上か拍間かを一部の種類にだけ交差させる．位相駆動の実装 [25] で大きさを連続的に操作する．大きさ 5 の上位基準として「変身」条件を 1 つ入れ，既存の観察（やりすぎで冷める）を再現できるか確かめる．
- **参加者**：ラジオ体操を経験した日本在住の成人 60〜100 人（オンラインの調査会社やクラウドソーシングで募集）．経験の深さ（月に何回やるか，子どものころの夏休みに参加したか）を共変量として取る．
- **手続き**：参加者内計画．クリップをランダムな順に提示する．1 人が見るのは 40 本程度に抑える（疲労の対策として，各参加者には一部の組み合わせだけを割り当てる不完全ブロックにする）．
- **測定**（クリップごと）：(1) 面白さ（1〜7），(2) 不気味さ（1〜7），(3) 「何かおかしい点に気づいたか」（はい／いいえ）と，その内容の自由記述（検出の正誤を判定する），(4) 良性違反の 2 項目「何か間違っている」「でも問題ない，許せる」（各 1〜7．[1] の「間違っているが間違っていない」の測り方を参考にした）．最後に，最も面白かったクリップと最も冷めたクリップを選ばせる．
- **分析**：参加者とクリップを変量効果とする線形混合モデルで，面白さ ～ 大きさ＋大きさ² ＋ 反応の有無 ＋ 位相 ＋ 写実度 ＋（1|参加者）＋（1|破綻の種類）．H1 は 2 次の項が負で，その頂点が範囲の内側にあることで判定する．検出（2 値）にはロジスティック混合モデルを使う．面白さを予測する変数として，「間違っている」と「許せる」の交互作用が効くかどうかも見る（良性違反理論の直接の検証になる）．
- **実務上の使い方**：H1 の頂点付近の大きさを各破綻の初期値とし，最も面白かったものと最も冷めたものの自由記述から，破綻の種類ごとに調整する．事前登録（OSF など）をしておくと，結果の説明責任が明確になる．

---

## 参考文献一覧

1. McGraw, A. P., & Warren, C. (2010). Benign violations: Making immoral behavior funny. *Psychological Science*, 21(8), 1141–1149. https://doi.org/10.1177/0956797610376073
2. McGraw, A. P., Warren, C., Williams, L. E., & Leonard, B. (2012). Too close for comfort, or too far to care? Finding humor in distant tragedies and close mishaps. *Psychological Science*, 23(10), 1215–1223. https://doi.org/10.1177/0956797612443831
3. Warren, C., & McGraw, A. P. (2016). Differentiating what is humorous from what is not. *Journal of Personality and Social Psychology*, 110(3), 407–430. https://doi.org/10.1037/pspi0000041
4. Warren, C., Barsky, A., & McGraw, A. P. (2021). What makes things funny? An integrative review of the antecedents of laughter and amusement. *Personality and Social Psychology Review*, 25(1), 41–65. https://doi.org/10.1177/1088868320961909
5. Mori, M., MacDorman, K. F., & Kageki, N. (2012). The uncanny valley [From the field]. *IEEE Robotics & Automation Magazine*, 19(2), 98–100. https://doi.org/10.1109/MRA.2012.2192811
6. Mathur, M. B., & Reichling, D. B. (2016). Navigating a social world with robot partners: A quantitative cartography of the Uncanny Valley. *Cognition*, 146, 22–32. https://doi.org/10.1016/j.cognition.2015.09.008
7. Saygin, A. P., Chaminade, T., Ishiguro, H., Driver, J., & Frith, C. (2012). The thing that should not be: Predictive coding and the uncanny valley in perceiving human and humanoid robot actions. *Social Cognitive and Affective Neuroscience*, 7(4), 413–422. https://doi.org/10.1093/scan/nsr025
8. Johansson, G. (1973). Visual perception of biological motion and a model for its analysis. *Perception & Psychophysics*, 14(2), 201–211. https://doi.org/10.3758/BF03212378
9. Piwek, L., McKay, L. S., & Pollick, F. E. (2014). Empirical evaluation of the uncanny valley hypothesis fails to confirm the predicted effect of motion. *Cognition*, 130(3), 271–277. https://doi.org/10.1016/j.cognition.2013.11.001
10. Kätsyri, J., Förger, K., Mäkäräinen, M., & Takala, T. (2015). A review of empirical evidence on different uncanny valley hypotheses: Support for perceptual mismatch as one road to the valley of eeriness. *Frontiers in Psychology*, 6, 390. https://doi.org/10.3389/fpsyg.2015.00390
11. Menkman, R. (2011). *The Glitch Moment(um)*. Network Notebooks 04. Institute of Network Cultures. https://networkcultures.org/blog/publication/no-04-the-glitch-momentum-rosa-menkman/
12. Shane, J. (2019). *You Look Like a Thing and I Love You*. Voracious / Little, Brown. https://www.hachettebookgroup.com/titles/janelle-shane/you-look-like-a-thing-and-i-love-you/9780316525220/
13. Chan, C., Ginosar, S., Zhou, T., & Efros, A. A. (2019). Everybody dance now. *ICCV 2019*, 5932–5941. https://doi.org/10.1109/ICCV.2019.00603 （https://arxiv.org/abs/1808.07371）
14. Karras, T., Laine, S., Aittala, M., Hellsten, J., Lehtinen, J., & Aila, T. (2020). Analyzing and improving the image quality of StyleGAN. *CVPR 2020*, 8107–8116. https://doi.org/10.1109/CVPR42600.2020.00813
15. Bansal, H., et al. (2024). VideoPhy: Evaluating physical commonsense for video generation. arXiv:2406.03520. https://arxiv.org/abs/2406.03520
16. Ragot, M., Martin, N., & Cojean, S. (2020). AI-generated vs. human artworks. A perception bias towards artificial intelligence? *CHI EA '20*, 1–10. https://doi.org/10.1145/3334480.3382892
17. Huron, D. (2006). *Sweet Anticipation: Music and the Psychology of Expectation*. MIT Press. https://doi.org/10.7551/mitpress/6575.001.0001
18. Koelsch, S., Vuust, P., & Friston, K. (2019). Predictive processes and the peculiar case of music. *Trends in Cognitive Sciences*, 23(1), 63–77. https://doi.org/10.1016/j.tics.2018.10.006
19. Witek, M. A. G., Clarke, E. F., Wallentin, M., Kringelbach, M. L., & Vuust, P. (2014). Syncopation, body-movement and pleasure in groove music. *PLoS ONE*, 9(4), e94446. https://doi.org/10.1371/journal.pone.0094446
20. Large, E. W., & Jones, M. R. (1999). The dynamics of attending: How people track time-varying events. *Psychological Review*, 106(1), 119–159. https://doi.org/10.1037/0033-295X.106.1.119
21. Attardo, S., & Pickering, L. (2011). Timing in the performance of jokes. *Humor*, 24(2). https://doi.org/10.1515/humr.2011.015
22. 津上英輔 (2010)．運動を管理する音楽：服部正作曲〈ラジオ体操第一〉の分析．『成城美学美術史』16，1–15．https://seijo.repo.nii.ac.jp/record/791/files/art-016-tugami.pdf
23. 権学俊 (2018)．近代日本における身体の国民化と規律化．『立命館産業社会論集』53(4)，31–49．https://www.ritsumei.ac.jp/file.jsp?id=371228
24. 崎田嘉寛・当山倫子・近藤雄大・若槻稜磨・松浦早希 (2024)．国民保健体操（初代ラジオ体操）の再現とバイオメカニクス的分析．『体育史研究』41，87–103．https://doi.org/10.24686/taiikushi.41.0_87
25. Holden, D., Komura, T., & Saito, J. (2017). Phase-functioned neural networks for character control. *ACM Transactions on Graphics*, 36(4), Article 42. https://doi.org/10.1145/3072959.3073663
26. Cao, Z., Hidalgo, G., Simon, T., Wei, S.-E., & Sheikh, Y. (2021). OpenPose: Realtime multi-person 2D pose estimation using part affinity fields. *IEEE TPAMI*, 43(1), 172–186. https://doi.org/10.1109/TPAMI.2019.2929257

## 未確認事項と今後の調査候補

- 本文を読めていない文献：[5] 森の原著（1970），[8] Johansson，[17] Huron，[20] Large & Jones，[24] 崎田ら，[26] OpenPose は，書誌だけを確認した．[14] の「位置への張り付き」も本文では未確認である．引用前に本文で確認すること．
- デッドパン，無反応のユーモアを直接扱った実験研究は見つからなかった．
- 黒田勇『ラジオ体操の誕生』（青弓社，1999 年）と高橋秀実『素晴らしきラジオ体操』（小学館，1998 年）は，[22][23] の中で引用されているのを見ただけで，現物は確認していない．
- 戦後韓国の国民体操との比較を論じた Lee, C. (2025), "Performing National Gymnastics," *Music & Politics* 19, https://doi.org/10.3998/mp.8031 は検索結果で存在を見ただけで，本文は開いていないため，本文の文献リストには入れなかった．
