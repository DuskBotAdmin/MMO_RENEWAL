# 動作環境(編集段階2020/04/05)
| パッケージ等 | version |  |パッケージ等 | version | 
|:-----------|:------------:| ------------ |:-----------|:------------:| 
| "Python" | 3.7.4 | | "discord.py" | 1.3.3 |
| "yarl" | 1.4.2 | | "chardet" | 3.0.4 |
| "websockets" | 8.1 | | "attrs" | 19.3.0 |
| "pip" | 20.0.2 | | "async-timeout" | 3.0.1 |
| "multidict" | 4.7.5 | | "aiosqlite" | 0.11.0 |
| "idna" | 2.9 | | "aiohttp" | 3.6.2 |
```markdown
書き方はdiscord.pyのrewriteでcogsを使用しています。
cogsとは: "https://qiita.com/kymybk/items/14d5d10f33315c8a9aee"
```

# このリニューアルコードについて:
```
どうもDiscordで兄じゃぁぁぁ#3454という名前で活動している一般ユーザーです。
今回はかなりお世話になってるMMOくんのコードを書き直したのでそれを公開します。

MMOくんのコード:https://github.com/dischanet/mmo-discord-bot
このコードの基盤はMMOくんでありそこからTAOの中身をいくつか持ってきたものです。
かなりコメントアウトをしていますが何故かというと
前にオープンソース化されたMMOくんの中身は
python初心者の自分にとって何が何だか全く分からなかったので
その悩みを解決するべく見る側が嫌がるほど書き込んでます。
なので「これならどんな初心者でも文句なしでコードを全て理解出来る！」と思いかなり書いてます。

反対に見にくいかも知れませんが慣れていってコメントアウトを消していけば
かなり見やすくなりますので頑張ってください！
```

# このコードを使用するにあたっての注意点:
```markdown
あとall_dataディレクトリ内に色んなデータを保存するようにしてるのですが
その中でもこの部分に分かりやすく下記のようにユーザーIDを記載してます。
私(兄じゃぁぁぁ#3454)のIDなのですがもしこのコードを使ってBOTを使用する際に
オープンソースにしてるから直さないと危険というERRORが発生した場合は私が
ERRORが出たBOTでERRORを再現するために
手ごろに対応できるように(なんとなく権限持っておきたいから)リストの中に入れてます。
それだけは頭に入れててください。(嫌な場合はそのID消してしまえ！！！！)
admin_list = [304932786286886912] # ここね 
```

# 使えるコマンド集:(運営のも含めて) ~~の部分はprefix
```markdown
一般ユーザーが使用できるコマンド集:
```
| コマンド: | 説明: |
|:-----------|:------------|
| ~~help | このメッセージの表示をします | 
| ~~attack/atk | チャンネル上のモンスターに攻撃します | 
| ~~item/i アイテム名 | 選択したアイテムを使用します [例 ~~i f] | 
| ~~status/st | 自分のステータスを表示します | 
| ~~reset/rs/re | バトルをやり直します | 
| ~~t | 4字熟語クイズトレーニングをします | 
| ~~ranking | 鯖ランキングをまとめました。 | 
```markdown
※本当はこれらのコマンド書くつもりはなかったなんて内緒
ボット運営が使用できるコマンド集:(このコードを使用するにあたっての注意)
```
| コマンド: | 説明: |
|:-----------|:------------|
| ~~eval | 何でもできる。最高コマンド。 (おまけで追加) |
| ~~all | 全データを削除する (おまけで追加) |
| ~~db | データベースに接続する (おまけで追加) |
| ~~zukan | 現在登録されてる敵の図鑑一覧 (おまけで追加) |
| ~~exp | 指定したユーザーに対して経験値の付与 (おまけで追加) |
| ~~ban | 指定したユーザーをBANする (おまけで追加) |
| ~~unban | 指定したユーザーのBANを解除する (おまけで追加) |

# データベースの中身:
| テーブル名:  | カラム名: | カラムの型: |  テーブル名:  | カラム名: | カラムの型: | 
|:-----------|:------------|:------------:|:-----------|:------------|:------------:|
| "player" | user_id | BIGINT(20) | "in_battle" | user_id | BIGINT(20) |
|  | exp | BIGINT(20) |  | channel_id | BIGINT(20) |
|  | isbot | INT |  | player_hp | INT |
|  |  |  |  |  |  |
| "item" | user_id | BIGINT(20) | "channel_status" | server_id | BIGINT(20) |
|  | item_id | INT |  | channel_id | BIGINT(20) |
|  | count | INT |  | boss_level | INT |
|  |  |  |  | boss_hp | INT |
| "ban_user" | user_id | BIGINT(20) | | | |

