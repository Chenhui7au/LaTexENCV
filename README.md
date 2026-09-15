# 简历模板

一个 LaTeX 简历模板。你在几个文本文件里填上自己的信息，就能生成一份排版整齐的 PDF 简历。

**推荐直接用 [Overleaf](https://www.overleaf.com) 在线编译**，不用装任何软件，把项目传上去就能出 PDF（操作方法见第四节）。

模板里现有的名字、邮箱、学校、论文都是**假的示例数据**，照着替换成你自己的就行。

<p align="center">
  <img src="asset/imgs/example.png" alt="简历模板效果预览" width="70%">
</p>

---

## 一、这个模板是怎么回事

一份简历分两部分：

- **外壳** —— 排版、字体、颜色、章节顺序。模板已经做好了，一般不用管。
- **内容** —— 你的名字、教育经历、工作经历、论文。这是你要填的。

模板把内容拆成了几个小文件，你只管往里面填字，不用碰排版代码。

---

## 二、文件都是干什么的

```
Template/
│
├── cv-main.tex       ← 你的名字、邮箱、照片、章节顺序
├── own-bib.bib       ← 你的论文列表
├── photo.jpg         ← 你的证件照
│
├── sections/         ← 各段经历的正文都在这里
│   ├── education.tex      教育经历
│   ├── employment.tex     工作经历
│   ├── publications.tex   论文列表
│   ├── project.tex        项目经历
│   ├── honors.tex         荣誉奖项
│   └── skills.tex         技能
│
└── README.md         本说明
```

**你只需要管上面列出的这几个文件** —— 其他文件是模板的运行部分，不用理会。

---

## 三、三步做出你的简历

### 第 1 步：填个人信息

打开 `cv-main.tex`，找到这一段，换成你自己的内容：

```latex
\leftheader{%
  % 姓名
  {\LARGE\bfseries\sffamily CH 7au} \\
  \\
  % 研究兴趣
  \textbf{Research Interest}: ...\\
  % 邮箱
  \textbf{E-mail}: \href{mailto: ch7au@my.gmail.com}{\texttt{ch7au@my.gmail.com}}
}
```

- 不需要的行直接删掉或注释掉
- 姓名下面那个单独的 `\\` 是占位换行，作用是让版面上留出一点空隙，删掉会变紧凑

**顺便打开「论文里加粗自己名字」的功能** —— 就在同一个文件往下几行：

```latex
\mynames{你的姓/你的名}
```

比如 `\mynames{Smith/John}`。只想写姓氏也行：`\mynames{Smith/}`。

### 第 2 步：填各段经历

打开 `sections/` 文件夹，每个文件对应简历上的一段，格式都一样：

```latex
\begin{rubric}{Education}

\entry*[2023 -- 2025]%
	\textbf{硕士, 某某大学}
    \par 某某学院，导师：某某某
    \par 平均分：88.19
%
\entry*[2019 -- 2023]%
	\textbf{学士, 某某大学}
    \par 某某学院
%
\end{rubric}
```

规律很简单：

- `\entry*[年份]` 起一条，方括号里写时间
- `\textbf{...}` 写粗体标题（学校名、公司名、项目名）
- 下面每多一行 `\par`，就多一行说明文字
- **两条经历之间空一行**，否则会粘在一起

⚠️ 记住这两个坑就够了：

| 遇到的问题 | 原因 | 怎么办 |
|---|---|---|
| 两条经历粘成一坨 | 中间没空行 | 两条之间**空一行** |
| 两条之间空太多 | 中间多空了行 | 想分隔就用单独一行 `%`，别空行 |

### 第 3 步：填论文（没有论文可跳过）

论文写在 `own-bib.bib` 里，一条一段：

```bibtex
@article{自己看得懂的编号,
  sortkey  = {1},
  title    = {论文标题},
  journal  = {期刊名},
  author   = {7au, CH and Smith, John and Brown, Alice},
  year     = {2025}
}
```

**作者的写法是「先姓后名，中间用逗号」** —— 比如本人是 `{7au, CH}`，合作者是 `{Smith, John}`。
作者按正常方式全部列出来（用 `and` 连接），显示到哪一位、要不要加 `et al.`，模板会自动处理。

只有两个地方需要你留意：

**① `sortkey` 填你是第几作者**

| 填写 | 含义 |
|---|---|
| `sortkey = {1}` | 你是第一作者 |
| `sortkey = {2}` | 你是第二作者 |

它管两件事：**决定同一年里哪篇排前面**（你署名越靠前排得越前），以及**让作者列表只显示到你自己为止**。

所以这里必须填**真实的署名顺序**，不能乱填。

**② 还没发表的论文，加一行 `keywords`**

```bibtex
  keywords = {ongoing},
```

加了这行的论文会自动归到「在投」那一节。等论文正式发表，**把这行删掉**，它就会自动移回「期刊论文」里。

**作者列表会自动截断**，只显示到你自己为止。截断词会根据你的位次自动选：

| 你是 | 共有几位作者 | 显示成 |
|---|---|---|
| 一作 | 1 位 | `C. 7au` |
| 一作 | 3 位 | `C. 7au et al.` |
| 二作 | 2 位 | `Smith and C. 7au` |
| 二作 | 3 位 | `Smith, C. 7au and others` |
| 三作 | 3 位 | `Smith, Brown and C. 7au` |
| 三作 | 5 位 | `Smith, Brown, C. 7au and others` |

规律很简单：

- **你是第一作者** → 后面还有作者就写 `et al.`（缩写，前面带逗号）
- **你不是第一作者** → 后面还有作者就写 `and others`（连词，前面不带逗号）
- **你已经是最后一位作者** → 用 `and` 正常连接，不加任何后缀

---

## 四、生成 PDF

用 [Overleaf](https://www.overleaf.com) 在线编译，不用装任何软件。

1. 打开 [Overleaf](https://www.overleaf.com)，注册或登录
2. 点 `New Project` → `Upload Project`
3. 把整个 `Template` 文件夹压缩成 zip，拖进去上传
4. 进项目后点左上角 `Menu`，在 `Main document` 里选 `cv-main.tex`
5. 点 `Recompile`，等几秒就有 PDF 了

之后每次改完文件，Overleaf 会自动重新编译。论文列表也会自动处理，不用你操心。

> 💡 打包前建议先把编译产生的中间文件删掉（`cv-main.aux`、`cv-main.bbl`、`cv-main.log` 等）。
> 它们不用上传，而且残留的旧 `cv-main.bbl` 可能让论文列表不更新。

**需要写中文时**（中文姓名、中文期刊名）：在 `Menu` → `Compiler` 里选 **XeLaTeX**，
并把 `cv-main.tex` 里 `\usepackage{ctex}` 前面那个 `%` 去掉。

---

## 五、调整版面

### 换照片

把 `photo.jpg` 换成你自己的照片，**文件名保持 `photo.jpg`** 就行。

不想要照片，在 `cv-main.tex` 里把这两行对调一下注释：

```latex
\includecomment{fullonly}   % ← 显示照片
% \excludecomment{fullonly} % ← 不显示照片
```

### 调整章节顺序

简历上各章节的先后，就是 `cv-main.tex` 里这几行的先后，上下挪动即可：

```latex
\makerubric{sections/education}
\makerubric{sections/employment}

\input{sections/publications}

\makerubric{sections/project}
\makerubric{sections/honors}
\makerubric{sections/skills}
```

- **不要某一段**：把对应那行注释掉
- **想让它另起一页**：在那行前面加 `\newpage`
- **想加新的一段**：在 `sections/` 里新建一个 `.tex` 文件，再在主文件里加一行 `\makerubric{sections/文件名}`

---

## 六、常见问题

**论文改了，但 PDF 里没变化？**
把项目里的 `cv-main.bbl` 删掉，再点 `Recompile`。

**编译报错，说论文列表是空的？**
确认 `Menu` → `Main document` 选的是 `cv-main.tex`，然后点 `Menu` → `Clear cached files`，再重新编译。

**加粗没生效？**
检查 `\mynames{...}` 里的拼写和论文里的姓名是否完全一致，末尾不要多空格。

**提示找不到 `sections` 里的文件？**
检查 `cv-main.tex` 里的写法：应该写成 `sections/education`，**不要加 `.tex`**。
