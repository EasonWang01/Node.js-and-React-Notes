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

## 使用公開數據源 Widget

如果是 TradingView 本身提供的數據，可以使用他們的 widget 圖表，為免費

{% embed url="https://www.tradingview.com/widget/advanced-chart/" %}

範例：

WidgetContainer

```javascript
import { useEffect, useState } from "react";
import axios from "axios";
import styles from "./index.module.scss";
import WidgetComponent from "../../components/WidgetComponent";

const pairs = ["BTCUSDT", "ETHUSDT"];

const WidgetContainer = () => {
  const [selectedPair, setSelectedPair] = useState(pairs[0]);
  return (
    <div className={styles.container}>
      <div className={styles.top_pairTagContainer}>
        <div style={{ color: "#999", fontSize: "16px", fontWeight: "600" }}>
          Popular Trading Pairs
        </div>
        {pairs.map((pair) => (
          <div
            onClick={() => {
              setSelectedPair(pair);
            }}
            className={`${styles.pairTag} ${
              selectedPair === pair && styles.pairTag_active
            }`}
          >
            {pair}
          </div>
        ))}
      </div>
      <div
        style={{
          width: "70%",
          height: "641px",
        }}
      >
        <WidgetComponent selectedPair={selectedPair} />
      </div>
    </div>
  );
};

export default WidgetContainer;
```

WidgetComponent

```javascript
import React, { useEffect, useRef } from 'react';

let tvScriptLoadingPromise;

export default function TradingViewWidget({ selectedPair }) {
  const onLoadScriptRef = useRef();

  useEffect(
    () => {
      console.log('selectedPair', selectedPair)
      onLoadScriptRef.current = createWidget;

      if (!tvScriptLoadingPromise) {
        tvScriptLoadingPromise = new Promise((resolve) => {
          const script = document.createElement('script');
          script.id = 'tradingview-widget-loading-script';
          script.src = 'https://s3.tradingview.com/tv.js';
          script.type = 'text/javascript';
          script.onload = resolve;

          document.head.appendChild(script);
        });
      }

      tvScriptLoadingPromise.then(() => onLoadScriptRef.current && onLoadScriptRef.current());

      return () => onLoadScriptRef.current = null;

      function createWidget() {
        if (document.getElementById('tradingview_ff115') && 'TradingView' in window) {
          new window.TradingView.widget({
            width: '100%',
            height: 641,
            symbol: `BIFINEX:${selectedPair}`,
            interval: "D",
            timezone: "Etc/UTC",
            theme: "light",
            style: "1",
            locale: "en",
            toolbar_bg: "#f1f3f6",
            enable_publishing: false,
            allow_symbol_change: true,
            container_id: "tradingview_ff115"
          });
        }
      }
    },
    [selectedPair]
  );

  return (
    <div className='tradingview-widget-container'>
      <div id='tradingview_ff115' />
      <div className="tradingview-widget-copyright">
        <a href="https://www.tradingview.com/symbols/BTCUSDT/?exchange=BITFINEX" rel="noopener" target="_blank"><span className="blue-text">BTCUSDT chart</span></a> by TradingView
      </div>
    </div>
  );
}
```
