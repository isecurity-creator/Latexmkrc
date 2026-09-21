# VSCodeでTeXを書く方法

このような画面で編集できるようになるのでとても便利です<br>
(これは大学の友人に向けて書いたものをコピペしたものであり、個人情報のため画像はご容赦ください)

また，保存すると即時にコンパイルされ，変更内容をすぐに確認することができます．<br>
(究極には自動保存にすれば即時反映されますが，コンパイル処理が立て続けに入ってPCが可哀想なのであまり推奨されません)


前提として，__**VSCodeおよびMacTeXのセットアップが済んでいる**__ものとします．(分からない場合は授業資料を見返す，AIに聞く，ググる，わかる人に聞くなどしてください)

①準備
ターミナルを開き，ホームディレクトリに移動してください．分からない場合はとりあえず次のコマンドを実行してください．
```sh
cd
```

②.latexmkrcの作成
まずターミナルに，次のコマンドを貼り付けてください．(__**まだ実行しないでください!!意図しない貼り付けがされます!!**__)
```sh
sudo pbpaste > .latexmkrc
```

次に，以下の内容をコピーしてからターミナルに戻り，↑で貼り付けておいたコマンドを実行してください．
```sh
#!/usr/bin/env perl

# LaTeX
$latex = 'uplatex -synctex=1 -halt-on-error -file-line-error %O %S';
$max_repeat = 15;

# BibTeX
$bibtex = 'upbibtex %O %S';
$biber = 'biber --bblencoding=utf8 -u -U --output_safechars %O %S';

# index
$makeindex = 'mendex %O -o %D %S';

# DVI / PDF
$dvipdf = 'dvipdfmx %O -o %D %S';
$pdf_mode = 3;

# preview
$pvc_view_file_via_temporary = 0;
if ($^O eq 'linux') {
    $dvi_previewer = "xdg-open %S";
    $pdf_previewer = "xdg-open %S";
} elsif ($^O eq 'darwin') {
    $dvi_previewer = "open %S";
    $pdf_previewer = "open %S";
} else {
    $dvi_previewer = "start %S";
    $pdf_previewer = "start %S";
}

# clean up
$clean_full_ext = "%R.synctex.gz"
```

この後，念のため
```sh
cat .latexmkrc
```
と実行し，正しくコピペされていることを確認してください．<br>
(この方法のほうが簡単だと思うので紹介しましたが，普通にvimやnanoなどを使って作成しても問題ありません)<br>
(後述するサイトに似たようなコードが書いてありますが，__**サイト上のものをコピペするとエラーが出る**__ので必ずこれをコピーしてください)

③ターミナルをexitコマンドで閉じてください．

④VSCodeに，拡張機能「LaTeX Workshop」をインストールしてください．

⑤VSCodeの設定を開き，右上のファイルマークをクリックして`settings.json`を開いてください．その後，次の内容をコピペしてください．(既に設定内容がある場合は，インデントや構文を崩さないように注意してコピペしてください．わからない場合は，既存の設定と下記の内容をAIに送りつけて「VSCodeのsettings.jsonの形式に整形して」などと送れば全文を返してくれると思います)
```json
{
    // ---------- Language ----------

    "[tex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[latex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[bibtex]": {
        // インデント幅を2にする
        "editor.tabSize": 2
    },


    // ---------- LaTeX Workshop ----------

    // 使用パッケージのコマンドや環境の補完を有効にする
    "latex-workshop.intellisense.package.enabled": true,

    // 生成ファイルを削除するときに対象とするファイル
    // デフォルト値に "*.synctex.gz" を追加
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk",
        "*.snm",
        "*.nav",
        "*.dvi",
        "*.synctex.gz"
    ],

    // 生成ファイルを "out" ディレクトリに吐き出す
    "latex-workshop.latex.outDir": "out",

    // ビルドのレシピ
    "latex-workshop.latex.recipes": [
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
    ],

    // ビルドのレシピに使われるパーツ
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-silent",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
        },
    ],
}
```


参考にしたサイト：https://qiita.com/namitech7373/items/cc94cf5d68a7ba21c4bf