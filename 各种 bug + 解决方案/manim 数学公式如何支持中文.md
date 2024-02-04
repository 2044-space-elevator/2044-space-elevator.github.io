#manim 

## 如果你使用的 manimlib

manimlib 网上有很多解决方案了，本文侧重于那些使用 manim 库的朋友。

## manim 库的解决方案

如果你有 CTEX，那么最好。没有建议重装一个 CTEX。

翻到 `manim目录/utils/tex_templates.py`，第 14 行到第 22 行：

```python
def _new_ams_template():
    """Returns a simple Tex Template with only basic AMS packages"""
    preamble = r"""
\usepackage[english]{babel}
\usepackage{amsmath}
\usepackage{amssymb}
"""
    return TexTemplate(preamble=preamble)
```

修改为：


```python
def _new_ams_template():
    """Returns a simple Tex Template with only basic AMS packages"""
    preamble = r"""
\usepackage[english]{babel}
\usepackage{amsmath}
\usepackage{CTEX}
\usepackage{amssymb}
"""
    return TexTemplate(preamble=preamble)
```

`manim目录/utils/tex.py`，第  50 行开始：

```python
    default_documentclass = r"\documentclass[preview]{standalone}"
    default_preamble = r"""
\usepackage[english]{babel}
\usepackage{amsmath}
\usepackage{amssymb}
"""
    default_placeholder_text = "YourTextHere"
    default_tex_compiler = "latex"
    default_output_format = ".dvi"
    default_post_doc_commands = ""
```

修改为：

```python
    default_documentclass = r"\documentclass[preview]{standalone}"
    default_preamble = r"""
\usepackage[english]{babel}
\usepackage{amsmath}
\usepackage{CTEX}
\usepackage{amssymb}
"""
    default_placeholder_text = "YourTextHere"
    default_tex_compiler = "latex"
    default_output_format = ".dvi"
    default_post_doc_commands = ""
```

就此大功告成：

