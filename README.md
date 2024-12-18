# Example of building a Beamer theme for a conference

Motivated by PEARC24, still a work in progress.

## Preparation

1. Opened the PowerPoint file from the [Presenters' FAQ](https://pearc.acm.org/pearc24/presenters-faq-2/) in actual PowerPoint for macOS.
2. Picked **View / Master / Slide Master** menu
3. Selected the primary master slide, right-clicked the background, Copy.
4. Opened Preview, **File / New from Clipboard**, resulted in a 13.35 × 7.5 inch document.
5. **File / Save**, named as `pearc24-background.pdf`.

Orange color for thumbprint logo is around RGB (230, 153, 72).
Blue color for slogan and contrast bar is RGB (37, 50, 130).

![](pearc24-background-sample.png)

Unrelated to the background image, going to use Karol Działowski's `scratch-example.tex` contents as a stress test for various types of slide content. I'll need to adjust it for a 16:9 aspect ratio, however, as that's what PEARC provided.

## Development and design

Four independent types of Beamer themes:

1. Outer
2. Inner
3. Color
4. Font

My usual inclination, as in [`beamerthemeCookeville`](http://github.com/mikerenfro/beamerthemeCookeville/), is to use Malmoe and a color scheme.
But the PEARC background already chews up a lot of space with the bottom left logo, and losing more space off the top for the section and subsection headings doesn't leave a great deal of space left for content.
So we'll try moving the headings into a narrow column on the left side above the logo.
Given I try to structure my presentations to have at most 5 sections and 5 subsections per section, we'll need space for up to 10 entries there.

From the Beamer User Guide, the Hannover theme resembles what I'm looking for in the left sidebar, and we can still use elements of Malmoe for the footline area provided by the PEARC background image.

Started by copying Karol's four theme files to `beamercolorthemePEARC24.sty`, `beamerouterthemePEARC24.sty`, `beamerinnerthemePEARC24.sty`, `beamerthemePEARC24.sty`, and removing anything to do with color or formatting, leaving only semantic or structural items.

### Making better use of the footline

By default, the footline contains a bunch of navigation symbols, but I usually prefer to hide those entirely and put the title, author, and maybe slide number information there instead.

Copy/pasting the footline template info from beamerouterthemesplit.sty into beamerouterthemePEARC24.sty mostly worked, except that I want to have the footline content centered vertically in the orange area.
As the orange area is approximately 27.5 points high, I increased the height of beamercolorbox to match, and then increased the  depth by about 50%.

### Fixing the footnote position

Looking through Karol's content, the footnote attached to one figure overlaps both the blue border between the footline and the slide content, and also the PEARC logo in the bottom left.
It's not too likely anyone at PEARC will need a footnote, but no reason to make them show up in an obviously wrong place.

We get rid of the footnote rule entirely since it's less applicable for a slide versus a long-form article.
We also use `addtobeamertemplate{footnote}` to adjust the position of the footnote text.
Both of these changes go into `beamerinnerthemePEARC24.sty`.

### Initial color scheme

The default Beamer colors for alerts, examples, blocks, etc. doesn't blend with the orange (RGB 230, 153, 72) and blue (RGB 37, 50, 130) for PEARC24.
We should be able to make good use of orange, blue, black, and white to get high contrast between text and background colors in all cases.
Defining the orange and blue colors in `beamercolorthemePEARC24.sty` and setting the hyperlink color to orange works, but hides the title in the footline, since that's technically a hyperlink to the title slide.
We can control the footline hyperlink color by adding `\addtobeamertemplate{footline}{\hypersetup{linkcolor=white}}{}` to `beamerouterthemePEARC24.sty` ([Changing URL colors in headline / footline of beamer template](https://tex.stackexchange.com/a/214090/3345)), making the hyperlink text contrast strongly with the orange background of the footline.
We'll also adjust the color of `\url{}` links by adding `urlcolor = PEARCOrange` to the `\hypersetup` command in  `beamercolorthemePEARC24.sty`.

### Headings on the left, above the PEARC logo

