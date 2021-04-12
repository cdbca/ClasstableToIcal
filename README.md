# ClasstableToIcal
Convert Classtable to iCal using Pything and Excel as data source.

该工具可以方便地将课程表转换为 `.ics` 格式以导入各种设备的「日程」中。

> :warning: **注意**: 由于作者学校不再使用自建教务系统，该项目短期内不会再有进一步的功能开发。欢迎提交 PR 更新功能。

## Usage

以下为简略说明，更详细教程请参看[少数派](https://sspai.com/post/59694)。

先安装依赖：

```shell
pip install uuid xlrd 
```

然后执行 `main.py`：

```shell
python main.py
# or
python3 main.py
```

测试环境为：Python 3.7.2，Windows 10 x64.

## 文件中格式解释

其中，各个字段的含义为：

className - 课程名称
startWeek - 开始周数
endWeek - 结束周数
weekday - 课程日期（周几）
classTime - ​conf_classTime.json​ 中定义的时间段代号
classroom - 教室
weekStatus - 是否单双周排课：正常排课 = 0，单周排课 = 1，双周排课 = 2
classSerial - 可选，课程序号
classTeacher - 可选，教师名
最后两个可选字段如果不需要可以关闭，只需在 ​excel_reader.py​ 中的 27 和 28 行将：
`self.config["isClassSerialEnabled"] = [1, 7]self.config["isClassTeacherEnabled"] = [1, 8]`

后方的方框中，要关闭的功能的 1 改成 0 即可（即 [0, 7] 或 ​[0, 8]​）。

注意：若课程有不同排课方式或一周有多节课，需要分多条记录录入。



### temp_classInfo.xlsx

课程的名称、起始周数等在文件里已标示清楚，weekStatus 是单双周标记。

> 0=不分单双周，1=单周，2=双周

是否显示教师、是否开启单双周功能可在 `excel_reader.py` 中更改。

### conf_classTime.json

```json
"1": {
    "name": "第 1 节", 
    "startTime": "082000",
    "endTime": "095500"
}
```

该文件为 JSON 格式，一开始的数字是**时段编号**，对应 `temp_classinfo.xls` 里的 `classTime` 字段；`startTime` 与 `endTime` 采用 `%H%M%S` 格式，即时、分、秒去掉分隔符。

## Feature

现在支持：

- 单双周排课
- 课前n分钟提醒（待进一步测试）
- 不同教室（添加多个条目）
- 跨时段上课（[Contributed by @BoisV](https://github.com/SunsetYe66/ClasstableToIcal/pull/4)），现在定义上课时间段的方式改为开始时间id + 结束时间id，可以应对更复杂的时间需求

## License

LGPLv3
