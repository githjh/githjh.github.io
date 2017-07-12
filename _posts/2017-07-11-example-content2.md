---
layout: post
title: 지분 증명 vs 작업 증명
categories: others
---

<h3> 초록 </h3>

지분 증명은 Bitcoin에서 사용되는 작업 증명의 대안인 디지털 화폐에 대한 합의 메커니즘이다. 스테이크 접근법을 증명하는 주요 장점은 값 비싼 계산이 없으므로 블록 생성 보상에 대한 진입 장벽이 낮다는 것입니다. 이 보고서에서는 두 합의 시스템의 장단점을 검토하고 Bitcoin 및 작업 증명 접근법에서 일반적으로 발생할 가능성이 거의없는 공격에 대해 기존의 지분 증명 구현이 취약하다는 것을 보여줍니다.


<h3> 1. 이론 </h3>
Bitcoin과 같은 암호화폐 시스템은 사용자가 자신의 잔액을 알 수 있게하는 상태 정보를 가지고 있다. Bitcoin의 경우, 시스템 상태는 각 출력을 암호로 잠겨있고 사용하지 않은 트랜잭션 출력(unspent transaction outputs - UTXO)의 모음이므로 사용자가 UTXO를 사용하기 위해 소유권 증명을 제공해야 한다. 숨김없이 잔액을 저장하는 대신에, Bitcoin 및 기타 디지털 통화는 사용자의 트랜잭션에 대한 전체 기록을 유지 관리하여 시스템의 현재 상태 및 모든 과거 상태를 추론 할 수 있습니다. 

트랜잭션은 시스템 상태에 대한 아주 작은 변경을 나타낸다. 트랜잭션은 블록으로 그룹화된다. 블록들은 블록 체인이라고하는 순서가 있는 거래더미로 구성된다. 블록 체인을 구성하기 위해, 각 블록은 이전 블록을 가리키는 정보(reference)를 가지고 있다. 사슬의 첫 번째 블록은 기원 블록(genesis block)이다. 이 블록은 이전 블록을 가지고 있지 않고, 보통 프로토콜에 하드 코딩된다. 암호 화폐 프로토콜에 의해 설정된 규칙에 따라 새 블록이 생성된다. 이 규칙의 중요한 기능은 블록 체인에 대한 공격으로부터 보호하고 블록 체인의 여러 인스턴스가 나타나는 경우 합의(consensus)에 도달하는 것이다. 작업 증명과 지분 증명는 유효 블록에 대한 두 종류의 제한을 나타내고, 두 가지 다른 합의 메커니즘을 의미한다.

<!--
<p><small>This demo page has been used from <a href="http://jasonm23.github.io/markdown-css-themes/" target="_blank">http://jasonm23.github.io/markdown-css-themes/</a>.</small></p>

<h1>A First Level Header</h1>

<h2>A Second Level Header</h2>

<h3>A Third Level Header</h3>

<h4>A Fourth Level Header</h4>

<h5>A Fifth Level Header</h5>

<h6>A Sixed Level Header</h6>

<p>Now is the time for all good men to come to
the aid of their country. This is just a
regular paragraph.</p>

<p>The quick brown fox jumped over the lazy
dog&rsquo;s back.</p>

<hr />

<h3>Header 3</h3>

<blockquote><p>This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.</p>

<p>Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.</p>

<h2>This is an H2 in a blockquote</h2>

<p>This is the first level of quoting.</p>

<blockquote><p>This is nested blockquote.</p></blockquote>

<p>Back to the first level.</p></blockquote>

<p>Some of these words <em>are emphasized</em>.
Some of these words <em>are emphasized also</em>.</p>

<p>Use two asterisks for <strong>strong emphasis</strong>.
Or, if you prefer, <strong>use two underscores instead</strong>.</p>

<ul>
<li>Candy.</li>
<li>Gum.</li>
<li>Booze.</li>
<li>Red</li>
<li>Green</li>
<li><p>Blue</p></li>
<li><p>A list item.</p></li>
</ul>


<p>With multiple paragraphs.</p>

<ul>
<li><p>Another item in the list.</p></li>
<li><p>This is a list item with two paragraphs. Lorem ipsum dolor
sit amet, consectetuer adipiscing elit. Aliquam hendrerit
mi posuere lectus.</p></li>
</ul>


<p>Vestibulum enim wisi, viverra nec, fringilla in, laoreet
vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
sit amet velit.*   Suspendisse id sem consectetuer libero luctus adipiscing.</p>

<ul>
<li>This is a list item with two paragraphs.</li>
</ul>

ㅇㅁㄴㅇㄹ
<p>This is the second paragraph in the list item. You&rsquo;re
only required to indent the first line. Lorem ipsum dolor
sit amet, consectetuer adipiscing elit.</p>

<ul>
<li><p>Another item in the same list.</p></li>
<li><p>A list item with a bit of <code>code</code> inline.</p></li>
<li><p>A list item with a blockquote:</p>

<blockquote><p>This is a blockquote
inside a list item.</p></blockquote></li>
</ul>


<p>Here is an example of a pre code block</p>

<pre><code>tell application "Foo"
    beep
end tell
</code></pre>

<p>This is an <a href="#">example link</a>.</p>

<p>I start my morning with a cup of coffee and
<a href="http://www.nytimes.com/">The New York Times</a>.</p>

### Code snippet

{% highlight python %}
if __name__ =='__main__':
    img_thread = threading.Thread(target=downloadWallpaper)
    img_thread.start()
    st = '\rDownloading Image'
    current = 1
    while img_thread.is_alive():
        sys.stdout.write(st+'.'*((current)%5))
        current=current+1
        time.sleep(0.3)
    img_thread.join()
    print('\nImage of the day downloaded.')
{% endhighlight %}

-->