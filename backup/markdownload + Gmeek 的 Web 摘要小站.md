# markdownload + Gmeek 的 Web 摘要小站

很喜欢类似[https://clip.owenyoung.com/](https://clip.owenyoung.com/)的网站，希望将一些有趣的文章全文留在便于浏览的地方，但不要进bookmark，也不要进知识管理系统，就是单纯地留个记录。

也没有时间折腾，就利用现有的工具串一个workflow。再次感谢 [Meekdai](https://github.com/Meekdai) 和 [owen](https://github.com/theowenyoung) 。

## 安装
1. [非必要]在浏览器中安装 [Markdown Web Clipper](https://chromewebstore.google.com/detail/markdownload-markdown-web/pcmpcfapbekmbjjkdalcgopdkipoggdi) 
2. 在系统中安装 Racycast 和 gh 命令行工具，并login，参考以下的代码生成一个 Script command
3. 按照这里的[步骤]安装 (https://github.com/Meekdai/Gmeek-templates) 到一个自己的 github 仓库中

## 收藏和发布流程
1. 浏览器浏览时，将内容用 Markdown Web Clipper 复制到剪贴板；或者在文本编辑里记下想要发布的内容，然后复制到剪贴板
2. 呼出 raycast，执行 Clipboard to GitHub Issue 命令，将剪贴板内容发布到为 github issues 中 
3. 让 github action 将内容转换为 post，并发布到 blog 中


```
#!/bin/bash

# Required parameters:
# @raycast.schemaVersion 1
# @raycast.title Clipboard to GitHub Issue
# @raycast.mode compact
# @raycast.packageName GitHub

# Documentation:
# @raycast.description Create GitHub issue from clipboard content
# @raycast.author <author>
# @raycast.authorURL https://github.com/username

REPO=username/repo
LABELS=post

# 获取剪贴板内容
CLIPBOARD_CONTENT=$(pbpaste)
if [ -z "$CLIPBOARD_CONTENT" ]; then
  echo "❌ Error: Clipboard is empty"
  exit 1
fi

# 提取标题（第一行）
TITLE=$(echo "$CLIPBOARD_CONTENT" | head -n 1)
echo "准备创建issue..."
echo "仓库: $REPO"
echo "标题: $TITLE"

# 显示确认对话框
CONFIRMATION=$(osascript <<EOD
tell application "System Events"
  set theButton to button returned of (display dialog "Create a new issue in $REPO?\n\nTitle: $TITLE" buttons {"Cancel", "Create"} default button "Create")
  return theButton
end tell
EOD
)

echo "确认结果: $CONFIRMATION"

# 检查用户是否确认
if [ "$CONFIRMATION" = "Create" ]; then
  echo "用户已确认，正在创建issue..."
  
  # 创建临时文件用于存储内容
  TEMP_FILE=$(mktemp)
  echo "$CLIPBOARD_CONTENT" > "$TEMP_FILE"
  
  # 构建gh命令
  CMD="gh issue create --repo \"$REPO\" --title \"$TITLE\" --body-file \"$TEMP_FILE\""
  
  # 如果提供了标签，添加到命令中
  if [ -n "$LABELS" ]; then
    CMD="$CMD --label \"$LABELS\""
  fi
  
  echo "执行命令: $CMD"
  
  # 执行命令
  eval $CMD
  RESULT=$?
  
  # 删除临时文件
  rm "$TEMP_FILE"
  
  # 检查命令执行结果
  if [ $RESULT -eq 0 ]; then
    osascript -e 'display notification "GitHub issue created successfully!" with title "Raycast"'
    echo "✅ Issue created successfully!"
  else
    osascript -e 'display notification "Failed to create issue. Check terminal output." with title "Raycast"'
    echo "❌ Failed to create issue. Error code: $RESULT"
  fi
else
  echo "❌ Operation cancelled by user."
fi
```