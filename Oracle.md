### 分页

oracle 的分页需要额外用 <font color='red'>ROWNUM</font> 列，数据库特性自带。

~~~sql
        SELECT s.*
        FROM (SELECT ROWNUM rn,t.*
              FROM student t
              where ROWNUM <= #{edge}
             ) s
        WHERE s.rn > #{offset}
~~~

先用一次子查询获取 row num 列，并且获取了 0-edge 行数据

最外查询设置偏移量 offset

那么返回的数据行只有 0ffset - edge



### 分组排序

假设审核日志，获取某个东西审核日志最新的一条



①原表效果：**外表主键，时间（VISIT_DATE）不同**

![img](assets/fsdf11111) 



②分组排序后取第一条的效果：**【实现对编号去重，且取访问时间最新的一条数据】**

![image-20221112030726337](assets/image-20221112030726337.png)



~~~sql
SELECT * 
FROM(
    SELECT ROW_NUMBER()over(PARTITION by 分组列 ORDER BY 排序列 DESC) as RN[,VI.分组列,VI.排序列]
    FROM ~tableName~ VI
) dual  WHERE RN=1
~~~





### 字符

~~~sql
# 查看oracle server端字符集
select userenv('language') from dual;

# 查看表的字符集
select distinct(userenv('language')) from tableName;
~~~

如果显示 **SIMPLIFIED CHINESE_CHINA.ZHS16GBK** ，一个汉字占用两个字节

如果显示 **SIMPLIFIED CHINESE_CHINA.AL32UTF8**， 一个汉字占用三个字节

可以用以下语句查询一个汉字占用的字节长度

```sql
select lengthb('你') from dual;
```





# Group By







# NVL

判空

```sql
NVL(expression, value_if_null)
NVL2(expression, value_if_not_null, value_if_null)
```



# Decode

Oracle 的 `DECODE` 函数是用于根据给定的表达式和一系列比较值进行条件判断并返回对应的结果。在你的代码中，使用了 `bjfp.cprs = bjfp.fprs` 作为表达式，这将返回一个布尔值（TRUE 或 FALSE）。然后，你试图将这个布尔值作为 `DECODE` 函数的表达式，这是不正确的。`DECODE` 函数的表达式应该是一个具体的值或列。

如果你想要将 `bjfp.cprs` 和 `bjfp.fprs` 的比较结果转换为 1 或 0，你可以使用 `CASE` 表达式来实现：

```sql
SELECT CASE WHEN bjfp.cprs = bjfp.fprs THEN 1 ELSE 0 END AS comparison_result
FROM bjfp;
```



# LISTAGG（分组聚合）

```sql
SELECT
  department,
  LISTAGG(employee, ', ') WITHIN GROUP (ORDER BY employee) AS employees
FROM
  employees_table
GROUP BY
  department;
```

将每个部门的员工姓名连接成一个字符串，并使用逗号和空格作为分隔符。WITHIN GROUP子句用于指定连接的顺序。



# INSTR（子串位置）

返回子串在字符串出现的位置

~~~sql
SELECT instr(str, subStr) FROM dual
~~~





# Single-Row Functions（单行函数）

单行函数为查询的表或视图的每一行返回单个结果行。这些函数可以出现在SELECT列、WHERE子句、START WITH和CONNECT BY子句以及HAVING子句中。



## 数字函数

数值函数接受数值输入并返回数值。大多数数值函数返回精确到38位十进制数字的NUMBER值。超越函数COS、COSH、EXP、LN、LOG、SIN、SINH、SQRT、TAN、TANH精确到36位十进制数字。超越函数ACOS、ASIN、ATAN和ATAN2精确到30位十进制数字。数值函数为:

