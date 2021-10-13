[TOC]

## 分类

1396 设计地铁系统 （中）：字典
355.设计推特 （待做）

## 总结

## 练习

### 1396 设计地铁系统

```python
class UndergroundSystem:

    def __init__(self):
        # {id：(站点，时间)}
        self.cur_route = {}
        #{起-止：[时间]}
        self.records = {}

    def checkIn(self, id: int, stationName: str, t: int) -> None:
        self.cur_route[id] = [stationName,t]

    def checkOut(self, id: int, stationName: str, t: int) -> None:
        s_station, time = self.cur_route[id][0],self.cur_route[id][1]
        c_key = s_station+'-'+stationName
        self.records.setdefault(c_key,[])
        self.records[c_key].append(t-time)

    def getAverageTime(self, startStation: str, endStation: str) -> float:
        c_key = startStation+'-'+endStation
        c_time = self.records[c_key]
        return ( sum(c_time)/len(c_time))
```
