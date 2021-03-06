---
title: "Shotcodes"
date: 2022-04-14T11:53:22+08:00
description: "自定义的一些shortcodes"
draft: false
image: 'featured-image.jpg'
categories:
    - tech
tags:
    - stack
    - hugo
hidden: false
lastmod: 
---
主要参考[荷戟独彷徨](https://guanqr.com/tech/website/hugo-shortcodes-customization/)
## 定义`notice`
新建`layouts\shortcodes\notice.html`文件，添加如下代码
```html
<!-- 文件位置：~/layouts/shortcodes/notice.html -->

<!--https://github.com/martignoni/hugo-notice-->

{{- $noticeType := .Get 0 -}} {{- $raw := (markdownify .Inner | chomp) -}} {{- $block := findRE
    "(?is)^<(?:address|article|aside|blockquote|canvas|dd|div|dl|dt|fieldset|figcaption|figure|footer|form|h(?:1|2|3|4|5|6)|header|hgroup|hr|li|main|nav|noscript|ol|output|p|pre|section|table|tfoot|ul|video)\\b"
    $raw 1 -}} {{ $icon := (replace (index $.Site.Data.SVG $noticeType) "icon" "icon notice-icon") }}
    <div class="notice {{ $noticeType }}" {{ if len .Params | eq 2 }} id="{{ .Get 1 }}" {{ end }}>
      <div class="notice-title">{{ $icon | safeHTML }}</div>
      {{- if or $block (not $raw) }}{{ $raw }}{{ else }}
      <p>{{ $raw }}</p>
      {{ end -}}
    </div>
    
```

在自定义样式文件`custom.scss`中添加如下代码
```css
// 文件位置：~/assets/scss/custom/_custom.scss
.notice {
  position: relative;
  padding: 1em 1em 1em 2.5em;
  margin: 1em 0;
  border-radius: 4px;
  box-shadow: 0 6px 10px 0 alpha(var(--color-contrast-high), 0.15);
  p:last-child {
    margin: 0;
  }
  .notice-title {
    position: absolute;
    left: 0.8em;
    .notice-icon {
      width: 1.2em;
      height: 1.2em;
    }
  }
  &.error {
    background: hsla(0, 65%, 65%, 0.25);
    border-left: 5px solid hsl(0, 65%, 55%);
    word-break: break-all;
    .notice-title {
      color: hsl(0, 65%, 55%);
    }
  }
  &.warning {
    background: hsla(30, 80%, 70%, 0.25);
    border-left: 5px solid hsl(30, 80%, 55%);
    word-break: break-all;
    .notice-title {
      color: hsl(30, 80%, 55%);
    }
  }
  &.tip {
    background: hsla(200, 65%, 65%, 0.25);
    border-left: 5px solid hsl(200, 65%, 55%);
    word-break: break-all;
    .notice-title {
      color: hsl(200, 65%, 55%);
    }
  }
  &.success {
    background: hsla(140, 65%, 65%, 0.25);
    border-left: 5px solid hsl(140, 65%, 55%);
    word-break: break-all;
    .notice-title {
      color: hsl(140, 65%, 55%);
    }
  }
}

[data-theme='dark'] .notice {
  &.error {
    background: hsla(0, 29%, 41%, 0.65);
    border-left: 5px solid hsl(0, 67%, 50%);
    .notice-title {
      color: hsl(0, 67%, 50%);
    }
  }
  &.warning {
    background: hsla(28, 46%, 58%, 0.493);
    border-left: 5px solid hsl(30, 72%, 50%);
    .notice-title {
      color: hsl(30, 72%, 50%);
    }
  }
  &.tip {
    background: hsla(200, 56%, 55%, 0.548);
    border-left: 5px solid hsl(200, 74%, 57%);
    .notice-title {
      color: hsl(200, 74%, 57%);
    }
  }
  &.success {
    background: hsla(140, 35%, 44%, 0.603);
    border-left: 5px solid hsl(140, 73%, 55%);
    .notice-title {
      color: hsl(140, 73%, 55%);
    }
  }
}

```

最后同样需要在 `/data/SVG.toml` 文件中插入图标：
```
# 文件位置：~/data/SVG.toml

# Notice Icon
error = '<svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 576 512"><path d="M570 440c18 32-5 72-42 72H48c-37 0-60-40-42-72L246 24c19-32 65-32 84 0l240 416zm-282-86a46 46 0 100 92 46 46 0 000-92zm-44-165l8 136c0 6 5 11 12 11h48c7 0 12-5 12-11l8-136c0-7-5-13-12-13h-64c-7 0-12 6-12 13z"/></svg>'
warning = '<svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 512 512"><path d="M256 8a248 248 0 100 496 248 248 0 000-496zm0 110a42 42 0 110 84 42 42 0 010-84zm56 254c0 7-5 12-12 12h-88c-7 0-12-5-12-12v-24c0-7 5-12 12-12h12v-64h-12c-7 0-12-5-12-12v-24c0-7 5-12 12-12h64c7 0 12 5 12 12v100h12c7 0 12 5 12 12v24z"/></svg>'
tip = '<svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 512 512"><path d="M504 256a248 248 0 11-496 0 248 248 0 01496 0zm-248 50a46 46 0 100 92 46 46 0 000-92zm-44-165l8 136c0 6 5 11 12 11h48c7 0 12-5 12-11l8-136c0-7-5-13-12-13h-64c-7 0-12 6-12 13z"/></svg>'
success = '<svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 512 512"><path d="M504 256a248 248 0 11-496 0 248 248 0 01496 0zM227 387l184-184c7-6 7-16 0-22l-22-23c-7-6-17-6-23 0L216 308l-70-70c-6-6-16-6-23 0l-22 23c-7 6-7 16 0 22l104 104c6 7 16 7 22 0z"/></svg>'

```
使用效果
`{\{< notice error>}内容{\{< /notice >}}`
{{<notice error>}}十里青山远，潮平路带沙。数声啼鸟怨年华。又是凄凉时候，在天涯。白露收残月，清风散晓霞。绿杨堤畔问荷花。记得年时沽酒，那人家。{{</notice>}}
`{\{< notice tip >}内容{\{< /notice >}}`
{{<notice tip>}}十里青山远，潮平路带沙。数声啼鸟怨年华。又是凄凉时候，在天涯。白露收残月，清风散晓霞。绿杨堤畔问荷花。记得年时沽酒，那人家。{{</notice>}}

`{\{< notice warning >}内容{\{< /notice >}}`
{{<notice warning>}}十里青山远，潮平路带沙。数声啼鸟怨年华。又是凄凉时候，在天涯。白露收残月，清风散晓霞。绿杨堤畔问荷花。记得年时沽酒，那人家。{{</notice>}}

`{\{< notice success >}内容{\{< /notice >}}`
{{<notice success>}}十里青山远，潮平路带沙。数声啼鸟怨年华。又是凄凉时候，在天涯。白露收残月，清风散晓霞。绿杨堤畔问荷花。记得年时沽酒，那人家。{{</notice>}}


## 未完待续
