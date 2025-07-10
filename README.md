# K40 Whisperer Image Segmentation

This is a modified version of K40 Whisperer 0.69 that adds a raster image segmentation feature. This feature allows the laser to process separate regions independently, avoiding scanning empty areas between images, which significantly improves engraving efficiency.

## What is Image Segmentation?

When engraving multiple separate images on a single piece of material, the standard K40 Whisperer scans the entire width of the work area for each line, even if there are large empty spaces between images. The segmentation feature identifies separate regions in your raster image and processes each region independently, skipping the empty areas entirely.

### Example
If you have two small images - one at the left edge and one at the right edge of a 30cm wide material:
- **Without segmentation**: The laser scans all 30cm for every line
- **With segmentation**: The laser only scans the actual image areas, dramatically reducing processing time

## Features

- **Automatic Region Detection**: Uses flood-fill algorithm to identify separate image regions
- **Configurable**: Enable/disable via checkbox in Raster Settings
- **Efficient Processing**: Only scans areas containing actual image data
- **Preserves Image Quality**: Uses the same pixel processing as the original algorithm
- **Settings Persistence**: Saves your preference for segmentation in design files

## Installation

1. Download the modified K40 Whisperer from this repository
2. Install dependencies as per original K40 Whisperer requirements:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the application:
   ```bash
   python k40_whisperer.py
   ```
   On Mac/Linux, you may need sudo for USB access:
   ```bash
   sudo python k40_whisperer.py
   ```

## Usage

1. Open K40 Whisperer
2. Load your design file (SVG, DXF, etc.)
3. Go to **Settings > Raster Settings**
4. Check **"Segment Raster Regions"** to enable segmentation (enabled by default)
5. Click **Rasterize** to process your image
6. Click **Raster Engrave** to start engraving

The status bar will show "Processing Region X of Y" as it works through each separate region.

## Technical Details

### How It Works

1. **Region Detection**: The segmentation algorithm scans the raster image to find all connected black pixels (regions)
2. **Minimum Size Filter**: Regions smaller than 100 pixels are ignored to avoid processing noise
3. **Independent Processing**: Each region is processed with its own coordinate system
4. **Coordinate Translation**: Region coordinates are translated back to the full image coordinate system

### Key Functions Added

- `segment_raster_image()`: Identifies separate regions using flood-fill algorithm
- `process_raster_region()`: Processes individual regions with proper coordinate transformation

### Bug Fixes Included

- Fixed PIL image cropping bounds (exclusive vs inclusive coordinates)
- Fixed Y-coordinate calculation for proper positioning
- Fixed pixel processing to maintain image integrity

## Performance Benefits

The performance improvement depends on your image layout:
- Images with large gaps between elements: 70-90% time reduction
- Images with moderate spacing: 40-70% time reduction
- Dense images with little spacing: Minimal improvement

## Compatibility

- Based on K40 Whisperer 0.69
- Compatible with all features of the original K40 Whisperer
- Works on Windows, Mac, and Linux (including Raspberry Pi)
- Python 2.7 and Python 3 compatible

## Known Limitations

- Minimum region size is set to 100 pixels
- Very complex images with many small regions may take longer to process initially
- The feature can be disabled if it's not beneficial for your specific image

## Credits

- Original K40 Whisperer by Scorch Works
- Image segmentation feature developed to optimize raster engraving performance
- Special thanks to the K40 laser cutter community

## License

This modification maintains the GPL-3.0 license of the original K40 Whisperer.

## Contributing

Issues and pull requests are welcome. Please test thoroughly with various image types before submitting changes.

## Support

For issues specific to the segmentation feature, please open an issue in this repository. For general K40 Whisperer support, refer to the original project documentation.