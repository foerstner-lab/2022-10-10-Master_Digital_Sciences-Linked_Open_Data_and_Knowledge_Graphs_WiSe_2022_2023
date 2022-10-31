
# Knowledge Graph Embeddings
## Definitions
> Knowledge graph embedding \[...\] represent entities and relations as low dimensional vectors
> (`Zhang et al., 2020`)

> Knowledge graph (KG) embedding is to embed components of a KG including entities and relations into continuous vector spaces
> (`Wang et al., 2017`)
---

## Semantic Similarities
### Goal: 
- Given an entity/document, find the most similar entities/documents.

----
### How to measure semantic similarities?

Can be described by similar facts, e.g.:
* A cat is a pet.
* A dog is a pet.
* A hammer is a tool.
* A screwdriver is a tool.

Is a dog more similar to a screwdriver or a cat?

&rarr; "You shall know a node by the company it keeps."

----

Problem: 
* algorithms for determining semantic similarities in graphs are highly complex
* not efficient for large KGs like Wikidata

Idea:
- Transferring the graph structures to vector spaces
    - ... that represent semantic information
    - ... computationally efficient (`Zhang et al., 2020`)

---
## Vector Space

<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="0 30 500 220" xmlns="http://www.w3.org/2000/svg" xmlns:bx="https://boxy-svg.com">
  <defs>
    <path id="path-0" d="M 49.901 176.987 C 53.059 163.077 55.847 159.384 57.806 155.248 C 60.223 150.147 56.114 139.79 61.759 136.968 L 61.759 135.979 C 61.759 135.632 62.064 134.497 62.253 134.497" style="fill: none;"/>
  </defs>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="36.09" cy="98.752" rx="4.698" ry="4.606"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="19.435" cy="156.653" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="71.108" cy="130.992" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="120.218" cy="123.535" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="118.509" cy="87.128" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="121.499" cy="163.891" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="167.193" cy="99.41" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="168.474" cy="130.553" rx="4.698" ry="4.606" bx:origin="-13.954 -8.69"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, -8.594444, 51.978565)" x="34.091" y="198.617">George</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 5.392992, -26.218885)" x="34.091" y="198.617">Acme Inc</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 48.097477, 24.663807)" x="34.091" y="198.617">Mike</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 95.499458, 57.562099)" x="34.091" y="198.617">Person</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 158.702087, 14.136353)" x="34.091" y="198.617">Football Team</text>
  <text style="white-space: pre; fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.8px;" x="34.091" y="198.617" transform="matrix(0.483989, 0, 0, 0.598839, 84.823334, 16.329575)">Liverpool FC</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 158.702103, -17.446005)" x="34.091" y="198.617">City</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.3431px; line-height: 21.3489px; white-space: pre;" transform="matrix(0.483989, 0, 0, 0.598839, 87.812645, -38.939556)" x="34.091" y="198.617">Liverpool</text>
  <path d="M 146.503 121.801 H 164.846 L 164.846 119.64 L 172.836 122.233 L 164.846 124.826 L 164.846 122.665 H 146.503 V 121.801 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.032435, -0.999474, 0.999416, -0.034219, 2.807863, 269.85437)" bx:shape="arrow 146.503 119.64 26.333 5.186 0.864 7.99 0 1@3c53b391"/>
  <path d="M 147.87 128.956 H 178.657 L 178.657 126.738 L 186.442 129.399 L 178.657 132.061 L 178.657 129.843 H 147.87 V 128.956 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.985344, 0.170579, -0.161923, 0.986842, 0.59263, -29.726322)" bx:shape="arrow 147.87 126.738 38.572 5.323 0.887 7.785 0 1@72fc0cd8"/>
  <path d="M 146.799 91.566 H 178.275 L 178.275 89.352 L 186.071 92.009 L 178.275 94.667 L 178.275 92.452 H 146.799 V 91.566 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.956905, 0.290401, -0.276462, 0.961135, 9.02511, -45.155373)" bx:shape="arrow 146.799 89.352 39.272 5.315 0.886 7.796 0 1@6e26fd30"/>
  <path d="M 43.767 153.312 H 83.836 L 83.836 151.146 L 91.809 153.745 L 83.836 156.344 L 83.836 154.178 H 43.767 V 153.312 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.284447, -0.958692, 0.95444, 0.298777, -138.900604, 147.290405)" bx:shape="arrow 43.767 151.146 48.042 5.198 0.866 7.973 0 1@056c441c"/>
  <path d="M 91.216 131.817 H 120.069 L 120.069 129.624 L 127.944 132.255 L 120.069 134.886 L 120.069 132.694 H 91.216 V 131.817 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.732514, -0.680752, 0.661217, -0.750669, 47.059353, 288.955505)" bx:shape="arrow 91.216 129.624 36.728 5.262 0.877 7.875 0 1@ee236732"/>
  <path d="M 89.727 137.97 H 129.716 L 129.716 135.762 L 137.536 138.411 L 129.716 141.061 L 129.716 138.853 H 89.727 V 137.97 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.893242, 0.449577, -0.430657, -0.902764, 206.59314, 217.458023)" bx:shape="arrow 89.727 135.762 47.809 5.299 0.883 7.82 0 1@c511ff09"/>
  <path d="M 46.553 164.128 H 130.338 L 130.338 161.909 L 138.118 164.572 L 130.338 167.235 L 130.338 165.016 H 46.553 V 164.128 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.998059, 0.062277, -0.059039, 0.998261, -12.613289, -8.899938)" bx:shape="arrow 46.553 161.909 91.565 5.326 0.888 7.78 0 1@158812f2"/>
  <path d="M 98.458 138.638 H 140.702 L 140.702 136.433 L 148.534 139.079 L 140.702 141.724 L 140.702 139.52 H 98.458 V 138.638 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.861324, 0.508056, -0.488118, 0.873085, 58.460133, -37.728706)" bx:shape="arrow 98.458 136.433 50.076 5.291 0.882 7.832 0 1@72d68c4d"/>
  <path d="M 97.408 134.26 H 130.536 L 130.536 132.041 L 138.318 134.703 L 130.536 137.366 L 130.536 135.147 H 97.408 V 134.26 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.991688, -0.128668, 0.122059, 0.992545, -38.089123, 7.292102)" bx:shape="arrow 97.408 132.041 40.91 5.325 0.887 7.782 0 1@22e8585e"/>
  <path d="M 95.501 131.003 H 143.972 L 143.972 128.809 L 151.844 131.441 L 143.972 134.073 L 143.972 131.88 H 95.501 V 131.003 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.742782, -0.669533, 0.649794, 0.760575, -84.384193, 90.6726)" bx:shape="arrow 95.501 128.809 56.343 5.264 0.877 7.872 0 1@41842b6d"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="29.644" y="155.248" transform="matrix(0.229582, -0.858018, 0.816124, 0.292375, -115.389641, 123.212967)">worksFor</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="88.014" y="128.568" transform="matrix(0.632839, 0.604715, -0.588724, 0.650028, 66.957001, -34.656052)" bx:origin="0.514 0.281">worksFor</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="29.644" y="155.248" transform="matrix(0.688386, 0.42745, -0.427792, 0.627224, 137.47197, 29.859283)">isA</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="88.014" y="128.568" transform="matrix(0.857173, 0.114082, -0.111065, 0.880455, -0.276896, 33.106148)" bx:origin="0.514 0.281">isA</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 6.96686px; white-space: pre;" transform="matrix(0.86159, -0.092705, 0.031979, 0.887208, 57.266743, -10.020614)" x="29.644" y="155.248">likes</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7.12756px; white-space: pre;" transform="matrix(0.840184, 0.216861, -0.266598, 0.844528, 152.165466, -15.824865)" x="29.644" y="155.248">isA</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7.13461px; white-space: pre;" transform="matrix(0.836422, 0.231697, -0.280724, 0.839685, 155.780807, -49.493679)" x="29.644" y="155.248">isA</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 6.77216px; white-space: pre;" transform="matrix(0.64248, -0.596906, 0.535338, 0.697029, -20.925728, 24.899628)" x="29.644" y="155.248">bornIn</text>
  <path d="M 141.798 218.418 H 159.387 L 159.387 208.918 L 176.387 224.418 L 159.387 239.918 L 159.387 230.418 H 141.798 V 218.418 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.999842, 0.017764, -0.017764, 0.999842, 98.13076, -96.664276)" bx:shape="arrow 141.798 208.918 34.589 31 12 17 0 1@7b8dfc18"/>
  <path d="M 47.776 335.392 H 211.677 L 211.677 334.629 L 217.049 335.774 L 211.677 336.919 L 211.677 336.156 H 47.776 V 335.392 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.000004, -1, 1, 0.000004, -35.459873, 261.489502)" bx:shape="arrow 47.776 334.629 169.273 2.29 0.764 5.372 0 1@37d7662a"/>
  <path d="M 30.906 412.222 H 195.697 L 195.697 411.281 L 199.173 412.692 L 195.697 414.102 L 195.697 413.162 H 30.906 V 412.222 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, -0.000067, 0.000067, 1, 269.403687, -198.747742)" bx:shape="arrow 30.906 411.281 168.267 2.821 0.94 3.476 0 1@461d169a"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="317.814" cy="154.926" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13.5165px; line-height: 21.6263px; white-space: pre;" transform="matrix(0.393816, 0, 0, 0.487023, 296.024658, 68.856155)" x="34.091" y="198.617">George</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 10.3779px; line-height: 16.6047px; white-space: pre;" transform="matrix(0.523454, 0, 0, 0.63431, 306.175446, 10.855365)" x="34.091" y="198.617">Mike</text>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="329.413" cy="126.432" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="335.406" cy="92.436" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="353.892" cy="106.385" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="361.744" cy="130.613" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="377.92" cy="168.237" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="388.994" cy="191.015" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="418.092" cy="213.692" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="439.338" cy="186.744" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="429.176" cy="92.657" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="459.637" cy="100.6" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="429.232" cy="140.027" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="459.693" cy="153.612" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="387.608" cy="234.6" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="352.969" cy="222.52" rx="3.739" ry="3.746" bx:origin="-13.954 -8.69"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7.01592px; white-space: pre;" transform="matrix(0.936944, 0.000012, -0.062938, 0.93827, 322.716156, 87.068985)" x="29.644" y="155.248">bornIn</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" transform="matrix(0.934818, 0.000028, -0.000028, 0.940404, 290.051117, 79.084061)" x="88.014" y="128.568" bx:origin="0.514 0.281">worksFor</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" transform="matrix(0.934818, 0.000028, -0.000028, 0.940404, 298.752808, 124.842728)" x="88.014" y="128.568" bx:origin="0.514 0.281">likes</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="316.699" y="457.126" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">basedIn</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="367.462" y="334.597" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">City</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="325.96" y="377.087" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Liverpool FC</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="322.007" y="327.185" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Liverpool</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="347.276" y="426.506" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">isA</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="363.073" y="391.415" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Football Team</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="222.463" y="308.905" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Acme Inc</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="246.673" y="340.525" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Employee</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="260.013" y="365.229" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">Person</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="67.325" y="165.624" transform="matrix(0.764368, -0.414473, 0.403513, 0.78513, -81.926941, 44.200043)">friendsWith</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="173.292" y="132.521" transform="matrix(-0.035024, 0.887086, -0.863629, -0.035975, 246.300018, -50.959087)">basedIn</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 7px; white-space: pre;" x="270.27" y="406.237" transform="matrix(0.934818, 0, 0, 0.940404, 110.026337, -203.645645)">friendsWith</text>
