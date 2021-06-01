
<br/>

D3.js
===================

Probably the most popular interactive DataViz library helping us to bring data to life using HTML, SVG, and CSS.  
It's a solid and low-level data visualization library, being used as a core of the many others libraries.  
The D3 team/community has published hundreds of examples and tutorials over the last 10 years (you are not alone!)  
https://d3js.org  
https://observablehq.com/@d3/gallery

React.js is a great library for developing web applications.  
D3 and React should be a pretty good choice to create a rich user interface for data visualization.  
There some options to integrate a D3.js chart within a React component without start a fight to control the DOM.  
Of course, each approach has both its own positive and negative side.

### D3 within a React component
This method is probably the most popular and simple way to integrate D3 within a React component.  
D3.js controls the DOM, but the mix with React code is not so good when the component becomes bigger.  
```javascript
import { useEffect, useRef } from "react"
import * as d3 from "d3"

export function BarChart({ data }) {
    const containerRef = useRef()
    
    useEffect(() => {
        const svg = d3.select(containerRef.current)
        svg.attr("viewBox", "0 0 100 100")
        const g = svg.append("g").attr("class", "group-bars")
        // ...
        g.select("rect.bar").on("click", event => { /* ... */ })
        // ...
    }, [data])

    return <div ref={ containerRef } />
}
```

### D3 chart on Facade Pattern
We can encapsulate our D3.js chart and export a few methods with a simple API.  
This method focuses on a clear separation between D3 and React code.  
Maintenance will be easier, flexible to use in another framework, and reuse community examples should be easier too.  
All D3.js features remain intact to use, but we need a well-defined interface to communicate.

```javascript
// only D3.js code (React is not allowed here)
import * as d3 from "d3"

export function createLineChart(container, data, options) {
    const svg = d3.select(container).append("svg")
    svg.attr("viewBox", "0 0 100 100")
    const g = svg.append("g")
    // ...
    g.select("path.line").on("click", onClick)
    // ...
    return {
        update: data => { /* update chart... */ },
        remove: () => { /* remove all subscription listeners... */ }
    }
}
```
```javascript
// only React code (D3 is not allowed here)
import { useEffect, useRef } from "react"
import { createLineChart } from "./createLineChart.js"

export function LineChartComponent({ data }) {
    const containerRef = useRef()
    const chartRef = useRef()

    useEffect(() => {
        if (!chartRef.current) {
            chartRef.current = createLineChart(containerRef.current, data)
        } else {
            chartRef.current.update(data)
        }
    }, [data])

    useEffect(() => {
        return () => chartRef.current.remove()
    }, [])

    return <div ref={ containerRef } />
}
```

### D3 for Math, React for the DOM
In this method, we use D3 as a simple utility library to generate scales, layouts, SVG paths, etc and React to manage the DOM.  
React developers will be happy but at the same time, they will need to reimplement/adapt D3 code to React way.  
D3 transitions should be replaced too by another library as react-spring, for example.
```javascript
import { useEffect, useRef } from "react"
import * as d3 from "d3"

export function LineChart({ data }) {
    const xScale = d3.scaleTime().domain // .domain(d3.max(data, ...
    const yScale = d3.scaleLinear().domain // ...

    const line = d3.line().x(d => xScale(d.date)) // ...
    const linePath = line(data)

    const clickHandler = () => {
        console.log("line clicked")
    }

    return (
        <svg width={ 300 } height={ 200 } viewBox="0 0 100 100">
            <path className="line" d={ linePath } onClick={ clickHandler } />
        </svg>
    )
}
```


<br/>

Libs based on D3.js
===================

### NVD3
Re-usable charts for D3 without taking away the power that D3.js gives you.  
https://nvd3.org/  
https://github.com/novus/nvd3

### Billboard.js
Re-usable, easy interface JavaScript chart library, based on D3 v4+  
https://naver.github.io/billboard.js/demo/#Chart.BubbleChart  
https://github.com/naver/billboard.js

