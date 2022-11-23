---
description: 圖表串接
---

# TradingView 圖表

使用 React 進行圖表串接。

{% embed url="https://tradingview.github.io/lightweight-charts/docs" %}

範例：

<figure><img src="../.gitbook/assets/截圖 2022-11-23 上午10.33.00.png" alt=""><figcaption></figcaption></figure>

```javascript
import { useEffect, useRef } from "react";
import { createChart } from "lightweight-charts";

// 範例資料
// priceHistoryData:
// [{
//   TIMESTAMP: "2021-11-30 09:59:00.000"
//   price: 2322
// }]

// swapHistoryData:
// [{
//   act:  "B",
//   block_time: "2022-08-26 07:03:32.000"
// }, ...]


// tradingView chart 使用 timestamp in seconds 顯示，而不是 timestamp in milliseconds
const PriceLineChart = ({ priceHistoryData, swapHistoryData }) => {
  const txDateList = [
    ...new Set(
      swapHistoryData.map(
        (_data) =>
          `${Date.parse(_data?.block_time.replace(":00.000", "")) / 1000}-${ // timestamp in seconds
            _data?.act
          }`
      )
    ),
  ];
  const isSellSignal = (s) => {
    return s.indexOf("S") !== -1;
  };
  const labelList = txDateList.map((_data) => ({
    time: Number(_data.slice(0, 10)) + 60 * 60 * 8,
    position: isSellSignal(_data) ? "aboveBar" : "belowBar",
    color: isSellSignal(_data) ? "#e91e63" : "Green",
    shape: isSellSignal(_data) ? "arrowDown" : "arrowUp",
    text: isSellSignal(_data) ? "Sell" : "Buy",
  }));

  const chartRef = useRef();

  useEffect(() => {
    const chart = createChart(chartRef.current, {
      width: chartRef.current.clientWidth,
      height: 300,
    });
    chart.applyOptions({
      timeScale: {
        // Adds hours and minutes scaling ability to the chart.
        timeVisible: true,
        secondsVisible: false,
      },
    });
    const lineSeries = chart.addLineSeries();
    lineSeries.setData(
      priceHistoryData.map((_data) => ({
        time: Date.parse(_data?.TIMESTAMP.replace(":00.000", "")) / 1000, // timestamp in seconds
        value: _data?.price,
      }))
    );
    lineSeries.setMarkers(labelList);
    return () => {
      chart.remove(); // 避免 Re-render 產生多個重複圖表
    };
  }, [priceHistoryData, swapHistoryData]);
  return (
    <div
      style={{
        width: "100%",
        height: "100%",
      }}
      ref={chartRef}
    ></div>
  );
};

export default PriceLineChart;

```
