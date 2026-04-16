# Judge Analyzer 法官行為分析器

Interactive single-file HTML tool for deep-dive judicial behavioral profiling, designed for Taiwan's legal system.

## Features

- **5 Analysis Dimensions**: Ruling tendencies, sentencing/award patterns, legal philosophy, language & tone, outlier detection
- **Taiwan Legal System**: Supports 民事/刑事/行政 categories, court hierarchy (地方法院/高等法院/最高法院)
- **Smart Data Extraction**: Auto-extracts from 裁判全文 — sentences (有期徒刑/拘役), awards (新臺幣), cited statutes & precedents
- **Chinese Numeral Support**: Handles 壹貳參肆伍陸柒捌玖拾佰仟萬 conversion
- **AI Integration**: Optional Claude/OpenAI API for deeper analysis
- **Predictive Insights**: Predict likely rulings based on historical patterns
- **Chart.js Visualizations**: Interactive charts and dashboard

## Usage

Open `judge-analyzer.html` in any modern browser. No server required.

### Input Methods
1. Paste raw judgment text (裁判全文)
2. Upload JSON/CSV from Taiwan's Judicial Yuan (judgment.judicial.gov.tw)
3. Use built-in demo data

## License

MIT
