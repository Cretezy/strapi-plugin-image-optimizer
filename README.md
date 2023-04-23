# Strapi-plugin-image-optimizer

![License](https://img.shields.io/github/license/marlokessler/strapi-plugin-image-optimizer)

## Table of contents

- [Requirements](#requirements)
- [Installation](#installation)
  - [1. Extend Strapi's upload plugin](#1-extend-strapis-upload-plugin)
  - [2. Add config options](#2-add-config-options)
- [Config options](#config-options)
  - [Object `Config`](#object-config)
  - [Object `ImageSize`](#object-imagesize)
  - [Type `SourceFormat`](#type-sourceformat)
  - [Type `OutputFormat`](#type-outputformat)
  - [Type `ImageFit`](#type-imagefit)
  - [Type `ImagePosition`](#type-imageposition)
  - [Example config](#example-config)
- [Usage](#usage)
- [Found a bug?](#found-a-bug)
- [Contributors](#contributors-✨)

## Requirements

Strapi Version >= v4.6.x

## Installation

### 1. Extend Strapi's upload plugin

To make this plugin work, you need to enter the following code to `./src/extensions/upload/strapi-server.ts`. If file and folders do not exist, you need to create them. This code overrides the default image manipulation service of Strapi's `upload` plugin.

```typescript
// ./src/extensions/upload/strapi-server.ts

import imageOptimizerService from "strapi-plugin-image-optimizer/dist/server/services/image-optimizer-service";

module.exports = (plugin) => {
  plugin.services["image-manipulation"] = imageOptimizerService;
  return plugin;
};
```

### 2. Add config options

Configure the plugin in the `.config/plugins.js/ts` file of your Strapi project.

## Config options

### Object `Config`

| Option                  | Type                                 | Description                                                                                                                                                     |
| ----------------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `additionalResolutions` | `number[]` <br/> Min: 0              | Create additional resolutions for high res displays (e.g. Apples Retina Display which has a resolution of 2x). Default is `[]`.                                 |
| `exclude`               | `SourceFormat[]`                     | Exclude image formats from being optimized. Default is `[]`.                                                                                                    |
| `formats`               | `OutputFormat[]`                     | Specifiy the formats images should be transformed to. Specifiying `original` means that the original format is kept. Default is `["original", "webp", "avif"]`. |
| `include`               | `SourceFormat[]`                     | Include image formats that should be optimized. Default is `["jpeg", "jpg", "png"]`.                                                                            |
| `sizes`<sup>\*</sup>    | `ImageSize[]`                        | (required) - Specify the sizes to which the uploaded image should be transformed.                                                                               |
| `quality`               | `number` <br/> Min: 0 <br/> Max: 100 | Specific the image quality the output should be rendered in. Default is `80`.                                                                                   |

### Object `ImageSize`

| Option               | Type                  | Description                                                                                                                        |
| -------------------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `fit`                | `ImageFit`            | The image fit mode if both width and height are specified. Default is `cover`.                                                     |
| `height`             | `number` <br/> Min: 0 | The height of the output image in pixels. If only height is specified then the width is calculated with the original aspect ratio. |
| `name`<sup>\*</sup>  | `string` <br/> Min: 0 | (required) - The name of the size. This will be used as part of generated image's name and url.                                    |
| `position`           | `ImagePosition`       | The position of the image within the output image. This option is only used when fit is cover or contain. Default is `center`.     |
| `width`              | `number` <br/> Min: 0 | The width of the output image in pixels. If only width is specified then the height is calculated with the original aspect ratio.  |
| `withoutEnlargement` | `boolean`             | When true, the image will not be enlarged if the input image is already smaller than the required dimensions. Default is `false`.  |

### Type `SourceFormat`

```typescript
type SourceFormat =
  | "avif"
  | "dz"
  | "fits"
  | "gif"
  | "heif"
  | "input"
  | "jpeg"
  | "jpg"
  | "jp2"
  | "jxl"
  | "magick"
  | "openslide"
  | "pdf"
  | "png"
  | "ppm"
  | "raw"
  | "svg"
  | "tiff"
  | "tif"
  | "v"
  | "webp";
```

### Type `OutputFormat`

```typescript
type OutputFormat = "original" | SourceFormat;
```

### Type `ImageFit`

```typescript
type ImageFit = "contain" | "cover" | "fill" | "inside" | "outside";
```

### Type `ImagePosition`

```typescript
type ImageFit =
  | "top"
  | "right top"
  | "right"
  | "right bottom"
  | "bottom"
  | "left bottom"
  | "left"
  | "left top"
  | "center";
```

### Example config

```typescript
// ./config/plugins.ts

export default ({ env }) => ({
  // ...
  "image-optimizer": {
    enabled: true,
    config: {
      include: ["jpeg", "jpg", "png"],
      exclude: ["gif"],
      formats: ["original", "webp", "avif"],
      sizes: [
        {
          name: "xs",
          width: 300,
        },
        {
          name: "s",
          width: 768,
        },
        {
          name: "m",
          width: 1280,
        },
        {
          name: "l",
          width: 1920,
        },
        {
          name: "xl",
          width: 2840,
        },
      ],
      additionalResolutions: [1.5, 3],
      quality: 70,
    },
  },
  // ...
});
```

## Usage

When uploading an image in the media library, Image Optimizer resizes and converts the uploaded images as specified in the config.

## Found a bug?

If you found a bug or have any questions please [submit an issue](https://github.com/marlokessler/strapi-plugin-image-optimizer/issues). If you think you found a way how to fix it, please feel free to create a pull request!

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

A special thanks to [@nicolashmln](https://github.com/nicolashmln), whose package [strapi-plugin-responsive-image](https://github.com/nicolashmln/strapi-plugin-responsive-image) served as inspiration for this one.