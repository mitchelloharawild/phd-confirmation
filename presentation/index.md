---
from: markdown+emoji
execute:
  cache: true
  keep-md: true
resources:
  - "resources"
filters:
  - custom-callouts
format: 
  letterbox-revealjs:
    css: [custom.css, timeline.css]
    progress: false
    menu: false
    width: 1280
    height: 720
    include-after-body: animate.html
callout-appearance: simple
bibliography: citations.bib
---





## {}

::: columns
::: {.column width="37.5%"}
:::
::: {.column width="60%"}

::: {.title data-id="title"}
Reconciliation of structured time series forecasts with graphs
:::

::: {.dateplace}
PhD confirmation presentation, 6th February 2024
:::

Mitchell O'Hara-Wild, Monash University

::: {.smaller}
Supervised by Rob Hyndman and George Athanasopolous
:::

::: {.callout-link}

## Useful links

![](resources/forum.svg){.icon} [social.mitchelloharawild.com](https://social.mitchelloharawild.com/)

![](resources/projector-screen-outline.svg){.icon} [slides.mitchelloharawild.com/phd-confirmation](https://slides.mitchelloharawild.com/phd-confirmation)

![](resources/github.svg){.icon} [mitchelloharawild/phd-confirmation](https://github.com/mitchelloharawild/phd-confirmation)

:::

:::
:::

![](backgrounds/emma-gossett-B645igbiKCw-unsplash.jpg){.image-left}


## {.fragment-remove}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### PhD thesis structure


::: {.container .fragment .fade-up fragment-index=1}
::: {.timeline .timeline-left style="height: 720px;"}



::: {.timeline-block}
::: {.timeline-icon}
![](resources/graphvec.svg)
:::
::: {.timeline-content}
**Graph coherency constraints**

::: {.timeline-details .fragment .fade-out fragment-index=2}

Graphs flexibly describe cross-sectional relationships between time series.

:::
::: {.timeline-date}
Topic 1
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=2}
::: {.timeline-icon}
![](resources/scissors.png)
:::
::: {.timeline-content}
**Pruning data with graphs**
  
::: {.timeline-details .fragment .fade-out fragment-index=4}
Pruning graphs to remove uninformative time series can both improve forecasting accuracy and computation time.
:::
::: {.timeline-date}
Topic 2
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=4}
::: {.timeline-icon}
![](resources/crystal-ball.png)
:::
::: {.timeline-content}
**Representing probabilistic forecasts**

::: {.timeline-details .fragment .fade-out fragment-index=5}
Vectorised distributions for use in a tidy forecasting workflow to adequately describe forecast uncertainty.

:::
::: {.timeline-date}
Topic 3
:::
:::
:::

::: {.timeline-block .fragment .fade-up fragment-index=5}
::: {.timeline-icon}
![](resources/clock.png)
:::
::: {.timeline-content}
**Temporal coherency constraints**

::: {.timeline-details .fragment .fade-out fragment-index=6}
Representing time with varied temporal granularities in a tidy time series data structure.
:::
::: {.timeline-date}
Topic 4
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=6}
::: {.timeline-icon}
![](resources/fable.png)
:::
::: {.timeline-content}
**Tidy forecasting framework**

::: {.timeline-details}
Combining the foundational tools described above to support a tidy forecasting workflow.
:::
::: {.timeline-date}
Topic 5
:::
:::
:::


:::
:::


:::
:::

![](backgrounds/chris-lee-70l1tDAI6rM-unsplash.jpg){.image-left}

## The basics of reconciliation

::: columns
::: {.column width="60%"}

::: {.callout-question}

## How many tourists will visit Melbourne?

::: {.fragment .fade-in}
Forecast $\text{Visitors}_{T+h|T}$ with a suitable model and data.
:::

:::

::: {.fragment .fade-in}
::: {.callout-question}
## How many visitors are from Australia and overseas?

::: {.fragment .fade-in}
Forecast $\text{Interstate}_{T+h|T}$ and $\text{International}_{T+h|T}$ with

suitable models and data.
:::
:::
:::

::: {.fragment .fade-in}
::: {.callout-important}
## Something doesn't add up here...

Independently produced forecasts are [**incoherent**]{.danger},

$\text{Visitors}_{T+h|T} \neq \text{Interstate}_{T+h|T} + \text{International}_{T+h|T}$.
:::
:::

:::
:::

![](backgrounds/linda-xu-K9NhnXS15ZY-unsplash.jpg){.image-right}

## {}
### Coherency constraints

::: columns
::: {.column width="60%"}

::: {.callout-tip}

## Impose constraints to ensure coherency

Adjust the forecasts to satisfy the constraint

$\text{Visitors}_{T+h|T} = \text{Interstate}_{T+h|T} + \text{International}_{T+h|T}$.

:::

::: {.fragment .fade-in}

::: {.callout-note}
## Structural representation

Constraints can be represented using matrices:

$$
\begin{bmatrix}
  \text{Visitors}_{t} \\
  \text{Interstate}_{t} \\
  \text{International}_{t} \\
\end{bmatrix}
=
\begin{bmatrix}
  1 & 1 \\
  1 & 0\\
  0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
  \text{Interstate}_{t} \\
  \text{International}_{t} \\
\end{bmatrix}
$$

or compactly, $\mathbf{y}_t = \mathbf{S} \mathbf{b}_t$

:::
:::

::: {.fragment .fade-in}
::: {.callout-paper}
More information in @hyndman2011.
:::
:::

:::
:::

![](backgrounds/linda-xu-K9NhnXS15ZY-unsplash.jpg){.image-right}

## {}
### Coherency constraints

::: columns
::: {.column width="60%"}

::: {.fragment .fade-in}

::: {.callout-note}
## Zero-constrained representation

Alternatively, the constraints can also be described with

$$
\begin{bmatrix}
  1 & -1 & -1 \\
\end{bmatrix}
\begin{bmatrix}
  \text{Visitors}_{t} \\
  \text{Interstate}_{t} \\
  \text{International}_{t} \\
\end{bmatrix}
=
\begin{bmatrix}
0
\end{bmatrix}
$$

or compactly, $\mathbf{C}\mathbf{y}_t = \mathbf{0}$.

:::
:::

::: {.fragment .fade-in}
::: {.callout-tip}
It's usually easy to switch between these representations.

Both $\mathbf{C}$ and $\mathbf{S}$ share a common aggregation matrix $\mathbf{A}$.

$$
\mathbf{S} = \begin{bmatrix}
\mathbf{A}\\
\mathbf{I}_{n_b}
\end{bmatrix},\hspace{2em}
\mathbf{C} = \begin{bmatrix}
\mathbf{I}_{n_a} & -\mathbf{A}
\end{bmatrix}.
$$

:::
:::

:::
:::

![](backgrounds/linda-xu-K9NhnXS15ZY-unsplash.jpg){.image-right}


## {.fragment-remove}

::: columns

::: {.column width="40%"}
:::
::: {.column width="60%"}
### Coherency constraints

::: {.callout-important}
## These representations can't represent all constraints

Hierarchical and grouped constraints work well, since they have single top series and common bottom series.
:::

::: {.fragment .fade-in}
::: {.callout-paper}
## General linearly constrained time series

@girolimetto2023point generalise the zero-constrained representation to support any constraint.
:::
:::

::: {.fragment .fade-in}
::: {.callout-tip}
## Graph representation of constraints

I propose representing constraints using **directed acyclical graphs** (DAGs) which achieve the same generality with added benefits.
:::
:::


:::
:::

![](backgrounds/eilis-garvey-MskbR8VLNrA-unsplash.jpg){.image-left}

## {.fragment-remove}

::: columns

::: {.column width="40%"}
:::
::: {.column width="60%"}
### General linear constraints

::: {.callout-tip}
## Generalisation from zero-constrained representation

::: {.fragment .fade-out fragment-index=1}
Rather than *upper and bottom* series, consider *constrained and unconstrained* series.

![](resources/dani-graph-constraint.png)
:::

::: {.fragment .fade-up fragment-index=1}
Similar to $\mathbf{C}$, $\mathbf{\Gamma}$ imposes zero-constraints: $\mathbf{\Gamma}\mathbf{y}_t = \mathbf{0}$.

$$
\mathbf{\Gamma} = \begin{bmatrix}
1 & 0 & -1 & -1 & -1 &  0 &  0\\
1 & 0 &  0 &  0 &  0 & -1 & -1\\
0 & 1 & -1 & -1 &  0 &  0 &  0\\
\end{bmatrix}
$$
:::

::: {.fragment .fade-up fragment-index=2}
However the structural representation and aggregation matrix $\mathbf{A}$ is hard to obtain since

