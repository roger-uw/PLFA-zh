---
src       : src/plfa/Relations.lagda
title     : "Relations: 关系的归纳定义"
layout    : page
prev      : /Induction/
permalink : /Relations/
next      : /Equality/
translators : ["Fangyi Zhou"]
progress  : 100
---

<pre class="Agda">{% raw %}<a id="190" class="Keyword">module</a> <a id="197" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}" class="Module">plfa.Relations</a> <a id="212" class="Keyword">where</a>{% endraw %}</pre>

在定义了加法和乘法等运算以后，下一步我们来定义关系（Relation），比如说*小于等于*。
{::comment}
After having defined operations such as addition and multiplication,
the next step is to define relations, such as _less than or equal_.
{:/}

## 导入
{::comment}
## Imports
{:/}

<pre class="Agda">{% raw %}<a id="480" class="Keyword">import</a> <a id="487" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="525" class="Symbol">as</a> <a id="528" class="Module">Eq</a>
<a id="531" class="Keyword">open</a> <a id="536" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="539" class="Keyword">using</a> <a id="545" class="Symbol">(</a><a id="546" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">_≡_</a><a id="549" class="Symbol">;</a> <a id="551" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a><a id="555" class="Symbol">;</a> <a id="557" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1170" class="Function">cong</a><a id="561" class="Symbol">;</a> <a id="563" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.Core.html#838" class="Function">sym</a><a id="566" class="Symbol">)</a>
<a id="568" class="Keyword">open</a> <a id="573" class="Keyword">import</a> <a id="580" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="589" class="Keyword">using</a> <a id="595" class="Symbol">(</a><a id="596" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="597" class="Symbol">;</a> <a id="599" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="603" class="Symbol">;</a> <a id="605" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a><a id="608" class="Symbol">;</a> <a id="610" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">_+_</a><a id="613" class="Symbol">;</a> <a id="615" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#433" class="Primitive Operator">_*_</a><a id="618" class="Symbol">;</a> <a id="620" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#320" class="Primitive Operator">_∸_</a><a id="623" class="Symbol">)</a>
<a id="625" class="Keyword">open</a> <a id="630" class="Keyword">import</a> <a id="637" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="657" class="Keyword">using</a> <a id="663" class="Symbol">(</a><a id="664" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#9708" class="Function">+-comm</a><a id="670" class="Symbol">;</a> <a id="672" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#9272" class="Function">+-suc</a><a id="677" class="Symbol">)</a>
<a id="679" class="Keyword">open</a> <a id="684" class="Keyword">import</a> <a id="691" href="https://agda.github.io/agda-stdlib/Data.List.html" class="Module">Data.List</a> <a id="701" class="Keyword">using</a> <a id="707" class="Symbol">(</a><a id="708" href="https://agda.github.io/agda-stdlib/Agda.Builtin.List.html#80" class="Datatype">List</a><a id="712" class="Symbol">;</a> <a id="714" href="https://agda.github.io/agda-stdlib/Data.List.Base.html#8785" class="InductiveConstructor">[]</a><a id="716" class="Symbol">;</a> <a id="718" href="https://agda.github.io/agda-stdlib/Agda.Builtin.List.html#132" class="InductiveConstructor Operator">_∷_</a><a id="721" class="Symbol">)</a>
<a id="723" class="Keyword">open</a> <a id="728" class="Keyword">import</a> <a id="735" href="https://agda.github.io/agda-stdlib/Function.html" class="Module">Function</a> <a id="744" class="Keyword">using</a> <a id="750" class="Symbol">(</a><a id="751" href="https://agda.github.io/agda-stdlib/Function.html#1068" class="Function">id</a><a id="753" class="Symbol">;</a> <a id="755" href="https://agda.github.io/agda-stdlib/Function.html#769" class="Function Operator">_∘_</a><a id="758" class="Symbol">)</a>
<a id="760" class="Keyword">open</a> <a id="765" class="Keyword">import</a> <a id="772" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="789" class="Keyword">using</a> <a id="795" class="Symbol">(</a><a id="796" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html#464" class="Function Operator">¬_</a><a id="798" class="Symbol">)</a>
<a id="800" class="Keyword">open</a> <a id="805" class="Keyword">import</a> <a id="812" href="https://agda.github.io/agda-stdlib/Data.Empty.html" class="Module">Data.Empty</a> <a id="823" class="Keyword">using</a> <a id="829" class="Symbol">(</a><a id="830" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a><a id="831" class="Symbol">;</a> <a id="833" href="https://agda.github.io/agda-stdlib/Data.Empty.html#360" class="Function">⊥-elim</a><a id="839" class="Symbol">)</a>{% endraw %}</pre>

## 定义关系
{::comment}
## Defining relations
{:/}

小于等于这个关系有无穷个实例，如下所示：
{::comment}
The relation _less than or equal_ has an infinite number of
instances.  Here are a few of them:
{:/}

    0 ≤ 0     0 ≤ 1     0 ≤ 2     0 ≤ 3     ...
              1 ≤ 1     1 ≤ 2     1 ≤ 3     ...
                        2 ≤ 2     2 ≤ 3     ...
                                  3 ≤ 3     ...
                                            ...

但是，我们仍然可以用几行有限的定义来表示所有的实例，如下文所示的一对推理规则：
{::comment}
And yet, we can write a finite definition that encompasses
all of these instances in just a few lines.  Here is the
definition as a pair of inference rules:
{:/}

    z≤n --------
        zero ≤ n

        m ≤ n
    s≤s -------------
        suc m ≤ suc n

以及其 Agda 定义：
{::comment}
And here is the definition in Agda:
{:/}
<pre class="Agda">{% raw %}<a id="1665" class="Keyword">data</a> <a id="_≤_"></a><a id="1670" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">_≤_</a> <a id="1674" class="Symbol">:</a> <a id="1676" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1678" class="Symbol">→</a> <a id="1680" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1682" class="Symbol">→</a> <a id="1684" class="PrimitiveType">Set</a> <a id="1688" class="Keyword">where</a>

  <a id="_≤_.z≤n"></a><a id="1697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a> <a id="1701" class="Symbol">:</a> <a id="1703" class="Symbol">∀</a> <a id="1705" class="Symbol">{</a><a id="1706" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1706" class="Bound">n</a> <a id="1708" class="Symbol">:</a> <a id="1710" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1711" class="Symbol">}</a>
      <a id="1719" class="Comment">--------</a>
    <a id="1732" class="Symbol">→</a> <a id="1734" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="1739" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="1741" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1706" class="Bound">n</a>

  <a id="_≤_.s≤s"></a><a id="1746" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="1750" class="Symbol">:</a> <a id="1752" class="Symbol">∀</a> <a id="1754" class="Symbol">{</a><a id="1755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1755" class="Bound">m</a> <a id="1757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1757" class="Bound">n</a> <a id="1759" class="Symbol">:</a> <a id="1761" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1762" class="Symbol">}</a>
    <a id="1768" class="Symbol">→</a> <a id="1770" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1755" class="Bound">m</a> <a id="1772" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="1774" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1757" class="Bound">n</a>
      <a id="1782" class="Comment">-------------</a>
    <a id="1800" class="Symbol">→</a> <a id="1802" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1806" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1755" class="Bound">m</a> <a id="1808" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="1810" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1814" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1757" class="Bound">n</a>{% endraw %}</pre>
在这里，`z≤n` 和 `s≤s`（无空格）是构造器的名称，`zero ≤ n`、`m ≤ n` 和
`suc m ≤ suc n` （带空格）是类型。在这里我们第一次用到了 *索引数据类型*
(Indexed datatype）。我们使用 `m` 和 `n` 这两个自然数来索引 `m ≤ n` 这个类型。
在 Agda 里，由两个及以上短横线开始的行是注释行，我们巧妙利用这一语法特性，用上述形式来表示相应的推理规则。在后文中，我们还会继续使用这一形式。
{::comment}
Here `z≤n` and `s≤s` (with no spaces) are constructor names, while
`zero ≤ n`, and `m ≤ n` and `suc m ≤ suc n` (with spaces) are types.
This is our first use of an _indexed_ datatype, where the type `m ≤ n`
is indexed by two naturals, `m` and `n`.  In Agda any line beginning
with two or more dashes is a comment, and here we have exploited that
convention to write our Agda code in a form that resembles the
corresponding inference rules, a trick we will use often from now on.
{:/}

这两条定义告诉我们相同的两件事：
{::comment}
Both definitions above tell us the same two things:
{:/}

* *起始步骤*: 对于所有的自然数 `n`，命题 `zero ≤ n` 成立。
* *归纳步骤*：对于所有的自然数 `m` 和 `n`，如果命题 `m ≤ n` 成立，
  那么命题 `suc m ≤ suc n` 成立。

{::comment}
* _Base case_: for all naturals `n`, the proposition `zero ≤ n` holds.
* _Inductive case_: for all naturals `m` and `n`, if the proposition
  `m ≤ n` holds, then the proposition `suc m ≤ suc n` holds.
{:/}

实际上，他们分别给我们更多的信息：

{::comment}
In fact, they each give us a bit more detail:
{:/}

* *起始步骤*: 对于所有的自然数 `n`，构造器 `z≤n` 提供了 `zero ≤ n` 成立的证明。
* *归纳步骤*：对于所有的自然数 `m` 和 `n`，构造器 `s≤s` 将 `m ≤ n` 成立的证明
  转化为 `suc m ≤ suc n` 成立的证明。

{::comment}
* _Base case_: for all naturals `n`, the constructor `z≤n`
  produces evidence that `zero ≤ n` holds.
* _Inductive case_: for all naturals `m` and `n`, the constructor
  `s≤s` takes evidence that `m ≤ n` holds into evidence that
  `suc m ≤ suc n` holds.
{:/}

例如，我们在这里以推理规则的形式写出 `2 ≤ 4` 的证明：

{::comment}
For example, here in inference rule notation is the proof that
`2 ≤ 4`:
{:/}

      z≤n -----
          0 ≤ 2
     s≤s -------
          1 ≤ 3
    s≤s ---------
          2 ≤ 4

下面是对应的 Agda 证明：

{::comment}
And here is the corresponding Agda proof:
{:/}
<pre class="Agda">{% raw %}<a id="3781" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3781" class="Function">_</a> <a id="3783" class="Symbol">:</a> <a id="3785" class="Number">2</a> <a id="3787" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="3789" class="Number">4</a>
<a id="3791" class="Symbol">_</a> <a id="3793" class="Symbol">=</a> <a id="3795" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="3799" class="Symbol">(</a><a id="3800" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="3804" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a><a id="3807" class="Symbol">)</a>{% endraw %}</pre>


## 隐式参数
{::comment}
## Implicit arguments
{:/}

这是我们第一次使用隐式参数。定义不等式时，构造器的定义中使用了 `∀`，
就像我们在下面的命题中使用 `∀` 一样：
{::comment}
This is our first use of implicit arguments.  In the definition of
inequality, the two lines defining the constructors use `∀`, very
similar to our use of `∀` in propositions such as:
{:/}

    +-comm : ∀ (m n : ℕ) → m + n ≡ n + m

但是我们这里的定义使用了花括号 `{ }`，而不是小括号 `( )`。
这意味着参数是 *隐式的* （Implicit），不需要额外声明。实际上，Agda 的类型检查器会 *推导* 出它们。因此，我们在 `m + n ≡ n + m` 的证明中需要写出 `+-comm m n`，
在 `zero ≤ n` 的证明中可以省略 `n`。同理，如果 `m≤n` 是 `m ≤ n`的证明，
那么我们写出 `s≤s m≤n` 作为 `suc m ≤ suc n` 的证明，无需声明 `m` 和 `n`。
{::comment}
However, here the declarations are surrounded by curly braces `{ }`
rather than parentheses `( )`.  This means that the arguments are
_implicit_ and need not be written explicitly; instead, they are
_inferred_ by Agda's typechecker. Thus, we write `+-comm m n` for the
proof that `m + n ≡ n + m`, but `z≤n` for the proof that `zero ≤ n`,
leaving `n` implicit.  Similarly, if `m≤n` is evidence that `m ≤ n`,
we write `s≤s m≤n` for evidence that `suc m ≤ suc n`, leaving both `m`
and `n` implicit.
{:/}

如果有希望的话，我们也可以在大括号里显式声明隐式参数。例如，下面是 `2 ≤ 4` 的 Agda
证明，包括了显式声明了的隐式参数：
{::comment}
If we wish, it is possible to provide implicit arguments explicitly by
writing the arguments inside curly braces.  For instance, here is the
Agda proof that `2 ≤ 4` repeated, with the implicit arguments made
explicit:
{:/}
<pre class="Agda">{% raw %}<a id="5252" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#5252" class="Function">_</a> <a id="5254" class="Symbol">:</a> <a id="5256" class="Number">2</a> <a id="5258" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="5260" class="Number">4</a>
<a id="5262" class="Symbol">_</a> <a id="5264" class="Symbol">=</a> <a id="5266" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5270" class="Symbol">{</a><a id="5271" class="Number">1</a><a id="5272" class="Symbol">}</a> <a id="5274" class="Symbol">{</a><a id="5275" class="Number">3</a><a id="5276" class="Symbol">}</a> <a id="5278" class="Symbol">(</a><a id="5279" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5283" class="Symbol">{</a><a id="5284" class="Number">0</a><a id="5285" class="Symbol">}</a> <a id="5287" class="Symbol">{</a><a id="5288" class="Number">2</a><a id="5289" class="Symbol">}</a> <a id="5291" class="Symbol">(</a><a id="5292" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a> <a id="5296" class="Symbol">{</a><a id="5297" class="Number">2</a><a id="5298" class="Symbol">}))</a>{% endraw %}</pre>
也可以额外加上参数的名字：
{::comment}
One may also identify implicit arguments by name:
{:/}
<pre class="Agda">{% raw %}<a id="5407" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#5407" class="Function">_</a> <a id="5409" class="Symbol">:</a> <a id="5411" class="Number">2</a> <a id="5413" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="5415" class="Number">4</a>
<a id="5417" class="Symbol">_</a> <a id="5419" class="Symbol">=</a> <a id="5421" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5425" class="Symbol">{</a><a id="5426" class="Argument">m</a> <a id="5428" class="Symbol">=</a> <a id="5430" class="Number">1</a><a id="5431" class="Symbol">}</a> <a id="5433" class="Symbol">{</a><a id="5434" class="Argument">n</a> <a id="5436" class="Symbol">=</a> <a id="5438" class="Number">3</a><a id="5439" class="Symbol">}</a> <a id="5441" class="Symbol">(</a><a id="5442" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5446" class="Symbol">{</a><a id="5447" class="Argument">m</a> <a id="5449" class="Symbol">=</a> <a id="5451" class="Number">0</a><a id="5452" class="Symbol">}</a> <a id="5454" class="Symbol">{</a><a id="5455" class="Argument">n</a> <a id="5457" class="Symbol">=</a> <a id="5459" class="Number">2</a><a id="5460" class="Symbol">}</a> <a id="5462" class="Symbol">(</a><a id="5463" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a> <a id="5467" class="Symbol">{</a><a id="5468" class="Argument">n</a> <a id="5470" class="Symbol">=</a> <a id="5472" class="Number">2</a><a id="5473" class="Symbol">}))</a>{% endraw %}</pre>
在后者的形式中，也可以只声明一部分隐式参数：
{::comment}
In the latter format, you may only supply some implicit arguments:
{:/}
<pre class="Agda">{% raw %}<a id="5608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#5608" class="Function">_</a> <a id="5610" class="Symbol">:</a> <a id="5612" class="Number">2</a> <a id="5614" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="5616" class="Number">4</a>
<a id="5618" class="Symbol">_</a> <a id="5620" class="Symbol">=</a> <a id="5622" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5626" class="Symbol">{</a><a id="5627" class="Argument">n</a> <a id="5629" class="Symbol">=</a> <a id="5631" class="Number">3</a><a id="5632" class="Symbol">}</a> <a id="5634" class="Symbol">(</a><a id="5635" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="5639" class="Symbol">{</a><a id="5640" class="Argument">n</a> <a id="5642" class="Symbol">=</a> <a id="5644" class="Number">2</a><a id="5645" class="Symbol">}</a> <a id="5647" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a><a id="5650" class="Symbol">)</a>{% endraw %}</pre>
但是不可以改变隐式参数的顺序，即便加上了名字。
{::comment}
It is not permitted to swap implicit arguments, even when named.
{:/}


## 优先级
{::comment}
## Precedence
{:/}

我们如下定义比较的优先级：
{::comment}
We declare the precedence for comparison as follows:
{:/}
<pre class="Agda">{% raw %}<a id="5907" class="Keyword">infix</a> <a id="5913" class="Number">4</a> <a id="5915" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">_≤_</a>{% endraw %}</pre>
我们将 `_≤_` 的优先级设置为 4，所以它比优先级为 6 的 `_+_` 结合的更紧，此外，
`1 + 2 ≤ 3` 将被解析为 `(1 + 2) ≤ 3`。我们用 `infix` 来表示运算符既不是左结合的，
也不是右结合的。因为 `1 ≤ 2 ≤ 3` 解析为 `(1 ≤ 2) ≤ 3` 或者 `1 ≤ (2 ≤ 3)` 都没有意义。
{::comment}
We set the precedence of `_≤_` at level 4, so it binds less tightly
that `_+_` at level 6 and hence `1 + 2 ≤ 3` parses as `(1 + 2) ≤ 3`.
We write `infix` to indicate that the operator does not associate to
either the left or right, as it makes no sense to parse `1 ≤ 2 ≤ 3` as
either `(1 ≤ 2) ≤ 3` or `1 ≤ (2 ≤ 3)`.
{:/}

## 可决定性
{::comment}
## Decidability
{:/}

给定两个数，我们可以很直接地决定第一个数是不是小于等于第二个数。我们在此处不给出说明的代码，
但我们会在 [Decidable]({{ site.baseurl }}{% link out/plfa/Decidable.md%}) 章节重新讨论这个问题。
{::comment}
Given two numbers, it is straightforward to compute whether or not the
first is less than or equal to the second.  We don't give the code for
doing so here, but will return to this point in
Chapter [Decidable]({{ site.baseurl }}{% link out/plfa/Decidable.md%}).
{:/}

## 序关系的性质
{::comment}
## Properties of ordering relations
{:/}

数学家对于关系的常见性质给出了约定的名称。
{::comment}
Relations pop up all the time, and mathematicians have agreed
on names for some of the most common properties.
{:/}

* *自反*（Reflexive）：对于所有的 `n`，关系 `n ≤ n` 成立。
* *传递*（Transitive）：对于所有的 `m`、 `n` 和 `p`，如果 `m ≤ n` 和 `n ≤ p`
  成立，那么 `m ≤ p` 也成立。
* *反对称*（Anti-symmetric）：对于所有的 `m` 和 `n`，如果 `m ≤ n` 和 `n ≤ m`
  同时成立，那么 `m ≡ n` 成立。
* *完全*（Total）：对于所有的 `m` 和 `n`，`m ≤ n` 或者 `n ≤ m` 成立。

{::comment}
* _Reflexive_. For all `n`, the relation `n ≤ n` holds.
* _Transitive_. For all `m`, `n`, and `p`, if `m ≤ n` and
`n ≤ p` hold, then `m ≤ p` holds.
* _Anti-symmetric_. For all `m` and `n`, if both `m ≤ n` and
`n ≤ m` hold, then `m ≡ n` holds.
* _Total_. For all `m` and `n`, either `m ≤ n` or `n ≤ m`
holds.
{:/}

`_≤_` 关系满足上述四条性质。
{::comment}
The relation `_≤_` satisfies all four of these properties.
{:/}

对于上述性质的组合也有约定的名称。
{::comment}
There are also names for some combinations of these properties.
{:/}

* *预序*（Preorder）：满足自反和传递的关系。
* *偏序*（Partial Order）：满足反对称的预序。
* *全序*（Total Order）：满足完全的偏序。

{::comment}
* _Preorder_. Any relation that is reflexive and transitive.
* _Partial order_. Any preorder that is also anti-symmetric.
* _Total order_. Any partial order that is also total.
{:/}

如果你进入了关于关系的聚会，你现在知道怎么样和人讨论了，可以讨论关于自反、传递、反对称和完全，
或者问一问这是不是预序、偏序或者全序。
{::comment}
If you ever bump into a relation at a party, you now know how
to make small talk, by asking it whether it is reflexive, transitive,
anti-symmetric, and total. Or instead you might ask whether it is a
preorder, partial order, or total order.
{:/}

更认真的来说，如果你在阅读论文时碰到了一个关系，本文的介绍让你可以对关系有基本的了解和判断，
来判断这个关系是不是预序、偏序或者全序。一个认真的作者一般会在文章指出这个关系具有（或者缺少）
上述性质，比如说指出新定义的关系是一个偏序而不是全序。
{::comment}
Less frivolously, if you ever bump into a relation while reading a
technical paper, this gives you a way to orient yourself, by checking
whether or not it is a preorder, partial order, or total order.  A
careful author will often call out these properties---or their
lack---for instance by saying that a newly introduced relation is a
partial order but not a total order.
{:/}

#### 练习 `orderings` {#orderings}
{::comment}
#### Exercise `orderings` {#orderings}
{:/}

给出一个不是偏序的预序的例子。
{::comment}
Give an example of a preorder that is not a partial order.
{:/}

<pre class="Agda">{% raw %}<a id="9137" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

给出一个不是全序的偏序的例子。
{::comment}
Give an example of a partial order that is not a total order.
{:/}

<pre class="Agda">{% raw %}<a id="9271" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

## 自反性
{::comment}
## Reflexivity
{:/}

我们第一个来证明的性质是自反性：对于任意自然数 `n`，关系 `n ≤ n` 成立。我们使用标准库的惯例来隐式申明参数，在使用自反性的证明时这样可以更加方便。
{::comment}
The first property to prove about comparison is that it is reflexive:
for any natural `n`, the relation `n ≤ n` holds.  We follow the
convention in the standard library and make the argument implicit,
as that will make it easier to invoke reflexivity:
{:/}
<pre class="Agda">{% raw %}<a id="≤-refl"></a><a id="9699" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9699" class="Function">≤-refl</a> <a id="9706" class="Symbol">:</a> <a id="9708" class="Symbol">∀</a> <a id="9710" class="Symbol">{</a><a id="9711" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9711" class="Bound">n</a> <a id="9713" class="Symbol">:</a> <a id="9715" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="9716" class="Symbol">}</a>
    <a id="9722" class="Comment">-----</a>
  <a id="9730" class="Symbol">→</a> <a id="9732" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9711" class="Bound">n</a> <a id="9734" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="9736" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9711" class="Bound">n</a>
<a id="9738" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9699" class="Function">≤-refl</a> <a id="9745" class="Symbol">{</a><a id="9746" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="9750" class="Symbol">}</a>   <a id="9754" class="Symbol">=</a>  <a id="9757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="9761" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9699" class="Function">≤-refl</a> <a id="9768" class="Symbol">{</a><a id="9769" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="9773" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9773" class="Bound">n</a><a id="9774" class="Symbol">}</a>  <a id="9777" class="Symbol">=</a>  <a id="9780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="9784" class="Symbol">(</a><a id="9785" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9699" class="Function">≤-refl</a> <a id="9792" class="Symbol">{</a><a id="9793" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9773" class="Bound">n</a><a id="9794" class="Symbol">})</a>{% endraw %}</pre>

这个证明直接在 `n` 上进行归纳。在起始步骤中，`zero ≤ zero` 由 `z≤n` 证明；在归纳步骤中，
归纳假设 `≤-refl {n}` 给我们带来了 `n ≤ n` 的证明，我们只需要使用 `s≤s`，就可以获得
`suc n ≤ suc n` 的证明。
{::comment}
The proof is a straightforward induction on the implicit argument `n`.
In the base case, `zero ≤ zero` holds by `z≤n`.  In the inductive
case, the inductive hypothesis `≤-refl {n}` gives us a proof of `n ≤
n`, and applying `s≤s` to that yields a proof of `suc n ≤ suc n`.
{:/}

在 Emacs 中来交互式地证明自反性是一个很好的练习，可以使用洞，以及 `C-c C-c`、
`C-c C-,` 和 `C-c C-r` 命令。
{::comment}
It is a good exercise to prove reflexivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.
{:/}

## 传递性
{::comment}
## Transitivity
{:/}

我们第二个证明的性质是传递性：对于任意自然数 `m` 和 `n`，如果 `m ≤ n` 和 `n ≤ p`
成立，那么 `m ≤ p` 成立。同样，`m`、`n` 和 `p` 是隐式参数：
{::comment}
The second property to prove about comparison is that it is
transitive: for any naturals `m`, `n`, and `p`, if `m ≤ n` and `n ≤ p`
hold, then `m ≤ p` holds.  Again, `m`, `n`, and `p` are implicit:
{:/}
<pre class="Agda">{% raw %}<a id="≤-trans"></a><a id="10823" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10823" class="Function">≤-trans</a> <a id="10831" class="Symbol">:</a> <a id="10833" class="Symbol">∀</a> <a id="10835" class="Symbol">{</a><a id="10836" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10836" class="Bound">m</a> <a id="10838" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10838" class="Bound">n</a> <a id="10840" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10840" class="Bound">p</a> <a id="10842" class="Symbol">:</a> <a id="10844" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="10845" class="Symbol">}</a>
  <a id="10849" class="Symbol">→</a> <a id="10851" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10836" class="Bound">m</a> <a id="10853" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="10855" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10838" class="Bound">n</a>
  <a id="10859" class="Symbol">→</a> <a id="10861" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10838" class="Bound">n</a> <a id="10863" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="10865" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10840" class="Bound">p</a>
    <a id="10871" class="Comment">-----</a>
  <a id="10879" class="Symbol">→</a> <a id="10881" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10836" class="Bound">m</a> <a id="10883" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="10885" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10840" class="Bound">p</a>
<a id="10887" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10823" class="Function">≤-trans</a> <a id="10895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>       <a id="10905" class="Symbol">_</a>          <a id="10916" class="Symbol">=</a>  <a id="10919" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="10923" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10823" class="Function">≤-trans</a> <a id="10931" class="Symbol">(</a><a id="10932" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="10936" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10936" class="Bound">m≤n</a><a id="10939" class="Symbol">)</a> <a id="10941" class="Symbol">(</a><a id="10942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="10946" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10946" class="Bound">n≤p</a><a id="10949" class="Symbol">)</a>  <a id="10952" class="Symbol">=</a>  <a id="10955" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="10959" class="Symbol">(</a><a id="10960" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10823" class="Function">≤-trans</a> <a id="10968" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10936" class="Bound">m≤n</a> <a id="10972" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10946" class="Bound">n≤p</a><a id="10975" class="Symbol">)</a>{% endraw %}</pre>
这里我们在 `m ≤ n` 的 *证据* 上进行归纳。在起始步骤里，第一个不等式因为 `z≤n` 而成立，
那么结论亦可由 `z≤n` 而得出。在这里，`n ≤ p` 的证明是不需要的，我们用 `_` 来表示这个证明没有被使用。
{::comment}
Here the proof is by induction on the _evidence_ that `m ≤ n`.  In the
base case, the first inequality holds by `z≤n` and must show `zero ≤
p`, which follows immediately by `z≤n`.  In this case, the fact that
`n ≤ p` is irrelevant, and we write `_` as the pattern to indicate
that the corresponding evidence is unused.
{:/}

在归纳步骤中，第一个不等式因为 `s≤s m≤n` 而成立，第二个不等式因为 `s≤s n≤p` 而成立，
所以我们已知 `suc m ≤ suc n` 和 `suc n ≤ suc p`，求证 `suc m ≤ suc p`。
通过归纳假设 `≤-trans m≤n n≤p`，我们得知 `m ≤ p`，在此之上使用 `s≤s` 即可证。
{::comment}
In the inductive case, the first inequality holds by `s≤s m≤n`
and the second inequality by `s≤s n≤p`, and so we are given
`suc m ≤ suc n` and `suc n ≤ suc p`, and must show `suc m ≤ suc p`.
The inductive hypothesis `≤-trans m≤n n≤p` establishes
that `m ≤ p`, and our goal follows by applying `s≤s`.
{:/}

`≤-trans (s≤s m≤n) z≤n` 不可能发生，因为第一个不等式告诉我们中间的数是一个 `suc n`，
而第二个不等式告诉我们中间的书是 `zero`。Agda 可以推断这样的情况不可能发现，所以我们不需要
（也不可以）列出这种情况。
{::comment}
The case `≤-trans (s≤s m≤n) z≤n` cannot arise, since the first
inequality implies the middle value is `suc n` while the second
inequality implies that it is `zero`.  Agda can determine that such a
case cannot arise, and does not require (or permit) it to be listed.
{:/}

我们也可以将隐式参数显式地声明。
{::comment}
Alternatively, we could make the implicit parameters explicit:
{:/}
<pre class="Agda">{% raw %}<a id="≤-trans′"></a><a id="12449" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12449" class="Function">≤-trans′</a> <a id="12458" class="Symbol">:</a> <a id="12460" class="Symbol">∀</a> <a id="12462" class="Symbol">(</a><a id="12463" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12463" class="Bound">m</a> <a id="12465" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12465" class="Bound">n</a> <a id="12467" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12467" class="Bound">p</a> <a id="12469" class="Symbol">:</a> <a id="12471" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="12472" class="Symbol">)</a>
  <a id="12476" class="Symbol">→</a> <a id="12478" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12463" class="Bound">m</a> <a id="12480" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="12482" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12465" class="Bound">n</a>
  <a id="12486" class="Symbol">→</a> <a id="12488" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12465" class="Bound">n</a> <a id="12490" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="12492" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12467" class="Bound">p</a>
    <a id="12498" class="Comment">-----</a>
  <a id="12506" class="Symbol">→</a> <a id="12508" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12463" class="Bound">m</a> <a id="12510" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="12512" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12467" class="Bound">p</a>
<a id="12514" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12449" class="Function">≤-trans′</a> <a id="12523" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="12531" class="Symbol">_</a>       <a id="12539" class="Symbol">_</a>       <a id="12547" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>       <a id="12557" class="Symbol">_</a>          <a id="12568" class="Symbol">=</a>  <a id="12571" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="12575" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12449" class="Function">≤-trans′</a> <a id="12584" class="Symbol">(</a><a id="12585" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12589" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12589" class="Bound">m</a><a id="12590" class="Symbol">)</a> <a id="12592" class="Symbol">(</a><a id="12593" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12597" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12597" class="Bound">n</a><a id="12598" class="Symbol">)</a> <a id="12600" class="Symbol">(</a><a id="12601" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12605" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12605" class="Bound">p</a><a id="12606" class="Symbol">)</a> <a id="12608" class="Symbol">(</a><a id="12609" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="12613" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12613" class="Bound">m≤n</a><a id="12616" class="Symbol">)</a> <a id="12618" class="Symbol">(</a><a id="12619" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="12623" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12623" class="Bound">n≤p</a><a id="12626" class="Symbol">)</a>  <a id="12629" class="Symbol">=</a>  <a id="12632" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="12636" class="Symbol">(</a><a id="12637" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12449" class="Function">≤-trans′</a> <a id="12646" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12589" class="Bound">m</a> <a id="12648" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12597" class="Bound">n</a> <a id="12650" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12605" class="Bound">p</a> <a id="12652" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12613" class="Bound">m≤n</a> <a id="12656" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12623" class="Bound">n≤p</a><a id="12659" class="Symbol">)</a>{% endraw %}</pre>
有人说这样的证明更加的清晰，也有人说这个更长的证明让人难以抓住证明的重点。
我们一般选择使用简短的证明。
{::comment}
One might argue that this is clearer or one might argue that the extra
length obscures the essence of the proof.  We will usually opt for
shorter proofs.
{:/}

对于性质成立证明进行的归纳（如上文中对于 `m ≤ n` 的证明进行归纳），相比于对于性质成立的值进行的归纳
（如对于 `m` 进行归纳），有非常大的价值。我们会经常使用这样的方法。
{::comment}
The technique of induction on evidence that a property holds (e.g.,
inducting on evidence that `m ≤ n`)---rather than induction on
values of which the property holds (e.g., inducting on `m`)---will turn
out to be immensely valuable, and one that we use often.
{:/}

同样，在 Emacs 中来交互式地证明传递性是一个很好的练习，可以使用洞，以及 `C-c C-c`、
`C-c C-,` 和 `C-c C-r` 命令。
{::comment}
Again, it is a good exercise to prove transitivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.
{:/}

## 反对称性
{::comment}
## Anti-symmetry
{:/}

我们证明的第三个性质是反对称性：对于所有的自然数 `m` 和 `n`，如果 `m ≤ n` 和 `n ≤ m`
同时成立，那么 `m ≡ n` 成立：
{::comment}
The third property to prove about comparison is that it is
antisymmetric: for all naturals `m` and `n`, if both `m ≤ n` and `n ≤
m` hold, then `m ≡ n` holds:
{:/}
<pre class="Agda">{% raw %}<a id="≤-antisym"></a><a id="13810" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13810" class="Function">≤-antisym</a> <a id="13820" class="Symbol">:</a> <a id="13822" class="Symbol">∀</a> <a id="13824" class="Symbol">{</a><a id="13825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13825" class="Bound">m</a> <a id="13827" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13827" class="Bound">n</a> <a id="13829" class="Symbol">:</a> <a id="13831" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="13832" class="Symbol">}</a>
  <a id="13836" class="Symbol">→</a> <a id="13838" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13825" class="Bound">m</a> <a id="13840" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="13842" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13827" class="Bound">n</a>
  <a id="13846" class="Symbol">→</a> <a id="13848" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13827" class="Bound">n</a> <a id="13850" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="13852" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13825" class="Bound">m</a>
    <a id="13858" class="Comment">-----</a>
  <a id="13866" class="Symbol">→</a> <a id="13868" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13825" class="Bound">m</a> <a id="13870" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="13872" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13827" class="Bound">n</a>
