# 実装が必要なもの

## git コマンド
- [ ] `symbolic-ref`
- [ ] `rev-list`
    * [ ] `--count`
    * [ ] `--left-right`
- [ ] `describe`
    * [ ] `--contains`
    * [ ] `--all`
- [ ] `status`
    * [ ] `--porcelain`
        - 出力がだいぶ変わるのでPorcelain用でメソッド作る方がいいかもしれない
    * [ ] `--ignore-submodules`
    * [ ] `--untracked-files=normal`
- [ ] `rev-parse`
    * [ ] `--is-inside-git-dir`
    * [ ] `--is-bare-repository`
    * [ ] `--abbrev-ref`
    * [ ] `--git-dir`
    * [ ] `--show-toplevel`
- [ ] `stash list`
- [ ] `now`
    * [ ] `grep` (not an option)


## コマンドで取得するもの
- [ ] git-dirの中にいるか？bareレポジトリか？
    * `command git rev-parse --is-inside-git-dir --is-bare-repository HEAD 2>/dev/null`
- [ ] デフォルトブランチ
    * `command git symbolic-ref refs/remotes/origin/HEAD`
    * 取得結果を `| sed 's#^refs/remotes/origin/##'`
- [ ] デフォルトブランチにマージされてないコミット数
    * `command git rev-list --count --left-right ${_default_branch}...HEAD 2>/dev/null`
    * ただし上記の「デフォルトブランチ」が取得できている場合
- [ ] ブランチ名
    * `command git symbolic-ref HEAD 2>/dev/null`
- [ ] （ブランチ名が取得できなければ） detachedとしてあらためてブランチ名を取得
    * `command git describe --contains --all HEAD 2>/dev/null`
    * これがエラーの場合に最後に何かするが要確認
- [ ] ファイルの変更を取得
    * `${(f)$(command git status --porcelain --ignore-submodules --untracked-files=normal 2>/dev/null)}`
    * ↑なんか変数展開？しているので要確認
        - 改行を単語の区切りとして処理している（つまり1行ずつに分けている）
    * 取得結果を解析して変更数を取得しなければならない
- [ ] upstreamと差分（ahead/behind）
    * `command git rev-list --count --left-right '@{upstream}'...HEAD 2>/dev/null`
- [ ] 上記でカウント取れたら upstream を取得
    * `command git rev-parse --abbrev-ref '@{upstream}' 2>/dev/null`
- [ ] stash状況
    * `command git stash list`
    * 取れたら行数をカウント
- [ ] `git-now` があれば `now` 数をカウント
    * `command git now grep`
    * 取れたら行数をカウント
- [ ] リポジトリ情報（gitディレクトリ、workディレクトリのルート）
    * `command git rev-parse --git-dir --show-toplevel 2>/dev/null`