$$
\mathbf{S} = \begin{bmatrix}
\mathbf{A}\\
\mathbf{I}_{n_b}
\end{bmatrix},\hspace{2em}
\mathbf{\Gamma} \neq \begin{bmatrix}
\mathbf{I}_{n_a} & -\mathbf{A}
\end{bmatrix}.
$$
:::

::: {.fragment .fade-up fragment-index=3}
For linear reconciliation, both graph coherence and 'general
linearly constrained' coherence are equivalent.
:::

:::

:::
:::

![](backgrounds/eilis-garvey-MskbR8VLNrA-unsplash.jpg){.image-left}


## {.fragment-remove}

::: columns

::: {.column width="40%"}
:::
::: {.column width="60%"}
### General linear constraints

::: {.callout-tip}
## Generalisation from zero-constrained representation

::: {.fragment .fade-out fragment-index=1}
Use standard linear algebra techniques to transform $\mathbf{\Gamma}$ into a full rank matrix $\mathbf{\bar{U}}$ of form of $\mathbf{C}$.

$$
\mathbf{\Gamma} = \begin{bmatrix}
1 & 0 & -1 & -1 & -1 &  0 &  0\\
1 & 0 &  0 &  0 &  0 & -1 & -1\\
0 & 1 & -1 & -1 &  0 &  0 &  0\\
\end{bmatrix}
$$
Two options are proposed:

* Reduced row echelon form [@meyer2023matrix]
* QR decomposition [@lyche2020numerical]

:::

::: {.fragment .fade-up fragment-index=1}
::: {.fragment .fade-out fragment-index=2}
Applying this technique provides an equivalent matrix from which $\mathbf{A}$ is easily obtained.




$$
\mathbf{\bar{U}} = \left[\begin{array}{ccc:cccc}
1 & 0 & 0 &  0 &  0 & -1 & -1\\
0 & 1 & 0 &  0 &  1 & -1 & -1\\
0 & 0 & 1 &  1 &  1 & -1 & -1\\
\end{array}\right]
$$
:::
:::

::: {.fragment .fade-up fragment-index=2}
From which a "structural-like" representation, $\mathbf{y}_t = \mathbf{\bar{S}}\mathbf{b}_t$ can be constructed:

$$
\mathbf{\bar{S}} = \left[\begin{array}{cccc}
0 &  0 & -1 & -1\\
0 &  1 & -1 & -1\\
1 &  1 & -1 & -1\\\hdashline
1 & 0 & 0 & 0\\
0 & 1 & 0 & 0\\
0 & 0 & 1 & 0\\
0 & 0 & 0 & 1
\end{array}\right].
$$
:::
:::

:::
:::

![](backgrounds/eilis-garvey-MskbR8VLNrA-unsplash.jpg){.image-left}

## {.fragment-remove}

::: columns

::: {.column width="40%"}
:::
::: {.column width="60%"}
### Graph constraints

::: {.callout-tip}
## A novel representation of coherency constraints

Graphs are often used to visualise the constraints...

but can they also be useful in imposing the constraints?

:::

::: {.callout-note}
## Graphs are used widely in computer science

Representing coherency constraints using graphs would:

* introduce **graph terminology** for navigating structures
* **simplify** the description of the **coherency constraints**
* allow representation of **non-linear** relationships
* **unify tools** for visualisation and reconciliation

:::

:::
:::

![](backgrounds/eilis-garvey-MskbR8VLNrA-unsplash.jpg){.image-left}


## {}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Hierarchical coherence

::: {.callout-note}
## Each aggregate has a single constraint

The basic constraint shown before is '[**hierarchical**]{.term}'