<a id="13874" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13810" class="Function">≤-antisym</a> <a id="13884" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>       <a id="13894" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>        <a id="13905" class="Symbol">=</a>  <a id="13908" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="13913" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13810" class="Function">≤-antisym</a> <a id="13923" class="Symbol">(</a><a id="13924" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="13928" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13928" class="Bound">m≤n</a><a id="13931" class="Symbol">)</a> <a id="13933" class="Symbol">(</a><a id="13934" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="13938" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13938" class="Bound">n≤m</a><a id="13941" class="Symbol">)</a>  <a id="13944" class="Symbol">=</a>  <a id="13947" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1170" class="Function">cong</a> <a id="13952" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13956" class="Symbol">(</a><a id="13957" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13810" class="Function">≤-antisym</a> <a id="13967" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13928" class="Bound">m≤n</a> <a id="13971" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13938" class="Bound">n≤m</a><a id="13974" class="Symbol">)</a>{% endraw %}</pre>
同样，我们对于 `m ≤ n` 和 `n ≤ m` 的证明进行归纳。
{::comment}
Again, the proof is by induction over the evidence that `m ≤ n`
and `n ≤ m` hold.
{:/}

在起始步骤中，两个不等式都因为 `z≤n` 而成立。因此我们已知 `zero ≤ zero` 和 `zero ≤ zero`，
求证 `zero ≡ zero`，由自反性可证。（注：由等式的自反性可证，而不是不等式的自反性）
{::comment}
In the base case, both inequalities hold by `z≤n`, and so we are given
`zero ≤ zero` and `zero ≤ zero` and must show `zero ≡ zero`, which
follows by reflexivity.  (Reflexivity of equality, that is, not
reflexivity of inequality.)
{:/}

在归纳步骤中，第一个不等式因为 `s≤s m≤n` 而成立，第二个等式因为 `s≤s n≤m` 而成立。因此我们已知
`suc m ≤ suc n` 和 `suc n ≤ suc m`，求证 `suc m ≡ suc n`。归纳假设 `≤-antisym m≤n n≤m`
可以证明 `m ≡ n`，因此我们可以使用同余性完成证明。

{::comment}
In the inductive case, the first inequality holds by `s≤s m≤n` and the
second inequality holds by `s≤s n≤m`, and so we are given `suc m ≤ suc n`
and `suc n ≤ suc m` and must show `suc m ≡ suc n`.  The inductive
hypothesis `≤-antisym m≤n n≤m` establishes that `m ≡ n`, and our goal
follows by congruence.
{::comment}

#### 练习 `≤-antisym-cases` {#leq-antisym-cases}
{::comment}
#### Exercise `≤-antisym-cases` {#leq-antisym-cases}
{:/}

上面的证明中省略了一个参数是 `z≤n`，另一个参数是 `s≤s` 的情况。为什么可以省略这种情况？
{::comment}
The above proof omits cases where one argument is `z≤n` and one
argument is `s≤s`.  Why is it ok to omit them?
{:/}

<pre class="Agda">{% raw %}<a id="15291" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

## 完全性
{::comment}
## Total
{:/}

我们证明的第四个性质是完全性：对于任何自然数 `m` 和 `n`，`m ≤ n` 或者 `n ≤ m` 成立。
在 `m` 和 `n` 相等时，两者同时成立。
{::comment}
The fourth property to prove about comparison is that it is total:
for any naturals `m` and `n` either `m ≤ n` or `n ≤ m`, or both if
`m` and `n` are equal.
{:/}

我们首先来说明怎么样不等式才是完全的：
{::comment}
We specify what it means for inequality to be total:
{:/}
<pre class="Agda">{% raw %}<a id="15708" class="Keyword">data</a> <a id="Total"></a><a id="15713" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="15719" class="Symbol">(</a><a id="15720" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15720" class="Bound">m</a> <a id="15722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15722" class="Bound">n</a> <a id="15724" class="Symbol">:</a> <a id="15726" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="15727" class="Symbol">)</a> <a id="15729" class="Symbol">:</a> <a id="15731" class="PrimitiveType">Set</a> <a id="15735" class="Keyword">where</a>

  <a id="Total.forward"></a><a id="15744" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="15752" class="Symbol">:</a>
      <a id="15760" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15720" class="Bound">m</a> <a id="15762" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="15764" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15722" class="Bound">n</a>
      <a id="15772" class="Comment">---------</a>
    <a id="15786" class="Symbol">→</a> <a id="15788" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="15794" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15720" class="Bound">m</a> <a id="15796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15722" class="Bound">n</a>

  <a id="Total.flipped"></a><a id="15801" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="15809" class="Symbol">:</a>
      <a id="15817" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15722" class="Bound">n</a> <a id="15819" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="15821" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15720" class="Bound">m</a>
      <a id="15829" class="Comment">---------</a>
    <a id="15843" class="Symbol">→</a> <a id="15845" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="15851" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15720" class="Bound">m</a> <a id="15853" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15722" class="Bound">n</a>{% endraw %}</pre>
`Total m n` 成立的证明有两种形式：`forward m≤n` 或者 `flipped n≤m`，其中
`m≤n` 和 `n≤m` 分别是 `m ≤ n` 和 `n ≤ m` 的证明。
{::comment}
Evidence that `Total m n` holds is either of the form
`forward m≤n` or `flipped n≤m`, where `m≤n` and `n≤m` are
evidence of `m ≤ n` and `n ≤ m` respectively.
{:/}

（如果你对于逻辑学有所了解，上面的定义可以由析取（Disjunction）表示。
我们会在 [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%}) 章节介绍析取。）
{::comment}
(For those familiar with logic, the above definition
could also be written as a disjunction. Disjunctions will
be introduced in Chapter [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%}).)
{:/}

