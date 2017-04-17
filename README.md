需要学习的知识点和书籍
======
## 知识点

### EF

### LINQ

### web api

### MVC

## 工作中遇到的一些知识点

### SqlServer
+ INSERT INTO SELECT语句
  + 语句形式为：Insert into Table2(field1,field2,...) select value1,value2,... from Table1,要求目标表Table2必须存在，由于目标表Table2已经存在，所以我们除了插入源表Table1的字段外，还可以插入`常量`。
* update 多条语句
  * update 表1 set 字段=变量 from 表2 b where b.字段=表1.字段
* output输出字段，跟触发器相似
  * 例子：<br>
```DECLARE @TMP TABLE(ID INT) 
update FKPrizesQuiz set winmoney='999' OUTPUT INSERTED.ID  
INTO @TMP where winmoney is not null
DECLARE @TMPT TABLE([OPENID] [varchar](255),[NUM] INT) 
insert into Fans_ScoreRecode (openId,nickname,num,datetime,describle,remark) OUTPUT INSERTED.openId,INSERTED.num INTO @TMPT select * from(
select openId,nickname,(-num*2) num,getdate() date,describle,remark from FKPrizesQuiz as a join Fans_ScoreRecode as b on a.id=b.remark where a.id in (SELECT ID FROM @TMP) and isnull(a.winmoney,0)-isnull(a.betmoney,0)>0 and a.result='大于' and b.describle='金价竞猜'
union all
select openId,nickname,(-num*2) num,getdate() date,describle,remark from FKPrizesQuiz as a join Fans_ScoreRecode as b on a.id=b.remark where a.id in (SELECT ID FROM @TMP) and isnull(a.winmoney,0)-isnull(a.betmoney,0)<0 and a.result='小于' and b.describle='金价竞猜') as aaa 
update Fans_VipData set totalwxscore=isnull(totalwxscore,0)+ b.NUM,nowwxscore=isnull(nowwxscore,0)+ b.NUM
from @TMPT b
where b.OPENID=Fans_VipData.openId
```
* 复制表
  * SELECT \* INTO 新表 FROM 旧表（复制表）
