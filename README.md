https://uaua.github.io/puyofu/viewer/

# puyofu

ぷよスポ対戦動画から起こしたぷよ譜です。

## JSON の構造
動画1本が1ファイル（`data/<再生リスト>/<動画ID>.json`）です。

```json
{
  "video": "動画ID", "source": "https://www.youtube.com/watch?v=...", "title": "...", "playlist": "...",
  "games": [
    {
      "video_start": "12:34", "rules": "puyospo",
      "players": [{"name": "Aさん", "side": "1P"}, {"name": "Bさん", "side": "2P"}],
      "winner": "1P", "colors": "RGBY", "seed": 12345, "tsumo": ["RG", "BY", "..."],
      "moves": {
        "1P": [
          {"n": 1, "t": 3.2, "pair": "RG", "next": "BY", "next2": "RR", "col": 3, "dir": "up", "chain": 0,
           "board_after": ["......", "...", "..RG.."], "ojama_before": 0}
        ],
        "2P": []
      }
    }
  ]
}
```

| 項目 | 内容 |
|---|---|
| `n` | その人の何手目か |
| `t` | 動画上の時刻（秒） |
| `pair` | 置いた組。軸ぷよ、子ぷよの順 |
| `next`, `next2` | そのときの NEXT と NEXT2 |
| `col`, `dir` | 軸ぷよの列（1〜6）と子ぷよの向き（up, right, down, left） |
| `chain` | この手の連鎖数 |
| `board_after` | 連鎖とおじゃまの落下が終わった後の盤面。13段目から下へ各段6文字。R G B Y P は色、O はおじゃま、. は空き、? は不明 |
| `ojama_before` | この手の前に降ったおじゃまの数 |
| `winner` | 勝った側（1P / 2P）。分からないときは null |
| `ended` | `no_death`: どちらも倒れずに終わった局（中断など）、`unseen`: 決着の場面が映っていない局 |
| `winner_inferred` | 決着は映っていないが、N本先取の勝敗数から決めた勝者 |
| `seed`, `tsumo` | ツモの種と、その局のツモ列 |
| `uncertain` | 置き方か盤面に不確かな部分がある手 |
| `same_result` | 別の置き方でも同じ盤面になる場合の、その置き方 |
| `first_observed_move` | 途中から記録が始まる局の、最初の手 |
