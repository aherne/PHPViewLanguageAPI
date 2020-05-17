<p>Standard tags work with attribute values of following types:</p>
<ul>
    <li><i>scalars</i>: strings or integers</li>
    <li><i>expressions</i>: ViewLanguage <a href="/view-language/expressions">expressions</a>. <strong>When native functions are used as attribute values, they must be left undecorated</strong>. Example: <i>count(${asd})</i> instead of <i>${count(${asd})}</i>.</li>
    <li><i>conditions</i>: syntax is expected to be C-like, but ultimately matches that of language code is compiled into (in our case, PHP). Example: <i>${x}==true</i> means in PHP <i>$x==true</i>.</li>
</ul>

<h2 id=":for" class="tag">:for</h2>
<p>Creates a FOR loop.</p>
<p>Syntax:</p>
<code class="html">&lt;:for var="..." start="..." end="..." (step="...")&gt;
    ...
&lt;/:for&gt;</code>
<p>Attributes:</p>
<table>
    <thead>
    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DEFAULT</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>var</p>
        </td>
        <td>
            <p>Name of counter variable.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>-</p>
        </td>
        <td>
            <p>STRING</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>start</p>
        </td>
        <td>
            <p>Value of begin counter.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>-</p>
        </td>
        <td>
            <p>INTEGER</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>end</p>
        </td>
        <td>
            <p>Value of end counter.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>-</p>
        </td>
        <td>
            <p>INTEGER</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>step</p>
        </td>
        <td>
            <p>Value of increment/decrement step</p>
        </td>
        <td>
            <p>N</p>
        </td>
        <td>
            <p>1</p>
        </td>
        <td>
            <p>INTEGER</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:for var="i" start="0" end="10"&gt;
    ...
&lt;/:for&gt;</code>
        </td>
        <td>
            <code class="php">for($i=0; $i&lt;=10; $i=$i+1){
    ...
}</code>
        </td>
    </tr>
    <tr>
        <td>
            <code class="html">&lt;:for var="i" start="10" end="0" step="-2"&gt;
    ...
