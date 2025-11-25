# 神奇代码岛(*dao3.fun*)获取个人数据并生成图表
- 获取项目仅需输入ID，**无需逐个输入**
- 支持与他人数据**对比**
- 使用*Pyecharts*渲染，输出为 .*html* 文件，功能较多
- 运行文件为***main.py***
## 效果展示
输入界面
![输入界面](/input.png "Image by VSCode")
渲染效果
![渲染效果](/preview.png "Image by Pyecharts snapshot")
## 代码展示
``` Python
# -----------------------------------------------------
# Copyright ©2025 by 不知名的屑Box. All Rights Reserved.
# -----------------------------------------------------

from pyecharts.charts import Radar
from pyecharts import options as opts

from requests import *
import os, json

# Input ID number to get your PROFILE url
while(1):
    try:
        print("\033[38;5;8m欢迎使用神奇代码岛(dao3.fun)个人数据分析生成器\033[0;0m")
        ID1 = int(input('请输入你的 ' + "\033[38;5;220mID\033[0;0m" + ' \n'))
        if ID1 == 0: ID1 = 1/0
        ID2 = int(input('请输入你将要对比的 ' + "\033[38;5;220mID\033[0;0m" + ' （无请输入0）\n'))
        break
    except: 
        os.system('cls')
        print("\033[38;5;9m请输入正确的ID\033[0;0m")
        

# Max NUMBER
def qianli_num(m, n, x):
    m = float(m)
    n = float(n)
    x = float(x)
    return max(int(x*(1+0.5*((n-m*x)/(n+m*x)))), x)
def get_info(ID):
    # Get your Info
    DAO3 = {
        "followers": json.loads(get("https://code-api-pc.dao3.fun/follow/relation-info/"+str(ID)).text), 
        "infos": json.loads(get("https://code-api-pc.dao3.fun/user/profile/"+str(ID)).text), 
        "maps": json.loads(get("https://code-api-pc.dao3.fun/user/experience?limit=32&offset=0&userId="+str(ID)).text),
        "models": json.loads(get("https://code-api-pc.dao3.fun/user/models?limit=32&offset=0&userId="+str(ID)).text)
        }

    # Dao3 simple infos
    INFO = {
        "followers": DAO3["followers"]['data']["followerNum"],
        "following": DAO3["followers"]['data']["followingNum"],
        "friends": DAO3["followers"]['data']["friendsNum"],
        "name": DAO3["infos"]['data']["nickname"],
        "maps": len(DAO3["maps"]['data']["rows"]),
        "models": len(DAO3["models"]['data']["rows"]),
        "count": 0,
    }
    try:INFO["best_map"] = {"name": DAO3["maps"]['data']["rows"][0]["name"], "click": DAO3["maps"]['data']["rows"][0]["playCount"]}
    except:INFO["best_map"] = {"name": '"暂无数据"', "click": 0}
    try:INFO["best_model"] = {"name": DAO3["models"]['data']["rows"][0]["title"], "click": DAO3["models"]['data']["rows"][0]["viewCount"]}
    except:INFO["best_model"] = {"name": '"暂无数据"', "click": 0}

    for i in DAO3["models"]['data']["rows"]:
        INFO["count"] += i["viewCount"]
    for j in DAO3["maps"]['data']["rows"]:
        INFO["count"] += j["playCount"]

    # Ready to set
    datas = [
        {
            "value": [
                int(INFO["followers"]), 
                int(INFO["following"]), 
                int(INFO["friends"]), 
                int(INFO["maps"]), 
                int(INFO["best_map"]["click"]), 
                int(INFO["models"]), 
                int(INFO["best_model"]["click"])
            ], 
            "name": "数据指标"
        }
    ]
    return datas, INFO


# Set THEME
COLORS = ["#ffdb00", "#06a6e5"]
init_opts = opts.InitOpts(
    page_title = "神奇代码岛数据分析",
    theme = COLORS[0]
)

# Datas
data1 = get_info(ID1)

# Build a CHART
radar_schema = [
    {"name": "粉丝",     "max": qianli_num(data1[1]["maps"]+data1[1]['models'], data1[1]['count'], data1[1]['followers']),   "min": 0},
    {"name": "关注",     "max": data1[1]["following"]*1.1,                                                                   "min": 0},
    {"name": "好友",     "max": data1[1]['followers']/2,                                                                     "min": 0},
    {"name": "地图总数",  "max": max(50, data1[1]['maps']),                                                                   "min": 0},
    {"name": f"最佳地图游玩量",      "max": int(float(data1[1]['best_map']['click'])*1.1),                                     "min": 0},
    {"name": "模型总数",  "max": max(50, data1[1]['models']),                                                                 "min": 0},
    {"name": f"最佳模型浏览量",    "max": int(float(data1[1]["best_model"]["click"]*1.1)),                                     "min": 0}
]
radar = (
    # 雷达图
    Radar()
    # 初始数据
    .add_schema(
        shape="circle",
        schema=radar_schema,
        textstyle_opts=opts.TextStyleOpts(color="#A0A0A0"),
        axisline_opt=opts.AxisLineOpts(
            linestyle_opts=opts.LineStyleOpts(color="rgba(128, 128, 128, 0.5)", width=1.5)
        ),
        splitline_opt=opts.SplitLineOpts(
            linestyle_opts=opts.LineStyleOpts(color="rgba(128, 128, 128, 0.4)", width=1),
            is_show=True
        ),
    )
    .add(
        series_name=data1[1]["name"],
        data=data1[0],
        color=COLORS[0],
        linestyle_opts=opts.LineStyleOpts(color=COLORS[0], width=2.5, opacity=0.8),
        areastyle_opts=opts.AreaStyleOpts(color=COLORS[0], opacity=0.15),
        symbol="circle",
    )
    .set_global_opts(
        legend_opts=opts.LegendOpts( textstyle_opts=opts.TextStyleOpts(color="#EEEEEE"), is_show=True ),
        title_opts=opts.TitleOpts(title=f"{data1[1]["name"]}的个人数据分析"),
        toolbox_opts=opts.ToolboxOpts(is_show=True) # 显示工具箱，可保存图片等
    )
    # 设置背景颜色为 #0d0d0d
    .add_js_funcs('document.body.style.backgroundColor = "#0d0d0d"')
)

if(ID2 != 0): 
    data2 = get_info(ID2)
    radar.add(
        series_name=data2[1]["name"],
        data=data2[0],
        color=COLORS[1],
        linestyle_opts=opts.LineStyleOpts(color=COLORS[1], width=2.5, opacity=0.8),
        areastyle_opts=opts.AreaStyleOpts(color=COLORS[1], opacity=0.15),
        symbol="circle",
    )
radar.render("神奇代码岛数据分析.html")

```
