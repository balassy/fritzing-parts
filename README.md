# Fritzing Parts

This repository contains various electronic parts I designed to be used in the Fritzing application.

## Parts

### [ZS-042 Real Time Clock Module - 4 pin only](<./ZS-042 Real Time Clock Module - 4 pin only>)
A simplified version of the ZS-042 (aka. HW-084) RTC module that is based on the DS3231 chip. This version supports only the 4-pin header that is often used to add the module to a PCB.

![Breadboard](<./ZS-042 Real Time Clock Module - 4 pin only/svg.breadboard.zs-042-4-pin-only_breadboard.svg>) ![Schematic](<./ZS-042 Real Time Clock Module - 4 pin only/svg.schematic.zs-042-4-pin-only_schematic.svg>) ![PCB](<./ZS-042 Real Time Clock Module - 4 pin only/svg.pcb.zs-042-4-pin-only_pcb.svg>)

## How to create Fritzing parts

Official guide: https://fritzing.org/learning/tutorials/creating-custom-parts

### Fritzing tips

- In the `part.[...].fzp` file every part needs a unique `moduleId`, preferably a GUID.
- Use the same unique ID in the file names, as they are copied to the `~\Documents\Fritzing\parts\svg\user` folder, so they do not overwrite files of other parts there.
- The `fritzingVersion` attribute is the version of the Fritzing application.
- In the PCB the pins should have copper on both layers. The easiest way is to create this hierarchy in the SVG file:

```svg
 <g id="copper1">
  <g id="copper0" fill="none" stroke="#ffbf0c" stroke-width="1.44">
    <circle id="gnd-pin" ... />
    <circle id="vcc-pin" ... />
  </g>
 </g>
```

- To test a new version of the part it must be first removed from Fritzing, then the app must be restarted, and then the module can be imported again. Otherwise import will fail because of existing module ID. 
- Add only the necessary labels to the PCB view. Users can add any custom text to the silkscreen layer, but cannot remove what comes with the part.

### Inkscape tips

- The `id` and the `label` are two different attributes in Inkscape, Fritzing relies on the `id`. In the Layers and Objects window Inkscape shows the `label`, you have to open the Object Properties window to set the `ID` and the `Label`.
- To save the SVG without Inkscape specific content in the Save As dialog select `Optimized SVG (*.svg)` as the type. Even in this format you can use VS Code to nicely format the content. 
- If the SVG contains a deep group hierarchy, instead of moving the objects from a deep location to a higher level always use Ungroup. This way transformations applied on the various group levels are resolved.

## Build and release process

This repository includes GitHub Actions workflows for packaging the Fritzing parts and publishing them as release assets.

When a commit is pushed to the `main` branch the workflow in `build.yml` is executed. It builds the .fzpz files from the parts folders and attaches them as build artifacts.

GitHub keeps build artifacts for 90 days.

When a release is created the workflow in `release.yml` is started. It builds the .fzpz files from the parts folders and attaches them as release artifacts.

The part packages are created with a reusable local action in the `./.github/actions/fritzing-create-part-package` folder. It first extracts the `module/version` attribute from the XML in the .fzp file and appends the version number to the artifact file, so it is **important to properly maintain the `version` attribute in the part file!**

Read more about the FZP file format here: https://github.com/fritzing/fritzing-app/wiki/2.1-Part-file-format

## About the author

This project is created and maintained by [György Balássy](https://www.linkedin.com/in/balassy).