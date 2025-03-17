---
layout: post
title:  "Mac Office 对 Windows Office 中文字体不兼容的问题"
date: 2024-12-02 22:27:23 +0800
tags:  
slug: p20241202222723
---

# Mac Office 对 Windows Office 中文字体不兼容的问题

下载的 ttf 字体，或者是从windows 中导出的字体，当被装入Mac 系统 的 Font Book 以后，发现在Mac Office 提供的字体列表中，字体的名称不再是你在windows 中看到的 “方正仿宋\_GBK“  或  “方正仿宋\_GB2312“。

这是因为 Mac 操作系统把 ttf 字体的 metaData (元数据) 中的字段值的 en 部分直接提取出来，显示在了 Office 或者 FontBook App 中。而Windows 操作系统用的是metaData (元数据) 中的字段值的 cn 部分的名称。二者不对应，导致字体不兼容。

### 查看 ”元数据“ 的方法：

Mac Terminal 安装 `fontconfig`

```
brew install fontconfig
```

使用fc-query命令查看字体信息：

``````
fc-query /path/to/your/font.ttf
``````

可见，Windows Word 中的字体设置，采用的是字体的 cn 名称，Mac Fontbook/Word 中，采用的是字体的 en 名称。



### 需要利用python脚本，为 docx 中的字体添加 alt name

```
pip install lxml fonttools

```



1. 读取 ttf 文件中的en,cn 字体名称对应关系：

```python
# Parse font files and extract their family names
# parse fonts from a path that storing those font files (ttf and TTF)
# 遍历字体文件存放路径 如 /Users/user_name/Library/Fonts
# 遍历字体，提取所有语言版本中，字体名称。如： FZQiTi-S14S,方正启体简体

import os
from fontTools.ttLib import TTFont

def parse_ttf_fonts(font_dir):
    """
    遍历指定目录中的 TTF 文件，提取文件名、family 和 fullname 信息。
    
    :param font_dir: 字体文件存放路径
    """
    if not os.path.exists(font_dir):
        print(f"指定的路径不存在: {font_dir}")
        return
    
    print(f"正在遍历目录: {font_dir}")
    
    font_mapping = {}  # 存储字体名称映射的字典

    # 遍历目录中的 ttf 文件
    for root, _, files in os.walk(font_dir):
        for file in files:
            if file.endswith(".ttf") or file.endswith(".TTF"):
                ttf_path = os.path.join(root, file)
                try:
                    # 加载字体文件
                    font = TTFont(ttf_path)

                    # 提取 family 和 fullname 的所有语言版本
                    family_names = extract_font_names(font, name_id=1)  # family name (ID=1)
                    fullname_names = extract_font_names(font, name_id=4)  # fullname (ID=4)

                    print(f"文件名: {file}")

                    # 检查 family_names 是否有 2 个元素
                    if len(family_names) != 2:
                        print(f"警告: {file} 的 family 名称数量不等于 2 (实际: {len(family_names)})")
                        print(f"  family: {family_names}")
                    else:
                        # 如果是 2 个，则存入映射表
                        font_mapping[family_names[0]] = family_names[1]
                        print(f"  family: {family_names[0]}, {family_names[1]}")


                    print("-" * 50)
                except Exception as e:
                    print(f"无法解析字体文件 {file}: {e}")

    # 输出映射表
    if font_mapping:
        print("\n字体名称映射表（正反两个映射）:")
        for family, alt_family in font_mapping.items():
            print(f"{family},{alt_family}")
            print(f"{alt_family},{family}")
    else:
        print("没有找到符合条件的字体文件映射关系。")

def extract_font_names(font, name_id):
    """
    提取字体文件中指定名称 ID 的所有语言版本名称。
    
    :param font: TTFont 对象
    :param name_id: 字体名称表的 ID（如 family 为 1，fullname 为 4）
    :return: 包含所有语言版本名称的列表
    """
    names = set()
    for record in font["name"].names:
        if record.nameID == name_id:
            try:
                # 尝试解码为 Unicode
                name = record.string.decode(record.getEncoding())
            except UnicodeDecodeError:
                name = record.string.decode("utf-8", errors="ignore")
            names.add(name)
    return list(names)


if __name__ == "__main__":
    # 手动输入字体文件路径
    font_dir = input("请输入字体文件存放路径: ").strip()
    parse_ttf_fonts(font_dir)
```



