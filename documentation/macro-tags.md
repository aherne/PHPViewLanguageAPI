**Macro tags** work in a way similar to C macros: before code is compiled, they are read and "expanded" so that compilation will run on a full source. Syntax is identical to that of normal HTML tags:

```html
<NAME ATTRIBUTE="value" .../>
```
Where:

- **NAME**: name of tag that performs a single logical operation.
- **ATTRIBUTE**: configures tag behavior

API defines following macro tags:

- [escape](#tag-escape): tag whose body will be ignored by compiler. This is necessary to mark content inside as not subject to parsing.
- [import](#tag-import): tag whose declaration will be replaced by compiler with the body of file pointed by its "file" attribute. This is crucial for layouting/templating. 
- [namespace](#tag-namespace): tag whose declaration will inform compiler where to look for tag libraries not found in default folder. 

At the moment, it is not possible for users to define their own macro tags!

## tag escape

Marks tag body to be ignored by ViewLanguage compiler.Syntax:

```html
<escape>
...
</escape>
```

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;escape&gt;<br/>${foo.bar}<br/>&lt;/escape&gt; | ${foo.bar} |

## tag import

Includes another view language template into current one. Syntax:

```html
<import file="..."/>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| file | Y | string | Location of file whose sources should replace tag declaration relative to views folder supplied to compiler |


Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;import file="header"/&gt; | require_once($viewsFolder."/header.html") |

## tag namespace

Marks custom location of user defined tag library (must be placed BEFORE latter declaration). Syntax:

```html
<namespace taglib="..." folder="..."/>
```

Attributes:

| Name | Mandatory | Data Type | Description |
| --- | --- | --- | --- |
| taglib | Y | string | Name of tag library to look for. |
| folder | Y | string | Location of folder tag library should be looked for relative to tags folder supplied to compiler |

Examples how this tag is compiled into PHP:

| ViewLanguage Example | PHP Translation |
| --- | --- |
| &lt;namespace taglib="foo" folder="bar"/&gt;<br/>...<br/>&lt;foo:baz attr="1"/&gt; | ...<br/>$contents = file_get_contents($tagsFolder."/bar/foo/baz.html"); <br/>// replaces attributes with values |

