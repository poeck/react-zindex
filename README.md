# react-layered

Welcome to react-layered! If you've ever found yourself lost in the jungle of z-index layers, fighting the chaos of overlapping UI elements, then buckle up! This tiny, mighty package is your guide to taming that wild z-index safari in your React projects. 🌿👓

## Features

- 🔒 **Type Safe:** Built with TypeScript, offering that snug, error-proof comfort.
- 🪶 **Super Lightweight:** Less than 2KB. Using zero dependencies. It's almost like it's not even there!
- 🧘 **Easy Configuration:** Set up your layers once, use them with zen-like calm.

## Setup

### Installation

First, install the package using your favourite package manager:

```bash
npm install react-layered
yarn add react-layered
pnpm add react-layered
bun add react-layered
```

### Configuring Layers

Create a hook to configure your layers. This example sets up common UI layers like background, navigation, and modals:

```javascript
// hooks/useLayer.ts
import { useLayerConfig } from "react-layered";

// Initialize the layer configuration with an array of layer descriptors.
// Lower layers in the list have a higher z-index by default.
export default useLayerConfig([
  // Define simple layers using strings.
  "navigation",
  "footer",
  "dropdown",

  // Define a layer with additional properties using an object.
  { key: "tooltip" },

  // Allocate a range of zIndex values for a layer by defining 'slots'.
  // For example, allocate 100 zIndex slots for the "toast" layer.
  // You can find an example how to use the slots below.
  { key: "toast", slots: 100 },

  // Create composite layers with multiple sub-layers using the 'parts' property.
  // You can find an example how to access the parts below.
  { key: "modal", parts: ["backdrop", "content"] },
]);
```

## Usage

### Using the style object

```javascript
import useLayer from "../hooks/useLayer";

const MyNavigation = () => {
  const { style } = useLayer("navigation");
  return <div style={style}>This is a navigation!</div>;
};
```

### Using only the zIndex

```javascript
import useLayer from "../hooks/useLayer";

const MyTooltip = () => {
  const { zIndex } = useLayer("tooltip");
  return <div style={{ zIndex }}>This is a tooltip!</div>;
};
```

### Using different parts of a layer

```javascript
import useLayer from "../hooks/useLayer";

const MyModal = () => {
  const { style } = useLayer("modal");

  // Or like this:
  // const { style: backgroundStyle } = useLayer("modal.background");
  // const { style: contentStyle } = useLayer("modal.content");

  return (
    <div style={style.background}>
      <p style={style.content}>Hello, I'm on top!</p>
    </div>
  );
};
```

### Using multiple slots of a layer

```javascript
import useLayer from "../hooks/useLayer";

const MyToast = ({ index }: { index: number }) => {
  // Pass the index of the item as the second argument.
  // Make sure it starts counting from 0.
  const { zIndex } = useLayer("toast", index);
  return <div style={{ zIndex }}>This works with multiple toasts!</div>;
};
```

### Using different stacking contexts

```javascript
// hooks/useLayer.ts
import { useLayerConfig } from "react-layered";

export const useFixedLayer = useLayerConfig([
  "modal",
  "alert",
  "toast",
  "tooltip",
]);

export const useAbsoluteLayer = useLayerConfig([
  "navigation",
  "footer",
  "dropdown",
]);
```

## API

### `useLayerConfig(layers, options)`

Use this function to generate your own `useLayer` hook.

| Parameter | Required | Type                          | Description                                                            |
| --------- | -------- | ----------------------------- | ---------------------------------------------------------------------- |
| `layers`  | ✅       | (string &#124; LayerObject)[] | An array of LayerObjects or strings defining the layers in the system. |
| `config`  | ❌       | LayersConfig                  | An optional configuration object specifying additional settings.       |

#### LayerObject

| Property | Required | Type   | Default | Description                                      |
| -------- | -------- | ------ | ------- | ------------------------------------------------ |
| `key`    | ✅       | string | -       | The key of the layer.                            |
| `slots`  | ❌       | number | 1       | Extend the layer across multiple z-index levels. |

#### LayersConfig

| Property  | Required | Type    | Default | Description                                 |
| --------- | -------- | ------- | ------- | ------------------------------------------- |
| `start`   | ❌       | number  | 1       | The initial value to start the zIndex with. |
| `reverse` | ❌       | boolean | false   | Reverse the layer order.                    |

### `useLayer(key[, slot])`

This function is a custom hook that you can create using useLayerConfig.

| Parameter | Required | Type   | Description                                                                                   |
| --------- | -------- | ------ | --------------------------------------------------------------------------------------------- |
| `key`     | ✅       | string | The key of the layer.                                                                         |
| `slot`    | ❌       | number | The slot to be used, starting at 0. Applicable only when 'slots' is configured for the layer. |

```

```
