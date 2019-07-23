#md 文件命名规则和路径详解
#路径详解
cn：中文
en：英文
0_PRO_INFO：概述
1_QUICK_START：快速开始
2_COMMON_FUNCTION：常用功能
3_API_DOCUMENT api文档
4_CODES_EXAMPLE demo sdk下载

#文件命名规则 英文+下划线
{d}_xxxx_+_{A}_{B}.md
d:如果需要排序用数字标示。
xxxx:标示文件含义
A：
    tab：标示文件是隐藏在切换tab中
    heard 标示头部文件
    bak:标示备份
    tab 是不显示在左边的动作
如果 A 是 tab
B： web 
   ios  
   Android
如果 A 是 heard 就不用B
如果 A 是 bak B就是备份日期
###示例
eg:bss_service_api.md
eg:1_history_version.md
eg:0_screen_sharing_heard.md    tab切换的父页
eg:0_screen_sharing_tab_web.md  tab切换
   