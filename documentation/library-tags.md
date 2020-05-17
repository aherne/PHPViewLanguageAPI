**Library tags** are compilable api/user-defined HTML snippets expected to implement scripting logic in a View Language application. They are non-static repeating snippets of template (html) code that depend on variables and thus can't be loaded using <include>.
 
Their syntax extends HTML standard:
```html
&lt;library:name attribute="value" ...&gt;...&lt;/library:tag&gt;
```
or, if they have no body:
```html
&lt;library:name attribute="value" .../&gt;
```

Where:
- *library*: namespace that contains related logical operations to perform on a template. Rules:
    - Value must be lowercase and alphanumeric.
    - "-" sign is allowed as well to replace spaces in multi-word values
- *name*: name of tag that performs a single logical operation.Rules:
    - Value must be lowercase and alphanumeric.
    - sign is allowed as well to replace spaces in multi-word values
- *attribute*: configures tag behavior. Rules:
    - Name must be lowercase and alphanumeric.
    - "_" sign is allowed as well to replace spaces in multi-word names
    - Value can only be primitive (string or number) or ViewLanguage expressions.
    - Unlike standard HTML, attributes cannot be multilined currently.
    
API includes a **standard library** containing tags for programming language instructions where *library* is empty:

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

Standard tags work with *attribute* values of following types:
- *scalars*: strings or integers
- *expressions*: ViewLanguage expressions. If helper functions are used as attribute values, they must be left undecorated: *count(${asd})* instead of *${count(${asd})}*.
- *conditions*: syntax is expected to be C-like, but ultimately matches that of language code is compiled into (in our case, PHP). Example: *${x}==true* means in PHP *$x==true*.

In order to break up HTML response into discrete units, developers must create their own libraries & tags. User defined tags are found on disk according to these rules:

- library name must be a folder inside tags folder supplied on compilation
- tag code muse be a HTML file inside library folder whose name equals *name*

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;foo:baz attr="1"/&gt; | $contents = file_get_contents($tagsFolder."/foo/baz.html"); <br/>// replaces attributes with values |

## tag :for
Creates a FOR loop.
Syntax:
```html
&lt;:for var="..." start="..." end="..." (step="...")&gt;
    ...
&lt;/:for&gt;
```
Attributes:
<table>
    <thead>
    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DEFAULT
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            var
        </td>
        <td>
            Name of counter variable.
        </td>
        <td>
            Y
        </td>
        <td>
            -
        </td>
        <td>
            STRING
        </td>
    </tr>
    <tr>
        <td>
            start
        </td>
        <td>
            Value of begin counter.
        </td>
        <td>
            Y
        </td>
        <td>
            -
        </td>
        <td>
            INTEGER
        </td>
    </tr>
    <tr>
        <td>
            end
        </td>
        <td>
            Value of end counter.
        </td>
        <td>
            Y
        </td>
        <td>
            -
        </td>
        <td>
            INTEGER
        </td>
    </tr>
    <tr>
        <td>
            step
        </td>
        <td>
            Value of increment/decrement step
        </td>
        <td>
            N
        </td>
        <td>
            1
        </td>
        <td>
            INTEGER
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:for var="i" start="0" end="10"&gt;
    ...
&lt;/:for&gt;
```
        </td>
        <td>
            ```php
for($i=0; $i&lt;=10; $i=$i+1){
    ...
}
```
        </td>
    </tr>
    <tr>
        <td>
            ```html
&lt;:for var="i" start="10" end="0" step="-2"&gt;
    ...
&lt;/:for&gt;
```
        </td>
        <td>
            ```php
