# UDIM boundary validation

Open **Tools → UDIM boundary validator…** after scanning a texture folder. The set validator also offers **UDIM boundaries…**. Choose a map type (Displacement is selected when available), a stored channel, and a non-negative threshold, then **Analyze boundaries**. **All non-alpha** compares every stored channel except A/Alpha. Alpha is an explicit independent channel choice.

## What is compared

Pairs use the existing texture-set identity, map type and observed UDIM coordinates. Asset folders remain separate unless explicitly assigned to one texture set. Excluded, unparsed and untiled files are not paired. Only existing direct neighbors are checked; empty UV cells are not presumed missing. Right-hand adjacency never wraps from U=9 to U=0 of the next row.

- **U:** rightmost column of tile A against leftmost column of tile B = A+1.
- **V:** top image row of lower tile A against bottom image row of upper tile B = A+10. Image Y points down; UV V points up.

The test compares a single stored pixel line on each border. Pixel centers lie on opposite sides of the mathematical tile boundary, so smooth gradients can naturally yield a nonzero result. It does not compare padding bands, model UV topology, mesh adjacency, filtering footprints or vertex tangents, and does not check derivative continuity. Numerically adjacent UDIM cells may hold unrelated UV islands. Tangent-space normal maps may legitimately differ across a seam. Treat results as inspection warnings; the tool never modifies or repairs originals.

## Threshold and statistics

Values are native stored units, without OCIO, display exposure/gamma, integer normalization or resampling. PNG8 uses code values 0–255; PNG16 uses 0–65535. Float data retains its actual stored scale and sign. Choose the threshold for the map and encoding being inspected; one threshold is rarely appropriate for all map types. Only differences **strictly greater** than the threshold are counted.

The table reports maximum absolute difference and the number/percentage of edge positions above threshold. A position is counted once even if several channels differ. A position containing an invalid sample in any checked channel is reported as invalid and excluded from the threshold count. The percentage denominator is the full edge length; invalid positions are reported separately. Mean absolute difference and RMS use finite checked sample pairs. Changing a threshold or channel requires **Analyze boundaries** again; the finished report retains the exact threshold used. **Problems / above threshold only** filters the existing table without rereading pixels.

## Border preview

Select a connection to see A and B border strips in a shared display range and a maximum-difference strip. A/B show the first checked channel; the difference strip uses the maximum over all checked channels at each position. The channel name is shown above the strips. Hover to inspect the retained native A/B values, difference and source paths. U edge indices grow top-to-bottom; V edge indices grow left-to-right, both starting at zero. A dashed marker identifies the largest valid difference.

Red bins exceed the analysis threshold. Magenta bins contain invalid samples. At most 600 display bins are retained; each bin keeps its largest valid difference and the A/B samples at that location so narrow peaks remain visible. Statistics use every border position, not this reduced preview. **Open tile A/B…** opens either original map in the existing pixel inspector.

## Results, limitations and reports

- **Within threshold / Above threshold:** all requested boundary samples were checked.
- **Ambiguous:** more than one source candidate exists; no file is guessed.
- **Incompatible:** dimensions, origins, stored channel names/order or integer encodings differ. No resizing or channel guessing is performed. Float depths may differ; integer types must match and be at most 32-bit for exact subtraction.
- **Nonfinite:** NaN, Inf or a non-representable subtraction occurred; these are never treated as equal.
- **Unsupported:** missing requested channels, ambiguous layered-channel selection, multipart EXR, multipage TIFF or an unsupported streaming encoding. Split complex containers before inspection. PSD/PSB use the saved composite and the existing preparation cache; layers are not compared.
- **Error / Stale / Not checked:** decoder failure, changed original or unfinished/cancelled work. These are never clean results.

**Cancel** preserves completed connections and leaves unfinished edges Not checked. Decoder calls finish at their next bounded cancellation point. **Export CSV / JSON…** exports every connection, including filtered-out rows and explicit incomplete states. JSON also contains compact preview data and file revisions. Exports check whether originals changed/disappeared after analysis and mark affected rows Stale; any retained metrics are historical. Reports cannot replace original texture files.

## Large textures

Analysis reads native boundary samples through the bounded scanline/tile reader, never a complete image array. Some scanline encodings must still decode intervening rows; edge-only output does not guarantee low I/O. Persistent PSD/PSB composites may need preparation first. Existing streaming format and native-block size limits still apply. The compact retained results use memory proportional to connection count, not total image area.

A development benchmark used three generated 16384×16384 float32 tiled TIFFs (3 GiB decoded), two neighboring connections and isolated one-pixel changes. It retained both peaks and stayed under 100 MiB peak process working set on the test machine. This synthetic uniform-data benchmark is not a speed or memory guarantee for arbitrary codecs, channel counts or sets.
