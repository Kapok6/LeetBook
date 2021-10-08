## 语法

1. `.` 匹配除“换行”外的任何单个字符

2. `.*`任何字符0个或多个

3. `.?`是指任何字符0个或1个

4. `.*?`  匹配任意字符到下一个符合条件的字符

   ```js
   let reg = RegExp('.*?a') // 可以匹配 test..a
   ```

## 案例

1. mstr.match(/.+(?=(\.xlsx|\.xls)$)/)

2. 字符串模糊匹配：如'hellow word',使用'hw'可以匹配到

   ```js
   let query  = 'hw' // 检索项
   let standard = 'hello word' // 标准值
   query = query.split(""); 
   let str = "(.*?)"; // 匹配任意字符到下一个符合条件的字符
   let regStr = str + query.join(str) + str; // 以hw为例生成的正则表达式为/(.*?)h(.*?)w(.*?)/i，
   let reg = RegExp(regStr, "i"); 
   // 测试
   reg.test(standard) // true
   ```

   

   

