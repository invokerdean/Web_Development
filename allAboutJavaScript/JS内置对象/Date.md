## 1. 实例化
```
//0.
var a=Date();//作为普通函数调用返回字符串，new返回对象

//1.
var a=new Date();//返回当前时间戳对象
//2.
var a=new Date(value);//接收一个UTC即距离1970.1.1-00:00:00的毫秒数
//3.
var a=new Date(dateString);//表示日期的字符串值。该字符串应该能被 Date.parse() 正确方法识别
//由于浏览器之间的差异与不一致性，强烈不推荐使用Date构造函数来解析日期字符串 (或使用与其等价的Date.parse)。
//4.
var a=new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
//传入参数大于2个时，返回对应日期的对象，除日期默认为1外其他默认为0，monthIndex为0-11（1-12月）

```
> 当Date作为构造函数调用并传入多个参数时，所定义参数代表的是当地时间。如果需要使用世界协调时 UTC，使用 new Date(Date.UTC(...)) 和相同参数。

## 2.属性
* Date.prototype：允许为 Date 对象添加属性。
* Date.length：Date.length 的值是 7。这是该构造函数可接受的参数个数。

## 3.静态方法
* Date.now():返回自 1970-1-1 00:00:00  UTC（世界标准时间）至今所经过的毫秒数。
* Date.parse():解析一个表示日期的字符串，并返回从 1970-1-1 00:00:00 所经过的毫秒数
* Date.UTC():接受和构造函数最长形式的参数相同的参数（从2到7），并返回从 1970-01-01 00:00:00 UTC 开始所经过的毫秒数。

## 4.Date.prototype方法
#### 本地时间
* Date.prototype.getFullYear()
> 根据本地时间返回指定日期对象的年份（四位数年份时返回四位数字）。

* Date.prototype.getMonth()
> 根据本地时间返回指定日期对象的月份（0-11）。

* Date.prototype.getDate()
> 根据本地时间返回指定日期对象的月份中的第几天（1-31）。
* Date.prototype.getDay()
> 根据本地时间返回指定日期对象的星期中的第几天（0-6）。

* Date.prototype.getHours()
> 根据本地时间返回指定日期对象的小时（0-23）。
* Date.prototype.getMinutes()
>根据本地时间返回指定日期对象的分钟（0-59）。
* Date.prototype.getSeconds()
> 根据本地时间返回指定日期对象的秒数（0-59）。
* Date.prototype.getMilliseconds()
> 根据本地时间返回指定日期对象的毫秒（0-999）。

#### UTC时间
* Date.prototype.getTime()
> 返回从1970-1-1 00:00:00 UTC（协调世界时）到该日期（会从本地时间转换成UTC，0时区时间）经过的毫秒数，对于1970-1-1 00:00:00 UTC之前的时间返回负值。
* Date.prototype.getUTCDate()
* Date.prototype.getUTCDay()
* Date.prototype.getUTCFullYear()
* Date.prototype.getUTCHours()
* Date.prototype.getUTCMilliseconds()
* Date.prototype.getUTCMinutes()
* Date.prototype.getUTCMonth()
* Date.prototype.getUTCSeconds()
* Date.prototype.getTimezoneOffset() 方法返回协调世界时（UTC）相对于当前时区的时间差值，单位为分钟。
## 5.奇淫技巧
获取每个月天数：
* 可以利用js中Date日期自动计算机制
```
//1.
function getDays(year, month) {
  if (month === 1) return new Date(year, month, 29).getMonth() === 1 ? 29 : 28;
  return new Date(year, month, 31).getMonth() === month ? 31 : 30;
}
```

* new Date()的第三个参数传小于1的值会怎么样了，比如传0，我们就获得了上个月的最后一天，当然传负数也没问题,-1即为倒数第二天
```
function getDays(year, month) {
  return new Date(year, month + 1, 0).getDate();
}
```

