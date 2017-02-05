---
title: Hexo
copyright: true
date: 2016-09-27 01:55:01
tags: Hexo
---

> 一个快速、简洁且高效的博客框架
<!-- more -->


## 常用简写
```bash
# hexo new
hexo n
# hexo generate
hexo g
# hexo server
hexo s
# hexo deploy
hexo d
# 生成部署
hexo d -g
# 生成预览
hexo g -s
```

## 添加文末版权声明
### 添加以下文件
- blog/scripts/add-tail.js
- blog/tail.html

### add-tail.js
```js
/**
 * 此脚本需要在文章头部设置 copyright: true
 */
var fs = require('fs')

hexo.extend.filter.register('before_post_render', function (data) {
	if (!data.copyright) return data

	// Add seperate line
	// 注意这里需要用两个换行确保分割线和前面部分完全隔离，避免前面部分被解析为子标题
	data.content += '\n\n---\n'

	// Try to read tail.md
	try {
		var file_content = fs.readFileSync('tail.html')
		if (file_content && data.content.length > 300) {
			data.content += file_content
		}
	} catch (e) {
		if (e.code !== 'ENOENT') throw e
		// No process for ENOENT exception
	}

	var declaration = '<div style="display: flex; justify-content: center; font-size: .8rem;"><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">' + data.title + '</span>'

	declaration += ' 由 <a xmlns:cc="http://creativecommons.org/ns#" href="' + data.permalink + '" property="cc:attributionName" rel="cc:attributionURL">Devin Zhong</a> 创作，'

	declaration += ' 采用 <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/deed.zh">知识共享 署名-相同方式共享 4.0 国际 许可协议</a> 进行许可。</div>'

	data.content += declaration

	return data
})
```

### tail.html
```html
<div style="display: flex; justify-content: center; text-decoration: none;">
  <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/deed.zh">
    <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" />
  </a>
</div>
```

## 常见错误解决方案

### 双大括号解析错误
此错误常见于行内代码
```md
`{% raw %}含有双大括号的内容{% endraw %}`
```