for($i=10; $i&gt;=0; $i=$i-2) {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>

## tag :foreach
Creates a FOR EACH loop.

Syntax:
```html
&lt;:foreach var="..." (key="...") val="..."&gt;
    ...
&lt;/:foreach&gt;
```
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            var
        </td>
        <td>
            Variable to iterate.
        </td>
        <td>
            Y
        </td>
        <td>
            EXPRESSION
        </td>
    </tr>
    <tr>
        <td>
            key
        </td>
        <td>
            Name of key variable.
        </td>
        <td>
            N
        </td>
        <td>
            STRING
        </td>
    </tr>
    <tr>
        <td>
            val
        </td>
        <td>
            Name of value variable.
        </td>
        <td>
            Y
        </td>
        <td>
            STRING
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:foreach var="${a}" key="k" val="v"&gt;
    ...
&lt;/:foreach&gt;
```
        </td>
        <td>
            ```php
foreach($a as $k=&gt;$v) {
    ...
}
```
        </td>
    </tr>
    <tr>
        <td>
            ```html
&lt;:foreach var="${a}" val="v"&gt;
    ...
&lt;/:foreach&gt;
```
        </td>
        <td>
            ```php
foreach($a as $v) {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>

## tag :if
Creates an IF condition.
Syntax:
```html
&lt;:if test="..."&gt;
    ...
&lt;/:if&gt;
```
<p class="highlighted">Tag must not be closed if folowed by a [:else](#tag-#:elseif">:elseif</a> or <a href="#:else).
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            test
        </td>
        <td>
            Condition when body is executed.
        </td>
        <td>
            Y
        </td>
        <td>
            CONDITION
        </td>
    </tr>
    </tbody>
</table>

Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:if test="${x}==2"&gt;
    ...
&lt;/:if&gt;
```
        </td>
        <td>
            ```php
if($x==2) {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>
<i>You can also run simple IF/ELSE statements from expressions using ternary operators ([see above](#tag-#expressions))!</i>
## tag :elseif
Creates an ELSE IF condition.

Syntax:
```html
&lt;:elseif test="..."&gt;
    ...
&lt;/:if&gt;
```
<p class="highlighted">Tag must not be closed if folowed by a [:else](#tag-#:elseif">:elseif</a> or <a href="#:else).
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            test
        </td>
        <td>
            Condition when body is executed.
        </td>
        <td>
            Y
        </td>
        <td>
            CONDITION
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:elseif test="${x}==2"&gt;
    ...
&lt;/:if&gt;
```
        </td>
        <td>
            ```php
} else if ($x==2) {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>

## tag :else
Creates an ELSE condition.
Syntax:
```html
&lt;:else&gt;
    ...
&lt;/:if&gt;
```
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:else&gt;
    ...
&lt;/:if&gt;
```
        </td>
        <td>
            ```php
} else {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>
<i>You can also run simple IF/ELSE statements from expressions using ternary operators ([see above](#tag-#expressions))!</i>

## tag :set
Sets a value to a variable.
Syntax:
```html
&lt;:set var="..." val="..."/&gt;
```
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            var
        </td>
        <td>
            Name of variable to be created/updated.
        </td>
        <td>
            Y
        </td>
        <td>
            STRING
        </td>
    </tr>
    <tr>
        <td>
            val
        </td>
        <td>
            Value to set variable as.
        </td>
        <td>
            Y
        </td>
        <td>
            STRING | EXPRESSION
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:set var="a" val="10"/&gt;
```
        </td>
        <td>
            ```php
$a = "10";
```
        </td>
    </tr>
    </tbody>
</table>

## tag :unset
    Removes variable from memory.
Syntax:
```html
&lt;:unset var="..."/&gt;
```
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            var
        </td>
        <td>
            Name of variable to be unset.
        </td>
        <td>
            Y
        </td>
        <td>
            STRING
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:unset name="a"/&gt;
```
        </td>
        <td>
            ```php
unset($a);
```
        </td>
    </tr>
    </tbody>
</table>

## tag :while
    Creates a WHILE loop.
Syntax:
```html
&lt;:while test="..."&gt;
    ...
&lt;/:while&gt;
```
Attributes:
<table>
    <thead>

    <tr>
        <td>
            NAME
        </td>
        <td>
            DESCRIPTION
        </td>
        <td>
            MANDATORY
        </td>
        <td>
            DATA TYPE
        </td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>
            test
        </td>
        <td>
            Condition when body is executed.
        </td>
        <td>
            Y
        </td>
        <td>
            CONDITION
        </td>
    </tr>
    </tbody>
</table>
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:while test="${x}==2"&gt;
    ...
&lt;/:while&gt;
```
        </td>
        <td>
            ```php
while($x==2) {
    ...
}
```
        </td>
    </tr>
    </tbody>
</table>

## tag :break
    Breaks a FOR/FOR EACH/WHILE loop.
Syntax:
```html
&lt;:break/&gt;
```
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:break/&gt;
```
        </td>
        <td>
            ```php
break;
```
        </td>
    </tr>
    </tbody>
</table>

## tag :continue
    Ignores code after within a FOR/FOR EACH/WHILE loop step.
Syntax:
```html
&lt;:continue/&gt;
```
Examples how this tag is compiled into PHP:
<table class="compilation">
    <tbody>
    <tr>
        <td>
            ```html
&lt;:continue/&gt;
```
        </td>
        <td>
            ```php
continue;
```
        </td>
    </tr>
    </tbody>
</table>