&lt;/:for&gt;</code>
        </td>
        <td>
            <code class="php">for($i=10; $i&gt;=0; $i=$i-2) {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":foreach" class="tag">:foreach</h2>
<p>Creates a FOR EACH loop.</p>

<p>Syntax:</p>
<code class="html">&lt;:foreach var="..." (key="...") val="..."&gt;
    ...
&lt;/:foreach&gt;</code>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>var</p>
        </td>
        <td>
            <p>Variable to iterate.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>EXPRESSION</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>key</p>
        </td>
        <td>
            <p>Name of key variable.</p>
        </td>
        <td>
            <p>N</p>
        </td>
        <td>
            <p>STRING</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>val</p>
        </td>
        <td>
            <p>Name of value variable.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>STRING</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:foreach var="${a}" key="k" val="v"&gt;
    ...
&lt;/:foreach&gt;</code>
        </td>
        <td>
            <code class="php">foreach($a as $k=&gt;$v) {
    ...
}</code>
        </td>
    </tr>
    <tr>
        <td>
            <code class="html">&lt;:foreach var="${a}" val="v"&gt;
    ...
&lt;/:foreach&gt;</code>
        </td>
        <td>
            <code class="php">foreach($a as $v) {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":if"  class="tag">:if</h2>
<p>Creates an IF condition.</p>
<p>Syntax:</p>
<code class="html">&lt;:if test="..."&gt;
    ...
&lt;/:if&gt;</code>
<p class="highlighted">Tag must not be closed if folowed by a <a href="#:elseif">:elseif</a> or <a href="#:else">:else</a>.</p>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>test</p>
        </td>
        <td>
            <p>Condition when body is executed.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>CONDITION</p>
        </td>
    </tr>
    </tbody>
</table>

<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:if test="${x}==2"&gt;
    ...
&lt;/:if&gt;</code>
        </td>
        <td>
            <code class="php">if($x==2) {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>
<p><i>You can also run simple IF/ELSE statements from expressions using ternary operators (<a href="#expressions">see above</a>)!</i></p>
<h2 id=":elseif"  class="tag">:elseif</h2>
<p>Creates an ELSE IF condition.</p>

<p>Syntax:</p>
<code class="html">&lt;:elseif test="..."&gt;
    ...
&lt;/:if&gt;</code>
<p class="highlighted">Tag must not be closed if folowed by a <a href="#:elseif">:elseif</a> or <a href="#:else">:else</a>.</p>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>test</p>
        </td>
        <td>
            <p>Condition when body is executed.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>CONDITION</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:elseif test="${x}==2"&gt;
    ...
&lt;/:if&gt;</code>
        </td>
        <td>
            <code class="php">} else if ($x==2) {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":else" class="tag">:else</h2>
<p>Creates an ELSE condition.</p>
<p>Syntax:</p>
<code class="html">&lt;:else&gt;
    ...
&lt;/:if&gt;</code>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:else&gt;
    ...
&lt;/:if&gt;</code>
        </td>
        <td>
            <code class="php">} else {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>
<p><i>You can also run simple IF/ELSE statements from expressions using ternary operators (<a href="#expressions">see above</a>)!</i></p>

<h2 id=":set" class="tag">:set</h2>
<p>Sets a value to a variable.</p>
<p>Syntax:</p>
<code class="html">&lt;:set var="..." val="..."/&gt;</code>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>var</p>
        </td>
        <td>
            <p>Name of variable to be created/updated.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>STRING</p>
        </td>
    </tr>
    <tr>
        <td>
            <p>val</p>
        </td>
        <td>
            <p>Value to set variable as.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>STRING | EXPRESSION</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:set var="a" val="10"/&gt;</code>
        </td>
        <td>
            <code class="php">$a = "10";</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":unset" class="tag">:unset</h2>
    <p>Removes variable from memory.</p>
<p>Syntax:</p>
<code class="html">&lt;:unset var="..."/&gt;</code>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>var</p>
        </td>
        <td>
            <p>Name of variable to be unset.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>STRING</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:unset name="a"/&gt;</code>
        </td>
        <td>
            <code class="php">unset($a);</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":while" class="tag">:while</h2>
    <p>Creates a WHILE loop.</p>
<p>Syntax:</p>
<code class="html">&lt;:while test="..."&gt;
    ...
&lt;/:while&gt;</code>
<p>Attributes:</p>
<table>
    <thead>

    <tr>
        <td>
            <p>NAME</p>
        </td>
        <td>
            <p>DESCRIPTION</p>
        </td>
        <td>
            <p>MANDATORY</p>
        </td>
        <td>
            <p>DATA TYPE</p>
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            <p>test</p>
        </td>
        <td>
            <p>Condition when body is executed.</p>
        </td>
        <td>
            <p>Y</p>
        </td>
        <td>
            <p>CONDITION</p>
        </td>
    </tr>
    </tbody>
</table>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:while test="${x}==2"&gt;
    ...
&lt;/:while&gt;</code>
        </td>
        <td>
            <code class="php">while($x==2) {
    ...
}</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":break" class="tag">:break</h2>
    <p>Breaks a FOR/FOR EACH/WHILE loop.</p>
<p>Syntax:</p>
<code class="html">&lt;:break/&gt;</code>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:break/&gt;</code>
        </td>
        <td>
            <code class="php">break;</code>
        </td>
    </tr>
    </tbody>
</table>

<h2 id=":continue" class="tag">:continue</h2>
    <p>Ignores code after within a FOR/FOR EACH/WHILE loop step.</p>
<p>Syntax:</p>
<code class="html">&lt;:continue/&gt;</code>
<p>Examples how this tag is compiled into PHP:</p>
<table class="compilation">
    <tbody>
    <tr>
        <td>
            <code class="html">&lt;:continue/&gt;</code>
        </td>
        <td>
            <code class="php">continue;</code>
        </td>
    </tr>
    </tbody>
</table>