- [ABS](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ABS.html#GUID-D8D3489A-44EA-4FEC-A6F0-B5E312FFC231)

- [ACOS](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ACOS.html#GUID-B4C70DD5-B908-4130-975A-6CFD5C1AC1F9)

- [ASIN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ASIN.html#GUID-809ACB4E-9FDA-4943-B234-DDB32522A523)

- [ATAN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ATAN.html#GUID-12E8F1AA-54D0-4A19-8648-27094946C588)

- [ATAN2](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ATAN2.html#GUID-D34E671B-F3C0-4390-A2D8-ABB702B4B5D3)

- [BITAND](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/BITAND.html#GUID-EADBED75-6AC5-4FBE-991A-E3B4D260F73B)

- [CEIL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CEIL.html#GUID-6DCC9AFB-9B80-4C27-AF63-5AA3B1E43660) -向上取整

- [COS](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/COS.html#GUID-C008F067-C6DC-4C13-9B7F-5A385415363A)

- [COSH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/COSH.html#GUID-A48CD625-5238-4259-9A1F-0FDBFD19841E)

- [EXP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/EXP.html#GUID-414FB4AE-03B5-41AD-AE33-E3755EFED0A0)

- [FLOOR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/FLOOR.html#GUID-67F61AC7-C097-4397-A122-213157BF584F)- 向下取整

- [LN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LN.html#GUID-DCC9EDAA-D308-4145-8E05-8D06A5EF5F6F)

- [LOG](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LOG.html#GUID-3739F356-A4A0-4D0D-A4EB-9725ACA05CD1)

- [MOD](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/MOD.html#GUID-E12A3928-2C50-45B0-B8C3-82432C751B8C) - 返回两数相除的余数

- [NANVL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NANVL.html#GUID-3C094646-2A70-41F5-984C-9BC0FB31494A) - 处理NVN（非法数字）

- [POWER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/POWER.html#GUID-D280B322-D2C3-46D0-8076-C88F16CBEDC2) - 将一个数指定次方

  ```sql
  SELECT POWER(3, 2) FROM DUAL;
  -- 9
  ```

- [REMAINDER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REMAINDER.html#GUID-430D4C4A-5779-4EBB-90C5-4D7CA7E73556) - 返回两数相除的余数

  ```sql
  SELECT REMAINDER(10, 3) FROM dual;
  -- 结果: 1
  ```

- [ROUND (number)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ROUND-number.html#GUID-849F6C45-0D72-4464-9C0F-8B6822BA85E1) - 将一个数值四舍五入到指定的小数位数

  ~~~sql
  SELECT ROUND(3.14159, 2) FROM dual;
  -- 结果: 3.14
  SELECT ROUND(3.14159) FROM dual;
  -- 结果: 3
  SELECT ROUND(6.78, -1) FROM dual;
  -- 结果: 10
  SELECT ROUND(6.78, 0) FROM dual;
  -- 结果: 7
  
  ~~~

- [SIGN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SIGN.html#GUID-08B75521-B5F5-4658-A005-4B4441C82945) - 接受一个数字作为参数，

  - 如果数字小于0，则`SIGN`函数返回-1
  - 如果数字等于0，则`SIGN`函数返回0
  - 如果数字大于0，则`SIGN`函数返回1

  ~~~sql
  SELECT SIGN(-23) FROM dual;
  -- 结果: -1
  SELECT SIGN(0) FROM dual;
  -- 结果: 0
  SELECT SIGN(23) FROM dual;
  -- 结果: 1
  
  ~~~

- [SIN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SIN.html#GUID-2AF4895F-5D23-4165-89D5-B1D404ED99BF)

- [SINH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SINH.html#GUID-1EB8626B-4D84-4EAD-BD23-1A97F186FD4A)

- [SQRT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SQRT.html#GUID-E28C0B65-AAD8-4077-A82E-2FB4CD261CCA)

- [TAN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TAN.html#GUID-473E2008-5951-4FC8-A356-14D3D085B8AA)

- [TANH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TANH.html#GUID-8DD0B75F-1BDB-4E41-8C6D-FB5B2908AF80)

- [TRUNC (number)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TRUNC-number.html#GUID-911AE7FE-E04A-471D-8B0E-9C50EBEFE07D)- 用于截断日期或数字的值。它接受一个日期或数字作为参数，并将其截断到指定的单位。

  （1）日期值

  ```sql
  SELECT TRUNC(SYSDATE, 'MM') result FROM dual;
  -- 结果: 本月的第一天
  
  SELECT TRUNC(SYSDATE, 'Q') result FROM dual;
  -- 结果: 本季度的第一天
  
  SELECT TRUNC(TO_DATE('04-Aug-2017 15:35:32', 'DD-Mon-YYYY HH24:MI:SS')) result FROM dual;
  -- 结果: 日期被截断到午夜
  ```

  - `'CC'`：截断到世纪的开始日期。
  - `'YYYY'`：截断到年份的开始日期。
  - `'YYY'`：截断到年份的开始日期，忽略世纪。
  - `'YY'`：截断到年份的开始日期，忽略世纪和年。
  - `'Y'`：截断到年份的开始日期，忽略世纪、年和月。
  - `'Q'`：截断到季度的开始日期。
  - `'MONTH'`：截断到月份的开始日期。
  - `'MON'`：截断到月份的开始日期，返回缩写的月份名称。
  - `'MM'`：截断到月份的开始日期。
  - `'W'`：截断到一周的开始日期。
  - `'IW'`：截断到ISO标准的一周的开始日期。
  - `'D'`：截断到一天的开始日期。
  - `'DDD'`：截断到一年中的第几天的开始日期。
  - `'DAY'`：截断到一周中的第几天的开始日期。
  - `'DY'`：截断到一周中的第几天的开始日期，返回缩写的星期几名称。

  （2）数字

  ```sql
  SELECT TRUNC(15.79, 1) "Truncate" FROM dual;
  -- 结果: 15.7
  
  SELECT TRUNC(15.79, -1) "Truncate" FROM dual;
  -- 结果: 10
  ```

- [WIDTH_BUCKET](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/WIDTH_BUCKET.html#GUID-5E9058E5-A91F-45ED-A90D-E21355D19A88) - 这个函数通常用于创建等宽直方图，以便对数据进行分组或分析

  根据输入值在指定范围内的位置，返回相应的桶号

  ~~~sql
  WIDTH_BUCKET(expr, min_value, max_value, num_buckets)
  ~~~

  - `expr`：输入值
  - `min_value`和`max_value`：
  - `num_buckets`：解析为指定桶的数量的常量表达式。这个表达式必须求值为正整数。

  `WIDTH_BUCKET`函数的工作原理如下：

  1. 首先，函数计算出每个桶的宽度。宽度的计算方式是将范围的大小（`max_value - min_value`）除以桶的数量（`num_buckets`）。
  2. 然后，函数将输入值减去范围下限（`min_value`），并将结果除以桶的宽度。这将给出一个相对于范围的位置，表示输入值在范围内的位置。
  3. 最后，函数将这个相对位置向上取整，得到桶号。如果相对位置小于等于0，函数返回0，表示下溢桶。如果相对位置大于等于桶的数量，函数返回`num_buckets`+1，表示上溢桶。否则，函数返回相对位置的整数部分作为桶号。

  示例：

  ```sql
  SELECT WIDTH_BUCKET(7, 1, 12, 3) FROM DUAL;
  
  WIDTH_BUCKET(7, 1, 12, 3)
  ------------------------
                         2
  ```

  在这个例子中，我们使用`WIDTH_BUCKET`函数将值7分配到1到12的范围内，分为3个桶。计算过程如下：

  1. 桶的宽度为`(12 - 1) / 3 = 3.6667`
  2. 相对位置为`(7 - 1) / 3.6667 = 1.7143`
  3. 向上取整得到桶号2。

  因此，函数返回2，表示值7位于第2个桶中



## 字符函数

### 返回字符值

如果输入参数是CHAR或VARCHAR2，则返回的值是VARCHAR2；如果输入参数为NCHAR或NVARCHAR2，则返回值为NVARCHAR2。

函数返回的值的长度受返回数据类型的最大长度的限制。对于返回CHAR或VARCHAR2的函数，如果返回值的长度超过限制，则Oracle数据库将其截断并返回结果而不显示错误消息；对于返回CLOB值的函数，如果返回值的长度超过限制，则Oracle抛出错误并不返回数据。

返回字符值的字符函数有:

- [CHR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CHR.html#GUID-35FEE007-D49C-4562-A904-041186AC8928) - 输入数字，返回对应字符集的字符（如：ASCALL字符集，输入65返回A）

- [CONCAT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CONCAT.html#GUID-D8723EA5-C93A-45C3-83FB-1F3D2A4CEAF2)

- [INITCAP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/INITCAP.html#GUID-9FE9E0EE-D6B6-4C2C-BDEF-4FF4E1314560) - 将单词每个首字母变成大写

- [LOWER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LOWER.html#GUID-C8682D4C-9BED-48AC-B73A-1D70BF307F48)

- [LPAD](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LPAD.html#GUID-0C27B59A-A6CF-43D3-BF4B-07A3D0F2CE20)

  在字符串的左侧填充指定的字符或字符序列，使其达到指定的长度

  ```
  LPAD(源字符串, 目标长度 [, 填充字符串]);
  ```

  - `源字符串`是要填充的字符串。
  - `目标长度`是要填充到的长度。
  - `填充字符串`是可选参数，用于指定填充时使用的字符或字符序列。如果未指定，则默认使用一个空格。

  如果`目标长度`小于`源字符串`的长度，则LPAD()函数会将`源字符串`截断为`目标长度`而不进行填充。

- [LTRIM](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LTRIM.html#GUID-81B3D53C-0BBC-4485-B057-C8012CD6E40F)

  从字符串的左侧删除指定的字符或字符序列

  ```
  LTRIM(源字符串 [, 要删除的字符]);
  ```

  - `源字符串`是要进行操作的字符串。
  - `要删除的字符`是可选参数，用于指定要从左侧删除的字符或字符序列。如果未指定，则默认删除空格。

  LTRIM()函数会从`源字符串`的左侧开始删除指定的字符或字符序列，直到遇到一个不匹配的字符为止。

- [NCHR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NCHR.html#GUID-3A1BDD54-6C0B-4067-99C5-A439C0F8D561)

- [NLS_INITCAP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_INITCAP.html#GUID-42C1581B-B5AA-4D4C-A489-BC5B38A754FD)

- [NLS_LOWER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_LOWER.html#GUID-96944213-377E-461C-9F02-2DC4EC2B1649)

- [NLS_UPPER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_UPPER.html#GUID-91D6302F-4DE2-49FA-8837-D46D3FD58DF8)

- [NLSSORT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLSSORT.html#GUID-781C6FE8-0924-4617-AECB-EE40DE45096D)

- [REGEXP_REPLACE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REGEXP_REPLACE.html#GUID-EA80A33C-441A-4692-A959-273B5A224490)

  高级的替换函数，用于使用正则表达式模式替换字符串中匹配的字符序列

  ```
  REGEXP_REPLACE(源字符串, 正则表达式模式 [, 替换字符串 [, 开始位置 [, 第n次出现 [, 匹配参数 ] ] ] ] )
  ```

  - `源字符串`是要进行操作的字符串。
  - `正则表达式模式`是用于匹配字符串的模式。
  - `替换字符串`是要替换匹配的字符串的字符串。
  - `开始位置`是可选参数，用于指定开始搜索的位置。如果未指定，则默认从字符串的起始位置开始搜索。
  - `第n次出现`是可选参数，用于指定要替换的模式的第n次出现。如果未指定，则默认替换所有匹配的模式。
  - `匹配参数`是可选参数，用于修改REGEXP_REPLACE函数的匹配行为。

  REGEXP_REPLACE()函数返回一个替换了匹配模式的字符串。

  示例：

  ```sql
  -- 将字符串中的所有数字替换为 'X'
  SELECT REGEXP_REPLACE('abc123def456', '[0-9]', 'X') FROM DUAL;
  -- 输出：'abcXXXdefXXX'
  
  -- 将字符串中的第一个元音字母替换为 'Z'
  SELECT REGEXP_REPLACE('TechOnTheNet', '[aeiou]', 'Z', 1, 1) FROM DUAL;
  -- 输出：'TzchOnTheNet'
  
  -- 查询特定列不为数值的数据
  SELECT * FROM DUAL WHERE regexp_replace(列名,'^[-\+]?\d+(\.\d+)?$','') is not null
  ```

- [REGEXP_SUBSTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REGEXP_SUBSTR.html#GUID-2903904D-455F-4839-A8B2-1731EF4BD099)

  基于正则表达式模式从字符串中提取子字符串

  ```
  REGEXP_SUBSTR(源字符串, 正则表达式模式 [, 开始位置 [, 第n次出现 [, 匹配参数 [, 子表达式 ] ] ] ] )
  ```

  - `源字符串`是要进行操作的字符串。
  - `正则表达式模式`是用于匹配字符串的模式。
  - `开始位置`是可选参数，用于指定开始搜索的位置。如果未指定，则默认从字符串的起始位置开始搜索。
  - `第n次出现`是可选参数，用于指定要提取的模式的第n次出现。如果未指定，则默认提取第一个匹配的模式。
  - `匹配参数`是可选参数，用于修改REGEXP_SUBSTR函数的匹配行为。
  - `子表达式`是可选参数，用于指定要提取的子匹配模式。

  `REGEXP_SUBSTR()`函数返回一个字符串值，该值是匹配正则表达式模式的子字符串。

  示例：

  ```sql
  -- 提取字符串中的第一个元音字母
  SELECT REGEXP_SUBSTR('TechOnTheNet', '[aeiou]', 1, 1, 'i') FROM DUAL;
  -- 输出：'e'
  
  -- 提取字符串中的第二个元音字母
  SELECT REGEXP_SUBSTR('TechOnTheNet', '[aeiou]', 1, 2, 'i') FROM DUAL;
  -- 输出：'O'
  
  -- 提取URL中的域名部分
  SELECT REGEXP_SUBSTR('http://www.oracle.com/products', 'http://([[:alnum:]]+\.?){3,4}/?') FROM DUAL;
  -- 输出：'http://www.oracle.com/'
  ```

- [REPLACE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REPLACE.html#GUID-1A79BDDF-2D3B-4AD4-98E7-985B2E59DA6B)

  将字符串中的所有指定子字符串替换为另一个字符串

  ```
  REPLACE(源字符串, 要替换的子字符串 [, 替换字符串])
  ```

  - `源字符串`是要进行操作的字符串。
  - `要替换的子字符串`是要在源字符串中替换的子字符串。
  - `替换字符串`是可选参数，用于指定替换子字符串的字符串。如果未指定，则默认将子字符串替换为空字符串。

- [RPAD](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/RPAD.html#GUID-064CFCAE-5902-49F9-800E-0AF311AEF595) - 效果和LPAD一样，但是在右边填充

- [RTRIM](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/RTRIM.html#GUID-95A7DAFB-F7AB-48F4-BE24-64B3C7A840AA) - 效果和LTRIM一样，但是在右边删除

- [SOUNDEX](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SOUNDEX.html#GUID-9C43625B-70CA-4B43-AE22-5EC2A02192F8)

- [SUBSTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SUBSTR.html#GUID-C8A20B57-C647-4649-A379-8651AA97187E)

  ```sql
  SUBSTR( str, start_position [, substring_length] );
  ```

  - `str`是要提取子字符串的源字符串
  - `start_position`是开始提取的位置
  - `substring_length`是要提取的子字符串的长度（可选）

  示例：

  ```sql
  SELECT SUBSTR('Hello, World!', 8, 5) AS result FROM dual;
  -- 'World!'
  
  SELECT SUBSTR('Hello, World!', -6) AS result FROM dual;
  -- 'World'
  ```

- [TRANSLATE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TRANSLATE.html#GUID-80F85ACB-092C-4CC7-91F6-B3A585E3A690)

  字符替换

  ```
  TRANSLATE( string1, string_to_replace, replacement_string );
  ```

  - `string1`是要进行字符替换的源字符串
  - `string_to_replace`是要被替换的字符
  - `replacement_string`是替换后的字符

  示例：

  ```sql
  SELECT TRANSLATE('SQL*Plus User''s Guide', ' */''', '___') FROM dual;
  -- SQL_Plus_Users_Guide
  ```

- [TRANSLATE ... USING](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TRANSLATE-USING.html#GUID-EC8DE4D2-4F24-456D-A2E7-AD8F82E3A148)

- [TRIM](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TRIM.html#GUID-00D5C77C-19B1-4894-828F-066746235B03)

- [UPPER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/UPPER.html#GUID-0518FB26-7FE5-43B9-AB31-9352F9F6029C)



### 返回数值

返回数值的字符函数可以接受任何字符数据类型作为参数。

- [ASCII](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ASCII.html#GUID-871D4171-FF70-475E-BC82-9B8F46239A5D) - 传入一个字符，返回ASCLL码；传入多个没用

- [INSTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/INSTR.html#GUID-47E3A7C4-ED72-458D-A1FA-25A9AD3BE113) 

  在字符串中搜索子字符串，并找到子字符串在字符串中的位置。如果找到与子字符串相等的子字符串，则该函数返回一个整数，表示该子字符串第一个字符的位置。如果找不到这样的子字符串，则函数返回0

  ```
  INSTR(string, substring, [position, [occurrence]])
  ```

  - `string`：要搜索的字符串。
  - `substring`：要查找的子字符串。
  - `position`：可选参数，指定搜索开始的位置。默认值为1，表示从字符串的第一个字符开始搜索。
  - `occurrence`：可选参数，指定要查找的子字符串的第几个出现。默认值为1，表示查找第一个出现的子字符串。

- [LENGTH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LENGTH.html#GUID-8F97F652-5AE8-4457-AFD7-7A6F25551E0C)

- [REGEXP_COUNT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REGEXP_COUNT.html#GUID-5148AF2E-9CED-497D-A78D-3A7847A45276)

  计算字符串中指定模式的出现次数

  ```
  REGEXP_COUNT(string, pattern [, start_position [, match_parameter ] ])
  ```

  - `string`：要搜索的字符串。
  - `pattern`：要匹配的模式。
  - `start_position`：可选参数，指定开始搜索的位置，默认为1。
  - `match_parameter`：可选参数，指定匹配的方式，如大小写敏感或不敏感，默认为'c'，表示大小写敏感。

  示例：

  ```sql
  SELECT REGEXP_COUNT('My dogs have dags', 'd.g') FROM DUAL;
  -- 2，因为字符串中有两个匹配模式'd.g'，分别是'dog'和'dag'。
  
  SELECT REGEXP_COUNT('John Doe', 'E', 1, 'i') FROM DUAL;
  -- 1，因为字符串中有一个匹配模式'E'，不区分大小写。
  
  SELECT REGEXP_COUNT('John Doe', 'do', 1, 'i') FROM DUAL;
  -- 1，因为字符串中有一个匹配模式'do'，不区分大小写。
  
  SELECT REGEXP_COUNT('John Doe', 'an', 1, 'c') FROM DUAL;
  -- 0，因为字符串中没有匹配模式'an'，区分大小写。
  ```

- [REGEXP_INSTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/REGEXP_INSTR.html#GUID-D21B53A1-83E2-4722-9BBB-638470715DD6)

  在字符串中搜索满足正则表达式模式的子串，并返回子串的开始位置或结束位置

  ```
  REGEXP_INSTR(string, pattern [, start_position [, nth_appearance [, return_option [, match_parameter [, sub_expression ] ] ] ] ] )
  ```

  - `string`：要搜索的字符串。
  - `pattern`：要匹配的正则表达式模式。
  - `start_position`：可选参数，指定开始搜索的位置，默认为1。
  - `nth_appearance`：可选参数，指定匹配模式的第n个出现，默认为1。
  - `return_option`：可选参数，指定返回开始位置还是结束位置，默认为0（开始位置）。
  - `match_parameter`：可选参数，指定匹配的方式，如大小写敏感或不敏感，默认为'c'，表示大小写敏感。
  - `sub_expression`：可选参数，指定正则表达式模式中的子表达式。

  REGEXP_INSTR函数返回一个整数，表示匹配子串的位置。如果未找到匹配的子串，返回0。



### Character Set Functions（字符集函数）

字符集函数返回有关字符集的信息。字符集功能如下:

- [NLS_CHARSET_DECL_LEN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_CHARSET_DECL_LEN.html#GUID-5F0939C0-4AFB-4CEA-9899-BDE85B9B2F11)
- [NLS_CHARSET_ID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_CHARSET_ID.html#GUID-733B03A0-CD66-4645-A323-401A176499E3)
- [NLS_CHARSET_NAME](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NLS_CHARSET_NAME.html#GUID-5DCFB255-92AD-4E94-9344-73B7918C106C)



## 时间函数

- [ADD_MONTHS](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ADD_MONTHS.html#GUID-B8C74443-DF32-4B7C-857F-28D557381543)
- [CURRENT_DATE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CURRENT_DATE.html#GUID-96795097-D6F0-4288-90E7-9D7C49B4F6E5)
- [CURRENT_TIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CURRENT_TIMESTAMP.html#GUID-CBD42B84-869D-45C7-9FFC-001DD7712097)
- [DBTIMEZONE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/DBTIMEZONE.html#GUID-F2368F72-7065-462F-80B9-E115F5A48025)
- [EXTRACT (datetime)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/EXTRACT-datetime.html#GUID-36E52BF8-945D-437D-9A3C-6860CABD210E)
- [FROM_TZ](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/FROM_TZ.html#GUID-84384FF7-6462-480C-BC40-60087016857B)
- [LAST_DAY](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LAST_DAY.html#GUID-296C7C02-7FB9-4AAC-8927-6A79320CE0C6)
- [LOCALTIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LOCALTIMESTAMP.html#GUID-3C3D1F29-5F53-41F2-B2D6-A3767DFB22CA)
- [MONTHS_BETWEEN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/MONTHS_BETWEEN.html#GUID-E4A1AEC0-F5A0-4703-9CC8-4087EB889952)
- [NEW_TIME](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NEW_TIME.html#GUID-1D1CC7DE-CA2A-4BEC-B404-89FD19EE36AC)
- [NEXT_DAY](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NEXT_DAY.html#GUID-01B2CC7A-1A64-4A74-918E-26158C9096F6)
- [NUMTODSINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NUMTODSINTERVAL.html#GUID-5A7392A8-7976-4465-8839-A65EFF1A80B6)
- [NUMTOYMINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NUMTOYMINTERVAL.html#GUID-B98B21AA-44F7-4A9D-A646-6775A1D5F46D)
- [ORA_DST_AFFECTED](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_DST_AFFECTED.html#GUID-EE288E4B-DE55-4104-813C-11E28F7B474A)
- [ORA_DST_CONVERT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_DST_CONVERT.html#GUID-3A991FB0-0E98-48F5-902F-55C6FCA8DA13)
- [ORA_DST_ERROR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_DST_ERROR.html#GUID-02FAF3EC-D90A-42FB-A212-513314AD774A)
- [ROUND (date)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ROUND-date.html#GUID-C6D342D0-6068-4986-A759-70EF4599EC41)
- [SESSIONTIMEZONE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SESSIONTIMEZONE.html#GUID-2A243878-C1C5-4B7C-81DE-D8B024796EAB)
- [SYS_EXTRACT_UTC](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYS_EXTRACT_UTC.html#GUID-C540A8C8-72B1-46AF-A9AA-18D011763AD8)
- [SYSDATE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYSDATE.html#GUID-807F8FC5-D72D-4F4D-B66D-B0FE1A8FA7D2)
- [SYSTIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYSTIMESTAMP.html#GUID-FCED18CE-A875-4D5D-9178-3DE4FA956516)
- [TO_CHAR (datetime)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CHAR-datetime.html#GUID-0C3EEFD1-AE3D-452D-BF23-2FC95664E78F)
- [TO_DSINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_DSINTERVAL.html#GUID-DEBB41BD-9438-4558-A53E-428CE93C05D3)
- [TO_TIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_TIMESTAMP.html#GUID-57E09334-E3CC-4CA2-809E-F0909458BCFA)
- [TO_TIMESTAMP_TZ](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_TIMESTAMP_TZ.html#GUID-3999303B-89CA-4AA3-9817-458F36ADC9DC)
- [TO_YMINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_YMINTERVAL.html#GUID-5DEBA096-7AC3-4B18-A4BE-D36FC9BDB450)
- [TRUNC (date)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TRUNC-date.html#GUID-BC82227A-2698-4EC8-8C1A-ABECC64B0E79)
- [TZ_OFFSET](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TZ_OFFSET.html#GUID-D2007072-34C2-4971-BD2B-64D93A3D7A31)



## 比较函数

从一组值中确定最大值和最小值（因为是单行函数，所以不用分组）

- [GREATEST](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/GREATEST.html#GUID-06B88B22-8466-44B6-93C7-50B222122ECE)

  ~~~sql
  select GREATEST(expr1 [, expr2, ... expr_n]) from dual;
  ~~~

  如果表达式的数据类型不同，所有表达式都会被转换为`expr1`的数据类型后进行比较

  ```sql
  -- 数字
  SELECT GREATEST(2, 5, 12, 3) FROM DUAL;
  -- 12
  
  -- 一个字符被认为比另一个字符大，取决于字符集中的值更高
  SELECT GREATEST('apples', 'oranges', 'bananas') FROM DUAL;
  
  -- 比较日期 
  SELECT GREATEST(DATE '2020-01-01', DATE '2021-01-01') FROM DUAL;
  -- 2021-01-01
  ```

  

- [LEAST](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LEAST.html#GUID-0198D71B-051A-41D9-8E9C-599E24692556)



## 类型转换函数

函数名的形式遵循约定的数据类型TO数据类型

- [ASCIISTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ASCIISTR.html#GUID-B6128485-4E86-4851-860F-AC03981E2388)

  将字符串中的每个字符转换为它的ASCII值

  ```sql
  SELECT ASCIISTR('abc') FROM DUAL;
  -- 616263
  ```

- [BIN_TO_NUM](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/BIN_TO_NUM.html#GUID-BF061402-D7F0-4557-B7D4-1CEE6E80F3B2)

  将二进制字符串转换为数字

  ```sql
  SELECT BIN_TO_NUM('1010') FROM DUAL;
  -- 10
  ```

- [CAST](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CAST.html#GUID-5A70235E-1209-4281-8521-B94497AAEF75)

  将一种数据类型转换为另一种数据类型。函数的语法是`CAST(value AS type)`，其中`value`是你想要转换的值，`type`是你想要转换到的数据类型。

  ```sql
  SELECT CAST('123' AS NUMBER) FROM DUAL;
  ```

- [CHARTOROWID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CHARTOROWID.html#GUID-F9C63933-F680-465D-AB22-6B8B882B5CF7)

- [COMPOSE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/COMPOSE.html#GUID-A16E7D53-E7F8-46A6-B3F8-BA322D129019)

  用于返回一个Unicode字符串，函数的语法是`COMPOSE(string)`

  可以使用`COMPOSE`函数和`UNISTR`函数来创建一个包含特殊字符的Unicode字符串

  ```sql
  SELECT COMPOSE('a' || UNISTR('\0303')) FROM DUAL;
  -- ã
  ```

  因为`UNISTR('\0303')`是Unicode字符`ã`的代码点，`COMPOSE`函数将字符'a'和Unicode字符'\0303'组合在一起

- [CONVERT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CONVERT.html#GUID-C8BA0657-61C8-4964-A4CB-9292390853F6)

  将一种数据类型转换为另一种数据类型

  ```sql
  SELECT CONVERT('123', 'NUMBER') FROM DUAL;
  -- 123
  ```

  这将返回`123`，因为字符串`'123'`被转换为了数字`123`。

  注意：在Oracle中，`CONVERT`函数在某些情况下可能不会按预期工作。例如，它不能用于将日期和时间数据类型转换为字符串。在这种情况下，你应该使用`TO_CHAR`函数。

- [DECOMPOSE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/DECOMPOSE.html#GUID-3E772756-F12C-4827-99A5-F7CF4F11A25A)

- [HEXTORAW](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/HEXTORAW.html#GUID-8571556F-C219-4814-A854-9F01581FFBDF)

- [NUMTODSINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NUMTODSINTERVAL.html#GUID-5A7392A8-7976-4465-8839-A65EFF1A80B6)

  返回一个时间间隔

  ~~~sql
  -- 返回一个间隔100秒的时间
  SELECT NUMTODSINTERVAL(100, 'SECOND') FROM DUAL;
  -- 0 0:1:40.0
  ~~~

  语法`NUMTODSINTERVAL(n, interval_unit)`，n是一个数值

  interval_unit是间隔单位，必须是以下

  - 'DAY'
  - 'HOUR'
  - 'MINUTE'
  - 'SECOND'

- [NUMTOYMINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NUMTOYMINTERVAL.html#GUID-B98B21AA-44F7-4A9D-A646-6775A1D5F46D)

  ```sql
  -- 间隔三年
  SELECT NUMTOYMINTERVAL(3, 'YEAR') FROM DUAL;
  -- 3-0
  ```

  语法`NUMTODSINTERVAL(n, interval_unit)`，n是一个数值

  interval_unit是间隔单位，必须是以下

  - 'YEAR'
  - 'MONTH'

- [RAWTOHEX](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/RAWTOHEX.html#GUID-F86E3B5B-7FEE-47FD-A0C2-2FC55DC21C9E)

- [RAWTONHEX](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/RAWTONHEX.html#GUID-5657B113-24CE-4DC6-BD11-63135B7DB009)

- [ROWIDTOCHAR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ROWIDTOCHAR.html#GUID-67998E5B-376A-45B5-B20B-1A87E5D370C1)

- [ROWIDTONCHAR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ROWIDTONCHAR.html#GUID-3178A4DA-2534-4A93-A819-7C14208AE9B5)

- [SCN_TO_TIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SCN_TO_TIMESTAMP.html#GUID-BCB0C8EE-0E03-4A61-A41A-69975FAC1803)

- [TIMESTAMP_TO_SCN](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TIMESTAMP_TO_SCN.html#GUID-58796E1A-9943-4966-96E6-78B636BD2859)

- [TO_BINARY_DOUBLE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_BINARY_DOUBLE.html#GUID-0BA2E065-8006-426C-A3CB-1F6B0C8F283C)

- [TO_BINARY_FLOAT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_BINARY_FLOAT.html#GUID-66A51BE2-BE4A-4B99-9C37-73B110452D27)

- [TO_BLOB (bfile)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_BLOB-bfile.html#GUID-232A1599-53C9-464B-904F-4DBA336B4EBC)

- [TO_BLOB (raw)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_BLOB-raw.html#GUID-C4308DB1-5BFE-48F0-99E5-9E03B80B4585)

- [TO_CHAR (bfile|blob)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CHAR-bfile-blob.html#GUID-F12F3C5A-8E3C-4FE1-BD7D-4AC0B79EA5A5)

- [TO_CHAR (character)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CHAR-character.html#GUID-EC078E16-11FE-4ABE-AE05-DA9AC1B4BEBC)

- [TO_CHAR (datetime)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CHAR-datetime.html#GUID-0C3EEFD1-AE3D-452D-BF23-2FC95664E78F)

  ~~~sql
  SELECT TO_CHAR(sysdate, 'yyyy/mm/dd') FROM DUAL;
  -- 2023/11/27
  SELECT TO_CHAR(sysdate, 'FMMonth DD, YYYY') FROM DUAL; -- 使用'FM'前缀来压缩日期字符串中的零和空格
  -- 11月 27, 2023
  SELECT TO_CHAR(sysdate, 'DY, DD MONTH YYYY') FROM DUAL;
  -- 星期一, 27 11月 2023
  
  WITH dates AS (
   SELECT date'2015-01-01' d FROM dual union
   SELECT date'2015-01-10' d FROM dual union
   SELECT date'2015-02-01' d FROM dual
  )
  ~~~

- [TO_CHAR (number)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CHAR-number.html#GUID-00DA076D-2468-41AB-A3AC-CC78DBA0D9CB)

  将数值类型转换为`VARCHAR2`数据类型的字符串

  ```sql
  SELECT TO_CHAR(-10000,'L99G999D99MI') "Amount" FROM DUAL;
  ```

  "9"表示数字，"G"表示千位分隔符，"D"表示小数点，"MI"表示负号

  ```sql
  SELECT TO_CHAR(-10000,'L99G999D99MI',
    'NLS_NUMERIC_CHARACTERS = '',.''  -- 使用逗号作为小数点，使用点作为千位分隔符
    NLS_CURRENCY = ''￥'' ') "Amount"  -- 增加前缀
  FROM DUAL;
  -- ￥10.000,00-
  ```

  

- [TO_CLOB (bfile|blob)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CLOB-bfile-blob.html#GUID-FD7D58FE-B97C-4B75-85A9-5F82FB1DE96A)

- [TO_CLOB (character)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_CLOB-character.html#GUID-82E2FAD3-B0C8-4A06-A882-26211EE0524C)

- [TO_DATE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_DATE.html#GUID-D226FA7C-F7AD-41A0-BB1D-BD8EF9440118)

- [TO_DSINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_DSINTERVAL.html#GUID-DEBB41BD-9438-4558-A53E-428CE93C05D3)

- [TO_LOB](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_LOB.html#GUID-35810313-029E-4CB8-8C27-DF432FA3C253)

- [TO_MULTI_BYTE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_MULTI_BYTE.html#GUID-58A9F91A-5B1E-4C14-8F48-046F176E2F4A)

- [TO_NCHAR (character)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_NCHAR-character.html#GUID-539E9F5C-CB47-4BCE-B468-C34CF6BABDC5)

- [TO_NCHAR (datetime)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_NCHAR-datetime.html#GUID-C40DBBC2-B9F2-49D8-8775-DDA99FF41EAC)

- [TO_NCHAR (number)](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_NCHAR-number.html#GUID-B0FA1B2F-3285-46C4-96DA-3C7AED48987C)

- [TO_NCLOB](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_NCLOB.html#GUID-56CEB237-8515-4030-A5D5-016CBC5FA6BB)

- [TO_NUMBER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_NUMBER.html#GUID-D4807212-AFD7-48A7-9AED-BEC3E8809866)

- [TO_SINGLE_BYTE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_SINGLE_BYTE.html#GUID-36364630-C62C-46C5-B29B-EFE3DFB5AA6D)

- [TO_TIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_TIMESTAMP.html#GUID-57E09334-E3CC-4CA2-809E-F0909458BCFA)

- [TO_TIMESTAMP_TZ](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_TIMESTAMP_TZ.html#GUID-3999303B-89CA-4AA3-9817-458F36ADC9DC)

- [TO_YMINTERVAL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TO_YMINTERVAL.html#GUID-5DEBA096-7AC3-4B18-A4BE-D36FC9BDB450)

- [TREAT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/TREAT.html#GUID-037C0CD3-C256-4A02-80E0-C6F15147C5BF)

- [UNISTR](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/UNISTR.html#GUID-AAF757DB-6E5D-4548-9E36-6B36BB0BD83E)

- [VALIDATE_CONVERSION](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/VALIDATE_CONVERSION.html#GUID-DC485EEB-CB6D-42EF-97AA-4487884CB2CD)



## 编解码函数

- [DECODE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/DECODE.html#GUID-39341D91-3442-4730-BD34-D3CF5D4701CE)

  嵌入if-then-else逻辑

  ```sql
  DECODE (e , s1, r1[, s2, r2], ...,[,sn,rn] [, d]);
  ```

  其中，`e`是要比较的表达式，`s1`、`s2`、...、`sn`是要与`e`比较的值，`r1`、`r2`、...、`rn`是如果`e`等于`s1`、`s2`、...、`sn`时返回的值，`d`是如果`e`不等于任何`s`时返回的默认值

  ```sql
  SELECT name, DECODE ( student_id, 1, 'Tom', 2, 'Mike', 3, 'Harry', 'Jim') result
  FROM students;
  ```

  这将返回每个学生的名字和ID，如果学生ID等于1，则返回'Tom'，如果学生ID等于2，则返回'Mike'，如果学生ID等于3，则返回'Harry'，否则返回'Jim'

- [DUMP](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/DUMP.html#GUID-A05793C9-B35D-4BA7-B68C-E3693BCF47A5)

  查找值的数据类型，长度和内部表示

  ```
  DUMP ( expression [, return_format] [, start_position] [, length] )
  ```

  其中，`expression`是要操作的表达式，`return_format`是返回的格式，`start_position`是开始位置，`length`是长度

  ```sql
  
  
  
  SELECT DUMP('ABC') FROM dual;
  -- Typ=96 Len=3: 65,66,67
  SELECT DUMP('ABC', 10) FROM dual;
  -- Typ=96 Len=3: 65,66,67
  SELECT DUMP('ABC', 10, 1, 2) FROM dual;
  -- Typ=96 Len=3: 65,66
  ```

- [ORA_HASH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_HASH.html#GUID-0349AFF5-0268-43CE-8118-4F96D752FDE6)

- [STANDARD_HASH](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/STANDARD_HASH.html#GUID-4A68DACE-CFCF-443B-8651-B6CEAA7C4FD7)

- [VSIZE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/VSIZE.html#GUID-CDDB2A17-9398-4AF8-96FB-4297DDA2665B)

  返回字符串的字节长度



## *NULL*相关函数

- [COALESCE](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/COALESCE.html#GUID-3F9007A7-C0CA-4707-9CBA-1DBF2CDE0C87)

  返回一组数据中第一个非`NULL`的值

  ~~~sql
  COALESCE(e1, e2, ..., en)
  -- e1、e2、...、en是要比较的表达式
  ~~~

- [LNNVL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/LNNVL.html#GUID-FBCCE9B1-614E-45FA-8EE1-DFAA4F936867)

  当条件的一个或两个操作数可能为空时，LNNVL提供了一种简洁的方法来计算条件。该函数只能在查询的WHERE子句中使用。

  它接受一个条件作为参数，如果条件为FALSE或UNKNOWN则返回TRUE，如果条件为TRUE则返回FALSE

  ~~~sql
  WITH data AS (
      SELECT 1000 prod_id, 20 qty, NULL reorder_nevel FROM DUAL
      UNION
      SELECT 1001 prod_id, 30 qty, 40 reorder_nevel FROM DUAL
      UNION
      SELECT 1002 prod_id, 80 qty, 60 reorder_nevel FROM DUAL
  )
  SELECT *
         FROM data
  WHERE lnnvl(qty >= REORDER_NEVEL);
  
  -- 1000,20,null
  -- 1001,30,40
  ~~~

- [NANVL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NANVL.html#GUID-3C094646-2A70-41F5-984C-9BC0FB31494A)

  如果表达式的值为`NaN`，则返回指定的值

  ~~~sql
  nanvl(expr,  value_if_nan)
  ~~~

- [NULLIF](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NULLIF.html#GUID-445FC268-7FFA-4850-98C9-D53D88AB2405)

  用于比较两个表达式的值。如果两个表达式的值相等，则返回`NULL`，否则返回第一个表达式的值。

  ~~~sql
  NULLIF(expr1, expr2)
  ~~~

- [NVL](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NVL.html#GUID-3AB61E54-9201-4D6A-B48A-79F4C4A034B2)

  表达式的值为`NULL`，则返回指定的值，否则返回表达式的值

  ~~~sql
  nvl(expr, value_if_null)
  ~~~

- [NVL2](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/NVL2.html#GUID-414D6E81-9627-4163-8AC2-BD24E57742AE)

  用于处理`NULL`值。它接受三个参数，如果第一个参数为`NULL`，则返回第二个参数，否则返回第三个参数。

  ~~~sql
  NVL2(expr, value_if_not_null, value_if_null)
  ~~~




## 环境和*ID*函数

- [CON_DBID_TO_ID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CON_DBID_TO_ID.html#GUID-9F38A14F-8E6A-4A4A-96D5-52E4480A8926)

- [CON_GUID_TO_ID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CON_GUID_TO_ID.html#GUID-F93F257F-BB58-427D-9E19-A22E43DB288F)

- [CON_NAME_TO_ID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CON_NAME_TO_ID.html#GUID-714E0914-5018-4E32-AB1E-134FDD0B28FE)

- [CON_UID_TO_ID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/CON_UID_TO_ID.html#GUID-14BE69F3-8519-4676-90CD-374152981901)

- [ORA_INVOKING_USER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_INVOKING_USER.html#GUID-FAE7B186-C40D-48BB-A2C9-AB7EE3878BF1)

- [ORA_INVOKING_USERID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/ORA_INVOKING_USERID.html#GUID-91F09A40-96CD-4759-8EDF-4C54219E8E83)

- [SYS_CONTEXT](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYS_CONTEXT.html#GUID-B9934A5D-D97B-4E51-B01B-80C76A5BD086)

- [SYS_GUID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYS_GUID.html#GUID-761E36B4-32DA-497D-8829-3D4653381F9B)

  生成并返回一个由16字节组成的全局唯一标识符(RAW值)。在大多数平台上，生成的标识符由主机标识符、调用函数的进程或线程的进程或线程标识符以及该进程或线程的非重复值(字节序列)组成。

- [SYS_TYPEID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/SYS_TYPEID.html#GUID-4E3D45A1-7433-495D-9062-88505A1496E0)

- [UID](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/UID.html#GUID-DFDC8E24-B911-4C42-B4B1-853E964D3644)

  UID返回一个唯一标识会话用户(登录的用户)的整数

- [USER](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/USER.html#GUID-AD0B927B-EFD4-4246-89B4-2D55AB3AF531)

- [USERENV](https://docs.oracle.com/en/database/oracle/oracle-database/18/sqlrf/USERENV.html#GUID-AC3C8AEF-A988-41C4-9242-69B54E5941D2)





# 聚合函数

- [ANY_VALUE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/ANY_VALUE.html#GUID-A3C47D5E-B145-40B2-93D2-CA3BA65C2D81)

  返回组内第一个非空的数据，函数的参数支持 ALL 以及 DISTINCT 关键字，但是它们并不会影响结果。

  ANY_VALUE 函数是 Oracle 19c 新增的一个聚合函数，可以为分组操作之后的每个组返回一个任意值，可以解决查询字段不属于 GROUP BY 字段的问题。随着数据量的增加，它的性能比 GROUP BY 子句增加字段或者使用 MIN 或者 MAX 函数更好。

- [APPROX_COUNT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_COUNT.html#GUID-7D07E04A-3F9A-425E-BADE-EDA9C6162E9C)

- [APPROX_COUNT_DISTINCT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_COUNT_DISTINCT.html#GUID-50055A05-0187-4481-AFE5-2414F7227713)

- [APPROX_COUNT_DISTINCT_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_COUNT_DISTINCT_AGG.html#GUID-EEDA9388-A066-422A-B5C0-639A3076A10B)

- [APPROX_COUNT_DISTINCT_DETAIL](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_COUNT_DISTINCT_DETAIL.html#GUID-8FBD2881-743D-425E-A104-472A720DEF50)

- [APPROX_MEDIAN](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_MEDIAN.html#GUID-F6A11DF2-121A-4057-9D0B-BF1A221B5622)

- [APPROX_PERCENTILE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_PERCENTILE.html#GUID-70D54091-EE2F-4283-A10B-1AB5A1242FE2)

- [APPROX_PERCENTILE_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_PERCENTILE_AGG.html#GUID-72A1DAB0-4A3E-42BF-9E20-92273AD62E11)

- [APPROX_PERCENTILE_DETAIL](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_PERCENTILE_DETAIL.html#GUID-F9A0B9B5-671F-43CA-9FA7-69A2DD174F54)

- [APPROX_RANK](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_RANK.html#GUID-4F20978C-3188-4225-863D-0F7A25FD78FD)

- [APPROX_SUM](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/APPROX_SUM.html#GUID-AC2A72A7-24E5-4FB8-B012-BD35CB560D6B)

- [AVG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/AVG.html#GUID-B64BCBF1-DAA0-4D88-9821-2C4D3FDE5E4A)

- [BIT_AND_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/BIT_AND_AGG.html#GUID-82497098-6D77-48D3-89EF-C1041BF8A258)

- [BIT_OR_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/BIT_OR_AGG.html#GUID-18B0E3CB-1C90-4625-8E36-B422FA4E04A8)

- [BIT_XOR_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/BIT_XOR_AGG.html#GUID-1563FB7E-9CC9-4D03-859E-BE336AF01F1D)

- [BOOLEAN_AND_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/boolean_and_agg.html#GUID-AF3C1A26-C7A1-4BD2-B15C-86B761D4D8D9)

- [BOOLEAN_OR_AGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/boolean_or_agg.html#GUID-C3E187DE-BD26-4440-B0AD-51342FFA4775)

- [CHECKSUM](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/checksum.html#GUID-3F55C5DF-F23A-4B2F-BC6F-E03B34B78BA8)

- [COLLECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COLLECT.html#GUID-A0A74602-2A97-449B-A3EC-847D38D3DA90)

- [CORR](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CORR.html#GUID-E73AF5E2-38A4-436A-955C-5122C079F49C)

- [CORR_*](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CORR_A.html#GUID-B2DED35A-2ECE-4DF0-BDA4-28F28B7BCA23)

- [COUNT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COUNT.html#GUID-AEF08B79-024D-4E3A-B362-9715FB011776)

- [COVAR_POP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COVAR_POP.html#GUID-D728D05F-D2E3-405C-986F-088B8353553A)

- [COVAR_SAMP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COVAR_SAMP.html#GUID-7850B9E1-83A4-41CB-8F17-DCD2E2A70C95)

- [CUME_DIST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CUME_DIST.html#GUID-B12C577C-A63C-4D19-8E18-FCCBBFBF8278)

- [DENSE_RANK](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/DENSE_RANK.html#GUID-BB66F574-09DF-4594-87A4-ABD83E8DC3FE)

- [EVERY](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/every.html#GUID-C34D8A50-3050-4F32-941A-8C2512DEC62D)

- [FIRST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/FIRST.html#GUID-85AB9246-0E0A-44A1-A7E6-4E57502E9238)

- [GROUP_ID](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/GROUP_ID.html#GUID-3A5A9C15-1B67-4FD7-AC41-EE8349B2E834)

- [GROUPING](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/GROUPING.html#GUID-82E6084A-0BDF-4587-A40E-36899783F073)

- [GROUPING_ID](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/GROUPING_ID.html#GUID-E20A5B8E-73B6-42FD-8AFB-DD3CD6D6DC61)

- [JSON_ARRAYAGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/JSON_ARRAYAGG.html#GUID-6D56077D-78DE-4CC0-9498-225DDC42E054)

- [JSON_OBJECTAGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/JSON_OBJECTAGG.html#GUID-09422D4A-936C-4D38-9991-C64101283D98)

- [KURTOSIS_POP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/KURTOSIS_POP.html#GUID-F820DFF7-B758-460E-AECC-053915069B9F)

- [KURTOSIS_SAMP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/KURTOSIS_SAMP.html#GUID-487DE503-A015-415F-B6CD-F9D095B91178)

- [LAST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/LAST.html#GUID-4E16BC0E-D3B8-4BA4-8F97-3A08891A85CC)

- [LISTAGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/LISTAGG.html#GUID-B6E50D8E-F467-425B-9436-F7F8BF38D466)

- [MAX](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/MAX.html#GUID-E5372020-A6DA-44BF-93BE-DA8C3F74CD01)

- [MEDIAN](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/MEDIAN.html#GUID-DE15705A-AC18-4416-8487-B9E1D70CE01A)

- [MIN](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/MIN.html#GUID-F7F04E18-1AD8-4D15-9491-4622AD847A74)

- [PERCENT_RANK](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/PERCENT_RANK.html#GUID-66A868F5-9EBA-482A-BF8C-09300B9EE165)

- [PERCENTILE_CONT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/PERCENTILE_CONT.html#GUID-CA259452-A565-41B3-A4F4-DD74B66CEDE0)

- [PERCENTILE_DISC](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/PERCENTILE_DISC.html#GUID-7C34FDDA-C241-474F-8C5C-50CC0182E005)

- [RANK](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/RANK.html#GUID-0950BD34-C994-41DA-A8F9-34B3FE53BBBA)

- [REGR_ (Linear Regression) Functions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/REGR_-Linear-Regression-Functions.html#GUID-A675B68F-2A88-4843-BE2C-FCDE9C65F9A9)

- [SKEWNESS_POP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SKEWNESS_POP.html#GUID-DF34158F-B681-4933-BA27-0A3885A9F43C)

- [SKEWNESS_SAMP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SKEWNESS_SAMP.html#GUID-E71D9AEC-0AAA-4A6C-BF70-29EE9AD8F7EC)

- [STATS_BINOMIAL_TEST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_BINOMIAL_TEST.html#GUID-3DDCDC0C-0DB2-479F-A6EB-E9FC0063ABF4)

- [STATS_CROSSTAB](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_CROSSTAB.html#GUID-AA0958AE-FF56-4970-B880-23426E0B7E6D)

- [STATS_F_TEST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_F_TEST.html#GUID-9E2A91FC-5BB3-449A-810C-DA6CB52B56ED)

- [STATS_KS_TEST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_KS_TEST.html#GUID-ADE2ACB3-C852-499F-8892-E4AA101EC80D)

- [STATS_MODE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_MODE.html#GUID-10BDACE0-C435-4E3F-BC50-FD1A41C0F508)

- [STATS_MW_TEST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_MW_TEST.html#GUID-77AF4F10-D4DC-45A9-94E8-F4F648F81222)

- [STATS_ONE_WAY_ANOVA](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_ONE_WAY_ANOVA.html#GUID-CC614CE5-56CB-4A54-8571-6FEAD2D2E75F)

- [STATS_T_TEST_*](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_T_TEST_.html#GUID-B570D6F6-E4D7-4033-AC83-7E76F2E9CC2A)

- [STATS_WSR_TEST](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STATS_WSR_TEST.html#GUID-80A8A9A9-7CD9-4358-B628-6D67BD42BA5B)

- [STDDEV](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STDDEV.html#GUID-CA0C3B1F-1A4C-4CFB-ADAB-D90216C4E099)

- [STDDEV_POP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STDDEV_POP.html#GUID-4F804DE5-7E20-4E08-A1BA-32DBB167B34B)

- [STDDEV_SAMP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/STDDEV_SAMP.html#GUID-7B2A708E-E73A-4CFE-978E-3F9C4BD37467)

- [SUM](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SUM.html#GUID-5610BE2C-CFE5-446F-A1F7-B924B5663220)

- [SYS_OP_ZONE_ID](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SYS_OP_ZONE_ID.html#GUID-947900CE-F4E0-43B5-B30C-4FDDA3913F17)

- [SYS_XMLAGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SYS_XMLAGG.html#GUID-BEDD241D-360A-46A2-AEBF-C8B70E465D75)

- [TO_APPROX_COUNT_DISTINCT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/TO_APPROX_COUNT_DISTINCT.html#GUID-42A18FFB-C992-44A0-AC3E-F4BBF005846F)

- [TO_APPROX_PERCENTILE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/TO_APPROX_PERCENTILE.html#GUID-463702B2-9199-41ED-AE03-865CABAD3E23)

- [VAR_POP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/VAR_POP.html#GUID-B62FB4A4-BD1F-47B0-B412-31A98B70C2E4)

- [VAR_SAMP](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/VAR_SAMP.html#GUID-314D5831-0E26-4ABF-9F46-35F78F97DA52)

- [VARIANCE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/VARIANCE.html#GUID-EC33717A-2509-402D-B3BB-7EECB2E4ED8B)

- [XMLAGG](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/XMLAGG.html#GUID-BCD1D755-5E26-4F73-BA22-521C30D275DA)





# 报错

## ORA-02396

**Exceeded Maximum Idle Time, Please Connect Again（超过最大空闲时间，请重新连接）**

应用程序长时间空闲后无法连接到数据库。

解决方案:
哪个用户连接到数据库，假设用户是WFWZHXG，查找用户IDLE_TIME资源限制设置的值

~~~sql
select a.username,b.profile,b.RESOURCE_NAME,b.LIMIT from dba_users a, dba_profiles b where
b.resource_name='IDLE_TIME' and a.profile=b.profile and a.username='WFWZHXG';

USERNAME 	PROFILE 	RESOURCE_NAME 	LIMIT
------------- -------------- -------------------------------- --------------
SIEBEL   	DEFAULT 	IDLE_TIME 		128000
~~~


这里idle_time设置为12800分钟。因此，如果会话空闲了这么长时间，那么它将被断开连接，并出现错误，


要解决这个问题，请修改配置文件，将IDLE_TIME设置为更高的值或UNLIMITED

~~~sql
ALTER PROFILE DEFAULT LIMIT IDLE_TIME UNLIMITED;

select a.username,b.profile,b.RESOURCE_NAME,b.LIMIT from dba_users a, dba_profiles b where
b.resource_name='IDLE_TIME' and a.profile=b.profile and a.username='&USERNAME';

USERNAME PROFILE RESOURCE_NAME LIMIT
------------- -------------- -------------------------------- --------------
SIEBEL DEFAULT IDLE_TIME UNLIMITED
~~~
