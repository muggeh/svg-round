# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains a single Jupyter notebook (`svg-arcs-calculator.ipynb`) — a mathematical tool for calculating the coordinates needed to create rounded corners (arc elements) in SVG path definitions.

The problem it solves: given two crossing lines and a radius, compute the tangent points and circle center coordinates needed for an SVG `A` (arc) command in a `<path>` element.

## Running the Notebook

```bash
jupyter notebook svg-arcs-calculator.ipynb
# or
jupyter lab svg-arcs-calculator.ipynb
```

Dependencies: `numpy`, `matplotlib` (standard scientific Python stack).

## Notebook Architecture

The notebook is a sequential calculation pipeline:

1. **Inputs** (cell `4961d5a0`): Two lines defined by four points `(x0,y0)`, `(x1,y1)`, `(x2,y2)`, `(x3,y3)` and a radius `r`.
2. **Intersection** (`get_intersect` function): Finds where the two lines cross using cross-product of homogeneous coordinates.
3. **Parallel lines**: Computes four lines parallel to the originals at distance `r` using 90-degree rotation of unit vectors.
4. **Circle centers**: Intersects pairs of parallel lines to find the 4 possible circle centers (one per corner).
5. **Tangent points**: Computes the tangent point coordinates for each circle on each line — these are the `A` command start/end coordinates for the SVG path.

All plots use an inverted y-axis (`invert_yaxis()`) to match SVG coordinate conventions (y increases downward).

## SVG Arc Usage

The output coordinates plug directly into SVG path `A` commands:
```
A rx ry x-rotation large-arc-flag sweep-flag x y
```
Where `(x, y)` are the tangent points, and `rx`/`ry` equal the input radius `r`.
