---
abbrlink: 94904d58
title: 基于学之思开源考试系统的SpringBoot学习笔记
date: 2022-05-01 11:20:00
---
# 1. 开始之前

**我的框架知识基础？**

SSM

**从哪里找的实战项目？**

[Java 开源项目精选 | JavaGuide](https://javaguide.cn/open-source-project/)，非常推荐这个网站！

**为什么选择这个项目？**

* 代码更新比较活跃
* 文档齐全，并且是中文
* 技术栈比较简单
* 有学习交流群
* “支持web端和微信小程序，能覆盖到pc机和手机等设备。”

**学习方式？**

项目驱动，通过基于项目的实战而不是基于知识点的视频教程来学习SpringBoot，因为后者太慢，而且容易懈怠。

更重要的是，在学校课程的象牙塔之外，项目驱动式学习才是常态，我必须适应。

# 2. 官方资料

【学之思开源考试系统 - Mysql版】（另有PostgreSQL版，PostgreSQL 是一个免费的对象-关系数据库服务器(ORDBMS)。所以我选择MySQL版，哈哈！）

项目地址：https://gitee.com/mindskip/xzs-mysql （Github也有，不过这次我想熟悉一下Gitee）

产品演示：https://www.mindskip.net/xzs.html

文档教程：https://www.mindskip.net:999/

视频教程：https://ke.qq.com/course/3614230

# 3. 上手

先在Gitee上将该项目fork到个人仓库，然后在本地git clone。

关于Git的一点补充知识：

* [使用 Git 的一点点疑惑：fork 跟 branch 的关系_曾经去过跨越一个小时的地方的博客-CSDN博客_fork和branch区别](https://blog.csdn.net/u012814856/article/details/84590065)
* [GitHub 的 Fork 是什么意思？ - 知乎](https://www.zhihu.com/question/20431718)

* [git: 为什么 pull request 不叫 push request ? - 知乎](https://www.zhihu.com/question/263901073/answer/274238027)
* [git中为什么是pull request而不是push request？ - 知乎](https://www.zhihu.com/question/334230718/answer/743900086)

关于对象存储的一点补充知识：

* [对象存储是什么？看过就明白了_杉岩数据的博客-CSDN博客_对象存储是什么通俗易懂](https://blog.csdn.net/sandstone2019/article/details/103505078)

* [对象存储 产品概述-产品简介-文档中心-腾讯云-腾讯云](https://cloud.tencent.com/document/product/436/6222)

数据库脚本下载地址：https://www.mindskip.net/sql/3.7.0/xzs-sql.zip

# 4. Bug



# 9. 需求

管理员添加学生时，选择出生日期时，默认年份为18年前，而不是今年，没有人会在今年出生就上学，使用者遇到这种情况100%需要往前翻。

接上一条，选择出生日期后，自动计算出年龄，不需要手动填写。



支持试题导出为word/pdf以方便打印？或从word/txt导入，比如按照一定格式编写好试题后，一次性导入，会不会比较快？