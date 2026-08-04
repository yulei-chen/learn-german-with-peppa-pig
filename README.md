# 跟小猪佩奇学德语

每天看 Peppa Pig 学德语的笔记网页：德语台词 + 中文解释。

## 本地打开

直接用浏览器打开 `index.html` 即可，无需安装依赖。

```bash
open index.html
```

或起一个本地静态服务：

```bash
python3 -m http.server 8080
```

然后访问 http://localhost:8080

## 每天怎么加内容

在 `index.html` 的 `<section class="day">` 里，复制一段 `article.line` 模板，去掉编号直接粘贴新句子：

```html
<article class="line">
  <p class="de">德语原文。</p>
  <p class="zh">中文解释。</p>
  <ul class="tips">
    <li><strong>词组</strong> = 含义</li>
  </ul>
</article>
```

新的一集可以再复制一整个 `<section class="day">`。
