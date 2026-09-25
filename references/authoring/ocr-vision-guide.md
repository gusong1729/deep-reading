# OCR 与识图工具指南

精读遇到**扫描版 PDF、图片、图表、电路图**时使用本指南。两套工具分工明确：

| 工具 | 能力 | 适用 | 输出 |
|---|---|---|---|
| **OnnxOCR**（本地） | 文字识别（PP-OCRv5 中文） | 扫描古籍页、书籍封面文字、图表文字 | 文本框 + 识别文本 |
| **mmx vision describe**（MiniMax VLM） | 图像语义理解 | 电路图拓扑、公式结构、版面内容、图片内容描述 | 自然语言描述 |

**分工原则**：OnnxOCR 负责"认出文字"，mmx vision 负责"看懂内容"。扫描件先 OCR 提文字，仍不懂的图（电路图/示意图）再交给 vision 理解。

## 1. OnnxOCR（本地 OCR）

- **位置**：`D:\桌面归档\OnnxOCR`（PaddleOCR 中文，PP-OCRv5 通用模型）
- **运行环境**：系统 Python（非 .venv），`sys.path` 加入 OnnxOCR 目录

```python
import sys, cv2
sys.path.insert(0, r'D:\桌面归档\OnnxOCR')
from onnxocr.onnx_paddleocr import ONNXPaddleOcr

model = ONNXPaddleOcr(use_angle_cls=False, use_gpu=False)
img = cv2.imread('页面图片.png')
result = model.ocr(img, cls=False)   # [[[box, (text, conf)], ...], ...]
```

### 扫描版 PDF 处理流程

1. **先判断是否文本型**：pymupdf `page.get_text()` 有内容 → 直接提取，无需 OCR（省时省钱）
2. 文本型失败 / 扫描件 → 每页渲染为图：`page.get_pixmap(dpi=200)` → PNG
3. 逐页 OnnxOCR 识别 → 文本拼合
4. 识别结果质量差（古籍竖排/异体字）→ 配合 `mmx vision describe --prompt "Extract the text"` 复核

### 模型补充（按需）

```bash
cd D:\桌面归档\OnnxOCR
python scripts/download_models.py          # 国内 ModelScope 源
# python scripts/download_models.py --source huggingface   # 国际源
```
表格识别/版面分析/方向分类等扩展模型按需下载（test_ocr.py 中取消注释对应调用）。

## 2. mmx vision describe（MiniMax 识图）

**命令**（已全局安装 `mmx`，API key 已配置）：

```bash
mmx vision describe --image <图片路径或URL> --prompt "<问题>"
mmx vision describe --image 电路图.png --prompt "描述这个电路的拓扑结构：元件连接关系、电流走向"
mmx vision describe --image 古籍页.png --prompt "Extract the text" 
```

**精读场景示例**：
- 电路图：`--prompt "识别图中所有元件（电阻/电容/电感/电源）及其连接拓扑"`
- 古籍扫描页：`--prompt "提取页面全部文字，注意繁体与竖排"`
- 数据图表：`--prompt "描述图表内容：坐标轴、趋势、关键数值"`
- 书籍封面/版权页：`--prompt "提取书名、作者、出版社、版本信息"`

**注意事项**：
- 大图先压缩（最长边 ≤ 2048px）再传，速度快且省配额
- 竖排古籍 OnnxOCR 识别不了时，vision 是兜底
- `--region cn` 若默认 global 超时可用

## 3. 在精读流程中的位置

- **Step 1 解析层**：PDF/EPUB 中的图片、扫描页 → OCR/识图，把图片内容并入统一文本结构
- **Step 2 精读层**：方向模块有图（电工电路图、古籍影印、哲学书的示意图）时，用 vision 理解后写进精读对应位置
- **概念页**：图片类概念（电路图、图解）可附识别出的文本描述
