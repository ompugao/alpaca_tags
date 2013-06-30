# alpaca_update_tags

## 概要

*ctagsの生成を便利にしよう！*

- vimprocを使って、非同期でtagsの生成
- Ruby/Bundlerに対応して、必要最低限のtagsのみを非同期生成

![Demo 実は少しミスってる](http://gifzo.net/BKv9ukBQ22q.gif)

## コマンド

*AlpacaTagsSet*
setl tagsの自動化

*AlpacaTagsUpdate*
Git配下のディレクトリを対象にtagsを生成する

*AlpacaTagsBundle*
Git直下のGemfileを元にtagsを生成する。

## 設定

*g:alpaca_update_tags_config*
自身でオプションを作る
'_'では、デフォルトする設定を記述する

g.u
let g:alpaca_update_tags_config = {
      \ '_' : '-R --sort=yes',
      \ 'ruby' : '--languages=Ruby',
      \ }

`:AlpacaTagsUpdate ruby`で、`ctags -R --sort=yes --languages=Ruby`が実行される。

## オススメの設定

```
" NeoBundle
NeoBundleLazy 'taichouchou2/alpaca_update_tags', {
      \ 'depends': 'Shougo/vimproc',
      \ 'autoload' : {
      \   'commands': ['AlpacaTagsUpdate', 'AlpacaTagsSet', 'AlpacaTagsUpdateBundle']
      \ }}

" example...
" ~/.ctagsにctagsの設定ファイルを設置します。現在無い人は、このディレクトリ内の.ctagsをコピーしてください。
" 適切なlanguageは`ctags --list-maps=all`で見つけてください。人によりますので。
let g:alpaca_update_tags_config = {
      \ '_' : '-R --sort=yes',
      \ 'js' : '--languages=+js',
      \ '-js' : '--languages=-js,JavaScript',
      \ 'vim' : '--languages=+Vim,vim',
      \ '-vim' : '--languages=-Vim,vim',
      \ '-style': '--languages=-css,sass,scss,js,JavaScript,html',
      \ 'scss' : '--languages=+scss --languages=-css,sass',
      \ 'sass' : '--languages=+sass --languages=-css,scss',
      \ 'css' : '--languages=+css',
      \ 'java' : '--languages=+java $JAVA_HOME/src',
      \ 'ruby': '--languages=+Ruby',
      \ 'coffee': '--languages=+coffee',
      \ '-coffee': '--languages=-coffee',
      \ 'bundle': '--languages=+Ruby --languages=-css,sass,scss,js,JavaScript,coffee',
      \ }

aug AlpacaUpdateTags
  au!
  au FileWritePost,BufWritePost * AlpacaTagsUpdate -style
  " bundleのオプションは自動で追加して実行します。
  au FileWritePost,BufWritePost Gemfile AlpacaTagsUpdateBundle
  au FileReadPost,BufEnter * AlpacaTagsSet
aug END
```

## TODO

- Gemfile以外のnpm, python ...etcのtags生成機能を追加
