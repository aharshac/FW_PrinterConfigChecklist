# HB: Marlin_main.cpp


## Instructions
1. Refer Marlin issue [10390](https://github.com/MarlinFirmware/Marlin/issues/10390)
2. Copy code snippet before `mbl_probe_index++;` in `inline void gcode_G29() -> switch (state) -> case MeshNext -> if (mbl_probe_index < GRID_MAX_POINTS)`.


## Code
```clike

// Move close to the bed for the first point
if (!mbl_probe_index) {
	current_position[Z_AXIS] = Z_MIN_POS;
	buffer_line_to_current_position();
}

```