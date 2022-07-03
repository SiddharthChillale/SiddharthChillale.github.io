---
title: "Rendering Pipeline of Unreal Engine"
date: 2022-02-23T21:47:05+05:30
draft: false
summary: A summary of the Rendering pipeline in Unreal Engine.
categories: [Graphics]
tags: [coding, data science, projects]
ShowToc: true
cover:
  image: "images/ue_rendering_schematic.png"
  alt: "dashboard[updated-1].png"
  caption: "A chart showing covid trends"
  relative: true
---

Below is an overview of the rendering pipeline that the Unreal Engine uses for rendering a single frame onto the screen. The following summary is written down as my understanding of the render engine of unreal given in the talk "Rendering Kickstart" on the Unreal Engine Community.

* Goal of the Unreal pipeline 
    - best visuals and best performance on all targets and platforms
    - supports both forward and deferred rendering

{{< figure src="images/ue_rendering_schematic.png" caption="Big world view of design schematic." >}}

* Schematic overview
    - **CPU on frame 0**: does work on physics, networking, animations, etc. then passes on the work to GPU
    - **Frame 1**: Computes the visibility of every object in the scene. what's seen and what's not seen by camera. this includes culing and types on culling.
    - **Frame 2: Rendering starts**
      - *Early z pass*: depth of objects are scanned
      - *BasePass*: Drawcalls on all meshes (static and dynamic) + base materials + light maps (static lights and shadows)
      - *GBuffer* : is a set of all buffers that is maintained by cpu for every frame
      - *Dynamic Lighting and shadows*: Direct and Indirect lights and shadows.
      - Reflections: Reflection captures, planar reflections, screen space (SS) reflections, ray traced reflections
      - *Additional* : 
        - Transparency
        - Translucency
        - Atmospheric fog
      - *PostProcessing*:
        - tonemapper
        - Blooom
        - SSAO
        - SSSSS
        - Depth of Field 
        - Exposure
        - Blendables
        - Camera Effects
    - **Final Frame: Performance**
        - GPU Profiler
        - Statistics
        - View modes
        - Scalability :
          - Using CVars for configs on different target platforms and devices. in-depth configs

The devs 

* Advices 
    - Shadow Rendering
    - Managing tris amd draw calls
    - Translucency
    - Material Cost
    - Forward / Mobile
    - Baking Light
    - Reflections
    - Scalibilty Cvars/Show