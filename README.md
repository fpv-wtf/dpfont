### dpfont specification

The following is a proposal for a specification for MSP Displayport fonts.

### a dpfont package

A dpfont package shall be a folder of **.png** images of font pages for various flight-controller glyph maps with the following accompanying *dpfont.json* metadata file:

```json
{
    "name": "My Cool Font",
    "version": "1.0.0",
    "author": "Your Name <email@example.com>",
    "license": "ISC",
    "homepage": "https://github.com/awesomeuser/awesomefont",
    "supports": [
        {
            "identifier": "BTFL",
            "min_version": "4.4",
            "png": {
                "24x36": ["bf44_hd_1.png", "bf44_hd_2.png"],
                "36x54": ["bf44_sd_1.png", "bf44_sd_2.png"]
            },
            "bmp": {
                "24x36": ["bf44_hd_1.bmp", "bf44_hd_2.bmp"],
                "36x54": ["bf44_sd_1.bmp", "bf44_sd_2.bmp"]
            }
        },
        {
            "identifier": "BTFL",
            "min_version": "4.4",
            "platform": "walksnail", //(optional) denotes an override for a given vrx platform
                                     //for cases where the font contents is actually different
            "png": {
                "24x36": ["bf44_hd_1.png", "bf44_hd_2.png"],
                "36x54": ["bf44_sd_1.png", "bf44_sd_2.png"]
            }
        },
        {
            "identifier": "BTFL",
            "min_version": "4.0",
            "max_version": "4.3", //optional
            "png": {
                "36x54": ["bf40_sd.png"]
            }
        }
    ]
}
```

## On min_version and max_version

An implementation should always pick the font with the biggest valid `min_version` for a given FC identifier and version if multiple versions are available.

`max_version` is optional and can be used to restrict a font from being used with newer incompatible versions when a newer compatible version isn't available (as the prior `min_version` rule would take precedence regardless of max_version if a newer version is available).

Both `min_version` and `max_version` can be a partial semver string in order to allow for matching against minor patch releases that are released after the font update. This means for example that a `max_version` of "4.4.0" should not match against an FC version of "4.4.1", however a `max_version` of "4.4" should match against "4.4.1".

## On platform
The optional `platform` key denotes that a specific font entry is meant as an override for said font on a specific VRX platform. This should only be used when the font contents actually differs between multiple platforms that share the same glyph sizes. If an override for the current platform isn't found the VRX should use the default entry lacking the `platform` key.

As such the `platform` key should _not_ be used to provide BMPs for HDZero as this would necessitate addition of new paltform definitions should other (hypothetical) VRX platforms also need to use BMPs.

Current "standardized" platform values:
- wtfos
- walksnail
- hdzero

## Font layouts
- *PNG* fonts must use a 1x256 (1 column, 256 rows) grid layout indexing from the top down. Therefore a page PNG's dimensions should be 1x(glyph width) by 256x(glyph height).
- *BMP* fonts must use the HDZero layout.

### User friendly distribution

For the purpose of manual installation, dpfont folders should be distributed and installable as a ZIP file with the extension **.dpfont** with all files present in the root of the ZIP archive.
