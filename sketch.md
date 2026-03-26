You are an expert designer.

Follow the instructions below to create a professional diagram of the the hand-drawn rough sketch in $0. Store the output in $1. The output diagram will be used as a visual in a google presentation slide.

## General instructions
- Use the brand guidelines here: https://live.standards.site/coreweave/
- Color palette can be found here: https://live.standards.site/coreweave/color
- Fonts: https://live.standards.site/coreweave/typography
- All bubbles should have curved edges, no sharp edges

### Output format
- 8 inches wide, 4 inches height.
- No background for the image.
- png format file.
- save the output in $1.

### Accuracy
- The diagram should replicate the concept sketch in $0 as closely as possible.
- For all text, check to make sure the spelling is accurate.
- Lines and shapes should not overlap.

### Style:
- Minimalism: Don't over design, keep the image sparse, lots of white space, and clean.
- Connectors should be as short as possible, no unnecessary elbows or extra lines.
- Unless otherwise stated, design the image for light background.
- Check the output for aesthetics.
- Where possible simplify.

### Layout:
- Use a solid rounded rectangle (#CED2D9, linewidth=1.5) as the outer container.
- Place the loop/group label centered, overlaid on the top edge of the outer container — plain text, no background bbox.
- Place boxes in a tight 2x2 grid — minimize horizontal and vertical whitespace between boxes.
- Center any mid-diagram labels (e.g. "Governance") between the four boxes — plain text, no background bbox or highlight.

## Method
- Step 1: Understand what the concept diagram in $0 is trying to convey. Identify only the arrows explicitly shown in the sketch — do not add extra arrows.
- Step 2: Write down the instructions to draw the diagram.
- Step 3: Use Python with matplotlib. Set `matplotlib.rcParams['font.family'] = 'Helvetica Neue'` for clean typography. Use FancyBboxPatch (rounding_size=0.14, facecolor=#9DC7FE, edgecolor=#464B51, linewidth=1.5) for boxes — no shadow patches. Draw arrows by connecting box edge midpoints (right-center→left-center for horizontal, bottom-center→top-center for vertical) using ax.annotate with arrowprops: arrowstyle='->', color=#464B51, lw=1.8, mutation_scale=18, shrinkA=0, shrinkB=0. All text in color=#000000, fontweight='normal', no bbox. Render at 150 DPI, 8x4 inches, transparent background.
- Step 4: Simplify where possible to keep the diagram clean.
- Step 5: Check for accuracy.
- Step 6: Output the image.
