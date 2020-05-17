These are tags that work in a way similar to C macros: before code is compiled, they are read and "expanded" so that compilation will run on a full source. Syntax is identical to that of normal HTML tags:

```html
<NAME ATTRIBUTE="value" .../>
```
Where:

-<i>name</i>: name of tag that performs a single logical operation.
-<i>attribute</i>: configures tag behavior

API defines following macro tags:

- <a href="/view-language/macro-tags#escape">escape</a>: tag whose body will be ignored by compiler. This is necessary to mark content inside as not subject to parsing.
- <a href="/view-language/macro-tags#import">import</a>: tag whose declaration will be replaced by compiler with the body of file pointed by its "file" attribute. This is crucial for layouting/templating. 
- <a href="/view-language/macro-tags#namespace">namespace</a>: tag whose declaration will inform compiler where to look for tag libraries not found in default folder. 

At the moment, it is not possible for users to define their own macro tags!

## escape

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

## import

Includes another view language template into current one. Syntax:

```html
&lt;import file="..."/&gt;
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
            file
        </td>
        <td>
            Name of file whose sources should replace tag declaration. File will be located using extension & template folder supplied to compiler. When sources are parsed, same operation repeats recursively until no import tags are left.
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

## namespace

Marks custom location of user defined tag library. Syntax:

```html
&lt;namespace taglib="..." folder="..."/&gt;
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
             taglib
         </td>
         <td>
             Name of tag library to look for.
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
             folder
         </td>
         <td>
             Location on disk (absolute or relative to project) where to look for tags (files) in library.
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
