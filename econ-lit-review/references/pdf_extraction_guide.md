# PDF批量提取工具

用于批量提取经济学论文PDF的文本内容，以便核验。

## 使用方式

在 Stata/工作目录中运行以下 Python 脚本，将 PDF 目录下的所有文件提取为 txt：

```python
import sys, os
sys.stdout.reconfigure(encoding='utf-8')
from PyPDF2 import PdfReader

pdf_dir = r"PATH_TO_PDF_DIRECTORY"
out_dir = r"PATH_TO_OUTPUT_DIRECTORY"

for fname in os.listdir(pdf_dir):
    if not fname.endswith('.pdf'):
        continue
    path = os.path.join(pdf_dir, fname)
    reader = PdfReader(path)
    out_path = os.path.join(out_dir, fname.replace('.pdf', '_extracted.txt'))
    with open(out_path, 'w', encoding='utf-8') as f:
        for i, page in enumerate(reader.pages):
            text = page.extract_text()
            if text:
                f.write(f'--- Page {i+1} ---\\n{text}\\n\\n')
    print(f'{fname}: {len(reader.pages)} pages')
```

## 提取后发现乱码的处理

PDF 中文字体可能编码不全（如 GBK-EUC-H），导致提取文本乱码。此时：
1. 优先读取论文前 2-3 页摘要部分（通常编码正常）
2. 若整篇乱码，回到网页搜索结果中获取核心信息
3. 标记该文献为"PDF 乱码，内容来自网页核验"