::: {.cell hash='index_cache/revealjs/unnamed-chunk-3_6ac12cf2abfe68fed20835249047edc8'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-9d3b939b8c5002b2de18" style="width:600px;height:300px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-9d3b939b8c5002b2de18">{"x":{"nodes":{"label":["Attendees","Academic","Industry"],"id":[1,2,3],"level":[0,1,1]},"edges":{"from":[1,1],"to":[2,3]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":600,"height":300,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

:::
:::

![](backgrounds/PXL_20230626_200731948.jpg){.image-left}


## {}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Hierarchical coherence

::: {.callout-note}
## Each aggregate has a single constraint

[**Hierarchical**]{.term} series often have multiple layers


::: {.cell hash='index_cache/revealjs/unnamed-chunk-4_f7eea716c444264919cc548fb6da0fb9'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-41ab8cefc05a76d0b864" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-41ab8cefc05a76d0b864">{"x":{"nodes":{"label":["Attendees","Academic","Industry","Students","Staff"],"id":[1,2,3,4,5],"level":[0,1,1,2,2]},"edges":{"from":[1,1,2,2],"to":[2,3,4,5]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


::: {.fragment .fade-in}
In graph terms, this is known as a [**polytree**]{.term}.
:::
:::

:::
:::

![](backgrounds/PXL_20230626_200731948.jpg){.image-left}


## {}

::: columns
::: {.column width="60%"}
### Grouped coherence

::: {.callout-note}
## Considering origin and workplace

Attendance can be disaggregated by both **origin** and **workplace**...

::: columns
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-5_8effb5d4845483b55f941a6c74cfb151'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-715fbc70ea420cdfd750" style="width:300px;height:300px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-715fbc70ea420cdfd750">{"x":{"nodes":{"label":["Attendees","Domestic","International"],"id":[1,2,3],"level":[0,1,1]},"edges":{"from":[1,1],"to":[2,3]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":300,"height":300,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-6_525cb9bc9b9d3a87247124a8c4272ba6'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-f89d415f6f763619d5c2" style="width:300px;height:300px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-f89d415f6f763619d5c2">{"x":{"nodes":{"label":["Attendees","Academia","Industry"],"id":[1,2,3],"level":[0,1,1]},"edges":{"from":[1,1],"to":[2,3]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":300,"height":300,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
:::
:::
:::
:::

![](backgrounds/zach-callahan--i51Ke0ROTo-unsplash.jpg){.image-right}



## {}

::: columns
::: {.column width="60%"}
### Grouped coherence

::: {.callout-note}
## Considering origin and workplace

and then further disaggregated by the other.

::: columns
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-7_8968e740cf9774204b73986c5885c712'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-4972543b3933af4d542a" style="width:300px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-4972543b3933af4d542a">{"x":{"nodes":{"label":["Attendees","Domestic","International","Domestic\n& Academia","Domestic\n& Industry","International\n& Academia","International\n& Industry"],"id":[1,2,3,4,5,6,7],"level":[0,1,1,2,2,2,2]},"edges":{"from":[1,1,2,2,3,3],"to":[2,3,4,5,6,7]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":300,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-8_a24582618848f2a87be048e8793cf4e8'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-58c5e11e561c970b1316" style="width:300px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-58c5e11e561c970b1316">{"x":{"nodes":{"label":["Attendees","Academia","Industry","Academia\n& Domestic","Academia\n& International","Industry\n& Domestic","Industry\n& International"],"id":[1,2,3,4,5,6,7],"level":[0,1,1,2,2,2,2]},"edges":{"from":[1,1,2,2,3,3],"to":[2,3,4,5,6,7]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":300,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
:::

A [**grouped**]{.term} structure has the **same top and bottom series**.

:::
:::
:::

![](backgrounds/zach-callahan--i51Ke0ROTo-unsplash.jpg){.image-right}

## {}

::: columns
::: {.column width="60%"}
### Grouped coherence

::: {.callout-note}
## Considering origin and workplace

The [**grouped**]{.term} structure can be plotted in a single graph.


::: {.cell hash='index_cache/revealjs/unnamed-chunk-9_bc998628fc93c453d44b4ff4f6dc3c2f'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-4cdcafa9eab9dc1df73b" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-4cdcafa9eab9dc1df73b">{"x":{"nodes":{"label":["Attendees","Domestic","International","Academia","Industry","Domestic\n& Academia","Domestic\n& Industry","International\n& Academia","International\n& Industry"],"id":[1,2,3,4,5,6,7,8,9],"x":[-0.003371926391818159,-0.8582194521345512,0.86356954610132,0.172705587374925,-0.1708640521216115,-0.6628108638547205,-1,1,0.6736804153398088],"y":[-0.01390576951516009,0.1644008084973776,-0.1675798740901829,0.8533471780243223,-0.8822395672944368,1,-0.6855433690152226,0.6766420294961848,-1]},"edges":{"from":[1,1,1,1,2,4,2,5,3,4,3,5],"to":[2,3,4,5,6,6,7,7,8,8,9,9],"color":["#C87A8A","#C87A8A","#6B9D59","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


::: {.fragment .fade-in}
In graph terms, this is a [**directed acyclical graph**]{.term} (DAG).
:::
:::

:::
:::

![](backgrounds/zach-callahan--i51Ke0ROTo-unsplash.jpg){.image-right}


## {}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Temporal coherence

::: {.callout-note}
A time series can be disaggregated by temporal granularity


::: {.cell hash='index_cache/revealjs/unnamed-chunk-10_fd428ed6c080a0ce22da6a23de7a637f'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-d091e39f7ed94177c868" style="width:600px;height:200px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-d091e39f7ed94177c868">{"x":{"nodes":{"label":["Year","Q1","Q2","Q3","Q4","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17],"level":[0,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2]},"edges":{"from":[1,1,1,1,2,2,2,3,3,3,4,4,4,5,5,5],"to":[2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":40}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":600,"height":200,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

::: {.callout-paper}
Temporal reconciliation is described in @ATHANASOPOULOS201760.
:::

::: {.fragment .fade-in}
::: {.callout-question}
## What type of coherence structure is this?

::: {.fragment .fade-in}
This is a [**polytree**]{.term}, so this structure is [**hierarchical**]{.term}.
:::
:::
:::
:::
:::

![](backgrounds/courtney-smith-pCLKkPHpCz0-unsplash.jpg){.image-left}


## {}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Temporal coherence


::: {.cell hash='index_cache/revealjs/unnamed-chunk-11_12a33ad4963fcea62bf11eb2d0f22fd4'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-799e090635104292c1b4" style="width:700px;height:200px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-799e090635104292c1b4">{"x":{"nodes":{"label":["Year","Q1","Q2","Q3","Q4","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17],"level":[0,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2]},"edges":{"from":[1,1,1,1,2,2,2,3,3,3,4,4,4,5,5,5],"to":[2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":40}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":700,"height":200,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

::: {.cell hash='index_cache/revealjs/unnamed-chunk-12_22623e022c68cebc1b18aba2bda711b6'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-d3ee9bdeec20d6216f19" style="width:700px;height:200px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-d3ee9bdeec20d6216f19">{"x":{"nodes":{"label":["Year","Jan-Apr","May-Aug","Sep-Dec","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16],"level":[0,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2]},"edges":{"from":[1,1,1,2,2,2,2,3,3,3,3,4,4,4,4],"to":[2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":40}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":700,"height":200,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


::: {.fragment .fade-in}
::: {.callout-question}
## What type of coherence structure is this?
::: {.fragment .fade-in}
This structure has the same top and bottom series, so 

temporal coherence is a [**grouped**]{.term} constraint.
:::
:::
:::
:::
:::

![](backgrounds/courtney-smith-pCLKkPHpCz0-unsplash.jpg){.image-left}

## {}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Temporal coherence

::: {.callout-tip}

Temporal coherence constraints are [**grouped**]{.term} can also be represented with [**directed acyclical graphs**]{.term} (DAGs).


::: {.cell hash='index_cache/revealjs/unnamed-chunk-13_55b5979e18e792e3860032e324043f65'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-f3b0f88c61b53123b372" style="width:700px;height:500px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-f3b0f88c61b53123b372">{"x":{"nodes":{"label":["Year","Q1","Q2","Q3","Q4","Jan-Apr","May-Aug","Sep-Dec","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],"x":[-0.05400594153204008,-0.4025836122675311,0.5279899127542234,0.3499252905890411,-0.6550819584534231,-0.1840021427311581,0.6690920750504299,-0.4897559363236491,-0.5080661329043736,-0.2400351501051694,-0.6834033281094694,0.3225316288369899,0.9185469261158925,1,0.7265907610337898,0.8740049766555253,-0.03854619266894377,-1,-0.9357883705518357,-0.7009937986365129],"y":[-0.08026365469781349,0.5549817302860942,0.2565900683007281,-0.602076668830982,-0.5290851774580543,0.6295752330530142,-0.2153387036532498,-0.6767056635977524,0.9669927230446371,1,0.7563276781751973,0.6913868441606599,0.2322349843697402,0.03229075048331187,-0.7226803954898391,-0.5592963964373876,-0.9135472332778896,-0.5978961503510797,-0.8600785125976133,-1]},"edges":{"from":[1,1,1,1,1,1,1,2,2,2,3,3,3,4,4,4,5,5,5,6,6,6,6,7,7,7,7,8,8,8,8],"to":[2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,9,10,11,12,13,14,15,16,17,18,19,20],"grp":[1,1,1,1,2,2,2,1,1,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2],"color":["#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20,"font":{"size":30}},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":700,"height":500,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

:::
:::

![](backgrounds/courtney-smith-pCLKkPHpCz0-unsplash.jpg){.image-left}

## {.fragment-remove}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Cross-temporal coherence

::: {.callout-note}

Since both [**grouped**]{.term} and [**temporal**]{.term} coherence are DAGs, the interaction between them is a single DAG.

::: {.fragment .fade-out fragment-index=1}
::: columns
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-14_de37a88a096f7395db65b39ff384758c'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-736bd213d090c9a095dc" style="width:300px;height:300px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-736bd213d090c9a095dc">{"x":{"nodes":{"label":["Year","Q1","Q2","Q3","Q4","Jan-Apr","May-Aug","Sep-Dec","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],"x":[-0.05400594153204008,-0.4025836122675311,0.5279899127542234,0.3499252905890411,-0.6550819584534231,-0.1840021427311581,0.6690920750504299,-0.4897559363236491,-0.5080661329043736,-0.2400351501051694,-0.6834033281094694,0.3225316288369899,0.9185469261158925,1,0.7265907610337898,0.8740049766555253,-0.03854619266894377,-1,-0.9357883705518357,-0.7009937986365129],"y":[-0.08026365469781349,0.5549817302860942,0.2565900683007281,-0.602076668830982,-0.5290851774580543,0.6295752330530142,-0.2153387036532498,-0.6767056635977524,0.9669927230446371,1,0.7563276781751973,0.6913868441606599,0.2322349843697402,0.03229075048331187,-0.7226803954898391,-0.5592963964373876,-0.9135472332778896,-0.5978961503510797,-0.8600785125976133,-1]},"edges":{"from":[1,1,1,1,1,1,1,2,2,2,3,3,3,4,4,4,5,5,5,6,6,6,6,7,7,7,7,8,8,8,8],"to":[2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,9,10,11,12,13,14,15,16,17,18,19,20],"grp":[1,1,1,1,2,2,2,1,1,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2],"color":["#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20,"font":{"size":30}},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":300,"height":300,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
::: {.column width="50%"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-15_4cbf4cf1023713fb896be0a0a75fe270'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-9e113c0e7a26e3b10905" style="width:300px;height:300px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-9e113c0e7a26e3b10905">{"x":{"nodes":{"label":["Attendees","Domestic","International","Academia","Industry","Domestic\n& Academia","Domestic\n& Industry","International\n& Academia","International\n& Industry"],"id":[1,2,3,4,5,6,7,8,9],"x":[-0.003371926391818159,-0.8582194521345512,0.86356954610132,0.172705587374925,-0.1708640521216115,-0.6628108638547205,-1,1,0.6736804153398088],"y":[-0.01390576951516009,0.1644008084973776,-0.1675798740901829,0.8533471780243223,-0.8822395672944368,1,-0.6855433690152226,0.6766420294961848,-1]},"edges":{"from":[1,1,1,1,2,4,2,5,3,4,3,5],"to":[2,3,4,5,6,6,7,7,8,8,9,9],"color":["#00A396","#00A396","#9189C7","#9189C7","#00A396","#9189C7","#00A396","#9189C7","#00A396","#9189C7","#00A396","#9189C7"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":300,"height":300,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
:::
:::

::: {.fragment .fade-up fragment-index=1}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-16_96968b6794c9e3f4cb3d2c373f05a9cb'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-07ff46277a639a9c16ed" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-07ff46277a639a9c16ed">{"x":{"nodes":{"label":["Attendees","Domestic","International","Academia","Industry","Domestic\n& Academia","Domestic\n& Industry","International\n& Academia","International\n& Industry"],"image":["resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png","resources/temporal-graph-node.png"],"shape":["image","image","image","image","image","image","image","image","image"],"id":[1,2,3,4,5,6,7,8,9],"x":[-0.003371926391818159,-0.8582194521345512,0.86356954610132,0.172705587374925,-0.1708640521216115,-0.6628108638547205,-1,1,0.6736804153398088],"y":[-0.01390576951516009,0.1644008084973776,-0.1675798740901829,0.8533471780243223,-0.8822395672944368,1,-0.6855433690152226,0.6766420294961848,-1]},"edges":{"from":[1,1,1,1,2,4,2,5,3,4,3,5],"to":[2,3,4,5,6,6,7,7,8,8,9,9],"color":["#00A396","#00A396","#9189C7","#9189C7","#00A396","#9189C7","#00A396","#9189C7","#00A396","#9189C7","#00A396","#9189C7"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

:::

:::
:::

![](backgrounds/aleksandar-radovanovic-mXKXJI98aTE-unsplash.jpg){.image-left}

## {}

::: columns
::: {.column width="60%"}
### Graph coherence

A [**directed acyclical graph**]{.term} does **not** require a common top and bottom series.

<br>

::: {.fragment .fade-in}

What happens if we relax this definition of grouped coherence, and use the more general DAG structure.

::: {.callout-question}
## Is it reasonable to leverage the full generality of DAGs?

::: {.fragment .fade-in}
Yes! Let's see why.
:::
:::
:::

:::
:::

![](backgrounds/eilis-garvey-MskbR8VLNrA-unsplash.jpg){.image-right}


## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}
### Unbalanced graphs

What if the coherency structure had **different bottom series**?

<br>

This often occurs in these circumstances:

1. Cross-temporal with series observed at **different granularities**.
2. Multiple **different approaches** to calculating the top series.
3. Partial coherency by **'trimming' sparse disaggregations**.

:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}


## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

1. Cross-temporal with series observed at **different granularities**.

::: {.fragment .fade-in}

::: {.callout-tip}
## Example
Suppose `Sales` is reported quarterly, but `Profit` and `Costs` twice yearly.

:::{data-id="graphtemp"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-17_32eff466982ee4d797c8adb68f878021'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-f90e71e225062c5f5728" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-f90e71e225062c5f5728">{"x":{"nodes":{"label":["Profit (Y)","Sales (Y)","-Costs (Y)","Profit (S1)","Sales (S1)","-Costs (S1)","Profit (S2)","Sales (S2)","-Costs (S2)","Sales (Q1)","Sales (Q2)","Sales (Q3)","Sales (Q4)"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13],"x":[-0.3582040390209666,-0.01049498420926265,-0.700218109949812,0.01263882916904779,0.4996034937053015,-0.3669663160980248,-0.6891066332450266,-0.420604205726013,-1,1,0.9110964283020246,-0.2857898768441832,-0.7420082841920523],"y":[0.3522192958286621,-0.007039236584347019,0.6987669242810171,0.6859429479520101,0.4041352682392674,1,-0.02160509927886167,-0.513009636272736,0.361759628924097,0.2780657174748593,0.7191427299340987,-1,-0.9216143712625707]},"edges":{"from":[1,1,4,4,7,7,1,1,2,2,3,3,5,5,8,8],"to":[2,3,5,6,8,9,4,7,5,8,6,9,10,11,12,13],"grp":[1,1,1,1,1,1,2,2,2,2,2,2,3,3,3,3],"color":["#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#5F96C2","#5F96C2","#5F96C2","#5F96C2"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20,"font":{"size":25}},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
:::
:::
:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}

## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

1. Cross-temporal with series observed at **different granularities**.

::: {.callout-tip}
## Example

:::{data-id="graphtemp"}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-18_1e440f300d634d6b61146dd32e1302a1'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-2516e405aafa92a4c172" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-2516e405aafa92a4c172">{"x":{"nodes":{"label":["Profit (Y)","Sales (Y)","-Costs (Y)","Profit (S1)","Sales (S1)","-Costs (S1)","Profit (S2)","Sales (S2)","-Costs (S2)","Sales (Q1)","Sales (Q2)","Sales (Q3)","Sales (Q4)"],"id":[1,2,3,4,5,6,7,8,9,10,11,12,13],"x":[-0.3582040390209666,-0.01049498420926265,-0.700218109949812,0.01263882916904779,0.4996034937053015,-0.3669663160980248,-0.6891066332450266,-0.420604205726013,-1,1,0.9110964283020246,-0.2857898768441832,-0.7420082841920523],"y":[0.3522192958286621,-0.007039236584347019,0.6987669242810171,0.6859429479520101,0.4041352682392674,1,-0.02160509927886167,-0.513009636272736,0.361759628924097,0.2780657174748593,0.7191427299340987,-1,-0.9216143712625707]},"edges":{"from":[1,1,4,4,7,7,1,1,2,2,3,3,5,5,8,8],"to":[2,3,5,6,8,9,4,7,5,8,6,9,10,11,12,13],"grp":[1,1,1,1,1,1,2,2,2,2,2,2,3,3,3,3],"color":["#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#6B9D59","#5F96C2","#5F96C2","#5F96C2","#5F96C2"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20,"font":{"size":25}},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::
This allows the higher frequency `Sales` data to be used with the less frequent `Profit` and `Costs` data!
:::
:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}

## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

2. Multiple **different approaches** to calculating the top series.

::: {.fragment .fade-in}
::: {.callout-tip}
## Example

Australian GDP is calculated with 3 approaches:

* Income approach (I)
* Expenditure approach (E)
* Production approach (P)

For simplicity consider a small part of these graphs. The complete graph structure has many more disaggregates.

:::

::: {.callout-paper}
This example is used in @Athanasopoulos2020.
:::
:::

:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}


## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

2. Multiple **different approaches** to calculating the top series.

::: {.callout-tip}

* Income approach (I)


::: {.cell hash='index_cache/revealjs/unnamed-chunk-19_3a3e890023776b9a00b54f061c9224bf'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-aac82d7e2e79d1116c90" style="width:600px;height:200px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-aac82d7e2e79d1116c90">{"x":{"nodes":{"label":["GDP","Statistical\nDiscrepancy (I)","Income","Taxes"],"id":[1,2,3,4],"level":[0,1,1,1]},"edges":{"from":[1,1,1],"to":[2,3,4]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":600,"height":200,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


* Expenditure approach (E)


::: {.cell hash='index_cache/revealjs/unnamed-chunk-20_f0f2591cb75f2753675315007cb0a739'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-40a3ec3bf9beac6b829d" style="width:600px;height:200px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-40a3ec3bf9beac6b829d">{"x":{"nodes":{"label":["GDP","Statistical\nDiscrepancy (E)","-Imports","Exports","Expenses"],"id":[1,2,3,4,5],"level":[0,1,1,1,1]},"edges":{"from":[1,1,1,1],"to":[2,3,4,5]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","font":{"size":16}},"manipulation":{"enabled":false},"layout":{"hierarchical":{"enabled":true,"direction":"UD","shakeTowards":"leaves"}},"edges":{"arrows":"to","scaling":{"label":{"enabled":false}}}},"groups":null,"width":600,"height":200,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"hierarchical","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":true,"fit":false,"resetHighlight":true,"clusterOptions":{"fixed":true,"physics":true},"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::



:::
:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}

## {auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

2. Multiple **different approaches** to calculating the top series.

::: {.callout-tip}

* Combined approach (I & E)


::: {.cell hash='index_cache/revealjs/unnamed-chunk-21_fdb6d1928aece13aa59a12242f120f3f'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-207615cf39b08a67202c" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-207615cf39b08a67202c">{"x":{"nodes":{"label":["GDP","Statistical\nDiscrepancy (I)","Income","Taxes","Statistical\nDiscrepancy (E)","-Imports","Exports","Expenses"],"id":[1,2,3,4,5,6,7,8],"x":[0.0181208309193075,0.8676001522280246,-0.7349134380824285,0.3967080063204802,0.09631010583399746,-1,-0.4959156544771076,1],"y":[-0.03547901733548309,0.5473023377285768,0.6735302376679089,-1,1,-0.1882988257743036,-0.9329726046020161,-0.3413304861903519]},"edges":{"from":[1,1,1,1,1,1,1],"to":[2,3,4,5,6,7,8],"color":["#C87A8A","#C87A8A","#C87A8A","#6B9D59","#6B9D59","#6B9D59","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


:::
:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}


## {.fragment-remove auto-animate=true}

::: columns
::: {.column width="40%"}
:::
::: {.column width="60%"}

3. Partial coherency by **'trimming' sparse disaggregations**.

::: {.callout-tip}

If the bottom level is too sparse, it is **more complicated** to model and reconciliation can **worsen forecast accuracy**.

<!-- Graph coherency allows us to ignore our problems (reconciling count data models) by trimming the leaf nodes of the graph. -->

:::{.fragment .fade-out fragment-index=1}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-22_32f3e22e84f87b82512db94b5f99371b'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-3ea692b0247d673ee070" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-3ea692b0247d673ee070">{"x":{"nodes":{"label":["Attendees","Domestic","International","Academia","Industry","Domestic\n& Academia","Domestic\n& Industry","International\n& Academia","International\n& Industry"],"id":[1,2,3,4,5,6,7,8,9],"x":[-0.003371926391818159,-0.8582194521345512,0.86356954610132,0.172705587374925,-0.1708640521216115,-0.6628108638547205,-1,1,0.6736804153398088],"y":[-0.01390576951516009,0.1644008084973776,-0.1675798740901829,0.8533471780243223,-0.8822395672944368,1,-0.6855433690152226,0.6766420294961848,-1]},"edges":{"from":[1,1,1,1,2,4,2,5,3,4,3,5],"to":[2,3,4,5,6,6,7,7,8,8,9,9],"color":["#C87A8A","#C87A8A","#6B9D59","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59","#C87A8A","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

:::{.fragment .fade-up fragment-index=1}

::: {.cell hash='index_cache/revealjs/unnamed-chunk-23_66b26b22febd9dc836d0cb66816d399f'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-7f30d2589f1eb3bca06c" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-7f30d2589f1eb3bca06c">{"x":{"nodes":{"label":["Attendees","Domestic","International","Academia","Industry"],"id":[1,2,3,4,5],"x":[-0.001759247510430439,0.4649221558329919,-1,1,-0.4701575758691139],"y":[-0.0004033440696582513,1,0.468617046048023,-0.4710768300799481,-1]},"edges":{"from":[1,1,1,1],"to":[2,3,4,5],"color":["#C87A8A","#C87A8A","#6B9D59","#6B9D59"]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::

:::

:::
:::
:::

![](backgrounds/oriento--OBffuUekfQ-unsplash.jpg){.image-left}


## {auto-animate=true}

::: columns
::: {.column width="60%"}
### Incomplete graphs

What if the coherency structure doesn't completely aggregate, so that there are **multiple top series**.

<br>

::: {.fragment .fade-in}
This can occur for many reasons:

1. Cross-validation with reconciliation
2. Partial/local coherency <br>(e.g. not including worldwide total)
:::

:::
:::

![](backgrounds/firosnv-photography-Rr3B0LH7W3k-unsplash.jpg){.image-right}


## {auto-animate=true}

::: columns
::: {.column width="60%"}

1. Cross-validation with reconciliation

::: {.callout-tip}
It makes no sense to aggregate folds of cross-validation.

::: {.fragment .fade-in}

A suitable DAG for cross-validated hierarchies is 


::: {.cell hash='index_cache/revealjs/unnamed-chunk-24_5157282372c0adc00caf8df7d29652fc'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-f5fd377a4a13c4c0746d" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-f5fd377a4a13c4c0746d">{"x":{"nodes":{"label":["Attendees\n(fold 1)","Academic\n(fold 1)","Industry\n(fold 1)","Attendees\n(fold 2)","Academic\n(fold 2)","Industry\n(fold 2)","Attendees\n(fold 3)","Academic\n(fold 3)","Industry\n(fold 3)"],"id":[1,2,3,4,5,6,7,8,9],"x":[-0.7771115886284494,-1,-0.5479317420191581,0.04555610562827783,0.5416865832808138,-0.4511010814451824,0.7336814933590186,0.4621687484879951,1],"y":[0.4508815641554542,-0.04886791008397484,0.9417089317630387,-0.9777716560130811,-0.9451423221379904,-1,0.5353849818406153,1,0.06400782226108581]},"edges":{"from":[1,1,4,4,7,7],"to":[2,3,5,6,8,9]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


:::

::: {.fragment .fade-in}
Each disjoint graph can be reconciled separately.
:::

:::

:::
:::

![](backgrounds/firosnv-photography-Rr3B0LH7W3k-unsplash.jpg){.image-right}


## {auto-animate=true}

::: columns
::: {.column width="60%"}

2. Partial/local coherency

::: {.callout-tip}

## Graph without a common top series


::: {.cell hash='index_cache/revealjs/unnamed-chunk-25_7961c8b2aaa557d7a0ea7c4db19822fc'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-60ab31c5b5ce0eaefe9b" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-60ab31c5b5ce0eaefe9b">{"x":{"nodes":{"label":["Facebook","Instagram","WhatsApp","Messenger","Oculus","","","","",""],"shape":["triangle","triangle","triangle","triangle","triangle","circle","circle","circle","circle","circle"],"id":[1,2,3,4,5,6,7,8,9,10],"x":[1,0.8090169943749475,0.3090169943749475,-0.3090169943749473,-0.8090169943749473,-1,-0.8090169943749476,-0.3090169943749476,0.3090169943749472,0.8090169943749475],"y":[0,0.6180339887498949,1,1,0.6180339887498951,2.220446049250313e-16,-0.6180339887498947,-0.9999999999999999,-1,-0.6180339887498951]},"edges":{"from":[1,1,1,2,2,3,3,4,4,5],"to":[6,7,8,9,10,6,7,8,9,10]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::



:::

:::
:::

![](backgrounds/PXL_20230908_083157452.jpg){.image-right}

## {auto-animate=true}

::: columns
::: {.column width="60%"}

2. Partial/local coherency

::: {.callout-tip}

## Adding Meta as the single parent results in a hierarchy


::: {.cell hash='index_cache/revealjs/unnamed-chunk-26_d1f42ff4f4d559c5852be99ae2cd5414'}
::: {.cell-output-display}

```{=html}
<div id="htmlwidget-7a6dcd3972a686419b18" style="width:600px;height:400px;" class="visNetwork html-widget "></div>
<script type="application/json" data-for="htmlwidget-7a6dcd3972a686419b18">{"x":{"nodes":{"label":["Facebook","Instagram","WhatsApp","Messenger","Oculus","","","","","","Meta"],"shape":["triangle","triangle","triangle","triangle","triangle","circle","circle","circle","circle","circle","star"],"id":[1,2,3,4,5,6,7,8,9,10,11],"x":[-0.571124951733712,0.6896991221030009,-0.5503295009007478,0.146553883693229,0.5537200131579443,-1,-0.9089508891878479,-0.3634786477218064,0.6637360306906206,1,0.06168987497947809],"y":[0.02938562779538478,-0.01784534055795728,-0.6978283133735672,0.8416319207142804,-1,-0.1864096742582104,-0.866363405872039,1,0.9408184169156235,-0.7967512930882792,-0.2484499166846748]},"edges":{"from":[1,1,1,2,2,3,3,4,4,5,11,11,11,11,11],"to":[6,7,8,9,10,6,7,8,9,10,1,2,3,4,5]},"nodesToDataframe":true,"edgesToDataframe":true,"options":{"width":"100%","height":"100%","nodes":{"shape":"dot","physics":false,"size":20},"manipulation":{"enabled":false},"edges":{"smooth":false,"width":3,"arrows":"to","scaling":{"label":{"enabled":false}}},"physics":{"stabilization":false}},"groups":null,"width":600,"height":400,"idselection":{"enabled":false,"style":"width: 150px; height: 26px","useLabels":true,"main":"Select by id"},"byselection":{"enabled":false,"style":"width: 150px; height: 26px","multiple":false,"hideColor":"rgba(200,200,200,0.5)","highlight":false},"main":null,"submain":null,"footer":null,"background":"rgba(0, 0, 0, 0)","igraphlayout":{"type":"square"},"highlight":{"enabled":true,"hoverNearest":false,"degree":{"from":50000,"to":0},"algorithm":"all","hideColor":"rgba(200,200,200,0.5)","labelOnly":true},"collapse":{"enabled":false,"fit":false,"resetHighlight":true,"clusterOptions":null,"keepCoord":true,"labelSuffix":"(cluster)"},"tree":{"updateShape":true,"shapeVar":"dot","shapeY":"square"}},"evals":[],"jsHooks":[]}</script>
```

:::
:::


*Note that this isn't a polytree, due to common/shared leafs.*

:::

:::
:::

![](backgrounds/PXL_20230908_083157452.jpg){.image-right}


## {.fragment-remove}

::: columns

::: {.column width="40%"}
:::
::: {.column width="60%"}
### Implementation in fable

Graph structures are embedded within tsibble keys using `graphvec` classes.


::: {.fragment .fade-out fragment-index=3}
::: {.callout-tip}
## Useful functions

* Create aggregates from bottom series with `aggregate_key()` <br> (and someday `aggregate_index()` for temporal)
* [Or get artisanal with `agg_vec()` and `graph_vec()` for complex aggregation structures and graphs.]{.fragment fragment-index=1}
* [Add coherency constraints to models/forecasts with `reconcile()` and `min_trace()`]{.fragment fragment-index=2}

:::

:::

::: {.fragment .fade-up fragment-index=3}
::: {.callout-paper}
## Succinctly describe constraints with symbolic language

Similar to modelling interactions in R [@rcore], common aggregation structures are described with symbolic language [@wilkinson1973symbolic].
:::
:::

::: {.fragment .fade-up fragment-index=4}
::: {.callout-tip}
## Enhanced functionality

* Tools for identifying sub-graphs for exploring series
* [Tools for changing the graph without removing data]{.fragment fragment-index=5}
* [Tools for validating operations which change the graph]{.fragment fragment-index=6}

:::
:::

:::
:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-27_529a80798c6d4448a9e8eef00d0c6bdb'}

:::

::: {.cell hash='index_cache/revealjs/unnamed-chunk-28_554caaba0359ef251cb549945e10473b'}

```{.r .cell-code}
library(tsibble)
library(fable)
tourism
```

::: {.cell-output .cell-output-stdout}
```
# A tsibble: 24,320 x 5 [1Q]
# Key:       Region, State, Purpose [304]
   Quarter Region   State           Purpose  Trips
     <qtr> <chr>    <chr>           <chr>    <dbl>
 1 1998 Q1 Adelaide South Australia Business  135.
 2 1998 Q2 Adelaide South Australia Business  110.
 3 1998 Q3 Adelaide South Australia Business  166.
 4 1998 Q4 Adelaide South Australia Business  127.
 5 1999 Q1 Adelaide South Australia Business  137.
 6 1999 Q2 Adelaide South Australia Business  200.
 7 1999 Q3 Adelaide South Australia Business  169.
 8 1999 Q4 Adelaide South Australia Business  134.
 9 2000 Q1 Adelaide South Australia Business  154.
10 2000 Q2 Adelaide South Australia Business  169.
# i 24,310 more rows
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-29_8cfcd8aad8be1ab8e1237ee616a19a7f'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose * (State / Region),
    Trips = sum(Trips)
  )
```

::: {.cell-output .cell-output-stdout}
```
# A tsibble: 34,000 x 5 [1Q]
# Key:       Purpose, State, Region [425]
   Quarter Purpose      State        Region        Trips
     <qtr> <chr*>       <chr*>       <chr*>        <dbl>
 1 1998 Q1 <aggregated> <aggregated> <aggregated> 23182.
 2 1998 Q2 <aggregated> <aggregated> <aggregated> 20323.
 3 1998 Q3 <aggregated> <aggregated> <aggregated> 19827.
 4 1998 Q4 <aggregated> <aggregated> <aggregated> 20830.
 5 1999 Q1 <aggregated> <aggregated> <aggregated> 22087.
 6 1999 Q2 <aggregated> <aggregated> <aggregated> 21458.
 7 1999 Q3 <aggregated> <aggregated> <aggregated> 19914.
 8 1999 Q4 <aggregated> <aggregated> <aggregated> 20028.
 9 2000 Q1 <aggregated> <aggregated> <aggregated> 22339.
10 2000 Q2 <aggregated> <aggregated> <aggregated> 19941.
# i 33,990 more rows
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}

## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-30_8954efa8bfd759e15bd2b2b564150465'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose * (State / Region),
    Trips = sum(Trips)
  ) |> 
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 425 x 4
   Purpose  State           Region                 .rows
   <chr*>   <chr*>          <chr*>                 <lis>
 1 Business ACT             Canberra                [80]
 2 Business ACT             <aggregated>            [80]
 3 Business New South Wales Blue Mountains          [80]
 4 Business New South Wales Capital Country         [80]
 5 Business New South Wales Central Coast           [80]
 6 Business New South Wales Central NSW             [80]
 7 Business New South Wales Hunter                  [80]
 8 Business New South Wales New England North West  [80]
 9 Business New South Wales North Coast NSW         [80]
10 Business New South Wales Outback NSW             [80]
# i 415 more rows
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-31_f0ec4166c6a93b02536687ca7af6ef79'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose * State,
    Trips = sum(Trips)
  ) |>  
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 45 x 3
   Purpose      State                    .rows
   <chr*>       <chr*>             <list<int>>
 1 Business     ACT                       [80]
 2 Business     New South Wales           [80]
 3 Business     Northern Territory        [80]
 4 Business     Queensland                [80]
 5 Business     South Australia           [80]
 6 Business     Tasmania                  [80]
 7 Business     Victoria                  [80]
 8 Business     Western Australia         [80]
 9 Business     <aggregated>              [80]
10 Holiday      ACT                       [80]
11 Holiday      New South Wales           [80]
12 Holiday      Northern Territory        [80]
13 Holiday      Queensland                [80]
14 Holiday      South Australia           [80]
15 Holiday      Tasmania                  [80]
16 Holiday      Victoria                  [80]
17 Holiday      Western Australia         [80]
18 Holiday      <aggregated>              [80]
19 Other        ACT                       [80]
20 Other        New South Wales           [80]
21 Other        Northern Territory        [80]
22 Other        Queensland                [80]
23 Other        South Australia           [80]
24 Other        Tasmania                  [80]
25 Other        Victoria                  [80]
26 Other        Western Australia         [80]
27 Other        <aggregated>              [80]
28 Visiting     ACT                       [80]
29 Visiting     New South Wales           [80]
30 Visiting     Northern Territory        [80]
31 Visiting     Queensland                [80]
32 Visiting     South Australia           [80]
33 Visiting     Tasmania                  [80]
34 Visiting     Victoria                  [80]
35 Visiting     Western Australia         [80]
36 Visiting     <aggregated>              [80]
37 <aggregated> ACT                       [80]
38 <aggregated> New South Wales           [80]
39 <aggregated> Northern Territory        [80]
40 <aggregated> Queensland                [80]
41 <aggregated> South Australia           [80]
42 <aggregated> Tasmania                  [80]
43 <aggregated> Victoria                  [80]
44 <aggregated> Western Australia         [80]
45 <aggregated> <aggregated>              [80]
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}

## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-32_797b65b8ffc376c1f1139fb3ea74c801'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose : State,
    Trips = sum(Trips)
  ) |> 
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 33 x 3
   Purpose      State                    .rows
   <chr*>       <chr*>             <list<int>>
 1 Business     ACT                       [80]
 2 Business     New South Wales           [80]
 3 Business     Northern Territory        [80]
 4 Business     Queensland                [80]
 5 Business     South Australia           [80]
 6 Business     Tasmania                  [80]
 7 Business     Victoria                  [80]
 8 Business     Western Australia         [80]
 9 Holiday      ACT                       [80]
10 Holiday      New South Wales           [80]
11 Holiday      Northern Territory        [80]
12 Holiday      Queensland                [80]
13 Holiday      South Australia           [80]
14 Holiday      Tasmania                  [80]
15 Holiday      Victoria                  [80]
16 Holiday      Western Australia         [80]
17 Other        ACT                       [80]
18 Other        New South Wales           [80]
19 Other        Northern Territory        [80]
20 Other        Queensland                [80]
21 Other        South Australia           [80]
22 Other        Tasmania                  [80]
23 Other        Victoria                  [80]
24 Other        Western Australia         [80]
25 Visiting     ACT                       [80]
26 Visiting     New South Wales           [80]
27 Visiting     Northern Territory        [80]
28 Visiting     Queensland                [80]
29 Visiting     South Australia           [80]
30 Visiting     Tasmania                  [80]
31 Visiting     Victoria                  [80]
32 Visiting     Western Australia         [80]
33 <aggregated> <aggregated>              [80]
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-33_097eea195cc1e718a359e5e03e00c511'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose + State,
    Trips = sum(Trips)
  ) |> 
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 13 x 3
   Purpose      State                    .rows
   <chr*>       <chr*>             <list<int>>
 1 Business     <aggregated>              [80]
 2 Holiday      <aggregated>              [80]
 3 Other        <aggregated>              [80]
 4 Visiting     <aggregated>              [80]
 5 <aggregated> ACT                       [80]
 6 <aggregated> New South Wales           [80]
 7 <aggregated> Northern Territory        [80]
 8 <aggregated> Queensland                [80]
 9 <aggregated> South Australia           [80]
10 <aggregated> Tasmania                  [80]
11 <aggregated> Victoria                  [80]
12 <aggregated> Western Australia         [80]
13 <aggregated> <aggregated>              [80]
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}

## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-34_8fce5ec225e0cdeeb88563a963248ded'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    0 + Purpose + State,
    Trips = sum(Trips)
  ) |> 
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 12 x 3
   Purpose      State                    .rows
   <chr*>       <chr*>             <list<int>>
 1 Business     <aggregated>              [80]
 2 Holiday      <aggregated>              [80]
 3 Other        <aggregated>              [80]
 4 Visiting     <aggregated>              [80]
 5 <aggregated> ACT                       [80]
 6 <aggregated> New South Wales           [80]
 7 <aggregated> Northern Territory        [80]
 8 <aggregated> Queensland                [80]
 9 <aggregated> South Australia           [80]
10 <aggregated> Tasmania                  [80]
11 <aggregated> Victoria                  [80]
12 <aggregated> Western Australia         [80]
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}

## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-35_83151d30ab306d1f1f647283dc966391'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose,
    Trips = sum(Trips)
  ) |> 
  stretch_tsibble(.step = 4, .init = 60) |> 
  key_data()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 30 x 3
     .id Purpose            .rows
   <int> <chr*>       <list<int>>
 1     1 Business            [60]
 2     1 Holiday             [60]
 3     1 Other               [60]
 4     1 Visiting            [60]
 5     1 <aggregated>        [60]
 6     2 Business            [64]
 7     2 Holiday             [64]
 8     2 Other               [64]
 9     2 Visiting            [64]
10     2 <aggregated>        [64]
11     3 Business            [68]
12     3 Holiday             [68]
13     3 Other               [68]
14     3 Visiting            [68]
15     3 <aggregated>        [68]
16     4 Business            [72]
17     4 Holiday             [72]
18     4 Other               [72]
19     4 Visiting            [72]
20     4 <aggregated>        [72]
21     5 Business            [76]
22     5 Holiday             [76]
23     5 Other               [76]
24     5 Visiting            [76]
25     5 <aggregated>        [76]
26     6 Business            [80]
27     6 Holiday             [80]
28     6 Other               [80]
29     6 Visiting            [80]
30     6 <aggregated>        [80]
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}

## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable




::: {.cell hash='index_cache/revealjs/unnamed-chunk-36_a2512ce256ccb3182008da93d56c422f'}

```{.r .cell-code}
tourism |> 
  aggregate_key(
    Purpose,
    Trips = sum(Trips)
  ) |> 
  stretch_tsibble(.step = 4, .init = 60) |> 
  model(ets = ETS(Trips)) |> 
  reconcile(ets_coherent = min_trace(ets)) |> 
  forecast(h = "1 year")
```

::: {.cell-output .cell-output-stdout}
```
# A fable: 240 x 6 [1Q]
# Key:     .id, Purpose, .model [60]
     .id Purpose  .model       Quarter            Trips
   <int> <chr*>   <chr>          <qtr>           <dist>
 1     1 Business ets          2013 Q1   N(3135, 46032)
 2     1 Business ets          2013 Q2   N(3832, 73712)
 3     1 Business ets          2013 Q3   N(4158, 93320)
 4     1 Business ets          2013 Q4   N(3781, 88006)
 5     1 Business ets_coherent 2013 Q1   N(3159, 45373)
 6     1 Business ets_coherent 2013 Q2   N(3848, 69353)
 7     1 Business ets_coherent 2013 Q3   N(4177, 86871)
 8     1 Business ets_coherent 2013 Q4   N(3795, 83437)
 9     1 Holiday  ets          2013 Q1 N(10442, 211377)
10     1 Holiday  ets          2013 Q2  N(8698, 146661)
# i 230 more rows
# i 1 more variable: .mean <dbl>
```
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {auto-animate=TRUE}

::: columns

::: {.column width="40%"}
:::

::: {.column width="60%"}
### Examples with fable


::: {.cell hash='index_cache/revealjs/unnamed-chunk-37_d69d3acf334d5bbaee1f2ee37b6d6bfc'}
::: {.cell-output-display}
![](index_files/figure-revealjs/unnamed-chunk-37-1.png){width=960}
:::
:::



:::


:::

![](backgrounds/sander-weeteling-KABfjuSOx74-unsplash.jpg){.image-left}


## {.fragment-remove}

::: columns
::: {.column width="60%"}
### Recap

::: {.callout-note}

## Coherence and graph theory

* [**Hierarchical**]{.term} coherence is a [**polytree**]{.term}.
* [**Grouped**]{.term} coherence is a *restricted* [**DAG**]{.term}.
* [**Graph**]{.term} coherence is an *unrestricted* [**DAG**]{.term}.

DAGs are a useful tool for representing structured time series and producing coherent forecasts.

:::

::: {.fragment .fade-in}

![](resources/set.svg){fig-align="center"}
:::

:::
:::

![](backgrounds/meric-dagli-7NBO76G5JsE-unsplash.jpg){.image-right}

## {}

### What else?

::: columns
::: {.column width="60%"}

::: {.callout-tip}

## Other benefits

1. Access to efficient graph algorithms and ideas
2. Exploration and description of structured time series
3. Familiar computing grammar for coherent data

:::

::: {.callout-paper}

## Future work

1. Explore automatic processes for reducing the size of the graph to reduce complexity and improve accuracy
2. Investigate alternative graph reconciliation methods (non-linear constraints, faster at-scale reconciliation)

:::
:::
:::

![](backgrounds/meric-dagli-7NBO76G5JsE-unsplash.jpg){.image-right}


## {.fragment-remove}

### PhD progression

  <!-- ~ Chapter, ~ `Estimated completion`, ~ Task, ~ Progress, -->
  <!-- "Topic 1: Reconciliation of structured time series forecasts with graphs", "June 2023", "Theory development", "100%", -->
  <!-- "Topic 1: Reconciliation of structured time series forecasts with graphs", "June 2023", "ISF2023 Presentation", "100%", -->
  <!-- "Topic 1: Reconciliation of structured time series forecasts with graphs", "February 2024", "Software development", "90%*", -->
  <!-- "Topic 1: Reconciliation of structured time series forecasts with graphs", "March 2024", "Paper submission", "75%", -->
  <!-- "Topic 2: Feature based graph pruning for improved forecast reconciliation", "May 2024", "Theory development", "60%", -->
  <!-- "Topic 2: Feature based graph pruning for improved forecast reconciliation", "June 2024", "ISF2024 Presentation", "0%", -->
  <!-- "Topic 2: Feature based graph pruning for improved forecast reconciliation", "June 2024", "Software development", "20%*", -->
  <!-- "Topic 2: Feature based graph pruning for improved forecast reconciliation", "August 2024", "Paper submission", "0%", -->
  <!-- "Topic 3: Statistical computing with vectorised operations on distributions", "April 2024", "Theory development", "90%", -->
  <!-- "Topic 3: Statistical computing with vectorised operations on distributions", "July 2024", "useR! Presentation", "0%", -->
  <!-- "Topic 3: Statistical computing with vectorised operations on distributions", "July 2024", "Software development", "80%*", -->
  <!-- "Topic 3: Statistical computing with vectorised operations on distributions", "November 2024", "Paper submission", "0%", -->
  <!-- "Topic 4: Reconciling mixed temporal granularities", "February 2025", "Theory development", "20%", -->
  <!-- "Topic 4: Reconciling mixed temporal granularities", "June 2025", "ISF2025 Presentation", "0%", -->
  <!-- "Topic 4: Reconciling mixed temporal granularities", "June 2025", "Software development", "10%*", -->
  <!-- "Topic 4: Reconciling mixed temporal granularities", "August 2025", "Paper submission", "0%", -->
  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "December 2025", "Theory development", "75%", -->
  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Software development", "60%*", -->
  <!-- # "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Paper submission", "0%", -->
  <!-- "PhD Milestones", "February 2024", "Confirmation", "100%", -->
  <!-- "PhD Milestones", "February 2025", "Mid-candidature review", "0%", -->
  <!-- "PhD Milestones", "February 2026", "Final review", "0%", -->

::: columns
::: {.column width="60%"}

::: {.container .fragment .fade-up fragment-index=1}
::: {.timeline .timeline-left style="height: 720px;"}



::: {.timeline-block}
::: {.timeline-icon}
![](resources/temporal-graph-node.png)
:::
::: {.timeline-content}
**Idea of graph coherency constraints**

::: {.timeline-details .fragment .fade-out fragment-index=2}

Working on improving reconciliation for fable, I set out to find a more general framework for representing coherency constraints.

:::
::: {.timeline-date}
Mar 2023
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=2}
::: {.timeline-icon}
![](resources/isf2023.png)
:::
::: {.timeline-content}
**Presented graph coherency at ISF 2023**

::: {.timeline-details .fragment .fade-out fragment-index=3}
Received great support and useful feedback from attendees.

Awarded with a best student presentation award for the work, provided by Elsevier.
:::
::: {.timeline-date}
Jun 2023
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=3}
::: {.timeline-icon}
![](resources/monash.png)
:::
::: {.timeline-content}
**Completed the coursework :mortar_board:**

::: {.timeline-details .fragment .fade-out fragment-index=4}
Passed all coursework :tada:

Now to focus on the research!
:::
::: {.timeline-date}
Nov 2023
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=4}
::: {.timeline-icon}
![](resources/graphvec.svg)
:::
::: {.timeline-content}
**Created the graphvec package**

::: {.timeline-details .fragment .fade-out fragment-index=5}
Split out `agg_vec()` from fabletools, and extended support with `graph_vec()` and developed tools for multi-column graphs in tidy data frames.

:::
::: {.timeline-date}
Dec 2023
:::
:::
:::

::: {.timeline-block .fragment .fade-up fragment-index=5}
::: {.timeline-icon}
![](resources/memo.png)
:::
::: {.timeline-content}
**Draft of graph coherency paper**

::: {.timeline-details .fragment .fade-out fragment-index=6}
Paper is well structured and has a clear path toward completion. Some more work on applications with stronger motivating examples is needed.
:::
::: {.timeline-date}
Feb 2024
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=6}
::: {.timeline-icon}
![](resources/monash.png)
:::
::: {.timeline-content}
**Confirmation report and presentation**

::: {.timeline-details}

:::
::: {.timeline-date}
Feb 2024
:::
:::
:::



:::
:::


:::
:::

![](backgrounds/estee-janssens-zni0zgb3bkQ-unsplash.jpg){.image-right}


## {.fragment-remove}

### PhD timeline (2024)

  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "December 2025", "Theory development", "75%", -->
  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Software development", "60%*", -->
  <!-- # "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Paper submission", "0%", -->
  <!-- "PhD Milestones", "February 2024", "Confirmation", "100%", -->
  <!-- "PhD Milestones", "February 2025", "Mid-candidature review", "0%", -->
  <!-- "PhD Milestones", "February 2026", "Final review", "0%", -->

::: columns
::: {.column width="60%"}

::: {.container .fragment .fade-up fragment-index=1}
::: {.timeline .timeline-left style="height: 720px;"}



::: {.timeline-block}
::: {.timeline-icon}
![](resources/vacation.png)
:::
::: {.timeline-content}
**Have a break!**

::: {.timeline-details .fragment .fade-out fragment-index=2}

On leave for two weeks in China :cold_face:

Happy new year! :dragon:

:::
::: {.timeline-date}
Feb 2024
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=2}
::: {.timeline-icon}
![](resources/temporal-graph-node.png)
:::
::: {.timeline-content}
**Complete graph coherency paper**

::: {.timeline-details .fragment .fade-out fragment-index=3}
Write, write, write.
:::
::: {.timeline-date}
Apr 2024
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=3}
::: {.timeline-icon}
![](resources/scissors.png)
:::
::: {.timeline-content}
**Feature based graph pruning**

::: {.timeline-details .fragment .fade-out fragment-index=4}
Develop a technique for pruning low-signal time series from a coherent graph structure to reduce complexity and increase forecasting performance.
:::
::: {.timeline-date}
May 2024
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=4}
::: {.timeline-icon}
![](resources/iif.png)
:::
::: {.timeline-content}
**Presentation at ISF 2024**

::: {.timeline-details .fragment .fade-out fragment-index=5}
Present about pruning coherent time series.

:::
::: {.timeline-date}
June 2024
:::
:::
:::

::: {.timeline-block .fragment .fade-up fragment-index=5}
::: {.timeline-icon}
![](resources/useR.png)
:::
::: {.timeline-content}
**Presentation at useR! 2024**

::: {.timeline-details .fragment .fade-out fragment-index=6}
Presentation about the distributional package, which is the foundation for representing probabilistic forecasts in fable.
:::
::: {.timeline-date}
July 2024
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=6}
::: {.timeline-icon}
![](resources/monash.png)
:::
::: {.timeline-content}
**Mid-candidature review and presentation**

::: {.timeline-details}

:::
::: {.timeline-date}
Feb 2025
:::
:::
:::



:::
:::


:::
:::

![](backgrounds/estee-janssens-zni0zgb3bkQ-unsplash.jpg){.image-right}

## {.fragment-remove}

### PhD timeline (2025)

  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "December 2025", "Theory development", "75%", -->
  <!-- "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Software development", "60%*", -->
  <!-- # "Topic 5: Probabilistic forecasting at scale using tidy data structures", "March 2026", "Paper submission", "0%", -->
  <!-- "PhD Milestones", "February 2024", "Confirmation", "100%", -->
  <!-- "PhD Milestones", "February 2025", "Mid-candidature review", "0%", -->
  <!-- "PhD Milestones", "February 2026", "Final review", "0%", -->

::: columns
::: {.column width="60%"}

::: {.container .fragment .fade-up fragment-index=1}
::: {.timeline .timeline-left style="height: 720px;"}



::: {.timeline-block}
::: {.timeline-icon}
![](resources/memo.png)
:::
::: {.timeline-content}
**Finish graph pruning and distribution papers**

::: {.timeline-details .fragment .fade-out fragment-index=2}

Write more, write more, write more...

:::
::: {.timeline-date}
Feb 2025
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=2}
::: {.timeline-icon}
![](resources/clock.png)
:::
::: {.timeline-content}
**Mixed temporal granularity vectors**
  
::: {.timeline-details .fragment .fade-out fragment-index=4}
Designing a framework for representing temporal moments, intervals, and granularities.
:::
::: {.timeline-date}
Apr 2025
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=4}
::: {.timeline-icon}
![](resources/iif.png)
:::
::: {.timeline-content}
**Presentation at ISF 2025**

::: {.timeline-details .fragment .fade-out fragment-index=5}
About mixed temporal granularity forecasting and reconciliation with fable.

:::
::: {.timeline-date}
Jun 2025
:::
:::
:::

::: {.timeline-block .fragment .fade-up fragment-index=5}
::: {.timeline-icon}
![](resources/fable.png)
:::
::: {.timeline-content}
**Formalise the design of fable**

::: {.timeline-details .fragment .fade-out fragment-index=6}
Incorporate the foundations developed in the research above to enhance the design of tidy forecasting with fable.
:::
::: {.timeline-date}
Dec 2025
:::
:::
:::



::: {.timeline-block .fragment .fade-up fragment-index=6}
::: {.timeline-icon}
![](resources/monash.png)
:::
::: {.timeline-content}
**Final review and presentation**

::: {.timeline-details}

:::
::: {.timeline-date}
Feb 2026
:::
:::
:::


:::
:::


:::
:::

![](backgrounds/estee-janssens-zni0zgb3bkQ-unsplash.jpg){.image-right}


## Thanks for your time!

::: columns
::: {.column width="60%"}

::: {.callout-tip}
## Final remarks

* I'm trying to build a user-friendly design framework for forecast reconciliation, and it's uncovered lots of potential directions for future research.
* Next focus is mixed temporal granularities, which works well in the graph framework.
* Graphs provide a useful representation for all coherency constraints, including non-linear structures.
:::

::: {.callout-link}

## Useful links

![](resources/forum.svg){.icon} [social.mitchelloharawild.com](https://social.mitchelloharawild.com/)

![](resources/projector-screen-outline.svg){.icon} [slides.mitchelloharawild.com/phd-confirmation](https://slides.mitchelloharawild.com/phd-confirmation)

![](resources/github.svg){.icon} [mitchelloharawild/phd-confirmation](https://github.com/mitchelloharawild/phd-confirmation)

:::

:::
:::

![](backgrounds/meric-dagli-7NBO76G5JsE-unsplash.jpg){.image-right}


<!-- ## {} -->

<!-- ::: columns -->
<!-- ::: {.column width="40%"} -->
<!-- ::: -->
<!-- ::: {.column width="60%"} -->
<!-- ### Visualising structured time series -->
<!-- ::: -->
<!-- ::: -->

<!-- ![](backgrounds/yoksel-zok-aEMEMsBNqeo-unsplash.jpg){.image-left} -->


## Unsplash credits

::: {.callout-unsplash}

## Thanks to these Unsplash contributors for their photos


::: {.cell hash='index_cache/revealjs/unsplash_0291571fab3d266b79a79cb254a4841f'}
::: {.cell-output-display}

```{=html}
<ul>
<li>Aleksandar Radovanovic: <a href="https://unsplash.com/photos/mXKXJI98aTE">Tree rings pattern</a></li>
<li>Courtney Smith: <a href="https://unsplash.com/photos/pCLKkPHpCz0">Photo</a></li>
<li>Eilis Garvey: <a href="https://unsplash.com/photos/MskbR8VLNrA">Exposed tree roots</a></li>
<li>Emma Gossett: <a href="https://unsplash.com/photos/B645igbiKCw">Banyan tree in Hawai&lt;U+2019&gt;i </a></li>
<li>Firosnv. Photography: <a href="https://unsplash.com/photos/Rr3B0LH7W3k">Top View, Shot on iPhone 7
Location: Fort…</a></li>
<li>Meriç Dağlı: <a href="https://unsplash.com/photos/7NBO76G5JsE">Photo</a></li>
<li>五玄土 ORIENTO: <a href="https://unsplash.com/photos/-OBffuUekfQ">Photo</a></li>
<li>Sander Weeteling: <a href="https://unsplash.com/photos/KABfjuSOx74">Photo</a></li>
<li>Yoksel 🌿 Zok: <a href="https://unsplash.com/photos/aEMEMsBNqeo">Photo</a></li>
<li>Zach Callahan: <a href="https://unsplash.com/photos/-i51Ke0ROTo">Photo</a></li>
</ul>

```

:::
:::


:::

## References
