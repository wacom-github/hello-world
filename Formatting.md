# Formatting comparison

BB = Bitbucket
GH = Github

## Heading

Headings requires space separator in Github

##H1
##H2
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

## Embedded images

- Github filename is case sensitive
- Relative paths handled differently in Typora/MarkdownEdit/Notepad++

(images/github.png) - preferred method
![description](images/github.png)

(/images/github.png)
![description](/images/github.png)

(./images/github.png)
![description](./images/github.png)
