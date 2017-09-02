# tablereport

![Python Version](https://img.shields.io/badge/python-2.7,3.4,3.5,3.6-blue.svg)
[![Build Status](https://travis-ci.org/jefffffrey/tablereport.svg?branch=master)](https://travis-ci.org/jefffffrey/tablereport)
[![Coverage Status](https://coveralls.io/repos/github/jefffffrey/tablereport/badge.svg?branch=master)](https://coveralls.io/github/jefffffrey/tablereport?branch=master)
[![Code Health](https://landscape.io/github/jefffffrey/tablereport/master/landscape.svg?style=flat)](https://landscape.io/github/jefffffrey/tablereport/master)

A python library for making table report. Now supports exporting to Excel.

## Example

### Basic



![](docs/images/basic.png)

```python
from tablereport import Table
from tablereport.shortcut import write_to_excel

table = Table(header=[['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4]])

write_to_excel('basic.xlsx', table)
```

### Style

![](docs/images/style.png)

```python
from tablereport import Table, Style
from tablereport.shortcut import write_to_excel

table_style = Style({
    'background_color': 'fff0f0f0',
})
title_style = Style({
    'background_color': 'ff87cefa',
    'font_weight': 'blod'
}, extend=table_style)

header_style = Style({
    'background_color': 'ff87cefa',
}, extend=table_style)
table = Table(header=[[('TEST', title_style), None, None, None],
                      [('HEADER1', header_style),
                       ('HEADER2', header_style),
                       ('HEADER3', header_style),
                       ('HEADER4', header_style)]],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4]],
              style=table_style)

write_to_excel('style.xlsx', table)
```

### Column Selector

![](docs/images/column_selector.png)

```python
from tablereport import Table, ColumnSelector, Style
from tablereport.shortcut import write_to_excel

table = Table(header=[['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4]])

style = Style({
    'background_color': 'fff0f0f0',
})
areas = table.body.select(ColumnSelector(lambda col: col % 2))
areas.set_style(style)
write_to_excel('column_selector.xlsx', table)
```

### Merge

![](docs/images/merge.png)

```python
from tablereport import Table, ColumnSelector
from tablereport.shortcut import write_to_excel

table = Table(header=[['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4]])

column = table.body.select(ColumnSelector(lambda col: col == 1)).one()
column.merge()
write_to_excel('merge.xlsx', table)
```

### Group

![](docs/images/group.png)

```python
from tablereport import Table, ColumnSelector
from tablereport.shortcut import write_to_excel

table = Table(header=[['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4],
                    ['Two', 'A', 1, 2],
                    ['Two', 'B', 2, 3]])

column = table.body.select(ColumnSelector(lambda col: col == 1)).one()
column.group().merge()
write_to_excel('group.xlsx', table)
```

### Summary

![](docs/images/summary.png)

```python
from tablereport import Table, Style
from tablereport.shortcut import write_to_excel

table = Table(header=[['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4]])

style = Style({
    'background_color': 'ffe6e6e6',
})
table.body.summary(label='Total', label_span=2, label_style=style,
                   value_style=style)
write_to_excel('summary.xlsx', table)
```

### Complex

![](docs/images/complex.png)

```python
from tablereport import Table, ColumnSelector, Style
from tablereport.shortcut import write_to_excel

title_style = Style({
    'font_size': 15,
    'background_color': 'ff87cefa',
    'font_weight': 'blod'
})

header_style = Style({
    'background_color': 'ff87cefa',
})

left_total_style = Style({
    'background_color': 'fff0f0f0',
})

bottom_total_style = Style({
    'background_color': 'ffe6e6e6',
})

table = Table(header=[['TEST', None, None, None],
                      ['HEADER1', 'HEADER2', 'HEADER3', 'HEADER4']],
              body=[['One', 'A', 1, 2],
                    ['One', 'A', 2, 3],
                    ['One', 'B', 3, 4],
                    ['Two', 'A', 1, 2],
                    ['Two', 'B', 2, 3]])

table.header[0].set_style(title_style)
table.header[1].set_style(header_style)

column = table.body.select(ColumnSelector(lambda col: col == 1)).one()
column.group().merge().left.summary(label_span=1, label='Total',
                                    label_style=left_total_style,
                                    value_style=left_total_style)

table.summary(label_span=2, label='Total',
              label_style=bottom_total_style,
              value_style=bottom_total_style)

write_to_excel('complex.xlsx', table)
```