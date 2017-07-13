---
layout: post
title: 지분 증명 vs 작업 증명
categories: others
html header: <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

---

<h3> 초록 </h3>

지분 증명은 Bitcoin에서 사용되는 작업 증명의 대안인 디지털 화폐에 대한 합의 메커니즘이다. 스테이크 접근법을 증명하는 주요 장점은 값 비싼 계산이 없으므로 블록 생성 보상에 대한 진입 장벽이 낮다는 것입니다. 이 보고서에서는 두 합의 시스템의 장단점을 검토하고 Bitcoin 및 작업 증명 접근법에서 일반적으로 발생할 가능성이 거의없는 공격에 대해 기존의 지분 증명 구현이 취약하다는 것을 보여줍니다.


<h3> 1. 이론 </h3>
Bitcoin과 같은 암호화폐 시스템은 사용자가 자신의 잔액을 알 수 있게하는 상태 정보를 가지고 있다. Bitcoin의 경우, 시스템 상태는 각 출력을 암호로 잠겨있고 사용하지 않은 트랜잭션 출력(unspent transaction outputs - UTXO)의 모음이므로 사용자가 UTXO를 사용하기 위해 소유권 증명을 제공해야 한다. 숨김없이 잔액을 저장하는 대신에, Bitcoin 및 기타 디지털 통화는 사용자의 트랜잭션에 대한 전체 기록을 유지 관리하여 시스템의 현재 상태 및 모든 과거 상태를 추론 할 수 있습니다. 

트랜잭션은 시스템 상태에 대한 아주 작은 변경을 나타낸다. 트랜잭션은 블록으로 그룹화된다. 블록들은 블록 체인이라고하는 순서가 있는 거래더미로 구성된다. 블록 체인을 구성하기 위해, 각 블록은 이전 블록을 가리키는 정보(reference)를 가지고 있다. 사슬의 첫 번째 블록은 기원 블록(genesis block)이다. 이 블록은 이전 블록을 가지고 있지 않고, 보통 프로토콜에 하드 코딩된다. 암호 화폐 프로토콜에 의해 설정된 규칙에 따라 새 블록이 생성된다. 이 규칙의 중요한 기능은 블록 체인에 대한 공격으로부터 보호하고 블록 체인의 여러 인스턴스가 나타나는 경우 합의(consensus)에 도달하는 것이다. 작업 증명과 지분 증명는 유효 블록에 대한 두 종류의 제한을 나타내고, 두 가지 다른 합의 메커니즘을 의미한다.

<strong> 정의 1.</strong > 이전 블록으로 B를 참조하는 블록을 발견하려고하면, 사용자는 블록 B의 위(on top)에서 작업한다고 말한다. 

B는 블록 체인 전체를 고유하게 결정하므로, 최신 블록이있는 블록 체인을 나타낼 수 있으며 발견되는 블록들은 블록 체인 B의 위에 있다.

암호화폐 시스템은 P2P(peer-to-peer)인터넷 프로토콜을 통해 통신하는 통화을 위한 인프라 공급자에 속한 데이터베이스 복사본과 함께 분산 데이터베이스로 볼 수 있다. 
CAP(일관성, 가용성 및 파티션 허용 오차) 정리 [2]의 관점에서 암호화폐 시스템은 사용할 수 있고,(모든 요청이 응답을 받음), 파티션 내성이 있다(일부 노드가 실패하더라도 서비스를 여전히 수행함). 그러나 일관적이지는 않다. 때때로 시스템의 서로다른 사용자는 시스템의 다른 상태를 현재 상태로 관찰 할 것이다. 경우에 따라 불일치는 새로운 블록 해시가 발견되었지만 시스템의 모든 사용자에게 아직 릴레이되지 않은 상황에 해당한다. 궁극적인 일관성을 얻기 위해서는[3], 사운드 컨센서스 프로토콜은 다음 요구 사항을 부과해야한다.

<strong> 조건 1.</strong> 블록을 발견 한 사용자는 네트워크를 통해 즉시 브로드 캐스트하고 자신을 위해 보유하지 않도록해야합니다.

다른 경우, 시스템 불일치는 여러 개의 분기로 분할되는 블록 체인으로 인해 발생하며(그림 1), 블록 체인 분기(forking)의 다양한 원인이 있습니다.

• 두 명의 사용자가 거의 동시에 새 블록을 발견한다. 

• 공격자가 블록 체인을 분기하여 완료된 트랜잭션을 되돌리려 고 시도합니다. 

의도적 인 분기를 막기 위해, 사운드 컨센서스 프로토콜은 다음 요구 사항을 추가해야한다.