2. 利用python 脚本为 docx 添加 字体名称对应关系：

   ```python
   
   # 为docx 文件添加备用字体，以适配不同操作系统上的字体。如“方正楷体_GBK”（windows）与“FZKai-Z03”（Mac）完成映射
   # 提前准备好要被本代码调用的映射表文件："altpairs.txt"  # 存放字体映射关系的文件
   # Usage：
   #    python add_alt_font_docx.py your_docx_file.docx
   
   import zipfile
   import os
   from lxml import etree
   import sys
   
   
   def main():
       # 判断命令行参数是否正确
       if len(sys.argv) < 2:
           print("用法：python add_alt_font_docx.py <docx文件路径>")
           sys.exit(1)
       
       # 从命令行获取 docx 文件的路径
       docx_path = sys.argv[1]
   
       # 这里如果需要输出到同一个文件或者相同目录，也可以直接把 output_path 设为 docx_path
       # output_path = docx_path
       base, ext = os.path.splitext(docx_path)
       output_path = f"{base}_patched{ext}"
   
       # 后续处理逻辑
       print(f"输入文档：{docx_path}")
       print(f"输出文档：{output_path}")
   
       # 加载存放字体映射关系的文件
       script_dir = os.path.dirname(os.path.abspath(__file__))
       alt_font_file = os.path.join(script_dir, "altpairs.txt")
       font_altname_dict = parse_alt_font_pairs(alt_font_file)
   
       # 检查是否解析成功
       if font_altname_dict:
           print("解析到以下字体替代名称映射：")
           for font, alt in font_altname_dict.items():
               print(f"{font} -> {alt}")
           print("------>>>-------")
           # 修改 .docx 文件
           modify_alt_names(docx_path, output_path, font_altname_dict)
       else:
           print("未能解析到有效的字体替代名称映射，请检查文本文件内容。")
   
   def parse_alt_font_pairs(file_path):
       """
       从文本文件中解析字体与替代名称的映射关系。
       :param file_path: 文本文件路径
       :return: 字典形式的字体映射关系 {font_name: alt_name}
       """
       font_altname_dict = {}
       try:
           with open(file_path, 'r', encoding='utf-8') as file:
               for line in file:
                   line = line.strip()
                   if line and "," in line:  # 忽略空行或无效行
                       font_name, alt_name = map(str.strip, line.split(",", 1))
                       font_altname_dict[font_name] = alt_name
       except FileNotFoundError:
           print(f"文件 {file_path} 未找到！")
       except Exception as e:
           print(f"解析文件时发生错误：{e}")
       return font_altname_dict
   
   def modify_alt_names(docx_path, output_path, font_altname_dict):
       import tempfile
       import shutil
       """
       为指定字体添加或强制修改 altName 标签。
       
       :param docx_path: 输入的 .docx 文件路径
       :param output_path: 输出的 .docx 文件路径
       :param font_altname_dict: 字体名称和替代名称的字典
       """
       # 解压 .docx 文件
       # extract_dir = "docx_contents"
       extract_dir = tempfile.mkdtemp(prefix="docx_contents_")
       try:
           # 解压 .docx 文件
           with zipfile.ZipFile(docx_path, 'r') as zip_ref:
               zip_ref.extractall(extract_dir)
   
           # 打开 fontTable.xml
           font_table_path = os.path.join(extract_dir, "word", "fontTable.xml")
           if not os.path.exists(font_table_path):
               print("警告：该 docx 文件中未找到 word/fontTable.xml，可能不包含字体定义。")
               return
   
           #开始处理
           tree = etree.parse(font_table_path)
           root = tree.getroot()
   
           # 定义命名空间
           namespaces = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}
   
           # 遍历字典，处理每个字体
           for font_name, alt_name in font_altname_dict.items():
               # 查找目标 <w:font w:name="font_name">
               font_elements = root.findall(f".//w:font[@w:name='{font_name}']", namespaces)
               if not font_elements:
                   print(f"字体 {font_name} 未找到！")
               else:
                   for font_element in font_elements:
                       # 检查是否已存在 <w:altName>
                       alt_name_element = font_element.find("w:altName", namespaces)
                       if alt_name_element is None:
                           # 添加 <w:altName>
                           alt_name_element = etree.Element("{http://schemas.openxmlformats.org/wordprocessingml/2006/main}altName")
                           alt_name_element.attrib["{http://schemas.openxmlformats.org/wordprocessingml/2006/main}val"] = alt_name
                           font_element.append(alt_name_element)
                           print(f"为字体 {font_name} 添加了 altName: {alt_name}")
                       else:
                           # 修改已存在的 <w:altName>
                           alt_name_element.attrib["{http://schemas.openxmlformats.org/wordprocessingml/2006/main}val"] = alt_name
                           print(f"字体 {font_name} 的 altName 已修改为: {alt_name}")
   
           # 保存修改后的文件
           tree.write(font_table_path, xml_declaration=True, encoding="utf-8", pretty_print=True)
   
           # 重新打包为 .docx 文件
           with zipfile.ZipFile(output_path, 'w', zipfile.ZIP_DEFLATED) as new_zip:
               for foldername, subfolders, filenames in os.walk(extract_dir):
                   for filename in filenames:
                       file_path = os.path.join(foldername, filename)
                       arcname = os.path.relpath(file_path, extract_dir)
                       new_zip.write(file_path, arcname)
   
       except Exception as e:
           print(f"处理 docx 文件时发生异常：{e}")
       finally:
           # 无论是否报错都清理临时目录
           shutil.rmtree(extract_dir, ignore_errors=True)
   
   # 主程序
   if __name__ == "__main__":
       main()
   
   
   ```

   