</svg>

*Own visualization based on`Costabello et al. (2021)`.*

Word Embeddings:
- Mapping natural language to dense vector representations

Basic Assumption:
* Similar words occur in similar contexts (dog, cat, bird, ...)
* Instead of counting co-occurences of words; predict the likelihood of a words appearance in the neighborhood of others 
* (`Hasso-Plattner-Institut, o.D.`)
----
## Word Embeddings
- Unsupervised learning for word representations
### Skip Gram

- Predict context words for a given word (before and after)

[![](https://mermaid.ink/img/pako:eNptzzEOwjAMBdCrRJ5a0QwwZmDqygRSFy-mNjSCpFVIBlT17iTtWDxZT1-W_wz9yAIGnoGmQd1a9CqP9VOKVVcrrc9qsMziKwStNUKttsima4CqLupTvfN78ePe--yHP87Fyx300ICT4Mhy_m0uSYQ4iBMEk1em8EJAv-QcpThev74HE0OSBtLEFKW1lCs5MA96f7IK2ziGy1Z27bz8AJ77S5g?type=png)](https://mermaid.live/edit#pako:eNptzzEOwjAMBdCrRJ5a0QwwZmDqygRSFy-mNjSCpFVIBlT17iTtWDxZT1-W_wz9yAIGnoGmQd1a9CqP9VOKVVcrrc9qsMziKwStNUKttsima4CqLupTvfN78ePe--yHP87Fyx300ICT4Mhy_m0uSYQ4iBMEk1em8EJAv-QcpThev74HE0OSBtLEFKW1lCs5MA96f7IK2ziGy1Z27bz8AJ77S5g)

*Own visualization based on `Mikolov et al. (2012)`.*
### CBOW (Continous Bag of Words)
- Predict a given word using context words

[![](https://mermaid.ink/img/pako:eNp90LESgjAMANBf6WWCkw46dnBiddI7liyRROkphatl8Dj-3UA3BzL10tckzQztwAIOnpHGztxqDEaDiibZU2msPZvOM0soEKy1CGUG9xUcd0Cr4LAHeAW5xd-9ySBntwJSNPoKA1TQS-zJs048rwwhddILgtMjU3whYFjU0ZSG6ze04FKcpIJpZEpSe9KP9uAe9P5oVtinIV7yCrZNLD_5608p?type=png)](https://mermaid.live/edit#pako:eNp90LESgjAMANBf6WWCkw46dnBiddI7liyRROkphatl8Dj-3UA3BzL10tckzQztwAIOnpHGztxqDEaDiibZU2msPZvOM0soEKy1CGUG9xUcd0Cr4LAHeAW5xd-9ySBntwJSNPoKA1TQS-zJs048rwwhddILgtMjU3whYFjU0ZSG6ze04FKcpIJpZEpSe9KP9uAe9P5oVtinIV7yCrZNLD_5608p)
*Own visualization based on `Mikolov et al. (2012)`.*

---
## Knowledge Graphs
### Triplet representation
- Representation as triplets $\mathbb{D}^+=\{(h, r, t)\}$:
    - Head $h \in \mathbb{E}$
    - Tail $t \in \mathbb{E}$ 
    - Relation $r \in \mathbb{R}$ 
- Example:
    - AlfredHitchcock, DirectorOf, Psycho
    - Head: `AlfredHitchcock`
    - Relation: `DirectorOf`
    - Tail: `Psycho`

----
## Graph Embeddings
### Encoder - Decoder - Approach
<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="0 20 500 160" xmlns="http://www.w3.org/2000/svg" xmlns:bx="https://boxy-svg.com">
  <path d="M 92.185 95.493 H 193.574 L 193.574 84.993 L 226.406 98.493 L 193.574 111.993 L 193.574 101.493 H 92.185 V 95.493 Z" style="fill: rgb(187, 187, 187);" bx:shape="arrow 92.185 84.993 134.221 27 6 32.832 0 1@e5733e9d"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="68.27" cy="71.235" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="34.093" cy="42.034" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="97.562" cy="39.857" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="18.223" cy="80.749" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="141.624" cy="105.75" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="74.44" cy="123.705" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="61.106" cy="98.824" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="131.646" cy="128.361" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="102.738" cy="144.373" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="111.479" cy="112.149" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="53.189" cy="155.875" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="27.473" cy="147.93" rx="3.376" ry="3.376"/>
  <ellipse style="stroke: rgb(0, 0, 0); fill: rgb(175, 0, 0);" cx="87.848" cy="98.667" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="141.186" cy="70.294" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="101.997" cy="78.887" rx="3.376" ry="3.376"/>
  <text style="fill: rgb(255, 255, 255); font-family: Arial, sans-serif; font-size: 6px; white-space: pre;" x="85.654" y="100.561">vᵢ</text>
  <path d="M 35.619 38.195 H 74.171 L 74.171 38.195 L 74.921 38.57 L 74.171 38.945 L 74.171 38.945 H 35.619 V 38.195 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.782161, 0.623077, -0.623077, 0.782161, 31.791323, -8.455237)" bx:shape="arrow 35.619 38.195 39.302 0.75 0.75 0.75 0 1@8da110f8"/>
  <path d="M 31.972 39.237 H 65.376 L 65.376 39.237 L 66.126 39.612 L 65.376 39.987 L 65.376 39.987 H 31.972 V 39.237 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.366124, 0.930566, -0.930566, -0.366124, 80.539673, 29.69882)" bx:shape="arrow 31.972 39.237 34.154 0.75 0.75 0.75 0 1@c0f4d48a"/>
  <path d="M 22.073 74.666 H 65.875 L 65.875 74.666 L 66.626 75.042 L 65.875 75.417 L 65.875 75.417 H 22.073 V 74.666 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.982339, -0.187112, 0.187112, 0.982339, -13.651325, 10.791865)" bx:shape="arrow 22.073 74.666 44.553 0.75 0.75 0.75 0 1@84ef627a"/>
  <path d="M 70.528 63.204 H 105.531 L 105.531 63.204 L 106.281 63.579 L 105.531 63.954 L 105.531 63.954 H 70.528 V 63.204 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.68491, -0.728628, 0.728628, 0.68491, -24.102705, 76.758179)" bx:shape="arrow 70.528 63.204 35.754 0.75 0.75 0.75 0 1@745bb211"/>
  <path d="M 98.663 36.631 H 130.886 L 130.886 36.631 L 131.636 37.006 L 130.886 37.382 L 130.886 37.382 H 98.663 V 36.631 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.094809, 0.995495, -0.995495, 0.094809, 126.149178, -59.384212)" bx:shape="arrow 98.663 36.631 32.973 0.75 0.75 0.75 0 1@200af3ed"/>
  <path d="M 105.437 72.061 H 138.613 L 138.613 72.061 L 139.363 72.437 L 138.613 72.812 L 138.613 72.812 H 105.437 V 72.061 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.982872, -0.184289, 0.184289, 0.982872, -11.543288, 26.007843)" bx:shape="arrow 105.437 72.061 33.927 0.75 0.75 0.75 0 1@bc9915d5"/>
  <path d="M 139.303 61.119 H 184.827 L 184.827 61.119 L 185.578 61.495 L 184.827 61.87 L 184.827 61.87 H 139.303 V 61.119 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.810679, -0.585491, 0.585491, -0.810679, 216.228485, 198.244492)" bx:shape="arrow 139.303 61.119 46.275 0.75 0.75 0.75 0 1@fc5b3dad"/>
  <path d="M 102.31 75.709 H 128.583 L 128.583 75.709 L 129.334 76.084 L 128.583 76.459 L 128.583 76.459 H 102.31 V 75.709 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.327773, 0.944757, -0.944757, 0.327773, 140.65625, -40.176632)" bx:shape="arrow 102.31 75.709 27.023 0.75 0.75 0.75 0 1@06971e71"/>
  <path d="M 108.042 105.407 H 126.953 L 126.953 105.407 L 127.703 105.782 L 126.953 106.157 L 126.953 106.157 H 108.042 V 105.407 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.847998, -0.529999, 0.529999, -0.847998, 143.596588, 258.082977)" bx:shape="arrow 108.042 105.407 19.662 0.75 0.75 0.75 0 1@7c882c2b"/>
  <path d="M 90.848 90.818 H 107.441 L 107.441 90.818 L 108.191 91.193 L 107.441 91.569 L 107.441 91.569 H 90.848 V 90.818 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.540758, -0.841178, 0.841178, 0.540758, -34.988506, 123.635498)" bx:shape="arrow 90.848 90.818 17.343 0.75 0.75 0.75 0 1@31cc9aef"/>
  <path d="M 114.294 105.928 H 137.965 L 137.965 105.928 L 138.715 106.303 L 137.965 106.678 L 137.965 106.678 H 114.294 V 105.928 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.981393, -0.192012, 0.192012, 0.981393, -18.284668, 29.260195)" bx:shape="arrow 114.294 105.928 24.421 0.75 0.75 0.75 0 1@1b11e6a3"/>
  <path d="M 114.294 108.533 H 131.365 L 131.365 108.533 L 132.116 108.908 L 131.365 109.283 L 131.365 109.283 H 114.294 V 108.533 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.789352, 0.613941, -0.613941, 0.789352, 90.938774, -41.892067)" bx:shape="arrow 114.294 108.533 17.822 0.75 0.75 0.75 0 1@e950f326"/>
  <path d="M 128.883 124.685 H 154.705 L 154.705 124.685 L 155.455 125.06 L 154.705 125.435 L 154.705 125.435 H 128.883 V 124.685 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.882353, 0.470588, -0.470588, -0.882353, 301.454132, 180.092178)" bx:shape="arrow 128.883 124.685 26.572 0.75 0.75 0.75 0 1@c171ee8a"/>
  <path d="M 102.31 135.105 H 128.259 L 128.259 135.105 L 129.009 135.48 L 128.259 135.855 L 128.259 135.855 H 102.31 V 135.105 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.292714, -0.9562, 0.9562, 0.292714, -57.183365, 198.988785)" bx:shape="arrow 102.31 135.105 26.699 0.75 0.75 0.75 0 1@5e3680e6"/>
  <path d="M 86.158 95.508 H 106.617 L 106.617 95.508 L 107.367 95.883 L 106.617 96.258 L 106.617 96.258 H 86.158 V 95.508 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.466759, 0.884385, -0.884385, -0.466759, 211.170944, 69.775246)" bx:shape="arrow 86.158 95.508 21.209 0.75 0.75 0.75 0 1@a18fb2aa"/>
  <path d="M 65.318 97.718 H 83.325 L 83.325 97.718 L 84.075 98.093 L 83.325 98.468 L 83.325 98.468 H 65.318 V 97.718 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" bx:shape="arrow 65.318 97.718 18.757 0.75 0.75 0.75 0 1@f6f972db"/>
  <path d="M 61.671 96.028 H 81.426 L 81.426 96.028 L 82.177 96.403 L 81.426 96.778 L 81.426 96.778 H 61.671 V 96.028 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.457348, 0.889288, -0.889288, 0.457348, 119.196053, 2.806957)" bx:shape="arrow 61.671 96.028 20.506 0.75 0.75 0.75 0 1@612ba088"/>
  <path d="M 59.065 95.508 H 110.052 L 110.052 95.508 L 110.802 95.883 L 110.052 96.258 L 110.052 96.258 H 59.065 V 95.508 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.563962, 0.825801, -0.825801, -0.563962, 171.555969, 106.51638)" bx:shape="arrow 59.065 95.508 51.737 0.75 0.75 0.75 0 1@a1d284d8"/>
  <path d="M 54.376 147.089 H 85.854 L 85.854 147.089 L 86.604 147.464 L 85.854 147.839 L 85.854 147.839 H 54.376 V 147.089 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.565843, -0.824513, 0.824513, 0.565843, -97.977844, 114.192863)" bx:shape="arrow 54.376 147.089 32.228 0.75 0.75 0.75 0 1@bde30e75"/>
  <path d="M 71.049 69.456 H 94.892 L 94.892 69.456 L 95.643 69.831 L 94.892 70.206 L 94.892 70.206 H 71.049 V 69.456 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.593199, 0.805056, -0.805056, 0.593199, 85.120857, -23.454632)" bx:shape="arrow 71.049 69.456 24.593 0.75 0.75 0.75 0 1@fd36834f"/>
  <rect x="161.364" y="180.42" width="9" height="9.751" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, 0, 0, 0, 0, 0)"/>
  <rect x="233.898" y="54.804" width="9" height="9" style="fill: rgb(213, 172, 128);"/>
  <rect x="233.884" y="66.095" width="9" height="9" style="fill: rgb(190, 204, 209);"/>
  <rect x="233.898" y="76.925" width="9" height="9" style="fill: rgb(187, 209, 215);"/>
  <rect x="233.898" y="87.815" width="9" height="9" style="fill: rgb(152, 156, 89);"/>
  <rect x="233.912" y="98.814" width="9" height="9" style="fill: rgb(190, 191, 167);"/>
  <rect x="233.898" y="110.105" width="9" height="9" style="fill: rgb(121, 121, 131);"/>
  <rect x="233.912" y="120.935" width="9" height="9" style="fill: rgb(77, 99, 100);"/>
  <rect x="233.912" y="131.825" width="9" height="9" style="fill: rgb(94, 131, 138);"/>
  <text style="fill: rgb(0, 0, 0); font-family: Arial, sans-serif; font-size: 10px; white-space: pre;" x="233.912" y="150.825">zᵢ</text>
  <text style="fill: rgb(0, 0, 0); font-family: Arial, sans-serif; font-size: 10px; white-space: pre;" x="213.912" y="160.825">(embedding)</text>
  <rect x="392" y="100.798" width="70" height="70" style="stroke: rgb(0, 0, 0); fill: rgb(255, 255, 255);"/>
  <rect x="392" y="23.538" width="70" height="70" style="stroke: rgb(0, 0, 0); fill: rgb(255, 255, 255);"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="410.328" cy="35.783" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="416.498" cy="88.253" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="403.164" cy="63.372" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="453.537" cy="76.697" rx="3.376" ry="3.376"/>
  <ellipse style="stroke: rgb(0, 0, 0); fill: rgb(175, 0, 0);" cx="429.906" cy="63.215" rx="3.376" ry="3.376"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="444.055" cy="43.435" rx="3.376" ry="3.376"/>
  <text style="fill: rgb(255, 255, 255); font-family: Arial, sans-serif; font-size: 6px; white-space: pre;" x="427.712" y="65.109">vᵢ</text>
  <path d="M 102.31 75.709 H 128.583 L 128.583 75.709 L 129.333 76.084 L 128.583 76.459 L 128.583 76.459 H 102.31 V 75.709 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.327773, 0.944757, -0.944757, 0.327773, 482.713959, -75.628281)" bx:shape="arrow 102.31 75.709 27.023 0.75 0.75 0.75 0 1@94daf832"/>
  <path d="M 108.042 105.407 H 126.954 L 126.954 105.407 L 127.704 105.782 L 126.954 106.157 L 126.954 106.157 H 108.042 V 105.407 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.847998, -0.529999, 0.529999, -0.847998, 485.654297, 222.631348)" bx:shape="arrow 108.042 105.407 19.662 0.75 0.75 0.75 0 1@5451fd96"/>
  <path d="M 90.848 90.818 H 107.441 L 107.441 90.818 L 108.191 91.193 L 107.441 91.568 L 107.441 91.568 H 90.848 V 90.818 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.540758, -0.841178, 0.841178, 0.540758, 307.069214, 88.183861)" bx:shape="arrow 90.848 90.818 17.343 0.75 0.75 0.75 0 1@491e5f39"/>
  <path d="M 86.158 95.508 H 106.617 L 106.617 95.508 L 107.367 95.883 L 106.617 96.258 L 106.617 96.258 H 86.158 V 95.508 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.466759, 0.884385, -0.884385, -0.466759, 553.228638, 34.323601)" bx:shape="arrow 86.158 95.508 21.209 0.75 0.75 0.75 0 1@a18fb2aa"/>
  <path d="M 407.376 62.266 H 425.383 L 425.383 62.266 L 426.133 62.641 L 425.383 63.016 L 425.383 63.016 H 407.376 V 62.266 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" bx:shape="arrow 407.376 62.266 18.757 0.75 0.75 0.75 0 1@92cf435c"/>
  <path d="M 61.671 96.028 H 81.427 L 81.427 96.028 L 82.177 96.403 L 81.427 96.778 L 81.427 96.778 H 61.671 V 96.028 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.457348, 0.889288, -0.889288, 0.457348, 461.253784, -32.644691)" bx:shape="arrow 61.671 96.028 20.506 0.75 0.75 0.75 0 1@ca71c66e"/>
  <path d="M 71.049 69.456 H 94.892 L 94.892 69.456 L 95.642 69.831 L 94.892 70.206 L 94.892 70.206 H 71.049 V 69.456 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.593199, 0.805056, -0.805056, 0.593199, 427.178558, -58.906273)" bx:shape="arrow 71.049 69.456 24.593 0.75 0.75 0.75 0 1@2cdf0162"/>
  <path d="M 137.661 95.264 H 248.339 L 248.339 84.789 L 284.179 98.257 L 248.339 111.724 L 248.339 101.25 H 137.661 V 95.264 Z" style="fill: rgb(187, 187, 187);" transform="matrix(0.948508, -0.316754, 0.382611, 0.926515, 81.230324, 45.970791)" bx:shape="arrow 137.661 84.789 146.518 26.935 5.986 35.84 0 1@c4269f6a"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 13px; font-weight: 700; white-space: pre;" x="394.434" y="117.407">node label</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 11px; text-anchor: middle; white-space: pre;" transform="matrix(1, 0, 0, 1, 87.751434, 0.784917)"><tspan x="338.116" y="134.283">e.g.</tspan><tspan x="338.116" dy="1em">​</tspan><tspan>community,</tspan><tspan x="338.116" dy="1em">​</tspan><tspan>function</tspan></text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 11px; text-anchor: middle; white-space: pre;" x="250.953" y="70.301" transform="matrix(0.953528, -0.301303, 0.301303, 0.953528, 38.30051, 78.721329)">decode neighborhood</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 11px; text-anchor: middle; white-space: pre;" x="270.498" y="107.988" transform="matrix(0.943598, 0.331096, -0.330609, 0.943768, 79.61245, -57.972446)">decode node label</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 11px; text-anchor: middle; white-space: pre;" x="155.115" y="93.075">encode node</text>
  <path d="M 137.661 95.264 H 248.339 L 248.339 84.789 L 284.179 98.257 L 248.339 111.724 L 248.339 101.25 H 137.661 V 95.264 Z" style="fill: rgb(187, 187, 187);" transform="matrix(0.948307, 0.317355, -0.251511, 0.970342, 141.754929, -35.452896)" bx:shape="arrow 137.661 84.789 146.518 26.935 5.986 35.84 0 1@c4269f6a"/>
</svg>

*Own visualization based on `Hamilton et al. (2017)`*

#### Encoder
* Translate features of a node into a low-dimensional representation:

* Features might be:
    * position in the graph
    * local neighborhood structure
    * attributes


#### Decoder
* takes the encoded data as input
* extracts user-specified information, e.g.:
    * local graph neighborhood of vᵢ (e.g., the identity of its neighbors) 
    * classification label associated with vᵢ (e.g., a community label)

*Optimize encoder and decoder*
&rarr; System learns to compress information about graph structure into the low-dimension

&rarr; Encode nodes so that the similarity in the embedding (e.g. dot product) approximates the similarity in the original network

---

## Examples for KGE Models
Rossi et al. propose a taxonomy of the embedding models and identifies three main families of models (`Rossi, et al., 2021`): 
- Tensor decomposition models
- Geometric models
- Deep learning models

They differ by method and features.

----
### Tensor decomposition models
- Initial group of algorithms
- Represent Knowledge graph as a 3-way tensor and ..
- Embeddings as vectors
- Light and easy to train

----
### Geometric models
- Encode relation as geometric transformation
    - Like Word2Vec &rarr; Word Embeddings
- Apply a transformation to the head and tail (entities)
- Can incorporate additional features (context)
- Various models: *TransE*, *TransH*, *TransR*, *TransD*, *TransA*, *STransE*

----
### TransE
<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="0 0 500 160" xmlns="http://www.w3.org/2000/svg" xmlns:bx="https://boxy-svg.com">
  <path d="M 43.128 166.586 H 177.966 L 177.966 164.745 L 183.002 166.816 L 177.966 168.886 L 177.966 167.046 H 43.128 V 166.586 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.000044, -1, 1, 0.000042, -149.665375, 183.436768)" bx:shape="arrow 43.128 164.745 139.874 4.141 0.46 5.035 0 1@faba6c1b"/>
  <path d="M 41.153 215.48 H 176.222 L 176.222 213.098 L 181.027 215.777 L 176.222 218.456 L 176.222 216.075 H 41.153 V 215.48 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, -0.000045, 0.000046, 1, -23.955608, -75.640686)" bx:shape="arrow 41.153 213.098 139.874 5.358 0.595 4.805 0 1@c8112ec5"/>
  <path d="M 29.403 164.24 H 119.635 L 119.635 162.421 L 123.113 164.468 L 119.635 166.515 L 119.635 164.696 H 29.403 V 164.24 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.909291, -0.416162, 0.425456, 0.905037, -79.316566, 2.626502)" bx:shape="arrow 29.403 162.421 93.71 4.094 0.456 3.478 0 1@857395c7"/>
  <path d="M 34.666 164.581 H 127.688 L 127.688 162.747 L 131.787 164.81 L 127.688 166.872 L 127.688 165.039 H 34.666 V 164.581 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.525949, -0.850516, 0.856986, 0.515486, -142.086487, 82.840981)" bx:shape="arrow 34.666 162.747 97.122 4.125 0.458 4.1 0 1@f4ecdd7c"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="169.088" y="175.493" transform="matrix(0.460192, 0, 0, 0.453868, -17.629509, -24.847292)">h</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="293.088" y="244.493" transform="matrix(0.460192, 0, 0, 0.453868, -33.737301, -16.69009)">r</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="270.088" y="265.493" transform="matrix(0.460192, 0, 0, 0.453868, -26.848471, -12.767268)">t</text>
  <text style="white-space: pre; fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 28px;" x="24.088" y="212.493" transform="matrix(0.365421, 0, 0, 0.368734, 11.362145, 73.341209)">Entity and Relation Space</text>
  <path d="M 58.286 55.275 H 111.049 L 111.049 54.117 L 114.028 55.441 L 111.049 56.765 L 111.049 55.606 H 58.286 V 55.275 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.605685, 0.795704, -0.795704, 0.605685, 77.186874, -24.220589)" bx:shape="arrow 58.286 54.117 55.742 2.648 0.331 2.979 0 1@2217303f"/>
</svg>

*Own visualization based on `Sun et al. (2022)`*

- the first algorithm that was proposed for Embeddings
- Translational Distance Model
- $h+r ≈ t$ when $(h, r, t)$
- Examples: 
    - `AlfredHitchcock` + `DirectorOf` ≈ `Psycho`
    - `Psycho` − `AlfredHitchcock` ≈ `Avatar` − `JamesCameron`
- Problem:
    - 1-to-N and N-to-N relations 

----
### TransH
<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="0 0 500 140" xmlns="http://www.w3.org/2000/svg" xmlns:bx="https://boxy-svg.com">
  <path d="M 34.927 134.911 H 144.127 L 144.127 133.42 L 148.205 135.097 L 144.127 136.774 L 144.127 135.283 H 34.927 V 134.911 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.000044, -1, 1, 0.000042, -118.339539, 151.640701)" bx:shape="arrow 34.927 133.42 113.278 3.354 0.372 4.078 0 1@75216791"/>
  <path d="M 33.329 174.507 H 142.716 L 142.716 172.578 L 146.607 174.748 L 142.716 176.917 L 142.716 174.989 H 33.329 V 174.507 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, -0.000045, 0.000046, 1, -16.532801, -58.174591)" bx:shape="arrow 33.329 172.578 113.278 4.339 0.482 3.891 0 1@c7325fbb"/>
  <path d="M 23.812 133.012 H 96.887 L 96.887 131.538 L 99.704 133.196 L 96.887 134.854 L 96.887 133.38 H 23.812 V 133.012 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.909291, -0.416162, 0.425456, 0.905037, -61.367176, 5.210515)" bx:shape="arrow 23.812 131.538 75.892 3.316 0.368 2.817 0 1@6c4d0485"/>
  <path d="M 28.074 133.286 H 103.409 L 103.409 131.802 L 106.729 133.472 L 103.409 135.143 L 103.409 133.658 H 28.074 V 133.286 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.525949, -0.850516, 0.856986, 0.515486, -112.201729, 70.172638)" bx:shape="arrow 28.074 131.802 78.655 3.341 0.372 3.32 0 1@ec19e000"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="169.088" y="175.493" transform="matrix(0.372689, 0, 0, 0.367567, -11.409575, -17.039284)">h</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="293.088" y="244.493" transform="matrix(0.372689, 0, 0, 0.367567, -26.599079, 11.280132)">r</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="270.088" y="265.493" transform="matrix(0.372689, 0, 0, 0.367567, -18.875601, -7.256212)">t</text>
  <text style="white-space: pre; fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 28px;" x="24.088" y="212.493" transform="matrix(0.295939, 0, 0, 0.298621, 12.069475, 62.479206)">Entity and Relation Space</text>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="68.761" cy="97.81" rx="0.901" ry="0.901"/>
  <ellipse style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" cx="91.674" cy="104.504" rx="0.901" ry="0.901"/>
  <path style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0); stroke-dasharray: 4px; stroke-dashoffset: 13px;" d="M 58.335 48.766 L 68.633 97.681"/>
  <path style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0); stroke-dasharray: 4px;" d="M 85.882 84.809 L 91.546 104.633"/>
  <rect x="169.181" y="25.172" width="79.901" height="27.799" style="stroke: rgb(0, 0, 0); stroke-dasharray: 2px; fill: none;" transform="matrix(0.995155, -0.09832, -1.094791, 1.113033, -94.142471, 74.185989)"/>
  <path d="M 58.464 57.839 H 78.8 L 78.8 57.186 L 81.127 57.933 L 78.8 58.679 L 78.8 58.026 H 58.464 V 57.839 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(0.958452, 0.285254, -0.285254, 0.958452, 29.976524, 25.722477)" bx:shape="arrow 58.464 57.186 22.663 1.493 0.187 2.327 0 1@39f02da8"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="169.088" y="175.493" transform="matrix(0.372689, 0, 0, 0.367567, 1.536479, 37.408791)">h</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" x="270.088" y="265.493" transform="matrix(0.372689, 0, 0, 0.367567, -7.445937, 5.614982)">t</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" transform="matrix(0.249221, 0, 0, -0.271606, 53.307312, 148.633957)" x="169.088" y="175.493">T</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 17px; white-space: pre;" transform="matrix(0.249221, 0, 0, -0.271606, 26.168289, 147.856064)" x="169.088" y="175.493">T</text>
</svg>

*Own visualization based on `Sun et al. (2022)`*

- Extension of TransE
- Improve the N-to-N relation representation
- Relation specific entity relations with dedicated spaces for:
    - entity
    - relation

----
### RDF2Vec
* builds further on Word2Vec; i.e. learning embeddings in two ways:
    1. the word based on its context: Continuous Bag-of-Words (CBOW);
    2. the context based on a word: Skip-Gram (SG)
* creates “sentences” (triplets) by extracting walks of a certain depth from a KG


----
### Gaussian Embedding Models
- entities as well as relations as deterministic points in vector spaces
- represents entities and relations as random vectors drawn from multivariate Gaussian distributions
- entities as well as relations as deterministic points in vector spaces
- take uncertainties into account 


----
### Deep Learning Models
- Expensive, often needs pre-training
- Types:
    - Convolutional neural networks (CNN)
    - Capsule neural networks
        - preserve spacial information
    - Recurrent neural networks (RNN)
        - relations as sequences
- Various models: *ConvKB*, *ConvE*, *CapsE*, *ConvR*, *RSN* (`Rossi, et al., 2021`)
---
## Applications
- NLP (`Zhang et al., 2020`)
- Recommender systems (`Zhang et al., 2020`)
- Question answering (`Zhang et al., 2020`)
- Drug Repurposing (Biomedical knowledgegraph; create links between drugs and diseases) (`Sosa et al., 2020`)
---
## Python Libraries for KGEs
- [pyRDF2Vec](https://github.com/IBCNServices/pyRDF2Vec)
- [PyTorchBigGraph](https://github.com/facebookresearch/PyTorch-BigGraph)
- [AmpliGraph](https://github.com/Accenture/AmpliGraph)
- [PyKeen](https://github.com/pykeen/pykeen)
- [OpenKE](https://github.com/thunlp/OpenKE)

---
## References

- Çelik, T. (2022). The Beginner’s Guide to Text Embeddings. deepset GmbH. https://www.deepset.ai/blog/the-beginners-guide-to-text-embeddings?utm_content=225132396
- Costabello, L. (2021, 25. Mai). Accenture/AmpliGraph: AmpliGraph 1.4.0. Zenodo. https://zenodo.org/record/4792436
- Hamilton, W. L., Ying, R. & Leskovec, J. (2017). Representation Learning on Graphs: Methods and Applications. IEEE Data(base) Engineering Bulletin, 40, 52–74. https://arxiv.org/pdf/1709.05584
- Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient Estimation of Word Representations in Vector Space (arXiv:1301.3781). arXiv. http://arxiv.org/abs/1301.3781
- Hasso-Plattner-Institut (o. D.). [Video]. 6.2 Knowledge Graph Embeddings. openHPI. Abgerufen am 30. Oktober 2022, von https://open.hpi.de/courses/knowledgegraphs2020/items/5SqYyJjrEZeOvsG9z0bmDw?locale=de
- Rossi, A., Barbosa, D., Firmani, D., Matinata, A., & Merialdo, P. (2021). Knowledge Graph Embedding for Link Prediction: A Comparative Analysis. ACM Transactions on Knowledge Discovery from Data, 15(2), 1–49. https://doi.org/10.1145/3424672
-  Sosa, Daniel N.; Derry, Alexander; Guo, Margaret; Wei, Eric; Brinton, Connor; Altman, Russ B. (2020). A Literature-Based Knowledge Graph Embedding Method for Identifying Drug Repurposing Opportunities in Rare Diseases. Pacific Symposium on Biocomputing. Pacific Symposium on Biocomputing. 25: 463–474.
- Sun, Y., Chun, S. J., & Lee, Y. (2022). Learned Semantic Index Structure Using Knowledge Graph Embedding and Density-Based Spatial Clustering Techniques. Applied Sciences, 12(13), 6713. https://doi.org/10.3390/app12136713
- Wang, Q., Mao, Z., Wang, B., & Guo, L. (2017). Knowledge Graph Embedding: A Survey of Approaches and Applications. IEEE Transactions on Knowledge and Data Engineering, 29(12), 2724–2743. https://doi.org/10.1109/TKDE.2017.2754499
- Zhang, Z., Cai, J., Zhang, Y., & Wang, J. (2020). Learning Hierarchy-Aware Knowledge Graph Embeddings for Link Prediction. Proceedings of the AAAI Conference on Artificial Intelligence, 34(03), 3065–3072. https://doi.org/10.1609/aaai.v34i03.5701