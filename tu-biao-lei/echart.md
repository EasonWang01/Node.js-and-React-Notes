---
description: https://echarts.apache.org/examples/zh/
---

# Echart

### React Component 範例：

```javascript
import * as echarts from "echarts/core";
import { GridComponent } from "echarts/components";
import { LineChart } from "echarts/charts";
import { UniversalTransition } from "echarts/features";
import { CanvasRenderer } from "echarts/renderers";

import { useEffect, useRef } from "react";

const LineAreaChart = ({ data }) => {
  const chartRef = useRef();
  useEffect(() => {
    echarts.use([
      GridComponent,
      LineChart,
      CanvasRenderer,
      UniversalTransition,
    ]);

    var myChart = echarts.init(chartRef.current);
    var option;

    option = {
      xAxis: {
        type: "category",
        boundaryGap: false,
        data: ["2022-08", "2022-09", "2022-10", "2022-11", "2022-12"],
      },
      yAxis: {
        type: "value",
      },
      series: [
        {
          showSymbol: false,
          data: [820, 932, 901, 934, 1290, 123],
          type: "line",
          areaStyle: {},
          smooth: true,
        },
      ],
    };
    option && myChart.setOption(option);
  }, [data]);
  return (
    <div
      style={{
        width: "100%",
        height: "100%",
      }}
      id="LineAreaChart"
      ref={chartRef}
    ></div>
  );
};

export default LineAreaChart;
```

### 隱藏 X 與 Y 軸標籤與軸線

```javascript
option = {
      xAxis: {
        axisTick: {
          show: false, // 座標刻度線
        },
        axisLine: {
          show: false, // 座標軸線
        },
        axisLabel: {
          show: false, // 座標軸文字
        },
        splitLine: {
          show: false, // 網格線
        },
        .....
      }
....
```

### 移除 Canvas 預設邊界 padding

e-chart 預設會有空白邊界，可使用以下移除或是調整。

<pre class="language-javascript"><code class="lang-javascript">option = {
<strong>  grid: {
</strong>    left: 0,
    top: 0,
    right: 0,
    bottom: 0
  },
}
</code></pre>