<strong> 조건 2.</strong> 사용자는 중급 사슬 위에 블록을 발견하지 말아야한다. 보다 정확하게, 블록 B를 참조하는 알려진 블록 B '가있는 경우, 사용자는 B를 빌드 할 이유가 없어야합니다

그림 1의 상황에서, 조건 1-2 : B_(i+2), B^''_(i+1) 및 B^''_(i+2)에 따라 블록 체인을 확장하기위한 기준으로 세 개의 블록만 사용할 수 있다. 유효한 블록 체인에는 추가 제한 사항이 있다. 대부분의 경우 이러한 조건은 최대 블록 수 (합리적인 사용자가 자신의 체인에서 작성할 수있는 블록에서 B''i + 1을 제외)가있는 체인을 선택하는 것이다. 합의 규칙의 목표는 단일 사슬의 선택을 보장하는 것이다. 그러나 일반적으로 일부 규칙은 사용자에 따라 다르다. 예를 들어 길이가 동일한 Bitcoin 블록 체인이 여러 개인 경우 먼저받은 블록 체인 블록 체인을 선택해야한다. 따라서 모든 사용자에 대해 동일한 블록 체인을 선택하는 방법은 없다 (사용자에 따라 규칙을 따르는 것이 매우 어렵거나 불가능하기 때문이다).

시스템이 궁극적으로 일관성을 가지려면 그 합의 프로토콜은 다음 세번째 요구 사항을 충족시켜야한다.

<strong> 조건 3.</strong> 합의 규칙은 블록 체인 분기(fork)를 해결하는 방식으로 구성되어야 한다. 즉, 경쟁 지점 중 하나가 합리적인 시간 내에 다른 지점을 모두 인수해야 한다. 

우리는 작업 증명과 지분 증명 알고리즘을 사용하여 블록을 발견하기위한 별도의 용어를 사용할 것이다:

<strong> 정의 2. </strong> 작업 증명 프로토콜에 의해 시행된 과도한
 계산 문제를 푸는 과정을 (블록) 마이닝(mining) 이라고 한다.

<strong >정의 3. </strong> 스테이크 프로토콜의 증명에 의해 과도한 계산 문제를 푸는 과정을 (블록) 마인팅(minting) 이라고 한다.

<h3> 1.1 작업 증명 </h3>

작업 증명 알고리즘으로 보호 된 암호화폐 시스템의 예로 Bitcoin을 고려해보자. Bitcoin의 각 블록은 두 부분으로 구성된다.

• 블록 생성 시간, 이전 블록에 대한 참조 및 트랜잭션 블록의 Merkle 트리 루트 [4]와 같은 주요 매개 변수의 블록 헤더 

• 트랜잭션 차단 리스트.

특정 블록을 가리키기 위해, 그 헤더는 SHA-256 함수 [5]로 두 번 해시된다. 결과 정수 값은 구간 [0, 2^256 - 1]에 속한다. 다른 방식의 가능한 구현을 설명하기 위해, 우리는 다양한 인수 및 범위 [0, M]와 함께 일반 해시 함수 hash (·)를 사용한다. 예를 들어, 함수의 인수는 이진 문자열로 처리되고 함께 병합되어 SHA-256 해싱 함수에 전달할 수있는 단일 인수를 형성 할 수 있다. 

블록 참조는 작업 증명 프로토콜에 사용된다. 블록이 유효하다고 간주 되려면 해당 참조가 특정 임계 값을 초과하지 않아야한다.

hash(B) <= M/D

여기서 D ∈ [1, M]은 목표 난이도를 뜻한다. 블록 헤더에서 가능한 모든 변수를 반복적으로 반복하는 것 이외에는 (1)을 만족하는 B를 찾기위한 알려진 방법이 없다. D의 값이 높을수록 유효한 블록을 찾는 데 더 많은 반복이 필요하다. 기대 연산 수는 정확히 D 이다. 

유효한 블록을 찾기 위해 초당 r 연산을 수행 할 수있는 하드웨어가있는 광부의 시간주기 T (r)는 비율 r / D로 기하 급수적으로 분산된다 (부록 A 참조).

해시 파워 r1, r2, ..., rn을 가진 n명의 Bitcoin 광부를 고려하자. 광부가 발견된 블록을 알리고 블록이 다른 광부에게 즉시 도달한다고 가정하자. 블록 T를 찾는 시간은 확률 변수 T(ri)의 최소값과 같다. 지수 분포의 특성에 따르면, T도 기하 급수적으로 분포한다.

마지막 방정식은 광산이 공정하다는 것을 보여준다. 해시 파워 p를 가진
 광부는 다른 광부보다 먼저 블록을 풀기 위해 동일한 확률 p를 가진다. Bitcoin에서 사용 된 작업 증명이 조건 1-3을 만족한다는 것을 알 수 있다.



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