这是我们第一次使用带*参数*的数据类型，这里 `m` 和 `n` 是参数。这等同于下面的索引数据类型：
{::comment}
This is our first use of a datatype with _parameters_,
in this case `m` and `n`.  It is equivalent to the following
indexed datatype:
{:/}
<pre class="Agda">{% raw %}<a id="16631" class="Keyword">data</a> <a id="Total′"></a><a id="16636" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Datatype">Total′</a> <a id="16643" class="Symbol">:</a> <a id="16645" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="16647" class="Symbol">→</a> <a id="16649" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="16651" class="Symbol">→</a> <a id="16653" class="PrimitiveType">Set</a> <a id="16657" class="Keyword">where</a>

  <a id="Total′.forward′"></a><a id="16666" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16666" class="InductiveConstructor">forward′</a> <a id="16675" class="Symbol">:</a> <a id="16677" class="Symbol">∀</a> <a id="16679" class="Symbol">{</a><a id="16680" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16680" class="Bound">m</a> <a id="16682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16682" class="Bound">n</a> <a id="16684" class="Symbol">:</a> <a id="16686" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="16687" class="Symbol">}</a>
    <a id="16693" class="Symbol">→</a> <a id="16695" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16680" class="Bound">m</a> <a id="16697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="16699" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16682" class="Bound">n</a>
      <a id="16707" class="Comment">----------</a>
    <a id="16722" class="Symbol">→</a> <a id="16724" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Datatype">Total′</a> <a id="16731" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16680" class="Bound">m</a> <a id="16733" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16682" class="Bound">n</a>

  <a id="Total′.flipped′"></a><a id="16738" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16738" class="InductiveConstructor">flipped′</a> <a id="16747" class="Symbol">:</a> <a id="16749" class="Symbol">∀</a> <a id="16751" class="Symbol">{</a><a id="16752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16752" class="Bound">m</a> <a id="16754" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16754" class="Bound">n</a> <a id="16756" class="Symbol">:</a> <a id="16758" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="16759" class="Symbol">}</a>
    <a id="16765" class="Symbol">→</a> <a id="16767" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16754" class="Bound">n</a> <a id="16769" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="16771" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16752" class="Bound">m</a>
      <a id="16779" class="Comment">----------</a>
    <a id="16794" class="Symbol">→</a> <a id="16796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Datatype">Total′</a> <a id="16803" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16752" class="Bound">m</a> <a id="16805" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16754" class="Bound">n</a>{% endraw %}</pre>
