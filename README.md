# 神奇代码岛(*dao3.fun*)获取个人数据并生成图表
- 获取项目仅需输入ID，**无需逐个输入**
- 支持与他人数据**对比**
- 使用*Pyecharts*渲染，输出为 .*html* 文件，功能较多
- 运行文件为***main.py***

## 效果展示
![渲染效果](/IOsnapst.png)
## 更新日志
- 上一个版本（v1.2内测版）
- 本次版本（v2.0正式版）
- **版本记录**

| 版本 | 日志 |
| :---: | :---: |
| v1.0 | × |
| v1.1 | 微调颜色 |
| v1.2 | 正式版发布 |
| **v2.0** | 调整输入方式 |
## 代码展示
```
python
# ---------------------------------------------------------------------------------------------
# Copyright ©2025 by 不知名的屑Box. All Rights Reserved.
# Licensed under the MIT License. See LICENSE in the project root for license information.
# ---------------------------------------------------------------------------------------------
from pyecharts.charts import Radar
from pyecharts import options as opts

import tkinter as tk
from tkinter import messagebox

import os, json, sys, requests

# Input ID number to get your PROFILE url
def get_ids_via_tk():
    root = tk.Tk()
    root.title("神奇代码岛(dao3.fun) - 输入用户 ID")
    root.resizable(False, False)
    try:
        root.geometry("360x150")
        root.eval('tk::PlaceWindow %s center' % root.winfo_toplevel())
    except Exception:
        pass

    tk.Label(root, text="主用户 ID（必填，正整数）:", anchor="w").grid(row=0, column=0, padx=12, pady=(12, 2), sticky="w")
    id1_var = tk.StringVar()
    entry_id1 = tk.Entry(root, textvariable=id1_var, width=30)
    entry_id1.grid(row=1, column=0, padx=12, sticky="w")

    entry_id1.focus_set()
    tk.Label(root, text="对比用户 ID（可选，留空或填 0 表示不对比）:", anchor="w").grid(row=2, column=0, padx=12, pady=(10, 2), sticky="w")
    id2_var = tk.StringVar()
    entry_id2 = tk.Entry(root, textvariable=id2_var, width=30)
    entry_id2.grid(row=3, column=0, padx=12, sticky="w")

    def validate_digits(P):
        return P.isdigit() or P == ""
    vcmd = (root.register(validate_digits), '%P')
    entry_id1.config(validate="key", validatecommand=vcmd)
    entry_id2.config(validate="key", validatecommand=vcmd)

    result = {}

    def on_ok(event=None):
        v1 = id1_var.get().strip()
        v2 = id2_var.get().strip()
        if v1 == "":
            messagebox.showerror("输入错误", "主用户 ID 为必填项。")
            return
        try:
            i1 = int(v1)
            if i1 <= 0:
                raise ValueError("主用户 ID 必须为正整数。")
        except Exception as e:
            messagebox.showerror("输入错误", f"主用户 ID 无效：{e}")
            return
        if v2 == "" or v2 == "0":
            i2 = 0
        else:
            try:
                i2 = int(v2)
                if i2 < 0:
                    raise ValueError("对比 ID 不能为负数。")
            except Exception as e:
                messagebox.showerror("输入错误", f"对比用户 ID 无效：{e}")
                return
        result['id1'] = i1
        result['id2'] = i2
        root.destroy()
    def on_cancel():
        if messagebox.askyesno("取消", "确定要退出吗？"):
            root.destroy()
            result['cancel'] = True

    btn_frame = tk.Frame(root)
    btn_frame.grid(row=4, column=0, pady=12)
    ok_btn = tk.Button(btn_frame, text="确定", width=10, command=on_ok)
    ok_btn.pack(side="left", padx=6)
    cancel_btn = tk.Button(btn_frame, text="取消", width=10, command=on_cancel)
    cancel_btn.pack(side="left", padx=6)

    root.bind("<Return>", on_ok)
    root.bind("<Escape>", lambda e: on_cancel())

    root.mainloop()
    return result

_res = get_ids_via_tk()

if not _res or _res.get('cancel'):
    print("已取消。")
    raise SystemExit(0)

ID1 = _res['id1']
ID2 = _res['id2']

# Max NUMBER
def qianli_num(m, n, x):
    m = float(m)
    n = float(n)
    x = float(x)
    denom = (n + m * x)
    if abs(denom) < 1e-9:
        return int(x)
    val = int(x * (1 + 0.5 * ((n - m * x) / denom)))
    return max(val, int(x))

def get_json(session, url, timeout=8):
    resp = session.get(url, timeout=timeout)
    resp.raise_for_status()
    return resp.json()

def get_info(ID):
    # Get your Info
    s = requests.Session()
    try:
        DAO3 = {
            'followers': get_json(s, f"https://code-api-pc.dao3.fun/follow/relation-info/{ID}"),
            'infos': get_json(s, f"https://code-api-pc.dao3.fun/user/profile/{ID}"),
            'maps': get_json(s, f"https://code-api-pc.dao3.fun/user/experience?limit=32&offset=0&userId={ID}"),
            'models': get_json(s, f"https://code-api-pc.dao3.fun/user/models?limit=32&offset=0&userId={ID}")
        }
    finally:
        s.close()

    # Dao3 simple infos
    INFO = {
        "followers": DAO3["followers"]['data'].get("followerNum", 0),
        "following": DAO3["followers"]['data'].get("followingNum", 0),
        "friends": DAO3["followers"]['data'].get("friendsNum", 0),
        "name": DAO3["infos"]['data'].get("nickname", f"ID_{ID}"),
        "maps": len(DAO3["maps"]['data'].get("rows", [])),
        "models": len(DAO3["models"]['data'].get("rows", [])),
        "count": 0,
    }
    try:
        first_map = DAO3["maps"]['data'].get("rows", [])[0]
        INFO["best_map"] = {"name": first_map.get("name", '"暂无数据"'), "click": first_map.get("playCount", 0)}
    except Exception:
        INFO["best_map"] = {"name": '"暂无数据"', "click": 0}
    try:
        first_model = DAO3["models"]['data'].get("rows", [])[0]
        INFO["best_model"] = {"name": first_model.get("title", '"暂无数据"'), "click": first_model.get("viewCount", 0)}
    except Exception:
        INFO["best_model"] = {"name": '"暂无数据"', "click": 0}

    for i in DAO3["models"]['data'].get("rows", []):
        INFO["count"] += i.get("viewCount", 0)
    for j in DAO3["maps"]['data'].get("rows", []):
        INFO["count"] += j.get("playCount", 0)

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
    {"name": "最佳地图游玩量",      "max": int(float(data1[1]['best_map']['click'])*1.1),                                      "min": 0},
    {"name": "模型总数",  "max": max(50, data1[1]['models']),                                                                 "min": 0},
    {"name": "最佳模型浏览量",    "max": int(float(data1[1]["best_model"]["click"]*1.1)),                                      "min": 0}
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
