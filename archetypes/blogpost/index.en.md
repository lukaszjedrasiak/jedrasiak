---
title: "{{replace .Name "-" " "}}"
tags:
- svarozyc
image:
    name: {{.Name}}
    extension: webp
    width: 1024
    height: 1024
readMore: true
publishDate: {{.Date | time.Format "2006-01-02"}}
---
**lead**
<!--more-->
{{<image src="{{.Name}}.webp" caption="{{replace .Name "-" " "}}" displayCaption="false">}}
