# Project: Interactive Procedural Noise Visualizer

**Project Overview:**

This project is to develop a web-based interactive educational tool that visually demonstrates the generation of various procedural noise algorithms. The goal is to create a clear and engaging resource for artists, designers, and developers to understand how different noise types are generated and how they can be used in computer graphics. The project will be implemented in Rust using the `cgtools` library, specifically the `minwebgl` crate, and compiled to WebAssembly (WASM) to run entirely within a static web page. The final product will be a visually appealing and informative website that not only showcases different noise algorithms but also allows users to interact with them and explore their properties in real-time.

**Key Features:**

*   **Interactive Noise Visualization:** A central canvas will display the generated noise textures in real-time.
*   **Algorithm Selection:** A user interface will allow the selection from a comprehensive list of procedural noise types, including:
    *   Value Noise
    *   Gradient Noise
    *   Perlin Noise
    *   Simplex Noise
    *   Worley Noise (Cellular/Voronoi)
    *   Fractional Brownian Motion (fBm)
    *   Sparse Convolution Noise
    *   Gabor Noise
    *   Wavelet Noise
    *   Anisotropic Noise
*   **Step-by-Step Visualization:** For each noise algorithm, the visualization will be broken down into distinct stages to clearly illustrate the underlying process. For example:
    *   **Value/Gradient Noise:** Show the grid points with their random values or gradients.
    *   **Worley Noise:** Display the scattered feature points and the resulting Voronoi diagram.
    *   **fBm:** Visualize the individual noise octaves before they are combined.
*   **Parameter-Tweaking:** A control panel will expose key parameters for each noise type, allowing users to experiment and observe the effects of their changes in real-time. Parameters could include frequency, amplitude, octaves, persistence, lacunarity, and more.
*   **Educational Descriptions:** Each noise type will be accompanied by a clear and concise description of its algorithm, characteristics, and common use cases, based on the provided list.

**Technical Stack:**

*   **Language:** Rust
*   **Core Library:** `cgtools`
*   **Rendering:** `minwebgl` for WebGL rendering in the browser.
*   **Deployment:** The Rust application will be compiled to **WebAssembly (WASM)** and executed client-side within a **static HTML page**. No backend server is required.
*   **Frontend:** HTML/CSS for the user interface elements and page structure.

**Learning Objectives:**

*   Gain practical experience with Rust in the context of computer graphics.
*   Develop a deep understanding of procedural noise generation algorithms.
*   Learn how to use WebGL for 2D graphics programming.
*   Master the use of the `cgtools` and `minwebgl` crates for building web-based graphics applications.
*   Understand the workflow for compiling Rust to WebAssembly and integrating it with web technologies.
*   Develop skills in creating interactive and user-friendly web applications.
*   Gain experience in structuring and documenting a software project for educational purposes.

**Potential Extensions:**

*   **3D Visualization:** Extend the visualizer to demonstrate 3D noise on meshes.
*   **Performance Comparison:** Implement a feature to compare the generation speed of different noise algorithms.
*   **Code Snippets:** Provide automatically generated Rust code snippets based on the current noise settings.
*   **Export Functionality:** Allow users to export the generated noise textures as image files.
*   **Advanced Techniques:** Incorporate more advanced noise-related topics such as domain warping and noise-based animation.
