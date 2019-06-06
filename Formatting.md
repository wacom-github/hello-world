# Formatting comparison

BB = Bitbucket
GH = Github

## Heading

Headings requires space separator in Github

##H1 (not a header in GH)
##H2 (not a header in GH)
## H3
## H4

---

## Table
Tables need balanced delimiters | in Github

Bad:

|Type         | SI unit   | Description               |
| :---        |    :----:   |          :---           |
|LENGTH        |	metres      |Length metric, used for coordinates.  |
|TIME          |seconds      |Time metric.       |
|FORCE         |Newton       |Force metric.
|ANGLE         |radians      |Angle.              |
|NORMALIZED    |             |Percentage, expressed as a fraction (1.0 = 100%). |
|LOGICAL_VALUE |             |Logical boolean value. |

Good:

|Type         | SI unit   | Description               |
| :---        |    :----:   |          :---           |
|LENGTH        |	metres      |Length metric, used for coordinates.  |
|TIME          |seconds      |Time metric.       |
|FORCE         |Newton       |Force metric. |
|ANGLE         |radians      |Angle.              |
|NORMALIZED    |             |Percentage, expressed as a fraction (1.0 = 100%). |
|LOGICAL_VALUE |             |Logical boolean value. |

---
## Graphics
Html characters not supported in BB: &#8593;

## Embedded images

- Github filename is case sensitive
- Relative paths handled differently in Typora/MarkdownEdit/Notepad++

(media/github.png) - preferred method
![description](media/github.png)

(/media/github.png)
![description](/media/github.png)

(./media/github.png)
![description](./media/github.png)