类型里的每个参数都转换成构造器的一个隐式参数。索引数据类型中的索引可以变化，正如在
`zero ≤ n` 和 `suc m ≤ suc n` 中那样，而参数化数据类型不一样，其参数必须保持相同，
正如在 `Total m n` 中那样。参数化的声明更短，更易于阅读，而且有时可以帮助到 Agda 的终结检查器，所以我们尽可能地使用它们，而不是索引数据类型。

{::comment}
Each parameter of the type translates as an implicit parameter of each
constructor.  Unlike an indexed datatype, where the indexes can vary
(as in `zero ≤ n` and `suc m ≤ suc n`), in a parameterised datatype
the parameters must always be the same (as in `Total m n`).
Parameterised declarations are shorter, easier to read, and
occasionally aid Agda's termination checker, so we will use them in
preference to indexed types when possible.
{:/}

在上述准备工作完成后，我们定义并证明完全性。
{::comment}
With that preliminary out of the way, we specify and prove totality:
{:/}
<pre class="Agda">{% raw %}<a id="≤-total"></a><a id="17578" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17578" class="Function">≤-total</a> <a id="17586" class="Symbol">:</a> <a id="17588" class="Symbol">∀</a> <a id="17590" class="Symbol">(</a><a id="17591" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17591" class="Bound">m</a> <a id="17593" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17593" class="Bound">n</a> <a id="17595" class="Symbol">:</a> <a id="17597" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17598" class="Symbol">)</a> <a id="17600" class="Symbol">→</a> <a id="17602" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="17608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17591" class="Bound">m</a> <a id="17610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17593" class="Bound">n</a>
<a id="17612" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17578" class="Function">≤-total</a> <a id="17620" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="17628" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17628" class="Bound">n</a>                         <a id="17654" class="Symbol">=</a>  <a id="17657" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="17665" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="17669" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17578" class="Function">≤-total</a> <a id="17677" class="Symbol">(</a><a id="17678" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17682" class="Bound">m</a><a id="17683" class="Symbol">)</a> <a id="17685" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="17711" class="Symbol">=</a>  <a id="17714" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="17722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="17726" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17578" class="Function">≤-total</a> <a id="17734" class="Symbol">(</a><a id="17735" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17739" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17739" class="Bound">m</a><a id="17740" class="Symbol">)</a> <a id="17742" class="Symbol">(</a><a id="17743" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17747" class="Bound">n</a><a id="17748" class="Symbol">)</a> <a id="17750" class="Keyword">with</a> <a id="17755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17578" class="Function">≤-total</a> <a id="17763" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17739" class="Bound">m</a> <a id="17765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17747" class="Bound">n</a>
<a id="17767" class="Symbol">...</a>                        <a id="17794" class="Symbol">|</a> <a id="17796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="17804" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17804" class="Bound">m≤n</a>  <a id="17809" class="Symbol">=</a>  <a id="17812" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="17820" class="Symbol">(</a><a id="17821" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="17825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17804" class="Bound">m≤n</a><a id="17828" class="Symbol">)</a>
<a id="17830" class="Symbol">...</a>                        <a id="17857" class="Symbol">|</a> <a id="17859" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="17867" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17867" class="Bound">n≤m</a>  <a id="17872" class="Symbol">=</a>  <a id="17875" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="17883" class="Symbol">(</a><a id="17884" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="17888" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17867" class="Bound">n≤m</a><a id="17891" class="Symbol">)</a>{% endraw %}</pre>
这里，我们的证明在两个参数上进行归纳，并按照情况分析：
{::comment}
In this case the proof is by induction over both the first
and second arguments.  We perform a case analysis:
{:/}

