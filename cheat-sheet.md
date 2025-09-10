# shell file
<(コマンドリスト)               # コマンドの結果をファイルとして扱う ()は必須
>(コマンドリスト)               # 出力先をコマンドに渡す

# shell regex

*.o~hoge.o                      # hoge.o 以外の全ての *.o
fix-<4-10>.gz                   # fix-4.gz～fix-10.gzのうち存在するファイル
{0..2}                          # 0 1 2
{00..02}                        # 00 01 02


# M(月), w(週), h(時間), m(分), s(秒)
**/*.orig(m+30) **/*(@)         # 30日以上古い**/*.origと**/*なsymlink
**/*(mm-1)                      # 1分以内のファイル
**/*.(cpp|d)                    # *.cpp or *.d
zmv -n '(*)-(*)' '${1}-$(printf "%02d" ${2})'


# debug
ulimit -c unlimited             # コアダンプ時にcoreを吐かせる(容量は無制限)

# term
sudo screen /dev/device 115200  # screen コマンドでシリアル通信 // 切断は C-a k

# Shell find

find . -name \*hoge\* -executable -type f # 実行ファイルのみ検索
find . -name '*.rb' -type f -not -path '*node_modules*' -not -path '*vendor*' # exclude dir
find . -name '*.cpp' | xargs wc -l # 行数カウンタ
find . -name '*.cpp' -type f | xargs grep -Ev '^[[:space:]]*((/?\*.*/?)|(//.*))$' | wc -l # 行数カウンタ(空行無視)

# network
socat -d -d pty,raw,echo=0 pty,raw,echo=0 # serial port
nc -zv $DOMAIN 443 # port check for macos domain or ip addr


# Text
sed -e 's/$/\r/' inputfile > outputfile                # UNIX to DOS  (adding CRs)
sed -e 's/\r$//' inputfile > outputfile                # DOS  to UNIX (removing CRs)
perl -pe 's/\r\n|\n|\r/\r\n/g' inputfile > outputfile  # Convert to DOS
perl -pe 's/\r\n|\n|\r/\n/g'   inputfile > outputfile  # Convert to UNIX
perl -pe 's/\r\n|\n|\r/\r/g'   inputfile > outputfile  # Convert to old Mac


# complession
tar xf foo.tar.gz # unzip. auto detection for gzip, zst
tar caf foo foo.tar.zst # zip. auto detection for gzip, zst

tar cf - . |lz4 - hoge.tar.lz4 # need `cd path/to/dir`
lz4 -d hoge.tar.lz4 | tar xf - -C hoge
tar -c somefolder | pv | lz4 -B4 | ssh example.com "lz4 -d | tar x"


# fs
fswatch -0 ~/Downloads | xargs -0 -n1 -I{} sh -c 'if [[ "{}" == *.csv ]]; then cat "{}" && echo "==="; fi' # [mac] monitor new file -> cat file

