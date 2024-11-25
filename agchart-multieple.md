# 

## AG Charts
https://www.ag-grid.com/charts/react/create-a-basic-chart/

- type : chat 의 형태 (bar, line, area, pie ...)
- data : chat 의 data ```{ month: "Jan", avgTemp: 2.3, iceCreamSales: 162000 }```
- series : type 이 포함됨. data 의 key 가 value 로 매칭됨
  - ```{type: "bar", xKey: "month", yKey: "avgTemp"}``` 

### e.g.
```javascript
import { AgCharts } from "ag-charts-react";

// Chart Options: Control & configure the chart
const [chartOptions, setChartOptions] = useState({
    // Data: Data to be displayed in the chart
    data: [
        { month: 'Jan', avgTemp: 2.3, iceCreamSales: 162000 },
        { month: 'Mar', avgTemp: 6.3, iceCreamSales: 302000 },
        { month: 'May', avgTemp: 16.2, iceCreamSales: 800000 },
        { month: 'Jul', avgTemp: 22.8, iceCreamSales: 1254000 },
        { month: 'Sep', avgTemp: 14.5, iceCreamSales: 950000 },
        { month: 'Nov', avgTemp: 8.9, iceCreamSales: 200000 },
    ],
    // Series: Defines which chart type and data to use
    series: [{ type: 'bar', xKey: 'month', yKey: 'iceCreamSales' }],
});

return (
    // AgCharts component with options passed as prop
    <AgCharts options={chartOptions} />
);
```

### 개발요건
- n개의 차트를 보여주는 대쉬보드가 있음
- 차트영역별로 chart type 과 data 를 선택가능
- 영역별 차트 설정 후 저장 (DB 관리? 상태관리?)

### Set status
```javascript
const [chartType1, setChartType1] = useState('bar');
const [chartType2, setChartType2] = useState('line');
const [chartType3, setChartType3] = useState('area');
...
```

### Select options
```javascript
const chartOptions1 = [
    {label: '선택안함', value: 'none'},
    {label: 'chart1', value: 'bar'},
    {label: 'chart2', value: 'line'},
];

const chartOptions2 = [
  {label: '선택안함', value: 'none'},
  {label: 'chart1', value: 'bar'},
  {label: 'chart2', value: 'line'},
];
...
```

### Select 구조
```javascript
const handleChartTypeChange = (value) => {
  setChartType1(value);
};

<Select
  suffixIcon={<BsChevronDown />}
  options={chartOptions1}
  placeholder="차트를 선택하세요"
  onChange={(value) =>
    handleChartTypeChange(value)
  }
/>
```

- 중복된 코드
- 공통화 필요

### 리팩토링

```javascript
// state
const [chartTypes, setChartTypes] = useState({
  chart1: '',
  chart2: '',
  chart3: '',
  chart4: '',
  chart5: '',
  chart6: '',
});

// options
const chartOptions = {
  1: [
    { label: '선택안함', value: 'none' },
    { label: 'Bar', value: 'bar' },
    { label: 'Line', value: 'line' },
    { label: 'Area', value: 'area' },
    { label: 'Pie', value: 'pie' },
    { label: 'Donut', value: 'donut' },
    { label: 'Bubble', value: 'bubble' },
  ],
  2: [
    { label: '선택안함', value: 'none' },
    { label: 'Bar', value: 'bar' },
    { label: 'Line', value: 'line' },
    { label: 'Area', value: 'area' },
    { label: 'Pie', value: 'pie' },
    { label: 'Donut', value: 'donut' },
    { label: 'Bubble', value: 'bubble' },
  ],
};

const compOptions = [
  { label: '선택안함', value: 'none' },
  { label: '법인1', value: 'comp1' },
  { label: '법인2', value: 'comp2' },
];

// 
const chartFormItems = [
  { key: '1', options: chartOptions[1] },
  { key: '2', options: chartOptions[2] },
  { key: '3', options: chartOptions[3] },
  { key: '4', options: chartOptions[4] },
  { key: '5', options: chartOptions[5] },
  { key: '6', options: chartOptions[6] },
]

//
{chartFormItems &&
  chartFormItems.map((item, index) => (
    <Form.Item key={index}>
      <Form.Item
        name={`cboChart${index + 1}`}
        initialValue="none"
      >
        <Select
          suffixIcon={<BsChevronDown />}
          options={item.options}
          placeholder="차트를 선택하세요"
          value={
            chartTypes[
              `chart${item.key}`
              ]
          }
          onChange={(value) =>
            handleChartTypeChange(
              value,
              item.key,
            )
          }
        />
      </Form.Item>
      <Form.Item
        name={`cboCompanyOfChart${index + 1}`}
        initialValue="none"
      >
        <Select
          suffixIcon={<BsChevronDown />}
          options={compOptions}
          placeholder="법인을 선택하세요"
        />
      </Form.Item>
      <div className="chart-area">
        <AgChartExample
          type={
            chartTypes[
              `chart${item.key}`
              ]
          }
        />
      </div>
    </Form.Item>
))}
```


