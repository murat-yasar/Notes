# Emmet Shortcuts

## Elements
> div>ul>li
```HTML
<div>
  <ul>
    <li> </li>
  </ul>
</div>
```

## Siblings
> div>h1+p
```HTML
<div>
  <h1></h1>
  <p></p>
</div>
```


## Upper Directories
> div+div>p>span+em^li
```HTML
<div></div>
<div>
  <p>
    <span></span>
    <em></em>
  </p>
  <li></li>
</div>
```


## Multiplication
> ul>li*3
```HTML
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

## Grouping
> div>(section>h2>p)+(section>ul>li*2)
```HTML
<div>
  <section>
    <h2>
      <p></p>
    </h2>
  </section>
  <section>
    <ul>
      <li></li>
      <li></li>
    </ul>
  </section>
</div>
```

## Id and Class
> div#header+div.page
```HTML
<div id="header"></div>
<div class="page"></div>
```

## Attributes
> td[title="Subject"]
```HTML
<td title="Subject"></td>
```

## Numbering
> ul>li.item$*3
```HTML
<ul>
  <li class="item1"></li>
  <li class="item2"></li>
  <li class="item3"></li>
</ul>
```
