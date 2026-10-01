# Three.js hair shader for WebGL and WebGPU

An open-source hair-card shader for **Three.js `WebGLRenderer` and `WebGPURenderer`**. The WebGL path patches built-in materials with GLSL; the WebGPU path uses Three.js Shading Language (TSL) node materials. It handles strand coverage, depth, soft edges, root-to-tip colour, and anisotropic highlights.

Created by **[creategamecharacters.com](https://creategamecharacters.com)**. If you want to see how the hair renders, visit [creategamecharacters.com](https://creategamecharacters.com) and use the web-based character creator.

This repository contains the shader source and its MIT licence. Bring your own hair-card meshes and coverage texture. The Create Game Characters SDK and character assets are separate.

## Use it

Copy [`hair-shader.js`](src/hair-shader.js) into a Three.js project, or import it directly as an ES module. Pass the `THREE` namespace and your renderer so the shader selects the correct path and multisampling mode.

```js
import * as THREE from 'three';
import { applyHairShader } from './hair-shader.js';

const renderer = new THREE.WebGLRenderer({ antialias: true });
const handle = applyHairShader(hairRoot, {
  THREE,
  renderer,
  atlas: strandDataTexture,
  color: '#6b4b2a',
});
```

For `WebGPURenderer`, import `THREE` from `three/webgpu` and pass that renderer to the same function. See the official [Three.js WebGPURenderer guide](https://threejs.org/manual/pages/webgpurenderer) for its initialization.

The default compact atlas uses **R for strand coverage, G for root-to-tip position, and B for strand variation**. Set its colour space to `THREE.NoColorSpace`; this is data, not a colour image. `applyHairShader()` applies the material passes to meshes under the supplied root. `createHairMaterials()` builds the passes without modifying a scene. `applyHairStyle()` can read an independently hosted style folder's `mh_materials.json` and atlas.

With MSAA, an opaque alpha-to-coverage pass provides solid hair and a blended fringe softens the edges. Without MSAA, a depth-writing blended outer pass sits over an alpha-tested core. Both paths keep the hair opaque against the page background.

## Licence

[MIT](LICENSE). This licence covers the shader in this repository. Hair meshes, textures, and characters from other sources retain their own licences.
