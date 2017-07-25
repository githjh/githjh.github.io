---
layout: post
title: Example content for posts  
categories: others
---


<p><small>This demo page has been used from <a href="http://jasonm23.github.io/markdown-css-themes/" target="_blank">http://jasonm23.github.io/markdown-css-themes/</a>.</small></p>

<h3> 초록 </h3>


<p>블록 체인 프로토콜은 본질적으로 트랜잭션 처리량과 대기 시간이 제한된다. 성능 향상과 규모 문제를 해결하려는 최근의 노력은 오프 체인 (off-chain) 지불 채널에 초점을 맞추고 있다. 이러한 채널은 낮은 대기 시간과 높은 처리량을 달성 할 수 있지만, Bitcoin 블록 체인 위에 안전하게 배포하는 것은 어렵다. 부분적으로는 안전한 구현을 위해서는 기본 프로토콜과 생태계를 변경해야하기 때문이다. 

우리는 신뢰할 수있는 실행 환경을 활용하는 full-duplex 지불 채널 프레임 워크 인 Teechan을 제시합니다. Teechan은 프로토콜을 수정할 필요없이 기존 Bitcoin 블록 체인에 안전하게 배치 할 수 있습니다. 그것은 : (i) 이전 솔루션보다 높은 트랜잭션 처리량과 낮은 트랜잭션 대기 시간을 달성합니다. (ii) 잔액이 채널의 크레딧을 초과하지 않는 한 무제한 전이중 지불을 가능하게합니다. (iii) 어떤 방향 으로든 지불 당 하나의 메시지 만 보내면됩니다. (iv) 임의의 실행 시나리오 하에서 블록 체인상의 최대 두 개의 트랜잭션 장소. 우리는 Bitcoin 네트워크에서 Intel SGX를 사용하여 Teechan 프레임 워크를 구축하고 배포했습니다. 우리의 실험에 따르면 네트워크 대기 시간을 제외하고 Teechan은 잠깐 지연된 대기 시간으로 단일 채널에서 초당 2,480 회의 트랜잭션을 처리 할 수 있습니다. </p>



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