### Plottable
Library of chart components for creating flexible, custom charts. It`s built o top of D3.js  
http://plottablejs.org/examples/  
https://github.com/palantir/plottable

### D3FC
Components to rapidly build fast, highly customisable, interactive charts with D3.js  
https://d3fc.io/  
https://github.com/d3fc/d3fc/blob/master/README.md


<br/>

React Charts
============

### VISX
A collection of reusable low-level components.  
visx combines the power of D3.js to generate your visualization with the benefits of react for updating the DOM.  
https://airbnb.io/visx/gallery  
https://github.com/airbnb/visx

### Victory
React.js components for module charting and data visualization.  
Ecosystem of composable React components.  
https://formidable.com/open-source/victory/gallery  
https://github.com/FormidableLabs/victory

### react-vis
A collection of React components to render common data visualization charts.  
https://uber.github.io/react-vis/examples/showcases/plots  
https://github.com/uber/react-vis

### Rechart
A composable charting library ~ is a redefined chart library built with React and D3.js  
https://recharts.org/en-US/examples  
https://github.com/recharts/recharts

### Regraph
A collection of graph (vertices and edges) related React components.  
https://regraph.js.org/examples/editors/mutli-tool-editor  
https://github.com/Izhaki/regraph

### react-graph-vis
A React component to display beaultiful network graphs using vis.js  
http://crubier.github.io/react-graph-vis/  
https://github.com/crubier/react-graph-vis  
https://visjs.github.io/vis-network/examples/ (lib vis.js)

### Nivo
Nivo provides supercharged React components to easily build dataviz apps, it`s built on top od D3.js  
https://nivo.rocks/components/  
https://github.com/plouc/nivo

### react-d3-graph
Interactive and configurable graphs with React and D3.js effortlessly.  
https://danielcaldas.github.io/react-d3-graph/sandbox/index.html?data=marvel  
https://github.com/danielcaldas/react-d3-graph

### react-force-graph
React bindings for the force-graph suite libraries:
[force-graph](https://github.com/vasturiano/force-graph) and
[3d-force-graph](https://github.com/vasturiano/3d-force-graph)  
https://github.com/vasturiano/react-force-graph  
https://vasturiano.github.io/react-force-graph/example/bloom-effect/


<br/>

Diagrams and Graph editor
=========================

### React Flow
Library for building node-based editors and diagrams.  
https://reactflow.dev  
https://github.com/wbkd/react-flow

### react-digraph
Create a directed graph editor without implementing any of the SVG drawing or event handling logic.  
https://github.com/uber/react-digraph

### AntV X6
JavaScript Diagramming library that uses SVG and HTML for rendering.  
Flowchart, Directed Acyclic Graph, Entity Relationship Diagram.  
https://x6.antv.vision/apps/draw/  
https://github.com/antvis/X6/blob/master/README.en-us.md

### AntV G6
A graph visualization engine. G6 concentrates on the principle of "good by default".  
https://g6.antv.vision/en/docs/manual/tutorial/preface  
https://github.com/antvis/g6

### AntV Graphin
A React toolkit for graph analysis based on G6 that focuses on relational visual analysis.  
https://graphin.antv.vision/en-US  
https://github.com/antvis/graphin/


<br/>

Maps
================

### D3.js
We can use D3 to create a projection and render an SVG map by generating **path** elements from GeoJSON data.  
https://observablehq.com/@d3/gallery#maps  
https://github.com/d3/d3-geo/

### Leaflet
Propably the most popular open-source library for interactive **raster tiles** web maps.  
The library has all the mapping features most developers ever need.  
https://leafletjs.com/examples.html  
https://github.com/Leaflet/Leaflet

### Mapbox.js
Library for interactive, customizable **vector tiles** maps on the web using WebGL for rendering.  
https://www.mapbox.com/mapbox-gljs  
https://github.com/mapbox/mapbox-gl-js


<br/>

Charts by command-line
============

### Vega
A visualization grammar for web and command line interface.  
https://vega.github.io/vega/

With Vega, you can describe the visual appearance and interactive behavior of a visualization in a JSON format.
```json
{
    "$schema": "https://vega.github.io/schema/vega/v5.json",
    "description": "A basic chart",
    "width": 500,
    "height": 300,
    "padding": 10,
    "scales": [
        {"name": "xcale", "type": "linear", "range": "... etc."}
    ],
    "data": ["... etc."]
}
```

The library includes three node.js-based command line utilities - **vg2png**, **vg2svg** and **vg2pdf**
```bash
$ npm install -g vega-cli
$ vg2png vega-chart.json out.png
```

<br/>