# 今回対策してるバグ集:
| バグ内容: | そしてその対策: |
|:-----------:|:------------|
| "権限不足" | commands.bot_has_permissionsを使用しあらかじめ権限がないと使用不可に |
| "2000文字超過" | ページ分けや2000文字超えたらそれ用のメッセージが出るように |
| "多重コマンド" | threading.Threadを使用し他にも少し色々手を加え同時にコマンドの処理を行わないように |
| "prefixやtokenの記入漏れ" | ERRORが分からない人用のためにprintするようにしてます。 |
| "全ての関数にtraceback" | traceback安定で有能すぎますね。ERROR読める人は詳しいところまで頑張ってください！ |
| "データベース接続過多" | 非同期にさせてるしon_messageで接続し全処理をその1回の接続で済ませるように工夫 |

# やろうと思ったけどやめたやつ:
| 内容: | 理由: |
|:-----------:|:------------|
| "self対策" | 公開したらTAOのやつが対策されるから。怖い；；ぴえん |
| "マクロ対策" | 一応残ってるけどERRORあってBAN祭りになったら危険だからね！ |
| "login機能" | ここは皆の工夫を見てみたかったから |
| "ログインしたときのメッセージ" | printで充分でしょ |
| "午前0時に全ファイルを自動zip化" | 最初はこのコード読み解くのに時間かかるしそんなに必要としないと思うから |

# 最後に:
```markdown
自分はMMOくんのおかげでDiscordを続けてこれたし
MMOくんのおかげでプログラミングをここまで続ける事ができる。
TAOの皆のおかげでもあるけどやっぱり原点は頂点だね。本当に感謝
そしてその身に着けた知識を使って今度は公開できる側になれたことにも感謝

敵に月島と猫が入ってるからそれは使っても大丈夫だよ！もしTAOの敵を使いたいなら運営じゃなくて
書いた人に頼んでね！TAOは皆で作るものやし基本的には自由だけど絵は絵師の努力のたまものだからね！

一応完全完璧だと思うけどこのERRORはヤバいよ！！！ってのがあったら伝えてほしい！
まあ自己責任みたいなところもあるけどやっぱり公開するなら公開した側にも責任あるからね
兄じゃぁぁぁ#3454でDiscordが消えるまで活動(現在は受験のためお休み中)してるはずやからいくらでもDMしてや！

あとTAOってBOTもよろしくね！自分はこっちをメインに活動してるからさ！
TAO-Discord鯖: https://discord.gg/tao
TAOの招待: https://discordapp.com/api/oauth2/authorize?client_id=526620171658330112&permissions=3533888&scope=bot
TAOの寄付も募ってる(欲張り): https://taqooto.wixsite.com/tao-donate
```

# 追記:(修正や追加した事などをメインに書きます。)
| 修正や追加した場所 | 一言 | 修正or追加日 |
|:-----------:|:------------|:-----------:|
| "ERRORにクールタイム対応" | ただ単に編集忘れです。 | 2020/04/05 |
| "monster_info関数の修正" | else:のところがミスを起こしてました。確認不足でした。 | 2020/04/05 |
| "動作環境の記入不足" | 別環境で動作ERROR起きたら萎えちゃうかも～^^ | 2020/04/05 |
| "再起動毎にデータ初期化問題" | ただ単にすまねぇ。これじゃゲームもくそもないよな | 2020/04/05 |
| "Attack毎に敵交代" | 仲間呼びすぎぃ^^ | 2020/04/05 |
| "Attack毎に敵交代(2回目)" | んひぃ～；； | 2020/04/19 |

# ERRORや分からないところがあった際の連絡先:
| 連絡先 | ここからDMとかよろしく | | 
|:-----------|:------------|:------------|
| "Twitter" | https://twitter.com/Anijaaatakoyaki | https://twitter.com/Discord_TAO |
| "Discord" | User: 兄じゃぁぁぁ#3454 | ID: 304932786286886912 |
| "メイン鯖" | https://discord.gg/tao | |
| "Instagram" | https://www.instagram.com/tiku_taku.py/?hl=ja | |
| "Github" | https://github.com/Anijaaaaaaaaaaa | |
| "Line" | OpenChat: | https://line.me/ti/g2/XNsUYGr-ZcJjEnfK-ALElw |
|  | LineID: | yawarakasensha1234 |
