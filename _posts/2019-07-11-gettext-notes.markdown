---
layout: post
title: "Python gettext notes"
date: 2019-07-11 10:36:20 -0400
categories: python gettext
---

# python本地化/国际化

最近的项目中需要把中文翻译成英文，由于文本是嵌入在代码里的，如果直接改掉会丢失原来的文本，并且减少了语言支持，所以考虑通过python的gettext类库实现多语言。

# 实现流程

1. 在代码中需要翻译的文本上添加标记。

由于pygettext脚本默认提取_()中的文本，考虑将需要翻译的文本用这个标记。

2. 通过pygettext脚本提取文本。

{% highlight bash %}
pygettext -d base -o local/test.pot test_1.py test_2.py
{% endhighlight %}

3. 以提取的文本文件为模版添加翻译文本。

```
msgid "你好，世界！"
msgstr "Hello World！"
```

4. 通过msgfmt脚本编译模版文件。

``` bash
msgfmt local/en_US/LC_MESSAGES/test.po -o local/en_US/LC_MESSAGES/test.mo
```

5. 在代码中添加相应的调用代码。

{% highlight python %}
import gettext
language = gettext.translation('test', localedir='local', languages=['en_US'])
language.install()
language = language.gettext
{% endhighlight %}