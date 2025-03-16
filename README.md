# markdownload + Gmeek 的 Web 摘要小站

很喜欢类似[https://clip.owenyoung.com/](https://clip.owenyoung.com/)的网站，希望将一些有趣的文章全文留在便于浏览的地方，但不要进bookmark，也不要进知识管理系统，就是单纯地留个记录。

也没有时间折腾，就利用现有的工具串一个workflow。再次感谢 [Meekdai](https://github.com/Meekdai) 和 [owen](https://github.com/theowenyoung) 。

## 安装
1. [非必须]在浏览器中安装 [Markdown Web Clipper](https://chromewebstore.google.com/detail/markdownload-markdown-web/pcmpcfapbekmbjjkdalcgopdkipoggdi) 
2. 在系统中安装 Racycast 和 gh 命令行工具，并login，参考[这里](https://github.com/suredream/gmeeklip/issues/1)生成一个 Script command
3. 按照这里的[步骤]安装 (https://github.com/Meekdai/Gmeek-templates) 到一个自己的 github 仓库中

## 收藏和发布流程
1. 浏览器浏览时，将内容用 Markdown Web Clipper 复制到剪贴板；或者在文本编辑里记下想要发布的内容，然后复制到剪贴板
2. 呼出 raycast，执行 Clipboard to GitHub Issue 命令，将剪贴板内容发布到为 github issues 中 
3. 让 github action 将内容转换为 post，并发布到 blog 中
