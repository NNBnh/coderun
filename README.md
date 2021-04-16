<h1 align="center"><i>Coderun</i></h1>
<p align="center">Code runner CLI that can run any languages</p>
<p align="center"><a href="https://github.com/NNBnh/coderun/blob/main/LICENSE"><img src="https://img.shields.io/github/license/NNBnh/coderun?labelColor=073551&color=4EAA25&style=for-the-badge" alt="License: GPL-3.0"></a> <img src="https://img.shields.io/badge/development-completed-%234EAA25.svg?labelColor=073551&style=for-the-badge&logoColor=FFFFFF" alt="Development completed"></p>
<p align="center"><a href="https://github.com/NNBnh/coderun/watchers"><img src="https://img.shields.io/github/watchers/NNBnh/coderun?labelColor=073551&color=4EAA25&style=flat-square"></a> <a href="https://github.com/NNBnh/coderun/stargazers"><img src="https://img.shields.io/github/stars/NNBnh/coderun?labelColor=073551&color=4EAA25&style=flat-square"></a> <a href="https://github.com/NNBnh/coderun/network/members"><img src="https://img.shields.io/github/forks/NNBnh/coderun?labelColor=073551&color=4EAA25&style=flat-square"></a> <a href="https://github.com/NNBnh/coderun/issues"><img src="https://img.shields.io/github/issues/NNBnh/coderun?labelColor=073551&color=4EAA25&style=flat-square"></a></p>

## 💡 About
**Coderun** is a code runner CLI that can run any languages.

https://user-images.githubusercontent.com/43980777/108585543-92714300-737b-11eb-8296-1bf0cf79437f.mp4

