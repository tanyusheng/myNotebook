### webp自动转gif或者png

无论是gif或者png转为webp之后都会变为webp格式，这里根据webpinfo的Animation值判断是否为动图，并转为对应格式；

```shell
#!/bin/bash

mkdir png
mkdir gif
for f in *; do
  if [[ "${f##*.}" = "webp" ]]; then
    # 使用dwebp命令将webp文件转换为png格式
    # 执行webpinfo脚本，并将结果保存到变量output中
	output=$(webpinfo "$f")
	# 查询"Animation"键的值
	animation_value=$(echo "$output" | awk -F': ' '/Animation/ { print $2; exit }')
	# 打印查询结果
	echo "$f Animation value: $animation_value"
	if [[ "$animation_value" = "1" ]]; then
      webp2gif "$f" -o "./gif/${f%.*}.gif"
    elif [[ "$animation_value" = "0" ]]; then
      dwebp "$f" -o "./png/${f%.*}.png"
      echo "Converted $f to ${f%.*}.png"
    fi
    # /usr/local/bin/dwebp "$f" -o "./png/${f%.*}.png"
    # 
  fi
done
```

