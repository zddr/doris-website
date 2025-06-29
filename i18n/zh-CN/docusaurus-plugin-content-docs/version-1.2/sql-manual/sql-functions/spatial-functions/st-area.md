---
{
    "title": "ST_AREA",
    "language": "zh-CN"
}
---

## ST_Area_Square_Meters,ST_Area_Square_Km

### Syntax

```sql
DOUBLE ST_Area_Square_Meters(GEOMETRY geo)
DOUBLE ST_Area_Square_Km(GEOMETRY geo)
```

## 描述

计算地球球面上区域的面积，目前参数geo支持St_Point,St_LineString,St_Circle和St_Polygon。

如果输入的是St_Point,St_LineString，则返回零。

其中，ST_Area_Square_Meters(GEOMETRY geo)返回的单位是平方米，ST_Area_Square_Km(GEOMETRY geo)返回的单位是平方千米。

## 举例

```
mysql> SELECT ST_Area_Square_Meters(ST_Circle(0, 0, 1));
+-------------------------------------------------+
| st_area_square_meters(st_circle(0.0, 0.0, 1.0)) |
+-------------------------------------------------+
|                              3.1415926535897869 |
+-------------------------------------------------+
1 row in set (0.04 sec)

mysql> SELECT ST_Area_Square_Km(ST_Polygon("POLYGON ((0 0, 1 0, 1 1, 0 1, 0 0))"));
+----------------------------------------------------------------------+
| st_area_square_km(st_polygon('POLYGON ((0 0, 1 0, 1 1, 0 1, 0 0))')) |
+----------------------------------------------------------------------+
|                                                   12364.036567076409 |
+----------------------------------------------------------------------+
1 row in set (0.01 sec)

mysql> SELECT ST_Area_Square_Meters(ST_Point(0, 1));
+-------------------------------------------+
| st_area_square_meters(st_point(0.0, 1.0)) |
+-------------------------------------------+
|                                         0 |
+-------------------------------------------+
1 row in set (0.05 sec)

mysql> SELECT ST_Area_Square_Meters(ST_LineFromText("LINESTRING (1 1, 2 2)"));
+-----------------------------------------------------------------+
| st_area_square_meters(st_linefromtext('LINESTRING (1 1, 2 2)')) |
+-----------------------------------------------------------------+
|                                                               0 |
+-----------------------------------------------------------------+
1 row in set (0.03 sec)
```
### keywords
ST_Area_Square_Meters,ST_Area_Square_Km,ST_Area,ST,Area