### AG Chart Component
```javascript
import { AgCharts } from 'ag-charts-react';

function AgChartExample({ type }) {
  const noDataOverlay = () => {
    return `<div class="chart-no-data"><em>No data to display</em>
        </div>`;
  };
  const getData = (chartType) => {
    switch (chartType) {
      case 'bar':
      case 'line':
      case 'area':
        return [
          { month: 'Jan', iceCreamSales: 162000 },
          { month: 'Mar', iceCreamSales: 302000 },
          { month: 'May', iceCreamSales: 800000 },
          { month: 'Jul', iceCreamSales: 1254000 },
          { month: 'Sep', iceCreamSales: 950000 },
          { month: 'Nov', iceCreamSales: 200000 },
        ];
      case 'pie':
      case 'donut':
        return [
          { category: 'Chocolate', iceCreamSales: 63000 },
          { category: 'Vanilla', iceCreamSales: 44000 },
          { category: 'Strawberry', iceCreamSales: 23000 },
          { category: 'Mint', iceCreamSales: 18000 },
          { category: 'Other', iceCreamSales: 52000 },
        ];
      case 'bubble':
        return [
          {
            flavor: 'Chocolate',
            popularity: 8,
            sales: 63000,
            temperature: 25,
          },
          {
            flavor: 'Vanilla',
            popularity: 7,
            sales: 44000,
            temperature: 28,
          },
          {
            flavor: 'Strawberry',
            popularity: 6,
            sales: 23000,
            temperature: 30,
          },
        ];
      default:
        return [];
    }
  };

  const chartOptions = useMemo(() => {
    const data = getData(type);
    let seriesOptions;

    switch (type) {
      case 'bar':
      case 'line':
      case 'area':
        seriesOptions = [
          {
            type,
            xKey: 'month',
            yKey: 'iceCreamSales',
          },
        ];
        break;
      case 'pie':
        seriesOptions = [
          {
            type,
            angleKey: 'iceCreamSales',
          },
        ];
        break;
      case 'donut':
        seriesOptions = [
          {
            type,
            angleKey: 'iceCreamSales',
            innerRadiusRatio: 0.7,
          },
        ];
        break;
      case 'bubble':
        seriesOptions = [
          {
            type: 'bubble',
            xKey: 'popularity',
            yKey: 'sales',
            sizeKey: 'temperature',
            title: 'Flavor',
            labelKey: 'flavor',
          },
        ];
        break;
      default:
        seriesOptions = [
          {
            type: 'bar',
            xKey: 'month',
            yKey: 'iceCreamSales',
          },
        ];
    }

    return {
      data: data,
      series: seriesOptions,
      overlays: {
        noData: {
          renderer: noDataOverlay,
        },
      },
      title: {
        text: `Ice Cream Sales by Month (${type.charAt(0).toUpperCase() + type.slice(1)} Chart)`,
      },
    };
  }, [type]);

  return (
          <>
            <AgCharts style={{ height: '100%' }} options={chartOptions} />
          </>
  );
}
```