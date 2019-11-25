![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 创建作业警报触发器如果不存在的话
#### If Not Exists Create Trigger For Job Alerts
**发布-日期: 2015年8月4日 (评论)**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这里是一些检查触发器 [trig_check_for_job_failure]的SQL逻辑（logic）。如果触发器不存在的话，逻辑（logic）将自动创建它。这个触发器执行的操作只是执行在以前的帖子上创建的发送SQL JOB警报作业。这些逻辑（logic）
是用来创建警报，触发器和作业，但是，以防你做这些之前想先检查一下触发器，我把逻辑（logic）也写在了下面。希望对你有用。



## English
Here’s some SQL logic that checks for the Trigger [trig_check_for_job_failure]. If it does not exist it will automatically create it. Again; what this trigger does is simply executes the Job: Send SQL JOB Alerts which was created on a former post. All the logic is there to create the Alerts, Trigger, and Job, but… In case you wanted something to check on the trigger first; I’ve included the logic below. Hope it’s helpful.

---
## Logic
```SQL
use msdb;
set nocount on
 
if not exists (select name from msdb..sysobjects where name = 'trig_check_for_job_failure' and type = 'tr') exec dbo.sp_executesql @statement = N'
create trigger [dbo].[trig_check_for_job_failure] on [dbo].[sysjobhistory] after insert
as
begin
set nocount on
declare @is_fail    int
set @is_fail    = (select case when [message] like ''%The step failed%'' then 1 else 0 end from msdb..sysjobhistory where instance_id in (select max(instance_id) from [msdb]..[sysjobhistory])) if @is_fail    = 1
begin
exec msdb.dbo.sp_start_job @job_name = ''SEND SQL JOB ALERTS'' end
end'


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

