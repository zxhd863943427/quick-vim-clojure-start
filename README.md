# quick-vim-clojure-start
快速搭建 vim clojure 开发环境
* clojure

  * 经过一天的探索，我不得不承认，light table 已经落幕了，它无论在 Ubuntu、windows 10、还是 wsl 上，都十分统一地报错，提示没能连接上。或许 light table 还有研究的价值，但是在目前的情况下，打算使用 light table 作为新手开发的 ide 只能说是异想天开。
  * 相比之下，vim 加上 fireplace 才是最不折腾的选项，在 vim 8 上，只需要使用 vim 加 pathogen 包管理器，再 git clone fireplace 和相关的依赖包就行了。在网速良好的情况下，不应超过 0.5h 就能完成配置
  * clojure vim 环境一键配置

    * 写了个脚本，来完成更容易的 clojure 环境配置

      * ```bash
        target=~/.vim/vimrc

        #备份原有vim配置
        mv ~/.vim ~/.vim.bak
        mv ~/.vimrc ~/.vimrc.bak
        mv ~/.gvimrc ~/.gvimrc.bak

        # 安装pathogen插件
        cd ~
        mkdir -p ~/.vim/autoload ~/.vim/bundle ~/temp
        git clone https://github.com/tpope/vim-pathogen.git ~/temp/pathogen
        cp ~/temp/pathogen/autoload/pathogen.vim ~/.vim/autoload/

        # 安装repl插件
        cd ~/.vim/bundle
        git clone https://github.com/tpope/vim-fireplace.git
        git clone https://github.com/tpope/vim-salve.git
        git clone https://github.com/tpope/vim-projectionist.git
        git clone https://github.com/tpope/vim-dispatch.git
        vim -u NONE -c "helptags fireplace/doc" -c q

        #备份原有lein配置
        mv ~/.lein/profiles.clj ~/.lein/profiles.clj.bak
        echo "{:user {:plugins [[cider/cider-nrepl "0.27.3"]]}}" >> ~/.lein/profiles.clj

        # 安装indent插件
        cd ~/.vim/bundle
        git clone https://github.com/guns/vim-clojure-static.git


        #安装彩色括号
        mkdir -p ~/.vim/pack/luochen1990/start
        cd ~/.vim/pack/luochen1990/start
        git clone https://github.com/luochen1990/rainbow.git


        cd ~
        #设置 vimrc 配置
        cat >>$target<<EOF
        execute pathogen#infect()
        syntax on
        filetype plugin indent on

        let g:rainbow_active = 1
        let g:rainbow_conf = {
                \       'guifgs': ['royalblue3', 'darkorange3', 'seagreen3', 'firebrick'],
                \       'operators': '_,_',
                \       'ctermfgs': [1,13,12,11,10,9,8,6,5,4,3,2,14],
                \       'parentheses': ['start=/(/ end=/)/ fold', 'start=/\[/ end=/\]/ fold', 'start=/{/ end=/}/ fold'],
                \       'separately': {
                \               '*': {},
                \               'tex': {
                \                       'parentheses': ['start=/(/ end=/)/', 'start=/\[/ end=/\]/'],
                \               },
                \               'lisp': {
                \                       'guifgs': ['royalblue3', 'darkorange3', 'seagreen3', 'firebrick', 'darkorchid3'],
                \               },
                \               'vim': {
                \                       'parentheses': ['start=/(/ end=/)/', 'start=/\[/ end=/\]/', 'start=/{/ end=/}/ fold', 'start=/(/ end=/)/ containedin=vimFuncBody', 'start=/\[/ end=/\]/ containedin=vimFuncBody', 'start=/{/ end=/}/ fold containedin=vimFuncBody'],
                \               },
                \               'html': {
                \                       'parentheses': ['start=/\v\<((area|base|br|col|embed|hr|img|input|keygen|link|menuitem|meta|param|source|track|wbr)[ >])@!\z([-_:a-zA-Z0-9]+)(\s+[-_:a-zA-Z0-9]+(\=("[^"]*"|'."'".'[^'."'".']*'."'".'|[^ '."'".'"><=\`]*))?)*\>/ end=#</\z1># fold'],
                \               },
                \               'css': 0,
                \       }
                \}
        EOF

        #设置 ftplugin/clojure.vim 配置
        mkdir -p ~/.vim/ftplugin
        cd ~/.vim/ftplugin
        cat >>clojure.vim<<EOF
        :inoremap ( ()<Esc>ha
        :inoremap [ []<Esc>ha
        :inoremap { {}<Esc>ha
        :inoremap " ""<Esc>ha
        EOF

        cd ~
        ```
