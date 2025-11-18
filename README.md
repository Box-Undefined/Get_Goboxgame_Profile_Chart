# 神奇代码岛图表生成
<h>示例</h>
<div id="0eea6e101984404d82243fdd2a9c91bd" class="chart-container" style="width:900px; height:500px; "></div>
<script>
        var chart_0eea6e101984404d82243fdd2a9c91bd = echarts.init(
            document.getElementById('0eea6e101984404d82243fdd2a9c91bd')， 'white'， {renderer: 'canvas'， locale: 'ZH'});
            document.body。style。backgroundColor = "#0d0d0d"
        var option_0eea6e101984404d82243fdd2a9c91bd = {
    "animation": true，
    "animationThreshold": 2000，
    "animationDuration": 1000，
    "animationEasing": "cubicOut"，
    "animationDelay": 0，
    "animationDurationUpdate": 300，
    "animationEasingUpdate": "cubicOut"，
    "animationDelayUpdate": 0，
    "aria": {
        "enabled": false
    }，
    "color": [
        "#5470c6"，
        "#91cc75"，
        "#fac858"，
        "#ee6666"，
        "#73c0de"，
        "#3ba272"，
        "#fc8452"，
        "#9a60b4"，
        "#ea7ccc"
    ]，
    "series": [
        {
            "type": "radar"，
            "name": "\u4e0d\u77e5\u540d\u7684\u5c51Box"，
            "data": [
                {
                    "value": [
                        179，
                        117，
                        70，
                        14，
                        130420，
                        22，
                        31555
                    ]，
                    "name": "\u6570\u636e\u6307\u6807"
                }
            ]，
            "symbol": "circle"，
            "label": {
                "show": true，
                "margin": 8，
                "valueAnimation": false
            }，
            "selectedMode": false，
            "zlevel": 0，
            "z": 2，
            "itemStyle": {
                "normal": {
                    "color": "#ffdb00"
                }
            }，
            "lineStyle": {
                "show": true，
                "width": 2.5，
                "opacity": 0.8，
                "curveness": 0，
                "type": "solid"，
                "color": "#ffdb00"
            }，
            "areaStyle": {
                "opacity": 0.15，
                "color": "#ffdb00"
            }
        }，
        {
            "type": "radar"，
            "name": "\u6e90\u7801\u6781\u5ba2"，
            "data": [
                {
                    "value": [
                        290，
                        32，
                        19，
                        13，
                        14157，
                        32，
                        361
                    ]，
                    "name": "\u6570\u636e\u6307\u6807"
                }
            ]，
            "symbol": "circle"，
            "label": {
                "show": true，
                "margin": 8，
                "valueAnimation": false
            }，
            "selectedMode": false，
            "zlevel": 0，
            "z": 2，
            "itemStyle": {
                "normal": {
                    "color": "#06a6e5"
                }
            }，
            "lineStyle": {
                "show": true，
                "width": 2.5，
                "opacity": 0.8，
                "curveness": 0，
                "type": "solid"，
                "color": "#06a6e5"
            }，
            "areaStyle": {
                "opacity": 0.15，
                "color": "#06a6e5"
            }
        }
    ]，
    "legend": [
        {
            "data": [
                "\u4e0d\u77e5\u540d\u7684\u5c51Box"，
                "\u6e90\u7801\u6781\u5ba2"
            ]，
            "selected": {}，
            "show": true，
            "padding": 5，
            "itemGap": 10，
            "itemWidth": 25，
            "itemHeight": 14，
            "textStyle": {
                "color": "#EEEEEE"
            }，
            "backgroundColor": "transparent"，
            "borderColor": "#ccc"，
            "borderRadius": 0，
            "pageButtonItemGap": 5，
            "pageButtonPosition": "end"，
            "pageFormatter": "{current}/{total}"，
            "pageIconColor": "#2f4554"，
            "pageIconInactiveColor": "#aaa"，
            "pageIconSize": 15，
            "animationDurationUpdate": 800，
            "selector": false，
            "selectorPosition": "auto"，
            "selectorItemGap": 7，
            "selectorButtonGap": 10
        }
    ]，
    "tooltip": {
        "show": true，
        "trigger": "item"，
        "triggerOn": "mousemove|click"，
        "axisPointer": {
            "type": "line"
        }，
        "showContent": true，
        "alwaysShowContent": false，
        "showDelay": 0，
        "hideDelay": 100，
        "enterable": false，
        "confine": false，
        "appendToBody": false，
        "transitionDuration": 0.4，
        "textStyle": {
            "fontSize": 14
        }，
        "borderWidth": 0，
        "padding": 5，
        "order": "seriesAsc"
    }，
    "radar": [
        {
            "indicator": [
                {
                    "name": "\u7c89\u4e1d"，
                    "max": 263，
                    "min": 0
                }，
                {
                    "name": "\u5173\u6ce8"，
                    "max": 128.70000000000002，
                    "min": 0
                }，
                {
                    "name": "\u597d\u53cb"，
                    "max": 89.5，
                    "min": 0
                }，
                {
                    "name": "\u5730\u56fe\u603b\u6570"，
                    "max": 50，
                    "min": 0
                }，
                {
                    "name": "\u6700\u4f73\u5730\u56fe\u6e38\u73a9\u91cf"，
                    "max": 143462，
                    "min": 0
                }，
                {
                    "name": "\u6a21\u578b\u603b\u6570"，
                    "max": 50，
                    "min": 0
                }，
                {
                    "name": "\u6700\u4f73\u6a21\u578b\u6d4f\u89c8\u91cf"，
                    "max": 34710，
                    "min": 0
                }
            ]，
            "shape": "circle"，
            "startAngle": 90，
            "name": {
                "textStyle": {
                    "color": "#A0A0A0"
                }
            }，
            "splitLine": {
                "show": true，
                "lineStyle": {
                    "show": true，
                    "width": 1，
                    "opacity": 1，
                    "curveness": 0，
                    "type": "solid"，
                    "color": "rgba(128, 128, 128, 0.4)"
                }
            }，
            "splitArea": {
                "show": true，
                "areaStyle": {
                    "opacity": 0
                }
            }，
            "axisLine": {
                "show": true，
                "onZero": true，
                "onZeroAxisIndex": 0，
                "lineStyle": {
                    "show": true，
                    "width": 1.5，
                    "opacity": 1，
                    "curveness": 0，
                    "type": "solid"，
                    "color": "rgba(128， 128， 128， 0.5)"
                }
            }
        }
    ]，
    "title": [
        {
            "show": true，
            "text": "\u4e0d\u77e5\u540d\u7684\u5c51Box\u7684\u4e2a\u4eba\u6570\u636e\u5206\u6790"，
            "target": "blank"，
            "subtarget": "blank"，
            "padding": 5，
            "itemGap": 10，
            "textAlign": "auto"，
            "textVerticalAlign": "auto"，
            "triggerEvent": false
        }
    ]，
    "toolbox": {
        "show": true，
        "orient": "horizontal"，
        "itemSize": 15，
        "itemGap": 10，
        "left": "80%"，
        "feature": {
            "saveAsImage": {
                "type": "png"，
                "backgroundColor": "auto"，
                "connectedBackgroundColor": "#fff"，
                "show": true，
                "pixelRatio": 1
            }，
            "restore": {
                "show": true
            }，
            "dataView": {
                "show": true，
                "readOnly": false，
                "backgroundColor": "#fff"，
                "textareaColor": "#fff"，
                "textareaBorderColor": "#333"，
                "textColor": "#000"，
                "buttonColor": "#c23531"，
                "buttonTextColor": "#fff"
            }，
            "dataZoom": {
                "show": true，
                "title": {}，
                "icon": {}，
                "filterMode": "filter"
            }，
            "magicType": {
                "show": true，
                "type": [
                    "line"，
                    "bar"，
                    "stack"，
                    "tiled"
                ]，
                "title": {}，
                "icon": {}
            }
        }
    }
};
        chart_0eea6e101984404d82243fdd2a9c91bd.setOption(option_0eea6e101984404d82243fdd2a9c91bd);
</script>
