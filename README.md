# 河南大学嵌入式系统实验报告 LaTeX 模板

本模板面向 **河南大学非全日制嵌入式专业《嵌入式系统》课程**，供后续同学编写实验报告使用。

模板参考原课程作业仓库中的 LaTeX 排版方式重新整理，只保留最容易理解的结构：**一个 LaTeX 源文件和一个图片文件夹**。不需要理解多文件引用，也不需要修改 `.sty` 样式文件。

## 文件结构

```text
.
├── embedded_report.tex    # 唯一需要打开和修改的 LaTeX 源文件
├── embedded_report.pdf    # 已编译的模板效果预览
├── image/
│   ├── henan.png           # 封面使用的原始河南大学校徽
│   └── henan-watermark.png # 已预置透明度的水印图片，请勿删除
├── README.md              # 使用说明
├── .gitignore
└── LICENSE
```

你自己拍摄的实验照片、原理图、流程图、串口截图和波形图也统一放进 `image/` 文件夹。

## 模板效果

- 封面顶部显示河南大学校徽；
- 封面不显示中央水印，目录及后续页面显示透明度适中的河南大学校徽水印；
- 自动生成封面、目录、页眉、页脚和页码；
- 支持中文、公式、三线表、图片、源代码和参考文献；
- 模板源码中附带大量中文注释，按顺序阅读即可完成修改；
- 示例内容已由“单片机原理”改为通用的“嵌入式系统实验报告”。

## 第一次使用

### 方法一：使用 Overleaf（无需在电脑安装 LaTeX）

1. 将本项目下载为 ZIP；
2. 登录 [Overleaf](https://www.overleaf.com/)；
3. 选择“新建项目 → 上传项目”，上传 ZIP；
4. 打开项目设置，将编译器改为 **XeLaTeX**；
5. 将主文档设为 `embedded_report.tex`；
6. 点击“重新编译”。

### 方法二：在本地使用

推荐安装：

- TeX Live；
- Visual Studio Code；
- VS Code 扩展 LaTeX Workshop。

安装后用 VS Code 打开整个文件夹，再打开 `embedded_report.tex`。选择 XeLaTeX 或 `latexmk (xelatex)` 编译。

也可以在该目录的终端中运行：

```powershell
xelatex embedded_report.tex
xelatex embedded_report.tex
```

运行两次是为了更新目录、页码和交叉引用。更推荐：

```powershell
latexmk -xelatex embedded_report.tex
```

生成的 PDF 为 `embedded_report.pdf`。

> 必须使用 **XeLaTeX**。不要使用 pdfLaTeX，否则中文字体可能报错。

## 修改步骤

### 1. 修改封面信息

打开 `embedded_report.tex`，找到：

```tex
% 第一部分：报告基本信息（每次实验主要修改这里）
```

修改花括号内的实验题目、姓名、学号、班级和教师等信息，例如：

```tex
\newcommand{\ExperimentTitle}{实验二：串口通信}
\newcommand{\StudentName}{张三}
\newcommand{\StudentID}{202600000000}
```

不要删除命令前面的反斜杠，也不要删除成对的花括号。

### 2. 修改正文

继续向下找到 `\section{实验目的}`。模板已给出完整的示例结构和写作提示，请将示例文字替换为本人的真实实验内容。

建议保留这些基本部分：

1. 实验目的；
2. 实验环境；
3. 实验原理；
4. 系统设计；
5. 实验步骤与程序实现；
6. 实验结果与分析；
7. 实验总结；
8. 参考文献和附录（按需要保留）。

如果老师给出了不同的报告结构，直接修改 `\section{...}` 内的标题即可。

### 3. 插入自己的图片

将图片放入 `image/`，例如：

```text
image/uart-waveform.png
```

然后在正文中写：

```tex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.8\textwidth]{uart-waveform.png}
  \caption{串口发送波形}
  \label{fig:uart-waveform}
\end{figure}
```

正文中引用这张图：

```tex
由\cref{fig:uart-waveform}可知……
```

建议图片文件名只使用小写英文、数字和连字符，不要包含空格。截图应裁掉无关窗口，并确保 PDF 放大后文字仍然清楚。

### 4. 插入代码

```tex
\begin{lstlisting}[language=C,caption={串口初始化程序},label={lst:uart}]
void uart_init(void) {
    /* 在这里放置本人的代码 */
}
\end{lstlisting}
```

正文中不要堆放大量无关代码。关键代码放在对应章节并解释，完整程序可放在报告附录。

## 校徽与水印

`image/henan.png` 来自参考仓库的 LaTeX 图片目录，用于封面顶部校徽；`image/henan-watermark.png` 是由该校徽生成的预透明水印，用于目录及后续页面。

请不要删除或重命名这两张图片。`image/henan-watermark.png` 已经在 PNG Alpha 通道中预置透明度，因此源码不再使用 TikZ 的 `opacity`。这样可以避免 XeLaTeX 在首次使用透明资源时将水印错误显示为完全不透明。

封面完成后才启用水印，所以第一页只保留顶部小校徽；目录及后续页面显示中央水印。如需调整透明度，建议重新生成水印图片，而不是在 LaTeX 中添加 `opacity`。

## 常见问题

### 中文乱码或字体报错

确认编译器为 XeLaTeX，并确认 `.tex` 文件保存为 UTF-8 编码。

### 目录或引用显示为 `??`

再编译一次，或者使用：

```powershell
latexmk -xelatex embedded_report.tex
```

### 图片找不到

检查以下内容：

- 图片确实位于 `image/`；
- 源码中的文件名和扩展名完全一致；
- 不要把 `.jpg` 错写成 `.png`；
- Linux 和 Overleaf 区分文件名大小写。

### 校徽水印不显示

检查 `image/henan.png` 和 `image/henan-watermark.png` 是否存在，并确认没有修改文件名或目录名。

### 注释会出现在 PDF 中吗

不会。以 `%` 开头的内容只是给同学阅读的说明。请不要随意删除这些注释，它们可以帮助后续使用者理解模板。

## 提交前检查

- [ ] 已替换示例姓名、学号、班级、教师和实验题目；
- [ ] 已将示例内容替换为本人的真实实验内容；
- [ ] 实验环境中的开发板、芯片、软件和版本填写准确；
- [ ] 图片来自本人实验，清晰且有标题；
- [ ] 代码与本人实际运行的工程一致；
- [ ] 有实验数据、理论分析和问题解决过程；
- [ ] 目录、页码、交叉引用和水印显示正常；
- [ ] 最终 PDF 中没有不应公开的账户或设备敏感信息。

## 许可

模板代码采用 [MIT License](LICENSE)。河南大学校徽及其他名称、标识的权利归其相应权利人所有，本模板仅用于课程教学和实验报告排版。