* *第一起始步骤*：如果第一个参数是 `zero`，第二个参数是 `n`，那么 forward
  条件成立，我们使用 `z≤n` 作为 `zero ≤ n` 的证明。

* *第二起始步骤*：如果第一个参数是 `suc m`，第二个参数是 `zero`，那么 flipped
  条件成立，我们使用 `z≤n` 作为 `zero ≤ suc m` 的证明。

* *归纳步骤*：如果第一个参数是 `suc m`，第二个参数是 `suc n`，那么归纳假设
  `≤-total m n` 可以给出如下推断：

  + 归纳假设的 forward 条件成立，以 `m≤n` 作为 `m ≤ n` 的证明。以此我们可以使用
    `s≤s m≤n` 作为 `suc m ≤ suc n` 来证明 forward 条件成立。

  + 归纳假设的 flipped 条件成立，以 `n≤m` 作为 `n ≤ m` 的证明。以此我们可以使用
    `s≤s n≤m` 作为 `suc n ≤ suc m` 来证明 flipped 条件成立。

{::comment}
* _First base case_: If the first argument is `zero` and the
  second argument is `n` then the forward case holds,
  with `z≤n` as evidence that `zero ≤ n`.

* _Second base case_: If the first argument is `suc m` and the
  second argument is `zero` then the flipped case holds, with
  `z≤n` as evidence that `zero ≤ suc m`.

* _Inductive case_: If the first argument is `suc m` and the
  second argument is `suc n`, then the inductive hypothesis
  `≤-total m n` establishes one of the following:

  + The forward case of the inductive hypothesis holds with `m≤n` as
    evidence that `m ≤ n`, from which it follows that the forward case of the
    proposition holds with `s≤s m≤n` as evidence that `suc m ≤ suc n`.

  + The flipped case of the inductive hypothesis holds with `n≤m` as
    evidence that `n ≤ m`, from which it follows that the flipped case of the
    proposition holds with `s≤s n≤m` as evidence that `suc n ≤ suc m`.
{:/}

这是我们第一次在 Agda 中使用 `with` 语句。`with` 关键字后面有一个表达式和一或多行。
每行以省略号（`...`）和一个竖线（`|`）开头，后面跟着用来匹配表达式的模式，和等式的右手边。
{::comment}
This is our first use of the `with` clause in Agda.  The keyword
`with` is followed by an expression and one or more subsequent lines.
Each line begins with an ellipsis (`...`) and a vertical bar (`|`),
followed by a pattern to be matched against the expression
and the right-hand side of the equation.
{:/}

