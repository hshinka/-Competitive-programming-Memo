# C# で競技プログラミングする際のメモ
特に型変換のところ、本題ではないにも関わらずよく忘れて引っかかるのでメモります。

### 0. C# で型変換のメソッド
C# での型変換には、大きくは int.Parse と Convert.ToInt32 のように 2 種類のメソッドがある(TryParse もあるようだが割愛)。Parse と Convert の違いは null が渡された際に例外になるかどうか。Parse は null を渡されると例外になるが、Convert の方はならない。

What's the main difference between int.Parse() and Convert.ToInt32  
<https://stackoverflow.com/questions/199470/whats-the-main-difference-between-int-parse-and-convert-toint32>

Convert の方は char への変換できて汎用性が高そうだが、int.Parse の方がコードをパット見でわかりやすいかな？気が向いたところは両方メモるようにします。

### 1. スペース区切りで標準入力された数字を扱える状態にする
`1 2 3 4 5` といったスペース区切りで入力される問題は多い。おそらく、一般的なのは 2 回に分ける書き方。

```C#
string[] S = Console.ReadLine().Split(' ');
int[] intS = new int[S.Length];
for (int i = 0; i < S.Length; i++)
{
    intS[i] = int.Parse(S[i]);
}
```

ここで LINQ を使うと new int[] すら不要になって以下の一行で済む。
```C#
int[] intS = Console.ReadLine().Split(' ').Select(e => int.Parse(e)).ToArray();
```

### 2. 数字の文字列(数字列?) を一文字ずつ解体して合計する (string → int)
複数桁の数字が入力された後、その数字を一文字ずつ別の値として扱う場合を想定。例えば 111 と入力されたものを 1 + 1 + 1 = 3 と使うイメージ。どちらを使う場合でも char の文字コードが int に変換されてしまわないように注意が必要。

int.Parse() を使うパターン。Substring で string から一文字抜き出すようなやり方にする。

```C#
static void Main(string[] args)
{
    string S = Console.ReadLine();
    int sum = 0;
    for (int i = 0; i < S.Length; i++)
    {
        sum += int.Parse(S.Substring(i,1)); // int.Parse(S[i]) ではダメ。
    }
    Console.WriteLine(sum);
}
```
Convert を使うパターン。こちらは ToString() で変換したものを Int に Convert する必要あり。
```C#
static void Main(string[] args)
{
    string S = Console.ReadLine();
    int sum = 0;
    for (int i = 0; i < S.Length; i++)
    {
        sum += Convert.ToInt32(S[i].ToString()); // Convert.ToInt32(S[i]) ではダメ
    }
    Console.WriteLine(sum);
}
```
### 3. 配列や List のソート
標準入力や回答のための出力は 1 回だけの処理なので問題にならないが、競プロ内ではソートにかかる時間が影響するケースもある。
それを踏まえてまとめようかと考えていたが、以下のサイトで時間計測してくれていた。

- [【C#入門】配列、Listをソートする方法(降順、カスタマイズも解説)](https://www.sejuku.net/blog/40456)
- [C#の各並び替えメソッド (Array.Sort, List.Sort, OrderBy) のパフォーマンス比較](https://qiita.com/kichi2004/items/faf81775053ccf8dd8b4)

競プロなら Array で事足りるだろうし Array.Sort を使おう。念のため、昇順と降順の基本形だけメモ。
```C#
int[] length = new int[] { 3, 5, 9, 1, 42, 101, 409, 8 };
Array.Sort(length); // 昇順
Array.Reverse(length); // 降順

// 文字列でも同じ
string[] atcoder = new string[] { "a","t","c","o","d","e","r" };
Array.Sort(atcoder);
Array.Reverse(atcoder);
```

### 4. 最大公約数を求める
後日。

### 5. 優先度付きキュー
後日。