### 📔 Story
After a long time searching for something like a CLI's version [Code Runner](https://github.com/formulahendry/vscode-code-runner) asking people on [r/kakoune](https://www.reddit.com/r/kakoune/comments/kuh4km/is_there_anything_like_code_runner_for_kakoune) and still doesn't find it, I decided to create my own with only **8 lines** of [`portable sh`](https://github.com/dylanaraps/pure-sh-bible):

```sh
#!/bin/sh
DIRECTORY=$(dirname "$1")
FILE=$(basename "$1")
FULL="$DIRECTORY/$FILE"
NAME="${FILE%.*}"
EXTENSION=$(printf '%s' "$FILE" | sed -e "s/^$NAME\.*//" -e 's/+/p/g' -e 's/-/_/g')
eval "$(eval "printf '%s' \"\$CODERUN_$EXTENSION\"")"
exit 0
```

and a [Kakoune](http://kakoune.org) plugin: [`coderun.kak`](https://github.com/NNBnh/coderun.kak).

## 🚀 Setup
### 🧾 Dependencies
- `sh` to process
- The language that you want to run (obviously)

### 📥 Installation
#### 🔧 Manually
- Option 1: using `curl`

```sh
curl https://raw.githubusercontent.com/NNBnh/coderun/main/bin/coderun > ~/.local/bin/coderun
chmod +x ~/.local/bin/coderun
```

- Option 2: using `git`

```sh
git clone https://github.com/NNBnh/coderun.git ~/.local/share/coderun
ln -s ~/.local/share/coderun/bin/coderun ~/.local/bin/coderun
```

#### 📦 Package manager
For [`bpkg`](https://github.com/bpkg/bpkg) user:

```sh
bpkg install NNBnh/coderun
```

For [Basher](https://github.com/bpkg/bpkg) user:

```sh
basher install NNBnh/coderun
```

> *If you can and want to port Coderun to other package managers, feel free to do so.*

## ⌨️ Usage
Run `coderun` in the terminal:

```sh
coderun FILE
```

> *NOTE: Coderun out of the box cannot run code, you need to [configure it](#%EF%B8%8F-configuration).*

## ⚙️ Configuration
Coderun is configured through environment variables: `export CODERUN_<extension>="<method>"`

`<extension>`:
- Extension is case sensitive (e.g: `c` is different than `C`)
- Because the shell does not accept values with some symbols so the characters in the side extension will be converted:
  - From `+` to `p` (e.g: `c++ => cpp`)
  - From `-` to `_` (e.g: `php-s => php_s`)

`<method>`:
- Keep in mind that the `<method>` will be run through `eval` (e.g: `// => /`)
- Supported parameters:

|  Parameter  |      Example     |   Description  |
|-------------|------------------|----------------|
|`\$FULL`     |`/home/user/foo.c`|File's full path|
|`\$DIRECTORY`|`/home/user      `|File's directory|
|`\$FILE`     |`           foo.c`|File's base name|
|`\$NAME`     |`           foo  `|File's name only|
|`\$EXTENSION`|`               c`|File's extension|

Examples:

```sh
export CODERUN_="chmod +x \$FULL && \$FULL"
export CODERUN_sh="$CODERUN_"
export CODERUN_textart="$CODERUN_sh"
export CODERUN_bash="bash \$FULL"
export CODERUN_zsh="zsh \$FULL"
export CODERUN_fish="fish \$FULL"
export CODERUN_1="man \$FULL"
export CODERUN_2="$CODERUN_1"
export CODERUN_3="$CODERUN_1"
export CODERUN_4="$CODERUN_1"
export CODERUN_5="$CODERUN_1"
export CODERUN_6="$CODERUN_1"
export CODERUN_7="$CODERUN_1"
export CODERUN_8="$CODERUN_1"
export CODERUN_9="$CODERUN_1"
export CODERUN_js="node \$FULL"
export CODERUN_cjs="$CODERUN_js"
export CODERUN_mjs="$CODERUN_js"
export CODERUN_java="cd \$DIRECTORY && javac \$FILE && java \$NAME"
export CODERUN_class="$CODERUN_java"
export CODERUN_jar="$CODERUN_java"
export CODERUN_c="cd \$DIRECTORY && gcc \$FILE -o \$NAME && \$DIRECTORY/\$NAME"
export CODERUN_h="$CODERUN_c"
export CODERUN_cc="cd \$DIRECTORY && g++ \$FILE -o \$NAME && \$DIRECTORY/\$NAME"
export CODERUN_C="$CODERUN_cc"
export CODERUN_cpp="$CODERUN_cc"
export CODERUN_cxx="$CODERUN_cc"
export CODERUN_hh="$CODERUN_cc"
export CODERUN_H="$CODERUN_cc"
export CODERUN_hpp="$CODERUN_cc"
export CODERUN_hxx="$CODERUN_cc"
export CODERUN_m="cd \$DIRECTORY && gcc -framework Cocoa \$FILE -o \$NAME && \$DIRECTORY/\$NAME"
export CODERUN_mm="$CODERUN_m"
export CODERUN_M="$CODERUN_m"
export CODERUN_php="php \$FULL"
export CODERUN_phtml="$CODERUN_php"
export CODERUN_php3="$CODERUN_php"
export CODERUN_php4="$CODERUN_php"
export CODERUN_php5="$CODERUN_php"
export CODERUN_php7="$CODERUN_php"
export CODERUN_phps="$CODERUN_php"
export CODERUN_php_s="$CODERUN_php"
export CODERUN_pht="$CODERUN_php"
export CODERUN_phar="$CODERUN_php"
export CODERUN_py="python -u \$FULL"
export CODERUN_pyi="$CODERUN_py"
export CODERUN_pyc="$CODERUN_py"
export CODERUN_pyd="$CODERUN_py"
export CODERUN_pyo="$CODERUN_py"
export CODERUN_pyw="$CODERUN_py"
export CODERUN_pyz="$CODERUN_py"
export CODERUN_perl="perl \$FULL"
export CODERUN_plx="$CODERUN_perl"
export CODERUN_pl="$CODERUN_perl"
export CODERUN_pm="$CODERUN_perl"
export CODERUN_xs="$CODERUN_perl"
export CODERUN_t="$CODERUN_perl"
export CODERUN_pod="$CODERUN_perl"
export CODERUN_rb="ruby \$FULL"
export CODERUN_go="go run \$FULL"
export CODERUN_gccgo="$CODERUN_go"
export CODERUN_lua="lua \$FULL"
export CODERUN_groovy="groovy \$FULL"
export CODERUN_gvy="$CODERUN_groovy"
export CODERUN_gy="$CODERUN_groovy"
export CODERUN_gsh="$CODERUN_groovy"
export CODERUN_ps1="powershell -ExecutionPolicy ByPass -File \$FULL"
export CODERUN_ps1xml="$CODERUN_ps1"
export CODERUN_psc1="$CODERUN_ps1"
export CODERUN_psd1="$CODERUN_ps1"
export CODERUN_psm1="$CODERUN_ps1"
export CODERUN_pssc="$CODERUN_ps1"
export CODERUN_psrc="$CODERUN_ps1"
export CODERUN_cdxml="$CODERUN_ps1"
export CODERUN_cmd="cmd /c \$FULL"
export CODERUN_bat="$CODERUN_cmd"
export CODERUN_btm="$CODERUN_cmd"
export CODERUN_fsi="fsi \$FULL"
export CODERUN_fs="$CODERUN_fsi"
export CODERUN_fsx="$CODERUN_fsi"
export CODERUN_fsscript="$CODERUN_fsi"
export CODERUN_cs="scriptcs \$FULL"
export CODERUN_csx="$CODERUN_cs"
export CODERUN_vbs="cscript //Nologo \$FULL"
export CODERUN_vbe="$CODERUN_vbs"
export CODERUN_wsf="$CODERUN_vbs"
export CODERUN_wsc="$CODERUN_vbs"
export CODERUN_ts="ts-node \$FULL"
export CODERUN_tsx="$CODERUN_ts"
export CODERUN_coffee="coffee \$FULL"
export CODERUN_litcoffee="$CODERUN_coffee"
export CODERUN_scala="scala \$FULL"
export CODERUN_sc="$CODERUN_scala"
export CODERUN_swift="swift \$FULL"
export CODERUN_jl="julia \$FULL"
export CODERUN_cr="crystal \$FULL"
export CODERUN_ml="ocaml \$FULL"
export CODERUN_mli="$CODERUN_ml"
export CODERUN_r="Rscript \$FULL"
export CODERUN_rdata="$CODERUN_r"
export CODERUN_rds="$CODERUN_r"
export CODERUN_rda="$CODERUN_r"
export CODERUN_scpt="osascript \$FULL"
export CODERUN_scptd="$CODERUN_scpt"
export CODERUN_applescript="$CODERUN_scpt"
export CODERUN_clj="lein exec \$FULL"
export CODERUN_cljs="$CODERUN_clj"
export CODERUN_cljc="$CODERUN_clj"
export CODERUN_edn="$CODERUN_clj"
export CODERUN_hx="haxe --cwd \$DIRECTORY --run \$NAME"
export CODERUN_hxml="$CODERUN_hx"
export CODERUN_rs="cd \$DIRECTORY && rustc \$FILE && \$DIRECTORY/\$NAME"
export CODERUN_rlib="$CODERUN_rs"
export CODERUN_rkt="racket \$FULL"
export CODERUN_scm="csi -script \$FULL"
export CODERUN_ss="$CODERUN_scm"
export CODERUN_ahk="autohotkey \$FULL"
export CODERUN_au3="autoit3 \$FULL"
export CODERUN_dart="dart \$FULL"
export CODERUN_pp="cd \$DIRECTORY && fpc \$FILE && \$DIRECTORY/\$NAME"
export CODERUN_pas="$CODERUN_pp"
export CODERUN_inc="$CODERUN_pp"
export CODERUN_d="cd \$DIRECTORY && dmd \$FILE && \$DIRECTORY/\$NAME"
export CODERUN_hs="runhaskell \$FULL"
export CODERUN_lhs="$CODERUN_hs"
export CODERUN_nim="nim compile --verbosity:0 --hints:off --run \$FULL"
export CODERUN_nims="$CODERUN_nim"
export CODERUN_nimbls="$CODERUN_nim"
export CODERUN_lisp="sbcl --script \$FULL"
export CODERUN_kit="kitc --run \$FULL"
export CODERUN_v="v run \$FULL"
export CODERUN_sass="sass --style expanded \$FULL"
export CODERUN_scss="scss --style expanded \$FULL"
export CODERUN_less="cd \$DIRECTORY && lessc \$FILE \$NAME.css"
export CODERUN_f="cd \$DIRECTORY && gfortran \$FILE -o \$NAME && \$DIRECTORY/\$NAME"
export CODERUN_for="$CODERUN_f"
export CODERUN_f90="$CODERUN_f"
```

> *This config is mostly ported from [Code Runner](https://github.com/formulahendry/vscode-code-runner/blob/101a718478136f5a7022fd4d4aaa22bf9a82176d/package.json#L127-L176).*

## 💌 Credits
Special thanks to:
- [**Code Runner**](https://github.com/formulahendry/vscode-code-runner) by [Jun Han](https://github.com/formulahendry)

<br><br><br><br>

---

> <h1 align="center">Made with ❤️ by <a href="https://github.com/NNBnh"><i>NNB</i></a></h1>
>
> <p align="center"><a href="https://www.buymeacoffee.com/nnbnh"><img src="https://img.shields.io/badge/buy_me_a_coffee%20-%23F7CA88.svg?logo=buy-me-a-coffee&logoColor=333333&style=for-the-badge" alt="Buy Me a Coffee"></p>