使用 `with` 语句等同于定义一个辅助函数。比如说，上面的定义和下面的等价：
{::comment}
Every use of `with` is equivalent to defining a helper function.  For
example, the definition above is equivalent to the following:
{:/}
<pre class="Agda">{% raw %}<a id="≤-total′"></a><a id="20110" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20110" class="Function">≤-total′</a> <a id="20119" class="Symbol">:</a> <a id="20121" class="Symbol">∀</a> <a id="20123" class="Symbol">(</a><a id="20124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20124" class="Bound">m</a> <a id="20126" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20126" class="Bound">n</a> <a id="20128" class="Symbol">:</a> <a id="20130" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20131" class="Symbol">)</a> <a id="20133" class="Symbol">→</a> <a id="20135" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="20141" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20124" class="Bound">m</a> <a id="20143" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20126" class="Bound">n</a>
<a id="20145" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20110" class="Function">≤-total′</a> <a id="20154" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="20162" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20162" class="Bound">n</a>        <a id="20171" class="Symbol">=</a>  <a id="20174" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="20182" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="20186" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20110" class="Function">≤-total′</a> <a id="20195" class="Symbol">(</a><a id="20196" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20200" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20200" class="Bound">m</a><a id="20201" class="Symbol">)</a> <a id="20203" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>     <a id="20212" class="Symbol">=</a>  <a id="20215" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="20223" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="20227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20110" class="Function">≤-total′</a> <a id="20236" class="Symbol">(</a><a id="20237" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20241" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20241" class="Bound">m</a><a id="20242" class="Symbol">)</a> <a id="20244" class="Symbol">(</a><a id="20245" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20249" class="Bound">n</a><a id="20250" class="Symbol">)</a>  <a id="20253" class="Symbol">=</a>  <a id="20256" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20288" class="Function">helper</a> <a id="20263" class="Symbol">(</a><a id="20264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20110" class="Function">≤-total′</a> <a id="20273" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20241" class="Bound">m</a> <a id="20275" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20249" class="Bound">n</a><a id="20276" class="Symbol">)</a>
  <a id="20280" class="Keyword">where</a>
  <a id="20288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20288" class="Function">helper</a> <a id="20295" class="Symbol">:</a> <a id="20297" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="20303" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20241" class="Bound">m</a> <a id="20305" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20249" class="Bound">n</a> <a id="20307" class="Symbol">→</a> <a id="20309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="20315" class="Symbol">(</a><a id="20316" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20320" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20241" class="Bound">m</a><a id="20321" class="Symbol">)</a> <a id="20323" class="Symbol">(</a><a id="20324" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20328" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20249" class="Bound">n</a><a id="20329" class="Symbol">)</a>
  <a id="20333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20288" class="Function">helper</a> <a id="20340" class="Symbol">(</a><a id="20341" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="20349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20349" class="Bound">m≤n</a><a id="20352" class="Symbol">)</a>  <a id="20355" class="Symbol">=</a>  <a id="20358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="20366" class="Symbol">(</a><a id="20367" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="20371" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20349" class="Bound">m≤n</a><a id="20374" class="Symbol">)</a>
  <a id="20378" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20288" class="Function">helper</a> <a id="20385" class="Symbol">(</a><a id="20386" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="20394" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20394" class="Bound">n≤m</a><a id="20397" class="Symbol">)</a>  <a id="20400" class="Symbol">=</a>  <a id="20403" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="20411" class="Symbol">(</a><a id="20412" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="20416" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20394" class="Bound">n≤m</a><a id="20419" class="Symbol">)</a>{% endraw %}</pre>

这也是我们第一次在 Agda 中使用 `where` 语句。`where` 关键字后面有一或多条定义，其必须被缩进。
之前等式左手边的约束变量（此例中的 `m` 和 `n`）在嵌套的定义中仍然在作用域内。
在嵌套定义中的约束标识符（此例中的 `helper` ）在等式的右手边的作用域内。
{::comment}
This is also our first use of a `where` clause in Agda.  The keyword `where` is
followed by one or more definitions, which must be indented.  Any variables
bound on the left-hand side of the preceding equation (in this case, `m` and
`n`) are in scope within the nested definition, and any identifiers bound in the
nested definition (in this case, `helper`) are in scope in the right-hand side
of the preceding equation.
{:/}

如果两个参数相同，那么两个情况同时成立，我们可以返回任一证明。上面的代码中我们返回 forward 条件，
但是我们也可以返回 flipped 条件，如下：
{::comment}
If both arguments are equal, then both cases hold and we could return evidence
of either.  In the code above we return the forward case, but there is a
variant that returns the flipped case:
{:/}
<pre class="Agda">{% raw %}<a id="≤-total″"></a><a id="21316" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21316" class="Function">≤-total″</a> <a id="21325" class="Symbol">:</a> <a id="21327" class="Symbol">∀</a> <a id="21329" class="Symbol">(</a><a id="21330" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21330" class="Bound">m</a> <a id="21332" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21332" class="Bound">n</a> <a id="21334" class="Symbol">:</a> <a id="21336" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="21337" class="Symbol">)</a> <a id="21339" class="Symbol">→</a> <a id="21341" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15713" class="Datatype">Total</a> <a id="21347" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21330" class="Bound">m</a> <a id="21349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21332" class="Bound">n</a>
<a id="21351" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21316" class="Function">≤-total″</a> <a id="21360" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21360" class="Bound">m</a>       <a id="21368" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="21394" class="Symbol">=</a>  <a id="21397" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="21405" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="21409" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21316" class="Function">≤-total″</a> <a id="21418" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="21426" class="Symbol">(</a><a id="21427" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="21431" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21431" class="Bound">n</a><a id="21432" class="Symbol">)</a>                   <a id="21452" class="Symbol">=</a>  <a id="21455" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="21463" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1697" class="InductiveConstructor">z≤n</a>
<a id="21467" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21316" class="Function">≤-total″</a> <a id="21476" class="Symbol">(</a><a id="21477" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="21481" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21481" class="Bound">m</a><a id="21482" class="Symbol">)</a> <a id="21484" class="Symbol">(</a><a id="21485" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="21489" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21489" class="Bound">n</a><a id="21490" class="Symbol">)</a> <a id="21492" class="Keyword">with</a> <a id="21497" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21316" class="Function">≤-total″</a> <a id="21506" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21481" class="Bound">m</a> <a id="21508" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21489" class="Bound">n</a>
<a id="21510" class="Symbol">...</a>                        <a id="21537" class="Symbol">|</a> <a id="21539" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="21547" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21547" class="Bound">m≤n</a>   <a id="21553" class="Symbol">=</a>  <a id="21556" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15744" class="InductiveConstructor">forward</a> <a id="21564" class="Symbol">(</a><a id="21565" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="21569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21547" class="Bound">m≤n</a><a id="21572" class="Symbol">)</a>
<a id="21574" class="Symbol">...</a>                        <a id="21601" class="Symbol">|</a> <a id="21603" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="21611" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21611" class="Bound">n≤m</a>   <a id="21617" class="Symbol">=</a>  <a id="21620" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15801" class="InductiveConstructor">flipped</a> <a id="21628" class="Symbol">(</a><a id="21629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="21633" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21611" class="Bound">n≤m</a><a id="21636" class="Symbol">)</a>{% endraw %}</pre>
两者的区别在于上述代码在对于第一个参数进行模式匹配之前先对于第二个参数先进行模式匹配。
{::comment}
It differs from the original code in that it pattern
matches on the second argument before the first argument.
{:/}

## 单调性
{::comment}
## Monotonicity
{:/}

如果在聚会中碰到了一个运算符和一个序，那么有人可能会问这个运算符对于这个序是不是 *单调的* （Monotonic）。
比如说，加法对于小于等于是单调的，这意味着：
{::comment}
If one bumps into both an operator and an ordering at a party, one may ask if
the operator is _monotonic_ with regard to the ordering.  For example, addition
is monotonic with regard to inequality, meaning:
{:/}

    ∀ {m n p q : ℕ} → m ≤ n → p ≤ q → m + p ≤ n + q

这个证明可以用我们学会的方法，很直接的来完成。我们最好把它分成三个部分，首先我们证明加法对于小于等于在右手边是单调的：
{::comment}
The proof is straightforward using the techniques we have learned, and is best
broken into three parts. First, we deal with the special case of showing
addition is monotonic on the right:
{:/}
<pre class="Agda">{% raw %}<a id="+-monoʳ-≤"></a><a id="22503" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="22513" class="Symbol">:</a> <a id="22515" class="Symbol">∀</a> <a id="22517" class="Symbol">(</a><a id="22518" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22518" class="Bound">m</a> <a id="22520" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22520" class="Bound">p</a> <a id="22522" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22522" class="Bound">q</a> <a id="22524" class="Symbol">:</a> <a id="22526" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="22527" class="Symbol">)</a>
  <a id="22531" class="Symbol">→</a> <a id="22533" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22520" class="Bound">p</a> <a id="22535" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="22537" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22522" class="Bound">q</a>
    <a id="22543" class="Comment">-------------</a>
  <a id="22559" class="Symbol">→</a> <a id="22561" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22518" class="Bound">m</a> <a id="22563" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="22565" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22520" class="Bound">p</a> <a id="22567" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="22569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22518" class="Bound">m</a> <a id="22571" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="22573" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22522" class="Bound">q</a>
<a id="22575" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="22585" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="22593" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22593" class="Bound">p</a> <a id="22595" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22595" class="Bound">q</a> <a id="22597" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22597" class="Bound">p≤q</a>  <a id="22602" class="Symbol">=</a>  <a id="22605" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22597" class="Bound">p≤q</a>
<a id="22609" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="22619" class="Symbol">(</a><a id="22620" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="22624" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22624" class="Bound">m</a><a id="22625" class="Symbol">)</a> <a id="22627" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22627" class="Bound">p</a> <a id="22629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22629" class="Bound">q</a> <a id="22631" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22631" class="Bound">p≤q</a>  <a id="22636" class="Symbol">=</a>  <a id="22639" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1746" class="InductiveConstructor">s≤s</a> <a id="22643" class="Symbol">(</a><a id="22644" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="22654" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22624" class="Bound">m</a> <a id="22656" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22627" class="Bound">p</a> <a id="22658" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22629" class="Bound">q</a> <a id="22660" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22631" class="Bound">p≤q</a><a id="22663" class="Symbol">)</a>{% endraw %}</pre>

我们对于第一个参数进行归纳。

* *起始步骤*：第一个参数是 `zero`，那么 `zero + p ≤ zero + q` 可以化简为 `p ≤ q`，
  其证明由 `p≤q` 给出。

* *归纳步骤*：第一个参数是 `suc m`，那么 `suc m + p ≤ suc m + q` 可以化简为
  `suc (m + p) ≤ suc (m + q)`。归纳假设 `+-monoʳ-≤ m p q p≤q` 可以证明
  `m + p ≤ m + q`，我们在此之上使用 `s≤s` 即可得证。

{::comment}
The proof is by induction on the first argument.

* _Base case_: The first argument is `zero` in which case
  `zero + p ≤ zero + q` simplifies to `p ≤ q`, the evidence
  for which is given by the argument `p≤q`.

* _Inductive case_: The first argument is `suc m`, in which case
  `suc m + p ≤ suc m + q` simplifies to `suc (m + p) ≤ suc (m + q)`.
  The inductive hypothesis `+-monoʳ-≤ m p q p≤q` establishes that
  `m + p ≤ m + q`, and our goal follows by applying `s≤s`.
{:/}

接下来，我们证明加法对于小于等于在左手边是单调的。我们可以用之前的结论和加法的交换律来证明：
{::comment}
Second, we deal with the special case of showing addition is
monotonic on the left. This follows from the previous
result and the commutativity of addition:
{:/}
<pre class="Agda">{% raw %}<a id="+-monoˡ-≤"></a><a id="23657" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23657" class="Function">+-monoˡ-≤</a> <a id="23667" class="Symbol">:</a> <a id="23669" class="Symbol">∀</a> <a id="23671" class="Symbol">(</a><a id="23672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23672" class="Bound">m</a> <a id="23674" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23674" class="Bound">n</a> <a id="23676" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23676" class="Bound">p</a> <a id="23678" class="Symbol">:</a> <a id="23680" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="23681" class="Symbol">)</a>
  <a id="23685" class="Symbol">→</a> <a id="23687" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23672" class="Bound">m</a> <a id="23689" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="23691" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23674" class="Bound">n</a>
    <a id="23697" class="Comment">-------------</a>
  <a id="23713" class="Symbol">→</a> <a id="23715" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23672" class="Bound">m</a> <a id="23717" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="23719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23676" class="Bound">p</a> <a id="23721" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="23723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23674" class="Bound">n</a> <a id="23725" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="23727" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23676" class="Bound">p</a>
<a id="23729" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23657" class="Function">+-monoˡ-≤</a> <a id="23739" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23739" class="Bound">m</a> <a id="23741" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23741" class="Bound">n</a> <a id="23743" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23743" class="Bound">p</a> <a id="23745" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23745" class="Bound">m≤n</a>  <a id="23750" class="Keyword">rewrite</a> <a id="23758" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#9708" class="Function">+-comm</a> <a id="23765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23739" class="Bound">m</a> <a id="23767" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23743" class="Bound">p</a> <a id="23769" class="Symbol">|</a> <a id="23771" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#9708" class="Function">+-comm</a> <a id="23778" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23741" class="Bound">n</a> <a id="23780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23743" class="Bound">p</a>  <a id="23783" class="Symbol">=</a> <a id="23785" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="23795" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23743" class="Bound">p</a> <a id="23797" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23739" class="Bound">m</a> <a id="23799" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23741" class="Bound">n</a> <a id="23801" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23745" class="Bound">m≤n</a>{% endraw %}</pre>
用 `+-comm m p` 和 `+-comm n p` 来重写，可以让 `m + p ≤ n + p` 转换成 `p + n ≤ p + m`，
而我们可以用 `+-moroʳ-≤ p m n m≤n` 来证明。
{::comment}
Rewriting by `+-comm m p` and `+-comm n p` converts `m + p ≤ n + p` into
`p + m ≤ p + n`, which is proved by invoking `+-monoʳ-≤ p m n m≤n`.
{:/}

最后，我们把前两步的结论结合起来：
{::comment}
Third, we combine the two previous results:
{:/}
<pre class="Agda">{% raw %}<a id="+-mono-≤"></a><a id="24176" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24176" class="Function">+-mono-≤</a> <a id="24185" class="Symbol">:</a> <a id="24187" class="Symbol">∀</a> <a id="24189" class="Symbol">(</a><a id="24190" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24190" class="Bound">m</a> <a id="24192" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24192" class="Bound">n</a> <a id="24194" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24194" class="Bound">p</a> <a id="24196" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24196" class="Bound">q</a> <a id="24198" class="Symbol">:</a> <a id="24200" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="24201" class="Symbol">)</a>
  <a id="24205" class="Symbol">→</a> <a id="24207" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24190" class="Bound">m</a> <a id="24209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="24211" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24192" class="Bound">n</a>
  <a id="24215" class="Symbol">→</a> <a id="24217" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24194" class="Bound">p</a> <a id="24219" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="24221" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24196" class="Bound">q</a>
    <a id="24227" class="Comment">-------------</a>
  <a id="24243" class="Symbol">→</a> <a id="24245" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24190" class="Bound">m</a> <a id="24247" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="24249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24194" class="Bound">p</a> <a id="24251" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1670" class="Datatype Operator">≤</a> <a id="24253" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24192" class="Bound">n</a> <a id="24255" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="24257" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24196" class="Bound">q</a>
<a id="24259" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24176" class="Function">+-mono-≤</a> <a id="24268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24268" class="Bound">m</a> <a id="24270" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24270" class="Bound">n</a> <a id="24272" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24272" class="Bound">p</a> <a id="24274" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24274" class="Bound">q</a> <a id="24276" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24276" class="Bound">m≤n</a> <a id="24280" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24280" class="Bound">p≤q</a>  <a id="24285" class="Symbol">=</a>  <a id="24288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10823" class="Function">≤-trans</a> <a id="24296" class="Symbol">(</a><a id="24297" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23657" class="Function">+-monoˡ-≤</a> <a id="24307" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24268" class="Bound">m</a> <a id="24309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24270" class="Bound">n</a> <a id="24311" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24272" class="Bound">p</a> <a id="24313" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24276" class="Bound">m≤n</a><a id="24316" class="Symbol">)</a> <a id="24318" class="Symbol">(</a><a id="24319" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#22503" class="Function">+-monoʳ-≤</a> <a id="24329" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24270" class="Bound">n</a> <a id="24331" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24272" class="Bound">p</a> <a id="24333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24274" class="Bound">q</a> <a id="24335" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#24280" class="Bound">p≤q</a><a id="24338" class="Symbol">)</a>{% endraw %}</pre>
使用 `+-monoˡ-≤ m n p m≤n` 可以证明 `m + p ≤ n + p`，
使用 `+-monoʳ-≤ n p q p≤q` 可以证明 `n + p ≤ n + q`，用传递性把两者连接起来，
我们可以获得 `m + p ≤ n + q` 的证明，如上所示。
{::comment}
Invoking `+-monoˡ-≤ m n p m≤n` proves `m + p ≤ n + p` and invoking
`+-monoʳ-≤ n p q p≤q` proves `n + p ≤ n + q`, and combining these with
transitivity proves `m + p ≤ n + q`, as was to be shown.
{:/}

#### 练习 `*-mono-≤` （延伸）
{::comment}
#### Exercise `*-mono-≤` (stretch)
{:/}

证明乘法对于小于等于是单调的。
{::comment}
Show that multiplication is monotonic with regard to inequality.
{:/}

<pre class="Agda">{% raw %}<a id="24892" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

## 严格不等关系 {#strict-inequality}
{::comment}
## Strict inequality {#strict-inequality}
{:/}

我们可以用类似于定义不等关系的方法来定义严格不等关系。
{::comment}
We can define strict inequality similarly to inequality:
{:/}
<pre class="Agda">{% raw %}<a id="25123" class="Keyword">infix</a> <a id="25129" class="Number">4</a> <a id="25131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25141" class="Datatype Operator">_&lt;_</a>

<a id="25136" class="Keyword">data</a> <a id="_&lt;_"></a><a id="25141" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25141" class="Datatype Operator">_&lt;_</a> <a id="25145" class="Symbol">:</a> <a id="25147" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="25149" class="Symbol">→</a> <a id="25151" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="25153" class="Symbol">→</a> <a id="25155" class="PrimitiveType">Set</a> <a id="25159" class="Keyword">where</a>

  <a id="_&lt;_.z&lt;s"></a><a id="25168" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25168" class="InductiveConstructor">z&lt;s</a> <a id="25172" class="Symbol">:</a> <a id="25174" class="Symbol">∀</a> <a id="25176" class="Symbol">{</a><a id="25177" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25177" class="Bound">n</a> <a id="25179" class="Symbol">:</a> <a id="25181" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="25182" class="Symbol">}</a>
      <a id="25190" class="Comment">------------</a>
    <a id="25207" class="Symbol">→</a> <a id="25209" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="25214" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25141" class="Datatype Operator">&lt;</a> <a id="25216" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="25220" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25177" class="Bound">n</a>

  <a id="_&lt;_.s&lt;s"></a><a id="25225" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25225" class="InductiveConstructor">s&lt;s</a> <a id="25229" class="Symbol">:</a> <a id="25231" class="Symbol">∀</a> <a id="25233" class="Symbol">{</a><a id="25234" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25234" class="Bound">m</a> <a id="25236" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25236" class="Bound">n</a> <a id="25238" class="Symbol">:</a> <a id="25240" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="25241" class="Symbol">}</a>
    <a id="25247" class="Symbol">→</a> <a id="25249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25234" class="Bound">m</a> <a id="25251" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25141" class="Datatype Operator">&lt;</a> <a id="25253" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25236" class="Bound">n</a>
      <a id="25261" class="Comment">-------------</a>
    <a id="25279" class="Symbol">→</a> <a id="25281" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="25285" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25234" class="Bound">m</a> <a id="25287" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25141" class="Datatype Operator">&lt;</a> <a id="25289" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="25293" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#25236" class="Bound">n</a>{% endraw %}</pre>
严格不等关系与不等关系最重要的区别在于，0 小于任何数的后继，而不小于 0。
{::comment}
The key difference is that zero is less than the successor of an
arbitrary number, but is not less than zero.
{:/}

显然，严格不等关系不是自反的。但它是 *非自反的* （Irreflexive），表示 `n < n` 对于任何值 `n` 都不成立。和不等关系一样，严格不等关系是传递的。严格不等关系不是完全的，但是满足一个相似的性质：*三分律*（Trichotomy）：对于任意的 `m` 和 `n`，`m < n`、`m ≡ n` 或者
`m > n` 三者有且仅有一者成立。（我们定义 `m > n` 当且仅当 `n < m` 成立时成立）
严格不等关系对于加法和乘法也是单调的。
{::comment}
Clearly, strict inequality is not reflexive. However it is
_irreflexive_ in that `n < n` never holds for any value of `n`.
Like inequality, strict inequality is transitive.
Strict inequality is not total, but satisfies the closely related property of
_trichotomy_: for any `m` and `n`, exactly one of `m < n`, `m ≡ n`, or `m > n`
holds (where we define `m > n` to hold exactly when `n < m`).
It is also monotonic with regards to addition and multiplication.
{:/}

我们把一部分上述性质作为习题。非自反性需要逻辑非，三分律需要证明三者是互斥的，因此这两个性质暂不做为习题。我们会在 [Negation]({{ site.baseurl }}{% link out/plfa/Negation.md%}) 章节来重新讨论。
{::comment}
Most of the above are considered in exercises below.  Irreflexivity
requires negation, as does the fact that the three cases in
trichotomy are mutually exclusive, so those points are deferred to
Chapter [Negation]({{ site.baseurl }}{% link out/plfa/Negation.md%}).
{:/}

我们可以直接地来证明 `suc m ≤ n` 蕴含了 `m < n`，及其逆命题。
因此我们亦可从不等关系的性质中，使用此性质来证明严格不等关系的性质。
{::comment}
It is straightforward to show that `suc m ≤ n` implies `m < n`,
and conversely.  One can then give an alternative derivation of the
properties of strict inequality, such as transitivity, by
exploiting the corresponding properties of inequality.
{:/}

#### 练习 `<-trans` （推荐） {#less-trans}
{::comment}
#### Exercise `<-trans` (recommended) {#less-trans}
{:/}

证明严格不等是传递的。
{::comment}
Show that strict inequality is transitive.
{:/}

<pre class="Agda">{% raw %}<a id="27061" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

#### 练习 `trichotomy` {#trichotomy}
{::comment}
#### Exercise `trichotomy` {#trichotomy}
{:/}

证明严格不等关系满足弱化的三元律，证明对于任意 `m` 和 `n`，下列命题有一条成立：
  * `m < n`，
  * `m ≡ n`，或者
  * `m > n`。

{::comment}
Show that strict inequality satisfies a weak version of trichotomy, in
the sense that for any `m` and `n` that one of the following holds:
  * `m < n`,
  * `m ≡ n`, or
  * `m > n`.
{:/}

定义 `m > n` 为 `n < m`。你需要一个合适的数据类型声明，如同我们在证明完全性中使用的那样。
（我们会在介绍[逻辑非]({{ site.baseurl }}{% link out/plfa/Negation.md%})以后证明三者是互斥的）
{::comment}
Define `m > n` to be the same as `n < m`.
You will need a suitable data declaration,
similar to that used for totality.
(We will show that the three cases are exclusive after we introduce
[negation]({{ site.baseurl }}{% link out/plfa/Negation.md%}).)
{:/}

<pre class="Agda">{% raw %}<a id="27806" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

#### 练习 `+-mono-<` {#plus-mono-less}
{::comment}
#### Exercise `+-mono-<` {#plus-mono-less}
{:/}

证明加法对于严格不等关系是单调的。正如不等关系中那样，你可以需要额外的定义。
{::comment}
Show that addition is monotonic with respect to strict inequality.
As with inequality, some additional definitions may be required.
{:/}

<pre class="Agda">{% raw %}<a id="28131" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

#### 练习 `≤-iff-<` (推荐) {#leq-iff-less}
{::comment}
#### Exercise `≤-iff-<` (recommended) {#leq-iff-less}
{:/}

证明 `suc m ≤ n` 蕴含了 `m < n`，及其逆命题。
{::comment}
Show that `suc m ≤ n` implies `m < n`, and conversely.
{:/}

<pre class="Agda">{% raw %}<a id="28387" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

#### 练习 `<-trans-revisited` {#less-trans-revisited}
{::comment}
#### Exercise `<-trans-revisited` {#less-trans-revisited}
{:/}

用另外一种方法证明严格不等是传递的，使用之前证明的不等关系和严格不等关系的联系，
以及不等关系的传递性。
{::comment}
Give an alternative proof that strict inequality is transitive,
using the relating between strict inequality and inequality and
the fact that inequality is transitive.
{:/}

<pre class="Agda">{% raw %}<a id="28792" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

## 奇和偶
{::comment}
## Even and odd
{:/}

作为一个额外的例子，我们来定义奇数和偶数。不等关系和严格不等关系是 *二元关系*，而奇偶性是 *一元关系*，有时也被叫做 *谓词* （Predicate）：
{::comment}
As a further example, let's specify even and odd numbers.  Inequality
and strict inequality are _binary relations_, while even and odd are
_unary relations_, sometimes called _predicates_:
{:/}
<pre class="Agda">{% raw %}<a id="29157" class="Keyword">data</a> <a id="even"></a><a id="29162" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="29167" class="Symbol">:</a> <a id="29169" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="29171" class="Symbol">→</a> <a id="29173" class="PrimitiveType">Set</a>
<a id="29177" class="Keyword">data</a> <a id="odd"></a><a id="29182" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a>  <a id="29187" class="Symbol">:</a> <a id="29189" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="29191" class="Symbol">→</a> <a id="29193" class="PrimitiveType">Set</a>

<a id="29198" class="Keyword">data</a> <a id="29203" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="29208" class="Keyword">where</a>

  <a id="even.zero"></a><a id="29217" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29217" class="InductiveConstructor">zero</a> <a id="29222" class="Symbol">:</a>
      <a id="29230" class="Comment">---------</a>
      <a id="29246" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="29251" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>

  <a id="even.suc"></a><a id="29259" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29259" class="InductiveConstructor">suc</a>  <a id="29264" class="Symbol">:</a> <a id="29266" class="Symbol">∀</a> <a id="29268" class="Symbol">{</a><a id="29269" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29269" class="Bound">n</a> <a id="29271" class="Symbol">:</a> <a id="29273" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="29274" class="Symbol">}</a>
    <a id="29280" class="Symbol">→</a> <a id="29282" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a> <a id="29286" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29269" class="Bound">n</a>
      <a id="29294" class="Comment">------------</a>
    <a id="29311" class="Symbol">→</a> <a id="29313" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="29318" class="Symbol">(</a><a id="29319" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="29323" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29269" class="Bound">n</a><a id="29324" class="Symbol">)</a>

<a id="29327" class="Keyword">data</a> <a id="29332" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a> <a id="29336" class="Keyword">where</a>

  <a id="odd.suc"></a><a id="29345" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29345" class="InductiveConstructor">suc</a>   <a id="29351" class="Symbol">:</a> <a id="29353" class="Symbol">∀</a> <a id="29355" class="Symbol">{</a><a id="29356" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29356" class="Bound">n</a> <a id="29358" class="Symbol">:</a> <a id="29360" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="29361" class="Symbol">}</a>
    <a id="29367" class="Symbol">→</a> <a id="29369" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="29374" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29356" class="Bound">n</a>
      <a id="29382" class="Comment">-----------</a>
    <a id="29398" class="Symbol">→</a> <a id="29400" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a> <a id="29404" class="Symbol">(</a><a id="29405" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="29409" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29356" class="Bound">n</a><a id="29410" class="Symbol">)</a>{% endraw %}</pre>
一个数是偶数，如果它是 0，或者是奇数的后继。一个数是奇数，如果它是偶数的后继。
{::comment}
A number is even if it is zero or the successor of an odd number,
and odd if it is the successor of an even number.
{:/}

这是我们第一次定义一个相互递归的数据类型。因为每个标识符必须在使用前声明，所以我们首先声明索引数据类型 `even` 和 `odd` （省略 `where` 关键字和其构造器的定义），
然后声明其构造器（省略其签名 `ℕ → Set`，因为在之前已经给出）。
{::comment}
This is our first use of a mutually recursive datatype declaration.
Since each identifier must be defined before it is used, we first
declare the indexed types `even` and `odd` (omitting the `where`
keyword and the declarations of the constructors) and then
declare the constructors (omitting the signatures `ℕ → Set`
which were given earlier).
{:/}

这也是我们第一次使用 *重载* （Overloaded）的构造器。这意味着不同类型的构造器拥有相同的名字。在这里 `suc` 表示下面三种构造器其中之一：
{::comment}
This is also our first use of _overloaded_ constructors,
that is, using the same name for constructors of different types.
Here `suc` means one of three constructors:
{:/}

    suc : ℕ → ℕ

    suc : ∀ {n : ℕ}
      → odd n
        ------------
      → even (suc n)

    suc : ∀ {n : ℕ}
      → even n
        -----------
      → odd (suc n)

同理，`zero` 表示两种构造器的一种。因为类型推导的限制，Agda 不允许重载已定义的名字，
但是允许重载构造器。我们推荐将重载限制在有关联的定义中，如我们所做的这样，但这不是必须的。
{::comment}
Similarly, `zero` refers to one of two constructors. Due to how it
does type inference, Agda does not allow overloading of defined names,
but does allow overloading of constructors.  It is recommended that
one restrict overloading to related meanings, as we have done here,
but it is not required.
{:/}

我们证明两个偶数之和是偶数：
{::comment}
We show that the sum of two even numbers is even:
{:/}
<pre class="Agda">{% raw %}<a id="e+e≡e"></a><a id="31032" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31032" class="Function">e+e≡e</a> <a id="31038" class="Symbol">:</a> <a id="31040" class="Symbol">∀</a> <a id="31042" class="Symbol">{</a><a id="31043" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31043" class="Bound">m</a> <a id="31045" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31045" class="Bound">n</a> <a id="31047" class="Symbol">:</a> <a id="31049" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="31050" class="Symbol">}</a>
  <a id="31054" class="Symbol">→</a> <a id="31056" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="31061" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31043" class="Bound">m</a>
  <a id="31065" class="Symbol">→</a> <a id="31067" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="31072" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31045" class="Bound">n</a>
    <a id="31078" class="Comment">------------</a>
  <a id="31093" class="Symbol">→</a> <a id="31095" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="31100" class="Symbol">(</a><a id="31101" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31043" class="Bound">m</a> <a id="31103" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="31105" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31045" class="Bound">n</a><a id="31106" class="Symbol">)</a>

<a id="o+e≡o"></a><a id="31109" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31109" class="Function">o+e≡o</a> <a id="31115" class="Symbol">:</a> <a id="31117" class="Symbol">∀</a> <a id="31119" class="Symbol">{</a><a id="31120" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31120" class="Bound">m</a> <a id="31122" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31122" class="Bound">n</a> <a id="31124" class="Symbol">:</a> <a id="31126" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="31127" class="Symbol">}</a>
  <a id="31131" class="Symbol">→</a> <a id="31133" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a> <a id="31137" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31120" class="Bound">m</a>
  <a id="31141" class="Symbol">→</a> <a id="31143" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29162" class="Datatype">even</a> <a id="31148" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31122" class="Bound">n</a>
    <a id="31154" class="Comment">-----------</a>
  <a id="31168" class="Symbol">→</a> <a id="31170" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29182" class="Datatype">odd</a> <a id="31174" class="Symbol">(</a><a id="31175" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31120" class="Bound">m</a> <a id="31177" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="31179" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31122" class="Bound">n</a><a id="31180" class="Symbol">)</a>

<a id="31183" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31032" class="Function">e+e≡e</a> <a id="31189" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29217" class="InductiveConstructor">zero</a>     <a id="31198" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31198" class="Bound">en</a>  <a id="31202" class="Symbol">=</a>  <a id="31205" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31198" class="Bound">en</a>
<a id="31208" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31032" class="Function">e+e≡e</a> <a id="31214" class="Symbol">(</a><a id="31215" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29259" class="InductiveConstructor">suc</a> <a id="31219" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31219" class="Bound">om</a><a id="31221" class="Symbol">)</a> <a id="31223" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31223" class="Bound">en</a>  <a id="31227" class="Symbol">=</a>  <a id="31230" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29259" class="InductiveConstructor">suc</a> <a id="31234" class="Symbol">(</a><a id="31235" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31109" class="Function">o+e≡o</a> <a id="31241" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31219" class="Bound">om</a> <a id="31244" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31223" class="Bound">en</a><a id="31246" class="Symbol">)</a>

<a id="31249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31109" class="Function">o+e≡o</a> <a id="31255" class="Symbol">(</a><a id="31256" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29345" class="InductiveConstructor">suc</a> <a id="31260" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31260" class="Bound">em</a><a id="31262" class="Symbol">)</a> <a id="31264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31264" class="Bound">en</a>  <a id="31268" class="Symbol">=</a>  <a id="31271" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#29345" class="InductiveConstructor">suc</a> <a id="31275" class="Symbol">(</a><a id="31276" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31032" class="Function">e+e≡e</a> <a id="31282" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31260" class="Bound">em</a> <a id="31285" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#31264" class="Bound">en</a><a id="31287" class="Symbol">)</a>{% endraw %}</pre>
与相互递归的定义对应，我们用两个相互递归的函数，一个证明两个偶数之和是偶数，另一个证明一个奇数与一个偶数之和是奇数。
{::comment}
Corresponding to the mutually recursive types, we use two mutually recursive
functions, one to show that the sum of two even numbers is even, and the other
to show that the sum of an odd and an even number is odd.
{:/}

这是我们第一次使用相互递归的函数。因为每个标识符必须在使用前声明，我们先给出两个函数的签名，
然后再给出其定义。
{::comment}
This is our first use of mutually recursive functions.  Since each identifier
must be defined before it is used, we first give the signatures for both
functions and then the equations that define them.
{:/}

要证明两个偶数之和为偶，我们考虑第一个数为偶数的证明。如果是因为第一个数为 0，
那么第二个数为偶数的证明即为和为偶数的证明。如果是因为第一个数为奇数的后继，
那么和为偶数是因为他是一个奇数和一个偶数的和的后续，而这个和是一个奇数。
{::comment}
To show that the sum of two even numbers is even, consider the
evidence that the first number is even. If it is because it is zero,
then the sum is even because the second number is even.  If it is
because it is the successor of an odd number, then the result is even
because it is the successor of the sum of an odd and an even number,
which is odd.
{:/}

要证明一个奇数和一个偶数的和是奇数，我们考虑第一个数是奇数的证明。
如果是因为它是一个偶数的后继，那么和为奇数，因为它是两个偶数之和的后继，
而这个和是一个偶数。

{::comment}
To show that the sum of an odd and even number is odd, consider the
evidence that the first number is odd. If it is because it is the
successor of an even number, then the result is odd because it is the
successor of the sum of two even numbers, which is even.
{:/}

#### 练习 `o+o≡e` (延伸) {#odd-plus-odd}
{::comment}
#### Exercise `o+o≡e` (stretch) {#odd-plus-odd}
{:/}

证明两个奇数之和为偶数。
{::comment}
Show that the sum of two odd numbers is even.
{:/}

<pre class="Agda">{% raw %}<a id="32910" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

#### 练习 `Bin-predicates` (延伸) {#Bin-predicates}
{::comment}
#### Exercise `Bin-predicates` (stretch) {#Bin-predicates}
{:/}

回忆我们在练习 [Bin]({{ site.baseurl }}{% link out/plfa/Naturals.md%}#Bin) 中定义了一个数据类型 `Bin` 来用二进制字符串表示自然数。
这个表达方法不是唯一的，因为我们在开头加任意个 0。因此，11 可以由以下方法表示：
{::comment}
Recall that
Exercise [Bin]({{ site.baseurl }}{% link out/plfa/Naturals.md%}#Bin)
defines a datatype `Bin` of bitstrings representing natural numbers.
Representations are not unique due to leading zeros.
Hence, eleven may be represented by both of the following:
{:/}

    x1 x1 x0 x1 nil
    x1 x1 x0 x1 x0 x0 nil

定义一个谓词
{::comment}
Define a predicate
{:/}

    Can : Bin → Set

其在一个二进制字符串的表示是标准的（Canonical）时成立，表示它没有开头的 0。在两个 11 的表达方式中，
第一个是标准的，而第二个不是。在定义这个谓词时，你需要一个辅助谓词：
{::comment}
over all bitstrings that holds if the bitstring is canonical, meaning
it has no leading zeros; the first representation of eleven above is
canonical, and the second is not.  To define it, you will need an
auxiliary predicate
{:/}

    One : Bin → Set

其仅在一个二进制字符串开头为 1 时成立。一个二进制字符串是标准的，如果它开头是 1 （表示一个正数），
或者它仅是一个 0 （表示 0）。
{::comment}
that holds only if the bistring has a leading one.  A bitstring is
canonical if it has a leading one (representing a positive number) or
if it consists of a single zero (representing zero).
{:/}

证明递增可以保持标准性。
{::comment}
Show that increment preserves canonical bitstrings:
{:/}

    Can x
    ------------
    Can (inc x)

证明从自然数转换成的二进制字符串是标准的。
{::comment}
Show that converting a natural to a bitstring always yields a
canonical bitstring:
{:/}

    ----------
    Can (to n)

证明将一个标准的二进制字符串转换成自然数之后，再转换回二进制字符串与原二进制字符串相同。
{::comment}
Show that converting a canonical bitstring to a natural
and back is the identity:
{:/}

    Can x
    ---------------
    to (from x) ≡ x

（提示：对于每一条习题，先从 `One` 的性质开始）
{::comment}
(Hint: For each of these, you may first need to prove related
properties of `One`.)
{:/}

<pre class="Agda">{% raw %}<a id="34781" class="Comment">-- 在此处书写你的代码</a>{% endraw %}</pre>

## 标准库
{::comment}
## Standard library
{:/}

标准库中有类似于本章介绍的定义：
{::comment}
Definitions similar to those in this chapter can be found in the standard library:
{:/}
<pre class="Agda">{% raw %}<a id="34981" class="Keyword">import</a> <a id="34988" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="34997" class="Keyword">using</a> <a id="35003" class="Symbol">(</a><a id="35004" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#845" class="Datatype Operator">_≤_</a><a id="35007" class="Symbol">;</a> <a id="35009" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#868" class="InductiveConstructor">z≤n</a><a id="35012" class="Symbol">;</a> <a id="35014" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#910" class="InductiveConstructor">s≤s</a><a id="35017" class="Symbol">)</a>
<a id="35019" class="Keyword">import</a> <a id="35026" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="35046" class="Keyword">using</a> <a id="35052" class="Symbol">(</a><a id="35053" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2115" class="Function">≤-refl</a><a id="35059" class="Symbol">;</a> <a id="35061" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2308" class="Function">≤-trans</a><a id="35068" class="Symbol">;</a> <a id="35070" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2165" class="Function">≤-antisym</a><a id="35079" class="Symbol">;</a> <a id="35081" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2420" class="Function">≤-total</a><a id="35088" class="Symbol">;</a>
                                  <a id="35124" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#12667" class="Function">+-monoʳ-≤</a><a id="35133" class="Symbol">;</a> <a id="35135" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#12577" class="Function">+-monoˡ-≤</a><a id="35144" class="Symbol">;</a> <a id="35146" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#12421" class="Function">+-mono-≤</a><a id="35154" class="Symbol">)</a>{% endraw %}</pre>
在标准库中，`≤-total` 是使用析取定义的（我们将在 [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%}) 章节定义）。
`+-monoʳ-≤`、`+-monoˡ-≤` 和 `+-mono-≤` 的证明方法和本书不同。
更多的参数是隐式申明的。
{::comment}
In the standard library, `≤-total` is formalised in terms of
disjunction (which we define in
Chapter [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%})),
and `+-monoʳ-≤`, `+-monoˡ-≤`, `+-mono-≤` are proved differently than here,
and more arguments are implicit.
{:/}

## Unicode

本章使用了如下 Unicode 符号：
{::comment}
This chapter uses the following unicode:
{:/}

    ≤  U+2264  小于等于 (\<=, \le)
    ≥  U+2265  大于等于 (\>=, \ge)
    ˡ  U+02E1  小写字母 L 标识符 (\^l)
    ʳ  U+02B3  小写字母 R 标识符 (\^r)

`\^l` 和 `\^r` 命令给出了左右箭头，以及上标字母 `l` 和 `r`。
{::comment}
The commands `\^l` and `\^r` give access to a variety of superscript
leftward and rightward arrows in addition to superscript letters `l` and `r`.
{:/}