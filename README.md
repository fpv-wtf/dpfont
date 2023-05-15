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
    "homepage": "https://github.com/aesomeuser/awesomefont",
    "supports": [
        {
            "identifier": "BTFL",
            "min_version": "1.45",
            "png": {
                "24x36": ["derp_hd_1.png", "derp_hd_2.png"],
                "36x54": ["derp_sd_1.png", "derp_sd_2.png"]
            },
            // optional
            "svg": {
                "*": ["derp_1.svg", "derp_2.svg"],
            }
        }
    ]
}
```

### Todo

 - Mandatory PNG sizes? Master PNG size for local scaling? Otherwise still not universally portable.
 - Does it make sense to consider svgs up front?
 - max_version?

### User friendly distribution

For the purpose of manual installation, dpfont folders should be distributed and installable as a ZIP file with the extension **.dpfont** with all files present in the root of the ZIP archive.
