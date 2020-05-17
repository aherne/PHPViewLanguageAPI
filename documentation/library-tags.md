**Library tags** are compilable api/user-defined HTML snippets expected to implement scripting logic in a View Language application. They are non-static repeating snippets of template (html) code that depend on variables and thus can't be loaded using <include>.
 
Their syntax extends HTML standard:
```html
<LIBRARY:TAG ATTRIBUTE="value" ...>...</LIBRARY:TAG>
```
or, if they have no body:
```html
<LIBRARY:TAG ATTRIBUTE="value" .../>
```

Where:
- *LIBRARY*: namespace that contains related logical operations to perform on a template. Rules:
    - Value must be lowercase and alphanumeric.
    - "-" sign is allowed as well to replace spaces in multi-word values
- *TAG*: name of tag that performs a single logical operation.Rules:
    - Value must be lowercase and alphanumeric.
    - sign is allowed as well to replace spaces in multi-word values
- *ATTRIBUTE*: configures tag behavior (can be zero or more). Rules:
    - Name must be lowercase and alphanumeric.
    - "_" sign is allowed as well to replace spaces in multi-word names
    - Value can only be primitive (string or number) or ViewLanguage expressions.
    - Unlike standard HTML, attributes cannot be multilined currently.
    
API includes a **standard library** containing tags for programming language instructions where *LIBRARY* is empty:

- [:for](#tag-for): iterates numerically through a list
- [:foreach](#tag-foreach): iterates through a dictionary by key and value
- [:if](#tag-if): evaluates body fitting condition
- [:elseif](#tag-elseif): evaluates body fitting condition that did not met previous if/elseif.
- [:else](#tag-else): evaluates body that did not met previous if/else if
- [:set](#tag-set): creates a variable and/or sets a value for it.
- [:unset](#tag-unset): unsets a variable.
- [:while](#tag-while): performs a loop on condition.
- [:break](#tag-break): ends loop.
- [:continue](#tag-continue): skips evaluating the rest of current loop and moves to next iteration.

Standard tags work with *ATTRIBUTE* values of following types:
- *scalars*: strings or integers
- *EXPRESSION*: ViewLanguage expressions. If helper functions are used as attribute values, they must be left undecorated: *count(${asd})* instead of *${count(${asd})}*.
- *CONDITION*: syntax is expected to be C-like, but ultimately matches that of language code is compiled into (in our case, PHP). Example: *${x}==true* means in PHP *$x==true*.

In order to break up HTML response into discrete units, developers must create their own libraries & tags. User defined tags are found on disk according to these rules:

- library name must be a folder inside tags folder supplied on compilation
- tag code muse be a HTML file inside library folder whose name equals *name*

| ViewLanguage Example | PHP Translation |
| --- | --- |
| <foo:baz attr="1"/> | $contents = file_get_contents($tagsFolder."/foo/baz.html"); <br/>// replaces attributes with values |

## tag :for

Creates a FOR loop. Syntax:

```html
<:for var="..." start="..." end="..." (step="...")>
    ...
</:for>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| var | Y | string | Name of counter variable. |
| start | Y | integer | Value of begin counter. |
| end | Y | integer | Value of end counter. |
| step | N | integer | Value of increment/decrement step (default: 1). |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:for var="i" start="0" end="10"&gt;<br/>...<br/>&lt;/:for&gt; | for($i=0; $i<=10; $i=$i+1){<br/>...<br/>} |
| &lt;:for var="i" start="10" end="0" step="-1"&gt;<br/>...<br/>&lt;/:for&gt; | for($i=10; $i>=0; $i=$i-1){<br/>...<br/>} |

## tag :foreach

Creates a FOR EACH loop. Syntax:

```html
<:foreach var="..." (key="...") val="...">
    ...
</:foreach>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| var | Y | EXPRESSION | Variable to iterate. |
| key | N | string | Name of key variable. |
| val | Y | string | Name of value variable. |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:foreach var="${a}" key="k" val="v"&gt;<br/>...<br/>&lt;/:foreach&gt; | foreach($a as $k=>$v) {<br/>...<br/>} |
| &lt;:foreach var="${a}" val="v"&gt;<br/>...<br/>&lt;/:foreach&gt; | foreach($a as $v) {<br/>...<br/>} |


## tag :if

Creates an IF condition. Syntax:

```html
<:if test="...">
    ...
</:if>
```

Tag must not be closed if folowed by a [:else](#tag-else) or [:elseif](#tag-elseif)!

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| test | Y | CONDITION | Condition when body is executed. |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:if test="${x}==2"&gt;<br/>...<br/>&lt;/:if&gt; | if($x==2) {<br/>...<br/>} |

<i>You can also run simple IF/ELSE statements from expressions using ternary operators!</i>

## tag :elseif

Creates an ELSE IF condition. Syntax:

```html
<:elseif test="...">
    ...
</:if>
```

Tag must not be closed if folowed by a [:else](#tag-else) or [:elseif](#tag-elseif)!

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| test | Y | CONDITION | Condition when body is executed. |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:elseif test="${x}==2"&gt;<br/>...<br/>&lt;/:if&gt; | } elseif($x==2) {<br/>...<br/>} |

## tag :else

Creates an ELSE condition. Syntax:

```html
<:else>
    ...
</:if>
```

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:else&gt;<br/>...<br/>&lt;/:if&gt; | } else {<br/>...<br/>} |

## tag :set

Sets a value to a variable.Syntax:

```html
<:set var="..." val="..."/>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| var | Y | string | Name of variable to be created/updated. |
| val | Y | string<br/>EXPRESSION | Value of variable. |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:set var="a" val="10" | $a = 10; |
| &lt;:set var="a" val="${x}" | $a = $x; |

## tag :unset

Removes variable from memory. Syntax:

```html
<:unset var="..."/>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| var | Y | string | Name of variable to be unset. |


Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:unset var="a" | unset($a); |

## tag :while

Creates a WHILE loop. Syntax:

```html
<:while test="...">
    ...
</:while>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| test | Y | CONDITION | Condition when body is executed. |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:while test="${x}!=2"&gt;<br/>...<br/>&lt;/:while&gt; | while($x!=2) {<br/>...<br/>} |

## tag :break

Breaks a FOR/FOR EACH/WHILE statement loop. Syntax:

```html
<:break/>
```

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:break/:while&gt; | break; |

## tag :continue

Continues to next step within a FOR/FOR EACH/WHILE statement loop. Syntax:

```html
<:continue/>
```

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;:break/:while&gt; | break; |